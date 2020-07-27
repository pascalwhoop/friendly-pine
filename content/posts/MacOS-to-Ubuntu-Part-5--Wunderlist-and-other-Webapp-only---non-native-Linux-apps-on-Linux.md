---
title: >-
  MacOS to Ubuntu Part 5: Wunderlist and other Webapp only / non native Linux
  apps on Linux
date: '2017-08-14T17:40:24.708Z'
excerpt: >-
  I recently switched away from iOS and therefore also away from Apples
  Reminders and Apples Notes. I liked them, they were good enough for…
thumb_img_path: >-
  images/MacOS-to-Ubuntu-Part-5--Wunderlist-and-other-Webapp-only---non-native-Linux-apps-on-Linux/1*v7QL8QQP8y1ZHSKYKwAdPg.png
layout: post
---
I recently switched away from iOS and therefore also away from Apples Reminders and Apples Notes. I liked them, they were good enough for what I wanted to do. But while Wunderlist doesn’t have all the platforms, Apple, as always, only has their own. So how do I get Wunderlist on the Linux?

Turns out they have a client for almost any platform except a native one for Linux. But I like my little dock at the bottom of the screen and I hate jumping through tabs to find a web-app I need. Sure, [Albert quickly lets me find it](https://medium.com/curiouscaloo/macos-to-ubuntu-part1-alfred-replacement-7864b4d26397) too using its chrome history plugin but I still like having native apps. So I remembered [Nativefier](https://github.com/jiahaog/Nativefier) on Mac and decided I’d see if it works for Linux too. Turns out it does.

So assuming you’ve got [Node and NPM installed](https://nodejs.org/en/download/) on your system, this should do the trick (remember to place a logo on your desktop)

    npm install -g nativefier  
     nativefier -n "Wunderlist" "[https://www.wunderlist.com/webapp](https://www.wunderlist.com/webapp)" -i ~/Desktop/wunderlist.png

you get the application. I created a folder ~/apps for all my custom applications and other apps that I have not installed through my package manager so they get backed up with my home folder.

Now just add the newly created app to the Mate Menu using right click → edit menus and then simply follow the instructions or check the screenshots

![](/images/MacOS-to-Ubuntu-Part-5--Wunderlist-and-other-Webapp-only---non-native-Linux-apps-on-Linux/1*v7QL8QQP8y1ZHSKYKwAdPg.png)

![](/images/MacOS-to-Ubuntu-Part-5--Wunderlist-and-other-Webapp-only---non-native-Linux-apps-on-Linux/1*seaLHFHO22kWnYAkTsgu4Q.png)

Now I have wunderlist as a first level application on my system. But really it’s just a frameless chromium window without a URL field. So it wrapps the wunderlist webapp. Still it’s a good webapp and why not use webapps? They are really rocking these days

![](/images/MacOS-to-Ubuntu-Part-5--Wunderlist-and-other-Webapp-only---non-native-Linux-apps-on-Linux/1*Zr7rSMs2mQL3fwuGtJr_vw.png)

In fact you can do this with any application that doesn’t have a native Linux app but a good webapp. Just use the nativefier CLI.

* * *

Please keep reading. Just pick a part in the index post

[**Getting started on Ubuntu Mate as a long time MacOS user**  
*Work in progress. This index post will be expanded with subposts as they are finished by me.*medium.com](https://medium.com/curiouscaloo/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98 "https://medium.com/curiouscaloo/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98")[](https://medium.com/curiouscaloo/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98)
