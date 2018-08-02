---
layout: post
img: /img/posts/blogs.jpg
img-bg: /img/wired-07.jpg
category: blogs
postTitle: Lightning Voting
tags: [AngularJs, Bootstrap, Firebase]
---

So it's 2014 and the buzzword of the year seems to be `Serverless`, having computation as a service or the ability to have a stateful application without the need to run and manage a backend server, sounds an interesting idea.  Just as the cloud has revolutionised how we deploy and build new systems, will `Serverless` give us the flexibility to quickly and effortlessly run code without the overhead of a server.

This is a write up of the 7 minute lightning talk on FirebaseAPI & AngularJS, to create a super simple voting app with only a few lines of code and no ops work.  A little bit of fun to show the power of `Serverless` architectures in 7 mins.

### The Power of the web

**AngularJs** is a client side MVC style framework written in Javascript. The framework adapts and extends traditional HTML to better serve dynamic content through two-way data-binding that allows for the automatic synchronisation of models and views. As a result, AngularJS de-emphasises DOM manipulation and improves testability.

**Firebase**, Don't just save data. Sync it.
When data changes, apps built with Firebase update instantly across every device -- web or mobile.
Firebase-powered apps work offline. Data is synchronised instantly when your app regains connectivity.

In a traditional AngularJS app you may talk to a RESTful backend, Firebase replaces the need for this backend and instead you simply communicate with Firebase directly.  Firebase provides a helpful AngularJs client library to interact with this service.

**AngularFire** also lets you automatically synchronise any changes to a local JS object to Firebase without having to explicitly call any methods at all. We call this "3-way data binding". Much like Angular provides 2-way data binding between JS models and the DOM, AngularFire provides a way to automatically keep JS models and Firebase in sync.

### The Demo app

[Lightning Voting](http://lashford.github.io/lightning-voting/) app shows a simple web application published to Github pages and providing a working integration with firebase Api.  The source is available [here](https://github.com/lashford/lightning-voting) and a quick overview of the code is coming up.

### The Code

Add the dependencies for **Angular**, **Firebase** and the **AngularFire**.

`<script src="http://ajax.googleapis.com/ajax/libs/angularjs/1.2.18/angular.min.js"></script>`

`<script src="https://cdn.firebase.com/js/client/1.0.15/firebase.js" ></script>`

```
<script src="https://cdn.firebase.com/libs/angularfire/0.7.1/angularfire.min.js" ></script>
```

Mix in the firebase module into the angular app, `angular.module("awesome-voter", ["firebase"])` this allows you to call the firebase service and store data, we use the `bind` method to give us 3-way binding of the data stored in the `votes` bucket and angular variable in the view.

The potential for this service is huge and the number of applications covers a huge spectrum, I'm sure firebase will be continue to grow and become a staple in the `Serverless` ecosystem.

Cheers

Alex.
