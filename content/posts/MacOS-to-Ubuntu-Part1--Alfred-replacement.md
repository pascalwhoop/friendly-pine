---
title: 'MacOS to Ubuntu Part1: Alfred replacement'
date: '2017-08-12T16:48:23.963Z'
excerpt: To see all the stories click below
thumb_img_path: images/MacOS-to-Ubuntu-Part1--Alfred-replacement/1*AyIjx-IxDjlK7h99n6kT2g.png
layout: post
---
To see all the stories click below

[**Getting started on Ubuntu Mate as a long time MacOS user**  
*Work in progress. This index post will be expanded with subposts as they are finished by me.*medium.com](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98 "https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98")[](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98)

> **Update 2018–07–14**: I updated this article after working on Linux for about a year now. Although I switched distro (Manjaro instead of Ubuntu), the tools are universal)

#### Replacing Alfred

Alfred on MacOS is probably responsible for half of my productivity increase in comparison to working with a windows machine. It is the gateway to anything I do.

1.  Open any app or file on my machine
2.  Copy paste several snippets then access the history of my clipboard
3.  quickly google something from anywhere in the system
4.  quickly run terminal commands from anywhere

But I found several articles claiming that there are alternatives so I dared to do the step. I found two alternatives that work, a simple GUI based approach and a more complex but customizable approach.

### GUI only tools

#### 1\. Albert

Opening any file or application is easy with Albert. It can search the file system with its files plugin and automatically looks for any installed application. With the `>` prefix, it runs shell scripts in a heartbeat.

[Albert](https://albertlauncher.github.io/docs/installing/) is an open source alternative to the OSX Alfred and takes a lot of inspiration from the UI of Alfred. But it’s also very different. It has an open ecosystem, it’s all Open Source and it supports dozens of themes. If non fit your fancy, add one yourself.

![](/images/MacOS-to-Ubuntu-Part1--Alfred-replacement/1*AyIjx-IxDjlK7h99n6kT2g.png)

#### 2\. CopyQ

Alfred core premium feature for me was the clipboard history. It’s not a novel idea but it’s an important feature that most OS don’t offer out of the box. On Linux, I found CopyQ to be the best alternative.

There is a [Github Feature request](https://github.com/albertlauncher/albert/issues/255) for Albert for this feature, but the consensus seems to be it’s not to be solved by Albert but rather by integrating Albert with something that is already good at it, namely [CopyQ](https://github.com/hluk/CopyQ).

The GitHub user BarbUk has created a [Gist](https://gist.github.com/BarbUk/d443d09c6649b4b1225c1d6b96d9c7fd) that you can drop into

    whereis albert #make sure albert is where I think it is  
    cd /usr/share/albert && mkdir external  
    touch copyq  
    vim copyq #paste the script  
    chmod +x copyq

This works, because Albert [supports external scripts](https://albertlauncher.github.io/docs/extending/external/)

![](/images/MacOS-to-Ubuntu-Part1--Alfred-replacement/1*Yb86c-ia8ZD_BK4aclK5TA.png)

But to be honest, this is not quiet what I want. I felt like the simplest way to do this is to leave those two apps be separate apps. And all I did was add a global keyboard shortcut.

![](/images/MacOS-to-Ubuntu-Part1--Alfred-replacement/1*YXy3GiY9kcPEUeV4nng7Vw.png)

#### 3\. Quickly search something online

Albert brings this out of the box. Not much to say. It just works.

![](/images/MacOS-to-Ubuntu-Part1--Alfred-replacement/1*G7zeo3Talwp93vtV9Q-WOQ.png)

#### 4\. quickly run terminal commands from anywhere

Same here

![](/images/MacOS-to-Ubuntu-Part1--Alfred-replacement/1*Qv2rDtw8T39IcS6Sh6lmIQ.png)

### Hands dirty but fun solution

This one is a bit more advanced and probably only suitable for those that feel comfortable in the Linux ecosystem. The tools required are the following

*   dmenu
*   dmenu-extended
*   rofi
*   pass
*   copyq
*   teiler

All my configurations for these tools are available on [Github](https://github.com/pascalwhoop/dotfiles). I will quickly summarize all of them here

*   **dmenu** is a CLI tool that opens a visual selection toolbar at the top of the screen. It’s standard with the i3 window manager and is a very minimalistic approach to opening applications. It’s highly customizable. But for me, it’s just the API spec that rofi, my preferred application, implements. Anything can be passed to dmenu via STDIN and it allows the selection of these alternatives. rofi implements this API so anything that works with dmenu works with rofi.
*   **dmenu-extended** allows searching the file system for any file. It also allows easy additions of online locations that you want to remember. simple enter `+https://github.com` and it remembers this so the next time you start typing, it will suggest it
*   **rofi** implements the dmenu API but offers much more beyond that. It allows switching between applications with ease, listing all open windows, running ssh connections, scripts etc. It is also the base UI for any CLI tool that makes use of it through the dmenu alias.
*   **pass** is a password manager for Unix. It stores all passwords in a local folder, encrypted with your private PGP key. Afterwards, you may synchronize these via Git(hub) with other devices like an android phone or other computers. it saves all passwords under `~/.password-store` and with `passmenu` you can look for your passwords with ease. passmenu usually uses dmenu but mine uses rofi
*   **teiler** is a screenshotting utility that uses dmenu/rofi. I bound it `Ctrl+Super_L+4` and it triggers a dialogue to determine area, window, delay and upload location.

To watch these things in action check this video

* * *

Overall, Linux offers something for everyone. Be ready to get your hands dirty with some config files of various styles (json, yaml, Xresources, bash, …) but in return you get a system that is tuned perfectly to your need!
