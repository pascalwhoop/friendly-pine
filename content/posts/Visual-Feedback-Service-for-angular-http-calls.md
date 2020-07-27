---
title: Visual Feedback Service for angular http calls
date: '2017-03-20T11:59:13.908Z'
excerpt: >-
  So I started writing a feedback service for angular’s http calls. The idea is
  simple. When my app does http calls, I want my spinner to…
thumb_img_path: >-
  images/Visual-Feedback-Service-for-angular-http-calls/1*3G8KwFIB37aCoGk9Qlx2HQ.gif
layout: post
---
**TL;DR:** A Service for hooking into the xhr events of all http calls. Use it to make spinners spin, turkeys dance or whatever floats your boat while your app fetches data and the user needs to know.

* * *

So I started writing a feedback service for angular’s http calls. The idea is simple. **When my app does http calls, I want my spinner to spin!**

Now you don’t want to constantly set a ‘loading’ flag in your components somewhere or manually show loading overlays all the time right? All you want is a small spinner in the top right that spins on activity. Maybe you also want a toast or some notification to be shown on completed transactions.

I started building this and wrote about it in [this blog post](https://medium.com/curiouscaloo/building-a-visual-feedback-service-for-angular-http-calls-with-material-f811da75142c#.n5u6x4m49), so if you want to read the full story, read that one first.

#### Version 2: Pulling the UI feedback code out of the API Service

In my previous post, you had to call the vFeedbackService from somewhere when you did an http call and pass it the observable. That was ugly! Sure you could get spinners and toasts in 2 lines of code, but you want 0 lines of code in your components! Just a spinner, is that too much to ask?

So new plan:

1.  Hook into the code that angular uses to make HTTP calls. Grab those events!
2.  Create an observable based on those events. Use it to notify spinner components of stuff going on
3.  Write a spinner component that hooks into this and spins every time when HTTP calls are going on. All other code is untouched

#### Digging through the Angular code

*If you don’t care about this, just skip below to the solution.*

Digging into the [http code of Angular 2](https://github.com/angular/angular/tree/master/packages/http/src), it becomes clear how it works and what to do. Http uses a *ConnectionBackend* which is used to create a connection. This connection is created using *BrowserXhr.build()* (in the browser). So if we wrap this, returning the original object (XMLHttpRequest) but hooking into its events, we’re good. We get all http requests and their open, load, error, abort and progress events.

#### Creating the new Service to subscribe to

To hook into the events, we override the *build()* method of *BrowserXhr* to ensure we grab the events before handing back the original object.

<script src="https://gist.github.com/pascalwhoop/bfc291d060cdc7a4dca27c033767e046.js"></script>

Now, we have something to subscribe to.

To ensure this is actually used, we of course also need to override the default BrowserXhr

<script src="https://gist.github.com/pascalwhoop/a2dc5aaecc10dc20f288257e33351feb.js"></script>

Now, we have successfully wrapped an angular internal injectable with our code!

Let’s build a spinner (I am using angular material) that shows on ongoing http events and hides when they are all completed/aborted/cancelled or otherwise closed

<script src="https://gist.github.com/pascalwhoop/5459445dda7f1458a4cad7a50fe9f34a.js"></script>

This can now be placed anywhere in the app, as often as you want and whereever a process should be indicated. It can also be any UI, all it needs is react to the events of the http calls.

The code above also ensures, the spinner only stops spinning on all http calls being finished. This is helpful, if users quickly perform calls or if they click once but this triggers 2+ http calls.

**The resulting UI**

![](/images/Visual-Feedback-Service-for-angular-http-calls/1*3G8KwFIB37aCoGk9Qlx2HQ.gif)

<figcaption>Link to Gif if it doesn’t work: <a href="https://cdn-images-1.medium.com/max/1440/1*3G8KwFIB37aCoGk9Qlx2HQ.gif" data-href="https://cdn-images-1.medium.com/max/1440/1*3G8KwFIB37aCoGk9Qlx2HQ.gif" class="markup--anchor markup--figure-anchor" rel="nofollow noopener" target="_blank">https://cdn-images-1.medium.com/max/1440/1*3G8KwFIB37aCoGk9Qlx2HQ.gif</a></figcaption>

If you like this, maybe you can help: I have built this and would like to share it. But I am unsure how to ‘correctly’ publish it as an npm module for others to just import and use.
