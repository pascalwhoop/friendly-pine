---
title: Migrating from ZFS to MergerFS and SnapRAID at home
date: '2019-02-10T13:28:01.774Z'
subtitle: >-
  A really intuitive file management setup that works great as a linux user and
  docker enthusiast
excerpt: >-
  A really intuitive file management setup that works great as a linux user and
  docker enthusiast
thumb_img_path: >-
  images/Migrating-from-ZFS-to-MergerFS-and-SnapRAID-at-home/1*veHD4UI63DX2K4KpgQ59VQ.jpeg
layout: post
---
This weekend I decided to migrate my data on my home server to a new file setup. I tend to do this things a lot, mostly because I’m curious and I want to try new things. I’ve run a [desktop Linux](https://medium.com/curiouscaloo/switching-from-macos-to-linux-1-year-later-3c34467deb5a) since mid 2017 and in the course of switching from my Macbook to a desktop, I also collected all of my random disks and tossed them into a RAID array. I had

*   5 HDD 2TB from ext HDDs and a former NAS
*   64GB M2 SSD from a former NAS
*   256GB PCIe SSD
*   256GB SATA SSD from my old laptop

I used to have the two 256gb SSDs setup as a Linux system base and as a (failed) hackintosh disk. The HDDs I had setup as such: 4x2TB as a raidz2 and one drive as an extra backup in case the zfs raid fails. I basically had 5 drives but only 2 available as disk space. I suffered several data losses in the past (partially during reorgs, due to drive failure and hardware raid controller failures) and I just wanted to be sure that this doesn't happen again. I also didn’t have that much data to spread on these disks, as I had lost most of my movies / shows from past crashes. The 64GB SSD was used as a [L2ARC read cache](https://wiki.archlinux.org/index.php/ZFS#L2ARC) which was a nice boost of performance for frequently used small files such as DB files etc.

![](/images/Migrating-from-ZFS-to-MergerFS-and-SnapRAID-at-home/1*veHD4UI63DX2K4KpgQ59VQ.jpeg)

<figcaption>ZFS pool with 2 offline disks during parity calculation with&nbsp;snapraid</figcaption>

This worked well for the last 20 months and ZFS is a cool system. I ran into issues with RAM usage (it eats everything that’s available for free) but it was solid otherwise. However, I switched back to a laptop as my primary driver once I started working full time and I moved the desktop back into a server status. It’s pathetically ill-suited for such a task (a 1060 GPU is just sitting around in there) but it’s still cheaper and less tedious than selling the whole thing and buying better suited hardware. However, the disks are expected to fail soon (see screenshot, top left window) and I wanted to switch to a setup where each drive is spun up only when accessed. I also wanted to move away from such a heavy setup as ZFS and I started using a lot of containers from the [linuxserver.io](https://blog.linuxserver.io/) guys which posted a [great blogpost](https://blog.linuxserver.io/2017/06/24/the-perfect-media-server-2017/) about one of their members setup. But ultimately, the reason was the realization that ZFS raidz2 doesn’t allow extension of a raid group with extra disks. Adding my 5th disk was simply not possible without destroying the entire group first. Hence, the weekend project.

### The migration

My goal of the migration was to absolutely avoid data loss while also not having to buy spare disks for the transfer. It turns out, mergerFS is extremely flexible! It’s basically just a merge of all disks into one virtual filesystem. If you are about my age, you probably had one of those *overhead projectors* at school. Imagine taking 2–3 of the plastic sheets and drawing a file tree on each. Now, when putting all on top of each other, the union of all those files is the final result. This is the basic idea of mergerFS. It of course has a bunch of features like determining where to write or read the data to / from but the core idea is quite simple. The nice benefit is the ability to read files from each drive on its own, even if all the others burn to ashes.

![](/images/Migrating-from-ZFS-to-MergerFS-and-SnapRAID-at-home/0*xagvSbAZaydnW1Aa.JPG)

<figcaption>mergerFS is like placing several plastic foils above each other and then projecting the result into a new&nbsp;FS</figcaption>

I had 4 disks in the pool and 1 extra HDD (the backup disk) to start the process. I `zpool offline` one of the HDDs from the pool (turning it into a degraded but working state) and created a mergerFS with 2 disks. A quick

    rsync -av /home/pascalwhoop/tank /mnt/storage

transferred all my zfs data into my new mergerFS system. For a guide on how to setup the mergerFS, check the [linuxserver post](https://blog.linuxserver.io/2017/06/24/the-perfect-media-server-2017/).

Once the transfer was complete, I had the exact same data on a non-parity mergerFS system and a 1 disk parity ZFS system. Because I am paranoid, I also synched my most dear files onto my raspberry pi on the other side of the house which has a 2TB backup drive attached to it. That took a while but it’s good to be backed up.

The next step is theoretically the most dangerous: Removing the last parity drive from the ZFS pool to run the `snapraid sync` command on a third drive. In this moment, I can survive 1 random disk failure or 2 disk failures if both are within the same subsystem. A loss of one disk in the ZFS pool and one disk in the mergerFS would lead to a partial data loss. But the probability of loosing 2 disks within a 4h time-span seemed a sufficiently low risk to take. Worst case, I’d loose some TV shows or movies but no personal data or source code.

Hence

    zpool offline tank ata-Hitachi_HUA723020ALA641_YGG2T6JA  
    fdisk /dev/sda             #delete partition, create new one  
    mkfs.ext4 /dev/sda1        #create new file system  
    vim /etc/fstab             #add partition to mount  
    mount -a                   #mount partition  
    vim /etc/snapraid.conf     #add new partition as parity disk  
    snapraid sync              #calculate parity for the 2 MergerFS

Now I have all my data on my new mergerFS with 1 disk parity. I can now destroy the ZFS pool and use the last two drives for a second disk parity as well as a third data drive.

### Results

This setup gives me 6TB of storage and 4TB of parity. Plus 2TB of backup on a Pi and some 10GB on AWS Glacier using `duply` and a PGP encrypted set of compressed archives of all my documents. This seems a good enough compromise for the risk of loosing 3 drives in my mergerFS/snapraid setup + my backup drive on the Pi which would make me loose my photo library but still not my documents. The biggest risk now is a burglar stealing all my disks. I guess I will move that Pi somewhere else (e.g. my parents house), as the likelihood of two thefts across two countries in the same night are rather low. But it’s nice to know (is it?) that the biggest risk of data loss now is due to a physical break-in
