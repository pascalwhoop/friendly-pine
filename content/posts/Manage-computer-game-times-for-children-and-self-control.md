---
title: Manage computer game times for children and self control
date: '2016-12-23T15:05:49.051Z'
excerpt: >-
  This is a story that many parents know and that even many gamers have thought
  about: Using extrinsic control to reduce the amount you (or…
thumb_img_path: >-
  images/Manage-computer-game-times-for-children-and-self-control/1*Pg2_EwFDsM5cGInDMUJNSg.jpeg
layout: post
---
![](/images/Manage-computer-game-times-for-children-and-self-control/1*Pg2_EwFDsM5cGInDMUJNSg.jpeg)

<figcaption>Lovely depiction from <a href="https://pixabay.com/en/users/Alexas_Fotos-686414/" data-href="https://pixabay.com/en/users/Alexas_Fotos-686414/" class="markup--anchor markup--figure-anchor" rel="nofollow noopener" target="_blank">https://pixabay.com/en/users/Alexas_Fotos-686414/</a></figcaption>

This is a story that many parents know and that even many gamers have thought about: Using extrinsic control to **reduce the amount** you (or your kids) spend **in front of the PC**, be it gaming, social networking or other addictive behaviors.

I want to show how I set up a system to manage my best friends gaming behaviors who asked me to help him focusing on the important stuff studying mainly).

#### What you need

*   Router with flexible firewall settings. Many high priced consumer models have these, DD-WRT has them too, but I would recommend a combination of the [Ubiquiti EdgeRouter X SFP](https://www.amazon.com/gp/product/B012X45WH6/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B012X45WH6&linkId=0899190a06891a7a37ddc48306f72f78) and a [Ubiquiti Unifi Ap-AC Lite](https://www.amazon.com/gp/product/B015PR20GY/ref=as_li_qf_sp_asin_il_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B015PR20GY&linkId=a4c976467111fd973c5261bc1b903479). The combination costs about € 160 and gives you the option of flexibly positioning the Wifi Access Point and leaving the router somewhere hidden.
*   Some time, depending on your skillset that might be more or less and depending on your salary that might be more or less valuable
*   Someone or something to manage. Remember doing this just for fun is cool and all, but it better actually do any good because limiting your cats computer consumption is not helping anyone

I will describe how this works for Ubiquiti which works with iptables afaik so this might be different for DD-WRT although I know it works there too (I have actually done this with DD too, just wasn’t happy with rest of the router but that is Cisco/Linksys fault).

#### Let’s only allow computer games twice a week for a certain time period

Go to your routers IP and navigate to [https://192.168.1.1/#Security/Firewall/Policies](https://192.168.1.1/#Security/Firewall/Policies)

Create a new Ruleset and call it LAN\_IN. Default Action: Allow, interface switch0/in. Depending on your configuration this might be different but the default config (switching ports 1–4) leads to this. It basically says: Traffic coming FROM the network TO the outside world, default accept.

![](/images/Manage-computer-game-times-for-children-and-self-control/1*-vWSbH7B8d1fK9esNOsguA.png)

<figcaption>Rulesets</figcaption>

Create the actual rules. My final ones look like this, how to get there in the following steps.

![](/images/Manage-computer-game-times-for-children-and-self-control/1*Z1-wYk5FwJTSwAXGmL9l8g.png)

<figcaption>Overview of all&nbsp;rules</figcaption>

Let’s look at one rule closer: We want to forbid a specific device to play computer games and they usually use ports in the range above 500 (actually 5000+ mostly but it doesn’t matter). My roommate uses his PC only for gaming so its ok that I forbid all ports but some lower ones.

**Drop all packages** (disabled right now, its vacation times ;-) )

![](/images/Manage-computer-game-times-for-children-and-self-control/1*4c7aFNe2rdbnqoD7PiFZmg.png)

**All package states**

![](/images/Manage-computer-game-times-for-children-and-self-control/1*Txc8GvxfSvEmVBcOCwE-RQ.png)

**From specific Mac address**

![](/images/Manage-computer-game-times-for-children-and-self-control/1*CDU9MIFgfviXR5b6L-4tww.png)

**Block all ports that could be gaming** (could be a bit more fine tuned but what the hell)

![](/images/Manage-computer-game-times-for-children-and-self-control/1*9wDQKStdxMZdk9D20keSMw.png)

**Block on certain days**. This can be customized very nicely.

![](/images/Manage-computer-game-times-for-children-and-self-control/1*mMP_jDMxaD0zjvhOEkUjWg.png)

As seen in the overview of all rules, I have more set up. I do not allow my Philips Hue bridge to connect to the internet, mainly because I don’t see a reason why it should and because it constantly insists to perform firmware updates before I can turn on lights. Very annoying! I also block two MAC addresses, the wifi card and the ethernet card of my mates computer. I’m sure I could make this more effective using only one rule and some form or OR operator but I just C&P the rule, why not.

**Wait there is more**

Of course simply closing ports is a cheap trick. But luckily, the engineers from Ubiquiti have included a list of things you can block on a more logical level. Did you get the 5th letter from a lawyer because your son keeps leeching Game of Thrones on Torrents? Block torrents. Got a Facebook addicted daughter? Block it after 9pm or during family and dinner time. Have a trader wife that is just always trading stocks when you try to talk to her (happens all the time right?) block it. The list offers many choices that are also helpful for doing inverted blocking (block everything BUT TopSites for Science and News), however I found these to not work as well as I had hoped. More [here](https://help.ubnt.com/hc/en-us/articles/218732788-EdgeMax-Create-a-Firewall-Rule-using-Deep-Packet-Inspection-DPI-)

![](/images/Manage-computer-game-times-for-children-and-self-control/1*fxaNNr8zV1rCAqaZZMNGTw.png)

* * *

**Quick disclaimer:** **I have not been paid for this!** I am just a geek with a girlfriend that told me I should write more since she thinks I am good at it (you may be the judge of that). Since I have used 100+ OpenSource frameworks and read tens of thousands of blog posts, now I want to start giving back! If you click on a link that links to amazon and you buy something, I might get money from it, but so far that has not yet happened so yeah, just enjoy the article.

Thanks for reading!
