---
title: Switching from MacOS to Linux — 1 year later
date: '2018-07-14T09:34:41.843Z'
subtitle: "A collection of tools, links and utilities to get you\_started"
excerpt: 'A collection of tools, links and utilities to get you started'
thumb_img_path: images/Switching-from-MacOS-to-Linux---1-year-later/0*Wv7TRtDOt1jDLVPO.jpg
layout: post
---
![](/images/Switching-from-MacOS-to-Linux---1-year-later/0*Wv7TRtDOt1jDLVPO.jpg)

So you have used Mac for a while now but you’re not happy anymore? Thinking about switching? Perfect! Read on. I will split this guide into two parts: GUI people and CLI people. Generally, I recommend anyone that wants to use their mouse and click stuff to stick with a desktop environment that feels familiar. Anyone who dares supercharging their productivity for the cost of learning some new tools, should check out the second part.

### Part 1: GUI style Linux

Okay, you’re a Mac user but you’re not really aware of the common roots between Linux and Mac. Unix? Sure, heard of it, but what does it really mean? No idea. That is fine. You’ll get there. For now, this is what I recommend:

#### Distribution

If you have worked with Ubuntu before, great. Stick with it. It’ll be nice to have familiar tools like `apt-get` that you know and you’ve worked with before. Just go for the default installation which will give you GNOME, or if you feel like having it a bit more “standard” try Ubuntu Mate or Ubuntu XFCE. All versions are officially supported and stable.

*   [Ubuntu Mate](https://start.ubuntu-mate.org/)
*   [Xubuntu (XFCE)](https://xubuntu.org/)
*   [Ubuntu GNOME](https://www.ubuntu.com/)

#### Desktop Environment

These are not the same as a distribution. Most of the time, any distribution can be matched with any other DE. But there are “defaults” and “official versions” as well as variants that are community supported. Check the video below for an overview of a variety of DE. Maybe, before you switch, get Virtualbox and install a number of VMs to try a few and select the one you like the most before installing that as your daily driver on your new system

**Spotlight alternative**

Look for Albert. It’s a close cousin to Alfred, the MacOS closed-source tool. It’s great. But make sure you get the current versions, not the one that comes with the repos originally. See the [installation page](https://albertlauncher.github.io/docs/installing/) for details.

*   [Albert](https://albertlauncher.github.io/docs/)

**Browser**

Try Firefox. Seriously, try it. It’s fast, efficient and secure. It’s also Open Source and a lot less data collecting than Chrome. It has [end-to-end encrypted Sync](https://www.mozilla.org/en-US/firefox/features/sync/) and integrates nicely with Pocket. It’s the Linux browser *period.*

*   `sudo apt install firefox`
*   `yaourt -S firefox`

**Window Management**

If you’re a Mac user, you probably don’t really know what you’re missing. But I recommend you to look into this field. It may increase your productivity by a lot. Check out [PyGrid](https://github.com/pkkid/pygrid) for a simple window ordering method if you have a numpad. It will help you declutter your desktop in a heartbeat. If you’re willing to go the “keyboard masterrace” way, check my second part which describes the i3 window manager.

![](/images/Switching-from-MacOS-to-Linux---1-year-later/1*SdCfD-MG5kksJXpLOKptvg.gif)

<figcaption>PyGrid in&nbsp;action</figcaption>

*   [i3](http://i3wm.org/) (advanced)
*   [pygrid](https://github.com/pkkid/pygrid) (simple’ish)

**Applications**

Okay, this one is too generic. All (good) IDEs are available for Linux. Just Google them. Email? Thunderbird. Calendar? Calendar! If you’re coding, check out the [Jetbrains Toolbox](https://www.jetbrains.com/toolbox/app/). Otherwise, just explore.

**Photo Editing**

This one is a problematic field. There are people that swear GIMP is great. I never made friends with it. There are other applications like *Darktable* or *Polarr* but I have to say, I am a Lightroom kiddie. Unfortunately, they don’t support Linux. But they have a crazy good Webclient (seriously!). It’s not the same as the desktop variant but the editing tools are all there. Mostly. Check out [this post](https://pascalbrokmeier.de/technology/2018/05/30/How_to_get_lightroom_running_on_Linux_with_Webassembly_and_Nativefier.html) to see how to get a desktop-like Lightroom onto your Linux. Good internet connection is essential here though!

* * *

### Part 2: Role up your sleeves

Okay so you have a week or so of slack time and are willing to go all in? Or you know Linux already from years of CLI experiences on servers? Perfect! Try this path instead. All applications are configurable, take [my dotfiles](https://github.com/pascalwhoop/dotfiles) as a guideline or check the many dotfiles projects on GitHub for many other peoples setups

#### Distribution

Go Manjaro! Seriously, look it up, install it, give it a day and be amazed. Install `yaourt` , set an `alias y="yaourt"` and enjoy things like `y cuda` to install the cuda libraries or `y gitkraken` to install niche applications. the `AUR` (Arch User Repository) is fantastic! Make sure to scan the install scripts before you install anything, especially rare, small or uncommon packages.

#### Window Manager

i3! Or Sway! Take a day, watch some videos and learn how it works. Once you go there, you won’t go back. If you want some motiviation, check the [unixporn](https://www.reddit.com/r/unixporn/) section on reddit. They show how pretty Linux can look. Many i3 setups are there.

#### File Manager

Check out [ranger](http://ranger.github.io/). It’s a terminal based file manager with massive performance offerings. Make it yours with keyboard shortcuts and get productive.

#### Customization

I mentioned it before, but check out the [unixporn](https://www.reddit.com/r/unixporn/) section. Many styles are out there. Take my dotfiles and get hacking. The results can be stunning

![](/images/Switching-from-MacOS-to-Linux---1-year-later/1*pz_Q1eqPVuIm6pNvKE5k7g.gif)

<figcaption>theming based on background colors with&nbsp;pywal</figcaption>

#### Docker

Seriously, Docker! OMG it’s such a great invention. I have four systems right now, a 15" XPS, a Raspberry Pi, an Android and a home desktop with all my data. I keep things in sync with [syncthing](http://syncthing.net/) which is also available as a [container](https://hub.docker.com/r/linuxserver/syncthing/). But it can do much more. I manage my media with Plex, which again, is also available as a [container](https://hub.docker.com/r/linuxserver/plex/). Basically anything headless is out there as a Docker and if not, make one, it’s very simple. Mailserver, Webserver, Mediaserver, Dropbox, Syncthing, Pihole, you name it!

* * *

Take some time and slowly work through these programs to check out a small portion of all the cool stuff out there:

*   manjaro / arch linux
*   i3 / i3-gaps
*   polybar
*   conky
*   pywal
*   ranger
*   urxvt
*   mpd / ncmpcpp
*   zathura
*   yaourt
*   pacman
*   Zeal

That should do for a while :-)
