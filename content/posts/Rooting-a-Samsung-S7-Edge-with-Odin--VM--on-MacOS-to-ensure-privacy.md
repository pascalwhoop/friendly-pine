---
title: Rooting a Samsung S7 Edge with Odin (VM) on MacOS to ensure privacy
date: '2017-08-08T10:06:23.184Z'
excerpt: >-
  TL;DR: Download Magisk, get Odin, and TWRP for the your S7 (Exynos), enable
  “OEM Unlock” in Dev settings. Flash TWRP, then boot into…
thumb_img_path: >-
  images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*rUfYAPhdPGPn3TwBlea2ww.jpeg
layout: post
---
TL;DR: Download [Magisk](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445), get Odin, and [TWRP](https://twrp.me/Devices/) for the your S7 (Exynos), enable “OEM Unlock” in Dev settings. Flash TWRP, then boot into recovery and flash Magisk. Ensure your device isn’t encrypted before you flash.

* * *

What you will find here

*   Rooting Guide for the S7 Edge using [Heimdall](http://glassechidna.com.au/heimdall/) on Linux
*   privacy securing guide with [XSecure](http://repo.xposed.info/module/biz.bokhorst.xprivacy) and [Xposed Framework](http://repo.xposed.info/module/de.robv.android.xposed.installer)

### Privacy possible without Root

So I switched from iOS to Android. It wasn’t a choice, I had to give up my work phone and got an S7 for free. But the privacy situation of the Android operating system was bothering me in the past already and now, that I came back from iOS it irritated me a lot. So I started digging into securing my privacy without rooting and found that I could do the following:

*   Disable most apps using the default “disable” feature for system apps in Android
*   Control the connectivity of many apps using [NoRoot Firewall](https://play.google.com/store/search?q=noroot%20firewall)
*   Move all my contacts, calendar and mail to a [secure mail provider](http://mailbox.org) that costs money (a bit) but ultimately is offering me more than Google does with its *free* services

The screenshots below give a good idea of the network connectivity of my “average” phone setup.

*   *Game Optimizing Service* is calling home with no game installed
*   *Wunderlist* (owned by MS funny enough) calling to AWS. I found out that is part of its functionality but I still don’t allow all calls
*   *fiil+,* the app that came with my headphones *constantly* tries to connect to servers
*   *Arrow Launcher,* also a MS product (my home launcher of choice) is trying to call servers quiet a bit. As this is holding a lot of information about me I’d rather it didn’t. And there is no feature in this home screen that I use that needs internet. So no.
*   *Couchsurfing* calling facebook, although I don’t use facebook login
*   *Gboard* keyboard connecting to Google [1e100](https://support.google.com/faqs/answer/174717?hl=en) server domain

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*rUfYAPhdPGPn3TwBlea2ww.jpeg)

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*WA3L4hi7F_E-3eH1eAYTFg.png)

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*wVlrsCxFtNyQEKWoDintTg.png)

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*H_oHwVfyxM9D2XgnZzYbJw.png)

#### What permission features are missing on Android

But this is still falling short in my eyes. You cannot control if applications run in the background or connect in the background. For example I don’t mind couchsurfing connecting to servers when I am using it but why does it have to do stuff when I am not using it? Push is disabled… The same goes for fiil+, RocketBook and many others.

Additionally, the normal permissions like Location and Contacts are of importance to me. On the one hand some apps need these features to function properly. On the other hand I see no reason why Google Maps has my location except for when it is open.

And here is where root starts to be a must. Because (as far as I know) there is no way to stop apps from acquiring location information or running in the background sending information out about me on Android. Either they have GPS access or they don’t. Either they have Internet or the don’t. **iOS allows you to give location access while the app is running.** And you can forbid the app to run unless you open it. So you know, Google knows where you are, when you use it and not otherwise.

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*cU3GVQ5KXo_bpQsO73ju_A.png)

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*Pb67SOSm2WuECGHhNc3I8g.png)

### Getting root on the S7

*(Beware this chapter has a few things you don’t need to do to succeed. It’s my story and it leads in a few dead ends so don’t follow without reading ahead!)*

So to root you need Odin. But I am on Mac. So I need [Heimdall](http://glassechidna.com.au/heimdall/). But it doesn’t work on Sierra (August 2017). But there is a good fix available on [Github](https://github.com/Benjamin-Dobell/Heimdall/issues/291#issuecomment-309951192).

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*1Jq21GpFf9qdFc-EWzGFOw.png)

Basically you

1.  copy the pkg file from the .dmg medium
2.  open a terminal and unpack the .pkg
3.  move some files around and
4.  repackage

    mkdir temp; cd temp; xar -xf ../Heimdall\ Suite\ 1.4.0.pkg  
    cat Payload | gunzip -dc |cpio -i  
    cd ./usr  
    mv bin/ lib/ local/  
    cd ..  
    rm -f Payload  
    cd ..  
    pkgutil --flatten temp/ heimdall.pkg

