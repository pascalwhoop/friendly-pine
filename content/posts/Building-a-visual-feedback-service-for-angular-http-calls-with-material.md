---
title: Building a visual feedback service for angular http calls with material
date: '2017-03-17T11:37:36.720Z'
excerpt: >-
  Building web applications using angular is fun, we all know that. With angular
  material, it’s also pretty to look at. So let’s build a…
thumb_img_path: >-
  images/Building-a-visual-feedback-service-for-angular-http-calls-with-material/1*xV_cJ_dU7Am0zH0Zq2dXlA.png
layout: post
---
![](/images/Building-a-visual-feedback-service-for-angular-http-calls-with-material/1*xV_cJ_dU7Am0zH0Zq2dXlA.png)

Building web applications using angular is fun, we all know that. With angular material, it’s also pretty to look at. So let’s build a service that gives us some convenience regarding visual feedback for users while performing http requests.

**Update: I improved this** [**here**](https://medium.com/curiouscaloo/visual-feedback-service-for-angular-http-calls-52f820e90c36#.4opipknc9)**.** Read on for educational purposes, but make sure to read the article linked as well since it makes the whole code prettier and more architecturally neat.

* * *

Angular Material has a cool [progress spinner component](https://material.angular.io/components/component/progress-spinner) and it also includes toasty notifications that pop from the bottom which they call [snack bar](https://material.angular.io/components/component/snack-bar).

![](/images/Building-a-visual-feedback-service-for-angular-http-calls-with-material/1*BDFDL83jaIY4M2aMhv4eFw.gif)

When performing simple CRUD operations on an arbitrary data structure, wouldn’t it be nice to give a generic feedback system that can be used while developing? Every time we have an http request, it gives feedback, in the form of a progress spinner and optionally also with these little pop up notifications that tell us if everything went right or if problems occurred.

#### Setting up and planing using tests

We need a service that wraps the MdSnackBar Service provided by the MaterialModule. This service can then also manage any spinner components that can register as a listener to this service and get notifications of any ongoing http calls so that they show visual feedback using the spinner. So we create these two:

*   VFeedbackService
*   WorkingSpinnerComponent

First let’s look at the VFeedbackService or rather its spec since we do this test driven:

<script src="https://gist.github.com/pascalwhoop/fb717f20a459c2cac2ee37f7874249dc.js"></script>

As is visible from the specs, the service manages a list of listeners and offers http services to hand over their observables so that the service gives feedback once they succeeded or failed.

The tests use angulars async helper functions to properly test all the observables and such. They mock away the SnackBar service as well as any listener components that subscribe to the service

Now that we have the spec, we can simply implement the service. It’s straight forward from the specs but I’ll share it anyways

<script src="https://gist.github.com/pascalwhoop/12ba5d3717293e4cd907033392ec1957.js"></script>

This service is not yet perfect. If you have several http requests running in parallel, the first one that completes stops all listeners and therefore stops them from spinning. This can be averted by keeping track of the number of calls to spinUntilCompleted and only notify all listeners of a stop, once all subscriptions are completed. Feel free to add that to your code if you have the need for it.

Now let’s look at the spinner components. They are so simple, writing tests for them is almost not worth it. I skipped it.

<script src="https://gist.github.com/pascalwhoop/0baf89b40dd27f6df51701ba363a9f93.js"></script>

#### Using it in our services

<script src="https://gist.github.com/pascalwhoop/006ca7d3d5c4e6d20126b0de43597088.js"></script>

as you can see, we just create an http call and then pass it to our service so it can hook into the event process. If we only want spinners, we just use spinUntilCompleted, if we also want a message, we call both methods.

It’s important to note here that failing to ensure you are working with a single subscription (using .share() or .publish() ), the VFeedbackService triggers a secondary http call by subscribing to your observable. So be sure to keep that in mind.

#### Next steps

Next, it would be nice to hook into the http calls event system to automate the spinner and clean this code out of our services. The http service doesn’t care about the visual feedback, it just does what it has to do. But our spinner should be able to know when http calls are performed and act upon it.

**Option A:** Wrap http and use the wrapping service instead of http directly. A similar approach has been chosen by [Auth0](https://github.com/auth0/angular2-jwt), to attach a jwt token to every http request to ensure its authenticated.

**Option B**: Hook into the stuff that angulars http uses behind the scences, namely BrowserXhr. I will investigate this in a future post.

#### Wrapping it up

I showed how to create a service that can quickly be used to allow for user feedback using angular material in angular 2 projects. You can replace my component using an md-progress-spinner by a dancing chicken or whatever you please. My suggestion:

![](/images/Building-a-visual-feedback-service-for-angular-http-calls-with-material/1*5DWQPvY61CZoTa0zdOJdWg.gif)

<figcaption>probably the must amusing loading animation I have seen, by <a href="https://dribbble.com/xjw" data-href="https://dribbble.com/xjw" class="markup--anchor markup--figure-anchor" rel="nofollow noopener noopener noopener" target="_blank">https://dribbble.com/xjw</a></figcaption>

In this spirit: Happy coding!
