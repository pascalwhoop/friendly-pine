---
title: Set up a Wifi for gaming
date: '2016-12-23T16:28:53.476Z'
excerpt: >-
  So: I am a computer scientists / software engineer and my roommate likes
  playing high speed ego shooters that demand <20ms ping and…
thumb_img_path: images/Set-up-a-Wifi-for-gaming/1*wxt5Fs1nMOAe--0xtK6QaA.jpeg
layout: post
---
![](/images/Set-up-a-Wifi-for-gaming/1*wxt5Fs1nMOAe--0xtK6QaA.jpeg)

So: I am a computer scientists / software engineer and **my roommate likes playing high speed ego shooters** that demand <20ms ping and virtually no package drops. And **I don’t like Cat5 cables lying** everywhere. How can we be friends?

![](/images/Set-up-a-Wifi-for-gaming/1*u6ZCMmwc_WKr8lgLftf6Gg.gif)

<figcaption>him playing quake live with max 20ms ping and decent network stats (bottom right red&nbsp;box)</figcaption>

Well I tried a new approach, switching from consumer grade Wifi gear to professional stuff for pretty much the same price but with much more options. Here it goes: First read [this article](http://bit.ly/2hjPtkY) about what I bought and where I came from/ what my requirements are. This one is just for the specialised task of making gaming fast on wifi without loosing all the perks of it like, you know, using it too.

### 3 Ingredients of success

TL;DR?

1.  QoS on the router. Limit the Up/Download and have the router manage it
2.  Place the AP at a good place in the apartment, not behind the TV or under the couch table
3.  Enable QoS on the AP and prioritize UDP packages (still a todo for me actually)

#### QoS in EdgeMAX on the Router

The most important part is QoS on the router level. If you don’t do this, buying a cable doesn’t help you either. If I start uploading 3GB of movie material and my mate wants to game, I win, he looses. The bottleneck is on the provider level not on our router. So I set upload and download QoS queueing slightly below our contract speed (6Up, 120Down) to ensure we are the bottleneck (and in control of it). Now the router has a prioritized queue. Outgoing UDP packages from games get treated like pretty girls in a line in front of a club. They just go in/out as they please. The rest must wait.

![](/images/Set-up-a-Wifi-for-gaming/1*lT4jLOvOguIaB3BisF3XoQ.png)

<figcaption>UI View of smart QoS by&nbsp;Ubiquiti</figcaption>

#### AP Positioning

It’s actually surprisingly simple. We bought a [Ubiquiti EdgeRouter X SFP](https://www.amazon.com/gp/product/B012X45WH6/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B012X45WH6&linkId=0899190a06891a7a37ddc48306f72f78) and a [Ubiquiti Unifi Ap-AC Lite](https://www.amazon.com/gp/product/B015PR20GY/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B015PR20GY&linkId=a4c976467111fd973c5261bc1b903479) totaling something like €160 so about $US 170. I then placed the router where the old one used to sit and put the AP in the middle of the apartment using the ethernet cable that was cleverly hidden in a ceiling cable line that was there when we moved in. I fixed the cable in there with candle wax (I know right) but its invisible and it puts the AP in a way better position.

![](/images/Set-up-a-Wifi-for-gaming/1*qrHXXPboWlL39T4VPb86LA.jpeg)

<figcaption>AP mounted on ceiling in the middle of the living room (and apartment)</figcaption>

#### QoS on Wifi Level

We got the bottleneck under control and the access point is placed in a good spot. But what about large file transfers within the Wifi network (e.g. Plex video streaming from a Wifi connected NAS or Laptop to a Chromecast)? Well these still hurt. There is surprisingly [little information](https://duckduckgo.com/?q=qos+unifi&atb=v41-3ds&ia=web) on QoS for the UniFi AP except [this](https://help.ubnt.com/hc/en-us/articles/205221100-UniFi-How-is-QoS-and-prioritization-handled) which doesn’t help me much though. I want to prioritize UDP traffic on the AP so that my mates UDP packages from the game get through faster than the rest. Two options:

![](/images/Set-up-a-Wifi-for-gaming/1*Ls15sgYekhpMEptASTo_ig.png)

<figcaption>Setting Airtime Fairness on might&nbsp;help</figcaption>

A) Tell the AP to always make the other clients wait when his client wants to talk (I believe the term we are looking for is airtime fairness?)

B) Limit other clients throughput to a certain amount, although this kind of defeats the idea of getting a fast AP (if I limit my Macbooks throughput to 150MBit, then why get a 800Mbit AP?)

C) Prioritize UDP packages (I fear this part is too late. The problem is probably not that the packages aren’t sent to the router fast enough but that the client actually has to wait for other clients to finish talking → point A)

I talked to a guy from customer service and he suggested to make a feature request for this last one. [So I did](https://community.ubnt.com/t5/UniFi-Feature-Requests/Allow-for-QoS-for-gaming-e-g-UDP-package-priority/idi-p/1773709).

I think Airtime Fairness is the right way to go here. It basically says don’t let other clients block the talking time. This can be compared with an annoying political dude on a podiums discussion hogging the microphone so others can’t get their point across. Everyone gets 1 minute (or milliseconds in Wifi timing), no hogging. But it doesn’t quiet seem to cut it for us. When I send P2P packages from one machine to another, my mate is killing me with dozens of dropped UDP packages and ping spikes.

The first two steps already help A LOT but the last step is still open. To be continued …

* * *

**Quick disclaimer:** **I have not been paid for this!** I am just a geek with a girlfriend that told me I should write more since she thinks I am good at it (you may be the judge of that). Since I have used 100+ OpenSource frameworks and read tens of thousands of blog posts, now I want to start giving back! If you click on a link that links to amazon and you buy something, I might get money from it, but so far that has not yet happened so yeah, just enjoy the article.

Thanks for reading!
