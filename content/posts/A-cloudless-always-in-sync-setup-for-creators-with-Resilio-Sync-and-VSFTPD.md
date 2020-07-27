---
title: A cloudless always in sync setup for creators with Resilio Sync and VSFTPD
date: '2016-12-01T19:33:48.184Z'
excerpt: "Or in component terms: Raspberry Pi + iPad Pro + Macbook =\U0001F4AA"
thumb_img_path: >-
  images/A-cloudless-always-in-sync-setup-for-creators-with-Resilio-Sync-and-VSFTPD/1*yzbn6Wm3UUel1VmOnP9p1Q.png
layout: post
---
*Or in component terms: Raspberry Pi + iPad Pro + Macbook =💪*

I recently made the move from Android to iOS. I was afraid of it honestly. The awful fact that you can not send a message on WhatsApp without being connected to the internet or the lack of geofencing rules for silencing the alarm are just two reasons. But the day came and went and I am not looking back. I actually went so far as to replace my Surface 3 with an iPad Pro 12.9. Which is the trigger to my story:

#### I want to explain how to be mobile with your data without being open about it. A private P2P+ network.

What you need:

*   A [RaspberryPi](https://www.amazon.com/gp/product/B01CD5VC92/ref=as_li_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B01CD5VC92&linkId=bc3089dbab9bfc04a816b66bd8c1fa22)
*   Some Unix Skills or [how to setup a Pi](https://www.raspberrypi.org/learning/getting-started-with-raspberry-pi-lesson/)
*   A custom domain or a static IP (I use a domain and [this script](https://gist.github.com/natarajmb/e9f263c1a592cfc8215e#file-aws-route53-py) to tell Route53 if my IP changes)

#### Step 1: Add Resilio to your Pi and Mac/PC

Download [Resilio Sync](http://resilio.com) on both the Pi and the main machine. Get it set up so that you have some folders on your machine that you want to be synced. Mine looks like this:

![](/images/A-cloudless-always-in-sync-setup-for-creators-with-Resilio-Sync-and-VSFTPD/1*yzbn6Wm3UUel1VmOnP9p1Q.png)

I basically stuff every file in AllDevices, **without a folder structure and all I use is OSX Tags**. Spotlight is very good at finding my stuff and I have stopped trying to force my files into a pattern that makes no sense anyways.

Next I put Resilio Sync on the Pi. I could rewrite the tutorial but [this post](https://www.resilio.com/blog/sync-wd-raspberry-pi) sums it up.

Alright so now you got Sync running on your machines. It syncs directly using the P2P torrent protocol (but encrypted) which means its also great for huge files and syncs blazing fast in the same LAN. Its also a great alternative for syncing your photos to your machine without going through Google or Apple.

#### Step 2: Make it iPad Pro + PDF Expert ready

Unfortunately with the switch from a Windows Machine to the [iPad Pro 12.9](https://www.amazon.com/gp/product/B0155OD1ME/ref=as_li_tl?ie=UTF8&tag=curiouscalo0b-20&camp=1789&creative=9325&linkCode=as2&creativeASIN=B0155OD1ME&linkId=66f72b5f1647231970e4a58c8764f2af), I had to give up the full Resilio Client. iOS just doesn’t allow for this kind of application, since it doesn’t like exposing the file system to the user. This is unfortunate, since I use the tablet for annotating PDFs and taking notes during lectures.

![](/images/A-cloudless-always-in-sync-setup-for-creators-with-Resilio-Sync-and-VSFTPD/1*ch75CUvQlhnUs9DkBHSN_g.png)

<figcaption>PDF Experts Cloud sync offers are various and offer custom integrations with&nbsp;sftp</figcaption>

But [PDF Expert](https://itunes.apple.com/us/app/pdf-expert-by-readdle/id1055273043?mt=12) easily solves this problem by including a bunch of awesome integrations. So let’s set up [VSFTPD](https://security.appspot.com/vsftpd.html), the default ftp daemon for Ubuntu and many other distros.

1.  Install vsftpd on the Pi using apt-get
2.  Configure the /etc/vsftpd.conf file, more detail can be found in the file and [over here at the ubuntu forums](https://ubuntuforums.org/showthread.php?t=518293)
3.  expose the ftp port to the outside world, this is dependent on your Router, but theres a bunch of guides [here](https://portforward.com/router.htm#C)
4.  Add the settings to PDF Experts SFTP integration. Now you can Sync folders and every time the application changes a PDF, it syncs to the ftp server.

![](/images/A-cloudless-always-in-sync-setup-for-creators-with-Resilio-Sync-and-VSFTPD/1*SPkv6nt1E9wnzxe1hqMRwA.png)

Now you can change files on the Tablet and they get synced to the Pi which in turn uses Resilio to sync to everything else. You can also directly drop files from the Tablet into the Sync P2P network if you add the Tablet to the P2P share.

One such example is writing a blogpost on the tablet using [Nebo](https://itunes.apple.com/us/app/myscript-nebo-note-taking/id1119601770?mt=8) which recognizes hand writing like a pro and then you can just export it as a HTML or PDF file to later use it on your Mac for publishing a blog post.

![](/images/A-cloudless-always-in-sync-setup-for-creators-with-Resilio-Sync-and-VSFTPD/1*Oh4hdNIU2gmxEV8tHGVCJg.png)

<figcaption>my first throw at this post, which I then exported as HTML and copied the text here to work on it&nbsp;further</figcaption>
