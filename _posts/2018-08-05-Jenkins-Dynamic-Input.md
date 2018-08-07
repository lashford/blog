---
layout: post
img: /img/posts/jenkins.png
img-bg: /img/wake-up-01.jpg
category: blogs
postTitle: Jenkins Pipeline With Dynamic User Input
tags: [Jenkins, CI, Groovy]
---

I am a big advocate of continuous integration and delivery, however there are times when you don't want an automatic release of an artifact to a given environment, for example; perhaps you are doing a specific long running performance test, or you need to manually ensure a specific new feature is ready in the sales or demo environment.  Can these parameters be loaded dynamically, let's find out...

I came across this very problem and decided to improve the teams existing implementation of manually copying the Docker Image Tag into an input box and to allow this input to be dynamically populated with an external call to Artifactory (The Docker Repo used for this project).

So here is the gif of the working pipeline in Jenkins.

![jenkins-pipeline]({{ "/img/posts/jenkins-pipeline.gif" | prepend: site.baseurl }}){: class="img-responsive" style="margin: 0 auto;"}

### Artifactory Query API

Artifactory provides a query api that allows to query metadata of the artifacts stored in the repos, for my example I wanted to list the latest docker tags for a given image name and repo.  A simple `CURL` allows us to get access to the json result and parsing this result with JsonSlurper allows us to build a list of values to populate a selection box with.    

``` groovy
import groovy.json.JsonSlurper

def getDockerImages() {
    final API_KEY = "FOOBARAPIKEY"
    final REPO_NAME = "service-docker"
    final APP_NAME = "myapp"

    def cmd = [ 'bash', '-c', "curl -H 'X-JFrog-Art-Api: ${API_KEY}'
                https://artifactory.acme.co/artifactory/api/docker/${REPO_NAME}/v2/${APP_NAME}/tags/list".toString()]
    def result = cmd.execute().text

    def slurper = new JsonSlurper()
    def json = slurper.parseText(result)
    def tags = new ArrayList()
    if (json.tags == null || json.tags.size == 0)
      tags.add("unable to fetch tags for ${APP_NAME}")
    else
      tags.addAll(json.tags)
    return tags.join('\n')
}
```

Note: The privileges needed to use the Artifactory api seem to be quite high, especially considering you can use the UI to search as guest, but non the less i needed to provide an admin user, in order for the query to return results.

### Pipeline Syntax
Jenkins Pipeline offers the functionality to wait for a user to input a value before the pipeline continues, this seems a useful feature and allows us to meet our requirement for the allowing the user to pick which version of the app to deploy to the specified environment.

``` groovy
    script {
        // Show the select input modal
       def INPUT_PARAMS = input message: 'Please Provide Parameters', ok: 'Next',
                        parameters: [
                        choice(name: 'IMAGE_TAG', choices: getDockerImages(), description: 'Available Docker Images')]
    }
```

### Working Example

Ok; so given the above, let's pull all this together and create a Pipeline script that contains a dynamic call to Artifactory to get the latest images tags and allows the user to select the tag they want in to use. We will assign the result to an environment variable, so that it is usable in following stages in the pipeline.

You will notice I have also added a Timeout section to the input stage so that Jenkins agents don't get blocked by users starting jobs and not providing the required input.

As a final addition to the pipeline I have shown how to manually add choices input from an array of strings, in my use case used to specify the destination environment. Notice the strange input type of a newline separated string, utilising the groovy array `.join()` function to code is much more readable, i.e `['dev','qa', 'perf'].join('\n')` compared to `"qa\ndev\nperf"`

```groovy
import groovy.json.JsonSlurper

def getDockerImages() {
    final API_KEY = "FOOBARAPIKEY"
    final REPO_NAME = "service-docker"
    final APP_NAME = "myapp"

    def cmd = [ 'bash', '-c', "curl -H 'X-JFrog-Art-Api: ${API_KEY}' https://artifactory.acme.co/artifactory/api/docker/${REPO_NAME}/v2/${APP_NAME}/tags/list".toString()]
    def result = cmd.execute().text

    def slurper = new JsonSlurper()
    def json = slurper.parseText(result)
    def tags = new ArrayList()
    if (json.tags == null || json.tags.size == 0)
      tags.add("unable to fetch tags for ${APP_NAME}")
    else
      tags.addAll(json.tags)
    return tags.join('\n')
}

pipeline {
    agent any
    stages {
        stage("Gather Deployment Parameters") {
            steps {
                timeout(time: 30, unit: 'SECONDS') {
                    script {
                        // Show the select input modal
                       def INPUT_PARAMS = input message: 'Please Provide Parameters', ok: 'Next',
                                        parameters: [
                                        choice(name: 'ENVIRONMENT', choices: ['dev','qa'].join('\n'), description: 'Please select the Environment'),
                                        choice(name: 'IMAGE_TAG', choices: getDockerImages(), description: 'Available Docker Images')]
                        env.ENVIRONMENT = INPUT_PARAMS.ENVIRONMENT
                        env.IMAGE_TAG = INPUT_PARAMS.IMAGE_TAG
                    }
                }
            }
        }
        stage("Use Deployment Parameters") {
         steps {
                script {
                    echo "All parameters have been set as Environment Variables"
                    echo "Selected Environment: ${env.ENVIRONMENT}"
                    echo "Selected Tag: ${env.IMAGE_TAG}"
                }
         }
        }
    }
}
```

I found this functionality not completely obvious to discover and implement, so hopefully someone finds it useful and you save a little time.

Happy hacking,

Alex
