---
title: Using virtualScroll in Ionic 2 with <ion-card>
date: '2016-07-11T11:01:01.977Z'
thumb_img_path: >-
  images/Using-virtualScroll-in-Ionic-2-with--ion-card/1*Qy3ylWTvZA0EvD3aLuFHgg.gif
layout: post
---
**Update: This article is over 8 months old now and Ionic 2 has developed a lot since then. Please don’t consider this information to be valid anymore, the team has worked hard and (without checking if it works now) I don’t want to criticize something that might very well have been fixed by now! Instead consider one of my newer posts about Ionic**

[**Using Ionic DeepLinker for Webapps**  
*I wanted to add deep routing capability to a website that I made using Ionic. It was supposed to be a mobile webapp…*medium.com](https://medium.com/curiouscaloo/using-ionic-deeplinker-for-webapps-ba8803e76916 "https://medium.com/curiouscaloo/using-ionic-deeplinker-for-webapps-ba8803e76916")[](https://medium.com/curiouscaloo/using-ionic-deeplinker-for-webapps-ba8803e76916)

[**Fixing wrap ion-row on iOS with ion-col without defined width**  
*I ran into this problem the other day:*medium.com](https://medium.com/curiouscaloo/fixing-wrap-ion-row-on-ios-with-ion-col-without-defined-width-92073f08b042 "https://medium.com/curiouscaloo/fixing-wrap-ion-row-on-ios-with-ion-col-without-defined-width-92073f08b042")[](https://medium.com/curiouscaloo/fixing-wrap-ion-row-on-ios-with-ion-col-without-defined-width-92073f08b042)

* * *

Context: I am writing a small platform for a closed community (~5000–50k ppl) to allow couchsurfing within this community. It doesn’t do much. Posts, comments, linking to FB and most importantly filtered notifications.

TL;DR: add some css into the ion-card and put a spacer below

Using virtual scroll in ionic is useful when you want to display very long lists. What does it do? Read up the article below to find out.

[**Boosting Scroll Performance in Ionic 2**  
*Follow @joshuamorony ·Scrolling is one of the most common interactions with a mobile application, and it is extremely…*www.joshmorony.com](http://www.joshmorony.com/boosting-scroll-performance-in-ionic-2/ "http://www.joshmorony.com/boosting-scroll-performance-in-ionic-2/")[](http://www.joshmorony.com/boosting-scroll-performance-in-ionic-2/)

To get some quick c&p ready code snippets I went [here instead](https://www.raymondcamden.com/2016/04/25/an-example-of-virtualscroll-and-infinite-scroll-in-ionic-2/). This guy was just straight to the point. But now came my problem: virtualScroll is made for <ion-list><ion-item></ion-item></ion-list> type of trees. I wanted a list of cards, no ion-item tags. They add lines, change margins and generally screw up the design of the card.

This is what I did:

    <ion-list [virtualScroll]="posts" no-lines>  
        <div *virtualItem="let post" style="width: 100%">  
            <post-card [post]="post"></post-card>  
            <div style="height:10px"></div>  
        </div>  
    </ion-list>

Basically you use a container div as a repeating container and make it 100% width again. Then you add a 10px spacer below each element, to fix the margin-folding, because virtual scroll seems to use some js to figure out how high the element is when its rendered and then puts a wrapper around it with a fixed height. This doesn’t bother with the ion-card margins though, so the spacer fixes the design.

You can change the inline css to make it more generalistic (css tag on \*\[virtualScroll\]>post-card e.g.) but this is the easiest to understand.

Here is the difference (old vs new):

![](/images/Using-virtualScroll-in-Ionic-2-with--ion-card/1*Qy3ylWTvZA0EvD3aLuFHgg.gif)

<figcaption>old</figcaption>

![](/images/Using-virtualScroll-in-Ionic-2-with--ion-card/1*3tmjpEVU4MewhsccfSz47g.gif)

<figcaption>new (with annoying jump bug)&nbsp;😏</figcaption>

There’s not really much to see aside from the fact that the new one is jumping back to the top which is an unpretty thing. I’m still working on this. But the benefit is that I can now handle huge lists (which I don’t have yet because my app is still not public and therefore posts aren’t rolling in yet.
