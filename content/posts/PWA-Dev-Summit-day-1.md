---
title: PWA Dev Summit day 1
date: '2016-06-20T10:13:04.318Z'
excerpt: Key points of welcome talk by Alex Russell & Thao Tran
thumb_img_path: images/PWA-Dev-Summit-day-1/1*t-rR8C46AppAt9bEdEefow.png
layout: post
---
<blockquote class="twitter-tweet">tweet<a href="https://twitter.com/koenkivits/status/744826010118553600"></a></blockquote><script async="" src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

#### Key points of welcome talk by Alex Russell & Thao Tran

*   **Everyone is here**. At the start of the summit, its made clear, this is being implemented. Google, Microsoft, Mozilla, Samsung. This technology is going to be implemented and made available to browser plattforms soon.
*   **Distributing native software is HARD.** Here, the distribution model of web based applications is far easier. You click the link and the app gets loaded. New versions don’t need to be “updated” on the device. They just get shown once they are pushed by the dev team. The reach here is far better than native apps. Users just *surf the web* whilst you don’t constantly surf in apps. You install some, uninstall some but the behavior is different.
*   Claim: **On desktop native came first but was taken over by web**. Will this repeat on mobile?
*   Webapps must adhere to similar **UX contracts as native apps**. This means never to show this

![](/images/PWA-Dev-Summit-day-1/1*t-rR8C46AppAt9bEdEefow.png)

<figcaption>Blaming the users instead of showing in app offline notification with UX experience upheld</figcaption>

Three building blocks are needed to make mobile webapps a first level citizen on mobile

*   **(reliable) performance** that includes offline capibility. Here Service Workers come in(proxy, 2nd 3rd 4th time. cache)
*   **push notifications** that don’t require an actual app to be installed and work even when the user is not on the website
*   **homescreen metadata** to uphold the UX contract idea of a first level application

#### Reach, acquisition and conversion

These three criteria are focused on a lot. This is all nice and well for consumer oriented applications but how does it work for big company internal applications? Are progressive web apps still a better choice than native applications where top-down decisions are made and these three criteria aren’t relevant?

While these aren’t as important, its still important that the users are satisfied by the UX. Enterprise applications cannot deploy bad UX applications anymore since most users are used to consumer grade apps which are the benchmark for the enterprise apps to ensure adaption and satisfaction. The pros of service workers and progressive concepts therefore help the apps to be adopted.

#### Instant-loading offline-first progressive web apps the next generation part II uncovered by Jake Archibald

This guy ([blog](https://jakearchibald.com/)) is quiet a piece.

![](/images/PWA-Dev-Summit-day-1/1*co6T5h0dqk9IPBw4atxORw.png)

He promotes the idea of **offline-first:** → rest can follow. This is also helpful to handle both offline and “false online” problems. There is also a lot about a new conpcept concerning streams in JS. A good summary is [this post concerning streams](https://jakearchibald.com/2016/streams-ftw/).

He also talks about the “white screen of death”, which is when the app or website thinks its online but has a really slow connection and therefore takes forever to load. This is avoided with the offline cached app-shell using a service worker and some cached data, just like a native app.

<blockquote class="twitter-tweet">tweet<a href="https://twitter.com/rcanu/status/744830326506037248"></a></blockquote><script async="" src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

**How do android instant apps and PWA work together?** There is no conflict. They are similar concepts, but because of the different distribution model, a PWA actually includes the core ideas of an instant app. Therefore the idea of an AIA is actually the native environment catching up with modular webapps.

#### Live ressources

*   **Youtube Channel: x1**[youtube channel](https://www.youtube.com/user/ChromeDevelopers)
*   **Slack:** Sign up [here](https://www.google.com/appserve/mkt/p/J94GxHDPW8rcIQv_n0PYJ1aveeWUn8HsWWPCkZjsNAr4DSTtcGw=) and join the conversation on[chromiumdev.slack.com](http://chromiumdev.slack.com/)!
*   **Twitter:** Follow [@ChromiumDev](https://www.google.com/appserve/mkt/p/Pa8w1J-RANFRffjcVWusPk__fc1O5PwOLA0ZxVC21GLPTYKGFPpPwpTqz9Xn8dcl) and use #pwadevsummit.
*   **Google Plus:** Follow [GoogleChromeDevelopers](https://www.google.com/appserve/mkt/p/Y3FC6_s4yCKMzhlYG2C5rG_wq_3gzqldUMUQFIG7HO0snQ90Ylp6pOraBAPE0-YcaCSTt8TtYsGh7gWmpIKVCD9A_f0OyA==).
