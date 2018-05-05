# Lashford's Blog and Profile Site

This blog and profile site is written in Jekyll with Bootstrap handling the CSS, the site is hosted on Github Pages.

The Jekyll templates are compiled on the Github servers and the static files are served from there.

### Pre-Requisites

* Jekyll [install Instructions here](https://jekyllrb.com/docs/installation/)
* Bundle `gem install bundle`

### Contributing

Install all the jekyll dependancies used by this site,

```
bundle install
```

Generate the site and ensure everything renders as expected.

```
bundle exec jekyll serve
```

The site should be available at 127.0.0.1:4000.

`jekyll serve` does hot folder watching so changes are automatically compiled for viewing in the browser.

### Adding a new post

Posts are written in `kramdown` this is a superset of `markdown` go [here](https://kramdown.gettalong.org/syntax.html) for syntax and examples. Any Jekyll specific syntax is available [here](https://jekyllrb.com/docs/posts/)

Create a new file in the `_posts` directory, i.e `2018-01-01-blog-awesome.md`

At the top of the file you need to include *metadata* section that will describe what the post is about and provide information to the Jekyll engine, this metadata is called 'Front Matter' in the Jekyll world and is written in `YAML`, The front matter must be the first thing in the file and must take the form of valid YAML set between triple-dashed lines, here is an example of the metadata:

```
---
layout: post
title: "Cats Blog"
date: 2018-01-01
desc: "An awesome Blog"
tags: [Awesomeness]
---
```

Copy the above into your post file and customise as necessary. You can add as much content as you would like after the last triple dash,

### Pushing to Github Pages

Go to settings and enable github pages for your repo. This means when you push new commits to the repo, the Github Jekyll engine will process the markdown files and render the html via the github pages url.  The `_site` folder is never committed, this is a generated directory that is used by the Github pages web server.  

### Topology of the site

There are three main page templates, *index.html*, *about.md* & *404.html* this shows the two variants of the page styles, raw html or Markdown.  Any additional pages that are created at the top level will be added to the Navigation bar.

### Credits:

For more Jekyll details, read [documentation](http://jekyllrb.com/).
Images on the site from [Death to Stock Photos](https://deathtothestockphoto.com/)
This Jekyll theme used [Landing Page Jekyll theme](https://github.com/swcool/landing-page-theme) as reference.
Jekyll theme based on [landing-page bootstrap theme ](http://startbootstrap.com/templates/landing-page/) as reference.
This Jekyll theme used [Freelancer Jekyll theme](https://github.com/jeromelachaud/freelancer-theme/) as reference.
