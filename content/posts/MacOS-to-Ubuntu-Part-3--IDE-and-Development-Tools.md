---
title: 'MacOS to Ubuntu Part 3: IDE and Development Tools'
date: '2017-08-12T17:55:18.526Z'
excerpt: Index Post
thumb_img_path: >-
  images/MacOS-to-Ubuntu-Part-3--IDE-and-Development-Tools/1*_xBL8gwVTodWoymyL-jXtQ.png
layout: post
---
Index Post

[**Getting started on Ubuntu Mate as a long time MacOS user**  
*Work in progress. This index post will be expanded with subposts as they are finished by me.*medium.com](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98 "https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98")[](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98)

My development tools broadly encompass the following

1.  [IntelliJ and its subversions (WebStorm, PyStorm, …)](https://www.jetbrains.com/idea/download/index.html#section=linux)
2.  SourceTree
3.  [NPM and NVM](https://github.com/creationix/nvm)
4.  [Docker](https://www.docker.com/get-docker)
5.  [Slack](https://slack.com/downloads/linux)
6.  Screenhero (for remote pair programming)
7.  Dash (quick lookup of many libraries, barely used though)
8.  [Atom](https://atom.io/)
9.  [RStudio](https://www.rstudio.com/)
10.  [LaTeX](https://www.latex-project.org/get/)

Those that don’t need to be replaced are just links to the Linux version of the tool. So let’s replace the rest, which are to my positive suprise only very few

#### SourceTree replacement

For this I would recommend [GitKraken](https://www.gitkraken.com/download).

<iframe src="https://www.youtube.com/embed/ZKkMwTeAij4?feature=oembed" width="700" height="393" frameborder="0" scrolling="no"></iframe>

There is also a rich [stackexchange answer](https://unix.stackexchange.com/a/257662) with a lot of alternatives listed. IMHO GitKraken is the best free alternative to SourceTree. Plus it’s got a great dark theme!

#### Screenhero replacement

Screenhero only has clients for Windows and OSX. As it was [purchased by Slack](https://slack.com/screenhero), it will most likely be rolled out with [Slack’s screen sharing features](https://slackhq.com/screen-sharing-comes-to-slack-video-calls-cd9afe732014) piece by piece. For now, Google Hangouts or similar WebRTC based solutions would need to suffice.

#### Dash replacement

A little disclaimer: I barely used dash. So this might barely scratch the surface. It doesn’t cache the docs offline as Dash does and it’s probably not as powerful simply because it’s a web based solution. But it should be OK as a first go.

[**DevDocs**  
*DevDocs is a fast, offline, and free documentation browser for developers. Search 100+ docs in one web app: HTML, CSS…*devdocs.io](https://devdocs.io "https://devdocs.io")[](https://devdocs.io)

But then! I found this guy’s awesome post!

[**Zeal — The Dash alternative for Linux and Windows**  
*A while ago, I came across Dash, which is a great piece of software which makes browsing the documentation really easy…*medium.com](https://medium.com/mozaix-llc/zeal-the-dash-alternative-for-linux-and-windows-a3c770690042 "https://medium.com/mozaix-llc/zeal-the-dash-alternative-for-linux-and-windows-a3c770690042")[](https://medium.com/mozaix-llc/zeal-the-dash-alternative-for-linux-and-windows-a3c770690042)

And the corresponding [link to the zeal project](https://zealdocs.org/) And you know why this is awesome?

    apt install zeal

that’s why! It’s distributed in the default Debian repository.

![](/images/MacOS-to-Ubuntu-Part-3--IDE-and-Development-Tools/1*_xBL8gwVTodWoymyL-jXtQ.png)
