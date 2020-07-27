---
title: Wrap http call in ionic loading to indicate blocking data fetch
date: '2016-07-12T10:43:43.287Z'
excerpt: >-
  Usually there are two types of data requests to the server. Those where the
  user has to wait and those in the background. While JavaScript…
thumb_img_path: >-
  images/Wrap-http-call-in-ionic-loading-to-indicate-blocking-data-fetch/1*UEWjnu30dUlisQzsSF6ghA.png
layout: post
---
![](/images/Wrap-http-call-in-ionic-loading-to-indicate-blocking-data-fetch/1*UEWjnu30dUlisQzsSF6ghA.png)

Usually there are two types of data requests to the server. Those where the user has to wait and those in the background. While JavaScript is all about async calls these days, there are moments when we actually want to block a user to interact with the app. Ionic apps are no exception and so I came of with this piece of code which I use in my ionic+firebase app.

Code and explanation below the gif…

![](/images/Wrap-http-call-in-ionic-loading-to-indicate-blocking-data-fetch/1*EU113pSiuhM3hm9QzGX5yw.gif)

Basically I have **one service which handles all my backend requests** to my server. I have several convenience functions defined which only take very few arguments and are exposed to my UI components. Each of those **functions has an additional flag called “blocking”**. If this is true, the service returns the observable but also subscribes itself to it. It additionally wraps the call in a showLoading() and hideLoading() which blocks the user interaction until the call is complete

    object(opts?:any, blocking?:**boolean**) {  
        **if** (blocking) **this**.l.showLoading(**this**.nc);  
        **let** obs = **this**.db.object(opts);  
        //hide loading on first answer from server and unsubscribe  
        **let** dispo = obs.subscribe(res=> {  
            //hack. sometimes this gets called synchronously which is annoying because dispo is not yet set..          
            **if** (blocking) **this**.l.hideLoading();  
            dispo.unsubscribe();  
      
        }, err=> {  
            **if** (blocking) **this**.l.hideLoading();  
        });  
      
        **return** obs;  
    }

The showLoading(nc : NavController) and hideLoading() functions then display the loading overlay. In case you’re wondering: *What about multiple async calls?* Well the implemented code keeps track of the show and hide calls and counts up / down

The full UI helper Component looks like this:

    **import** {Loading, NavController, Toast} **from** "ionic-angular";  
    **import** {Injectable} **from** "@angular/core";  
      
      
    @Injectable()  
    **export class** FeedbackHelper {  
      
        **constructor**(){  
            **this**.count = 0;  
        }  
      
        l:Loading;  
        count:**number**;  
      
        showLoading(navCtrl:NavController) {  
              
            **this**.count++;  
            //increase requester count by one and if already over 0, return  
      
            **if** (**this**.count-1 || **this**.l) **return**;  
            //count was 0, we create a loading and show it.  
            **this**.l = Loading.*create*({  
                content: "loading ..."  
            });  
            navCtrl.present(**this**.l);  
            console.log("show loading");  
        }  
      
        hideLoading() {  
            **this**.count < 1 ? **this**.count = 0 : --**this**.count;  
            **if** (!**this**.count && **this**.l) {  
                **this**.l.dismiss();  
                **this**.l = **null**;  
                console.log("hide loading ");  
            }  
        }  
          
    }

Full code repository is on Github: [ESN-Couchsurfing](https://github.com/pascalwhoop/esn-couchsurfing)

Looking for something like this in Angular Material? Read this post

[**Building a visual feedback service for angular http calls with material**  
*Building web applications using angular is fun, we all know that. With angular material, it’s also pretty to look at…*medium.com](https://medium.com/curiouscaloo/building-a-visual-feedback-service-for-angular-http-calls-with-material-f811da75142c "https://medium.com/curiouscaloo/building-a-visual-feedback-service-for-angular-http-calls-with-material-f811da75142c")[](https://medium.com/curiouscaloo/building-a-visual-feedback-service-for-angular-http-calls-with-material-f811da75142c)

* * *

Like what I post? **Follow me** on [Twitter](http://twitter.com/PascalBrokmeier) or [Github](https://github.com/pascalwhoop) to keep up with my Ionic2, TypeScript, IoT and Angular 2 posts.

[**Pascal Brokmeier (@PascalBrokmeier) | Twitter**  
*The latest Tweets from Pascal Brokmeier (@PascalBrokmeier). software developer, extreme sports and IT enthusiast…*twitter.com](http://twitter.com/PascalBrokmeier "http://twitter.com/PascalBrokmeier")[](http://twitter.com/PascalBrokmeier)

[**pascalwhoop (Pascal Brokmeier)**  
*pascalwhoop has 47 repositories written in Shell, JavaScript, and Ruby. Follow their code on GitHub.*github.com](https://github.com/pascalwhoop "https://github.com/pascalwhoop")[](https://github.com/pascalwhoop)