Now the installation works. Our .kext is installed under */System/Library/Extensions* and the system needs to be rebooted

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*vG3UUwBtEZGeE52_k6aH0g.jpeg)

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*t0PhMT2th4MpRuLWVKfzcQ.jpeg)

But also this doesn’t help as I was still receiving

    Initialising connection...  
    Detecting device...  
    [timestamp] [threadID] facility level [function call] <message>  
    --------------------------------------------------------------------------------  
    [ 0.002898] [00000307] libusb: debug [libusb_get_device_list]   
    [ 0.002943] [00000307] libusb: debug [libusb_get_device_descriptor]   
    ERROR: Failed to detect compatible download-mode device.  
    [ 0.002954] [00000307] libusb: debug [libusb_exit]   
    [ 0.002956] [00000307] libusb: debug [libusb_exit] destroying default context

when calling

    heimdall print-pit --verbose --usb-log-level debug

So i started up a VM, threw **Win 10** in there, copied and pasted the zip downloaded from [chainfire](https://autoroot.chainfire.eu/) and got my phone rooted within a minute. I am sure there is a way to get it to work but on my machine (15" USB C macbook pro) I did neither get Heimdall to work on a Linux VM nor on OSX so I gave up with my S7, attributing it to new Samsung software or some incapability of mine.

Now the phone was stuck in a bootloop. **Turns out you shouldn’t use chainfire but** [**Magisk**](https://forum.xda-developers.com/apps/magisk/official-magisk-v7-universal-systemless-t3473445)**.**

#### Making it work finally

So I downloaded the stock firmware again, flashed the stock img files onto my phone, lost all my data and started again. This time with

1.  TWRP flash
2.  Magisk flash

still, my phone wouldn’t boot. Whenever I touched the Samsung stock image but didnt’t go all the way with a custom rom, it wouldn’t get past the Samsung boot screen. This drove me nuts (and late into the night). So I went with the [Superman Rom](https://forum.xda-developers.com/s7-edge/development/rom-s7-edge-rom-v1-1-t3356699).

**Now I found out something important: I kept having an encrypted device which was visible through a bunch of red error codes in TWRP. I hadn’t noticed until now.**

> To get rid of encryption, go to “wipe”, format data (keyboard will pop up) then type “yes” and format it, then reboot to recovery ([source](https://forum.xda-developers.com/s7-edge/development/rom-s7-edge-rom-v1-1-t3356699))

This could have been the reason for the issues with booting before but by now I took a liking to the custom rom so I decided to stick with it.

Anyways, Superman Rom booted, worked, done.

**Xposed is not available yet for Nougat (Android 7), so XPrivacy will have to wait a bit…**

But at least we have root now. So we have a way to disable a bunch of apps, remove some permissions for apps we dislike and still control all the network flow. That’s a start. Sadly, there is no good way as with XPrivacy to serve fake contact lists yet but I will update this once I get Xposed on Nougat running.

#### The end

Current state is:

*   [NoRoot Firewall](https://play.google.com/store/apps/details?id=app.greyshirts.firewall) for controlling apps connections
*   [AdAway](https://f-droid.org/packages/org.adaway/) for ad control
*   [Brave](https://play.google.com/store/apps/details?id=com.brave.browser) (Open source fork of Chromium) for anything Browser Privacy related
*   manually editing some permissions away. E.g. I really don’t like how internet horny the Arrow Launcher is. Either it gets no data or no internet, not both! Same goes for the Gallery App or any other of the stock apps that don’t do internet so why do they need it.
*   Removing almost all Samsung apps with [Titanium Backup](https://play.google.com/store/apps/details?id=com.keramidas.TitaniumBackup) freeze feature
*   Managing Backups with TWRP and Titanium Backup
*   Managing Passwords with [Lastpass](http://lastpass.com) and [Google Authenticator](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)
*   Using DuckDuckGo as my search engine with a slick AMOLED skin
*   Downloaded a black\_red or black\_white theme with the Samsung Themes app, applied it, then froze the app and blocked all internet for it

The resulting Android is both beautiful and (it feels) more secure. Here are some screenshots with the dark themes

[**Android AMOLED**  
*9 new photos · Album by Michael Johnson*goo.gl](https://goo.gl/photos/BUusiC3VYxq4CLtdA "https://goo.gl/photos/BUusiC3VYxq4CLtdA")[](https://goo.gl/photos/BUusiC3VYxq4CLtdA)

And finally the lock screen which I really dig now

![](/images/Rooting-a-Samsung-S7-Edge-with-Odin--VM--on-MacOS-to-ensure-privacy/1*4lArLwyYjHioLhgS8FAoOQ.png)
