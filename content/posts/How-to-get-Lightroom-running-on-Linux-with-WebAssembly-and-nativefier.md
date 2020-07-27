---
title: How to get Lightroom running on Linux with WebAssembly and nativefier
date: '2018-05-30T00:00:00.000Z'
excerpt: >-
  So… Adobe we need to talk: Your stuff is great, pretty pricy and we all know
  you’re trying to lock us in, but hey. Can’t blame you. But! No…
thumb_img_path: >-
  images/How-to-get-Lightroom-running-on-Linux-with-WebAssembly-and-nativefier/1*8vc_eAr46MxYZaHM2FI9Uw.jpeg
layout: post
---
![](/images/How-to-get-Lightroom-running-on-Linux-with-WebAssembly-and-nativefier/1*8vc_eAr46MxYZaHM2FI9Uw.jpeg)

<figcaption>Artists paintings in Naples,&nbsp;Italy</figcaption>

So… Adobe we need to talk: Your stuff is great, pretty pricy and we all know you’re trying to lock us in, but hey. Can’t blame you. But! No love for Linux users? Well I’ll help you out.

Adobe released Lightroom for the Web. It’s based on [webassembly](https://en.wikipedia.org/wiki/WebAssembly) which (if you don’t know) is pretty awesome stuff. Adobe has Lightroom compiled for the web and it’s available [here](https://lightroom.adobe.com/). They also have a [news channel](https://lightroom.adobe.com/news) which keeps you up to date with the latest updates for the web version. If you have a decent internet connection and don’t mind uploading your images to Adobe servers for editing, then go ahead and give it a shot. But wait. I know you want it as a destkop application right? So here is a little trick:

*Make sure you have NodeJS in a recent version installed and also ensure you have* `*npm*` *in your path. If you don’t know what that is give* [*this*](https://nodejs.org/en/download/) *a go. Got it? Good. Now run the following in a folder where you usually store portable apps (I got mine in* `*~/Apps*`*):*

    `npm install -g nativefier   
    nativefier -n lightroom-web [https://lightroom.adobe.com/](https://lightroom.adobe.com/)` --file-download-options '{"saveAs": true}'

now cd into the new folder and run the executable `lightroom-web`. Log in with your Adobe account and give it a go. should work right out of the box! Below is a screenshot of me running lightroom to edit a picture inside of my i3-gaps-wm.

![](/images/How-to-get-Lightroom-running-on-Linux-with-WebAssembly-and-nativefier/1*SzI5E1Hbpit1gDfxcDQPCg.png)

<figcaption>running lightroom on debian+i3 like a&nbsp;champ</figcaption>

* * *

*Originally published at* [*pascalbrokmeier.de*](https://pascalbrokmeier.de/technology/2018/05/30/How_to_get_lightroom_running_on_Linux_with_Webassembly_and_Nativefier.html) *on May 30, 2018.*
