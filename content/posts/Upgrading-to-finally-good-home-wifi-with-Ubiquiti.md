---
title: Upgrading to finally good home wifi with Ubiquiti
date: '2016-12-23T15:06:27.416Z'
excerpt: >-
  I have been listening to the guys from geekspeak.org the last couple of weeks
  and I really enjoyed their shows. One that caused me to…
thumb_img_path: >-
  images/Upgrading-to-finally-good-home-wifi-with-Ubiquiti/1*xV51srkP45IwrY3zGlZk2w.png
layout: post
---
![](/images/Upgrading-to-finally-good-home-wifi-with-Ubiquiti/1*xV51srkP45IwrY3zGlZk2w.png)

I have been listening to the guys from [geekspeak.org](http://bit.ly/2hPEEpz) the last couple of weeks and I really enjoyed their shows. One that caused me to actually change something in my own life right away was the episode about the Ubiquiti EdgeRouter X. It’s a $50 router (a bit more expensive in Europe) that has enterprise grade software on it and allows you to do all sorts of cool stuff.

[**Home Networking - Ubiquiti EdgeRouter X - GeekSpeak for 2016-09-07**  
*I've been working on a zone initialization wizard. Ever since I got a couple EdgeRouter X's for home, I've been really…*geekspeak.org](http://geekspeak.org/episodes/2016/09/07 "http://geekspeak.org/episodes/2016/09/07")[](http://geekspeak.org/episodes/2016/09/07)

But first let me summarize what I had and where I wanted to go

**Quick disclaimer:** **I have not been paid for this!** I am just a geek with a girlfriend that told me I should write more since she thinks I am good at it (you may be the judge of that). Since I have used 100+ OpenSource frameworks and read tens of thousands of blog posts, now I want to start giving back! If you click on a link that links to amazon and you buy something, I might get money from it, but so far that has not yet happened so yeah, just enjoy the article.

### Current state and goals

Our previous setup was a modem from our cable provider with a 50 Mbit Down and 2.5Mbit upload link. We had a Linksys WRT610N v2 that supported 802.11n networking on 2.5 and 5Ghz. It was alright. I had DD-WRT on it of course but it was an old build and whenever I changed too much in the config the NVRAM would overflow, bricking the device and I would have to roll back to a previous backup. It worked but it was not pretty. Our use cases are that of classic student dormitory or just male dominated shared living with girlfriends coming over sometimes:

#### **What we had**

*   Wifi based network
*   Consumer grade infrastructure
*   **Streaming heaps of video**
*   one guy wanting to **game** QuakeLive with good ping and **minimal package loss**
*   **Uploading** photos from phones and cameras to fb, clouds, sending videos to friends etc.
*   3 Laptops, 3 Tablets, 3 Phones, 3–5 ‘IoT devices’ (pi, hue, 433mhz bridge, printer, camera) + guests and girlfriends.

The issue was that we have two floors. Our router stood in the living room next to the TV together with the RaspberryPi, Chromecast and Philips Hue. My office is upstairs and between those is a brick wall and a floor. Our Download dropped from 50Mbit to 20Mbit for me upstairs. I have a Macbook Pro 2013 with 802.11n.

#### **Where I wanted to get to**

*   **Keeping an eye on my IoT devices**: What traffic do they produce, where do they send their data to etc. This was because I read on some opening VPN tunnels to their manufacturers servers and I *do not* want to invite Philips and others into my home.
*   **Improve gaming lag** so my roommate stops laying out his 25m long Cat5 cable through all of our apartment.
*   **Manage times for playing games**. My roommate likes gaming. I mean he reeeeeally likes gaming. So he asked me to keep an eye on it so he doesn’t play too much and looses track of his college goals. Twice a week he wants to play, the rest of the time he doesn’t want to be able to.
*   Enable **high speed Wifi transfer speeds** for my upstairs office. This is, because I use [Resilio Sync](http://bit.ly/2hPAYnO) as an alternative to Dropbox, Google Drive etc. It’s a P2P Torrent Protocol based sync service that *does not send your data to some server*. It also syncs in the local wifi if it can. All my photos are synced to my Pi (NAS) and to my macbook without leaving my devices and wandering into some cloud.
*   **Analyze traffic** of my phones and devices. Here is where it gets tricky. I could also analyze my roommates stuff I know. But I don’t care about their behaviors so why should I? I am interested in what servers my devices talk to. But it is interesting to see how many GB are actually consumed by a Chromecast or an iPad over the time of a few weeks.

### Switching to Ubiquiti

I bought a [Ubiquiti EdgeRouter X SFP](https://www.amazon.com/gp/product/B012X45WH6/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B012X45WH6&linkId=0899190a06891a7a37ddc48306f72f78) and a [Ubiquiti Unifi Ap-AC Lite](https://www.amazon.com/gp/product/B015PR20GY/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B015PR20GY&linkId=a4c976467111fd973c5261bc1b903479) totaling something like €160 so about $US 170. Setup was quiet easy. I used the video below for a quick overview and then played around with it myself.

<iframe src="https://www.youtube.com/embed/SianDqAQaR0?feature=oembed" width="700" height="393" frameborder="0" scrolling="no"></iframe>

It was a Thursday so my roommate was all into his gaming session while I wanted to replace the router and do open heart surgery on our core infrastructure item. I asked him for twenty minutes. I plugged the old stuff out, hooked up the EdgeRouter, connected his computer (LAN for now) again, put the AP into eth4 and mine into eth0. Opening 192.168.1.1 I arrived at the setup wizard.

![](/images/Upgrading-to-finally-good-home-wifi-with-Ubiquiti/1*a-xv8A67WHcqnnaG0eyjqA.png)

Apply, switch cables again (me into eth1, WAN into eth0), reboot, done. My mate called from his room “working again” and I switched everything out in 5 minutes. I created quick posts for the next steps to keep this one short and modularize the information better:

*   Firewalls → [timed gaming](http://bit.ly/2h9nq2x)
*   Quality of Service (QoS) → Gamelag and IoT logging
*   Traffic Analysis → curiosity
*   Wifi AP → [better connectivity all around the house](http://bit.ly/2hPJ1RB) as described by [this guy](http://arstechnica.com/author/lee-hutchinson/) far better than I ever could in a short time
