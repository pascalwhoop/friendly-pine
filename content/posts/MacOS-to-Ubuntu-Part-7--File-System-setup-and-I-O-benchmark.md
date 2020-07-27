---
title: 'MacOS to Ubuntu Part 7: File System setup and I/O benchmark'
date: '2017-08-13T07:31:39.379Z'
excerpt: Setting up a 2 level storage on a linux machine using SSD’s and ZFS HDD
thumb_img_path: >-
  images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*tH8xkt5gl1xnf50kAPzG5Q.png
layout: post
---
To check out the whole story, see the index post of this series:

[**Getting started on Ubuntu Mate as a long time MacOS user**  
*Work in progress. This index post will be expanded with subposts as they are finished by me.*medium.com](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98 "https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98")[](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98)

* * *

I have a 250gb NVMe SSD with a read/write rate of 1500MByte/Sec give or take a few hundred megs. I mean in that dimension, who’s counting right?

But I also have a large collection of photos and music so I want these to be safely stored. I have 5 x 2TB hard drives in my setup and I intend on having 4 of those 5 in a ZFS based RaidZ2 based setup and one as a extra layer of backup plus another external hard drive that I will sync with rsync. I might also add another layer of geo redundancy with [AWS Glacier](https://aws.amazon.com/glacier/pricing/) but for that I’ll first need to solve the encryption problem.

#### Step 1: Create the zfs pool

this step is surprisingly easy on Ubuntu 16.04

    apt install zfs  
    zpool create tank raidz2 -f /dev/sda /dev/sdb /dev/sdc /dev/sdd  
    zfs set mountpoint=/home/pascalwhoop/tank tank

#### Step 2: Backups!

Now I have a zfs pool with 2 disks fail-able before I loose anything. But because this is not good enough, I’ll add another layer of security in there and add an external hard drive to my Raspberry Pi. First I’ll attach it to my new machine and use:

    #backing up the system partition  
    rsync -aAXv --delete --exclude="/dev/*" --exclude="/proc/*" --exclude="/sys/*" --exclude="/tmp/*" --exclude="/run/*" --exclude="/mnt/*" --exclude="/media/*" --exclude="/lost+found" --exclude="/home/pascalwhoop/tank/*" / /media/pascalwhoop/BackupDrive/System/
    #backing up the private collection of personal photos/videos etc  
    rsync -aAXv --delete /home/pascalwhoop/tank/private /media/pascalwhoop/BackupDrive/tank/private

and now I can plug this external hard drive into my Pi and setup a CRON job to backup my data every day to the Pi, ensuring that I don’t loose anything. This doesn’t back up movies and tv shows but they aren’t as precious to me so they won’t clutter my backup drive. I trust zpool enough to not loose my movies. But I don’t trust it enough to not loose my personal photos. Movies I can get somewhere else. The photos I cannot.

So this is how it looks in the end:

*   250GB SSD → root “/”
*   ~4TB ZFS RaidZ2 storage “~/tank”
*   2TB secondary backup in another room using rsync over the LAN
*   TODO: Encrypted Glacier storage on AWS
*   TODO: Using a spare 120GB MSATA as cache for the ZPOOL

I might also use the MSATA for a secondary OS, maybe I’ll play around with hackintosh or keep Windows 10 on there for the occasional gaming session. That all depends on the VM setup that’s still to come.

### Benchmarking the setup

I am using sysbench, a CLI utility for Linux. More info [here](https://wiki.mikejung.biz/Sysbench#Sysbench_Fileio_file-test-mode)

#### Samsung 960 EVO M.2 SSD

To make clear what we are aiming for, the specs of the drive described as such:

*   seq read: 3200MB/s, seq write: 1500MB/s
*   IOPS 4K read/write : 330k/ 300k

    sysbench --test=fileio --file-total-size=10G prepare  
    sysbench --test=fileio --file-total-size=10G --file-test-mode=**rndrw** --init-rng=on --max-time=60 --max-requests=0 run  
    sysbench 0.4.12:  multi-threaded system evaluation benchmark
    Operations performed:  84240 Read, 56160 Write, 179591 Other = 319991 Total  
    Read 1.2854Gb  Written 877.5Mb  Total transferred 2.1423Gb  (**36.561Mb/sec**)  
     2339.91 Requests/sec executed

Well that is rather disappointing isn’t it. You buy an SSD and get 36.6Mb/sec. Let’s try sequential read

    sysbench --test=fileio --file-total-size=10G --file-test-mode=**seqrd** --init-rng=on --max-time=60 --max-requests=0 run  
    sysbench 0.4.12:  multi-threaded system evaluation benchmark
    Running the test with following options:  
    Number of threads: 1  
    Initializing random number generator from timer.
    Extra file open flags: 0  
    128 files, 80Mb each  
    10Gb total file size  
    Block size 16Kb  
    Periodic FSYNC enabled, calling fsync() each 100 requests.  
    Calling fsync() at the end of test, Enabled.  
    Using synchronous I/O mode  
    Doing sequential read test  
    Threads started!  
    Done.
    Operations performed:  655360 Read, 0 Write, 0 Other = 655360 Total  
    Read 10Gb  Written 0b  Total transferred 10Gb  (**1.1322Gb/sec**)  
    74199.46 Requests/sec executed
    Test execution summary:  
        total time:                          8.8324s  
        total number of events:              655360  
        total time taken by event execution: 8.7477  
        per-request statistics:  
             min:                                  0.00ms  
             avg:                                  0.01ms  
             max:                                 12.14ms  
             approx.  95 percentile:               0.07ms
    Threads fairness:  
        events (avg/stddev):           655360.0000/0.00  
        execution time (avg/stddev):   8.7477/0.00

That looks better but not what they promised. 3500 is not the same as 1157.

For now, let’s look at write-speed:

    sysbench --test=fileio --file-total-size=10G --file-test-mode=**seqwr** --init-rng=on --max-time=60 --max-requests=0 run  
    sysbench 0.4.12:  multi-threaded system evaluation benchmark
    Running the test with following options:  
    Number of threads: 1  
    Initializing random number generator from timer.
    Extra file open flags: 0  
    128 files, 80Mb each  
    10Gb total file size  
    Block size 16Kb  
    Periodic FSYNC enabled, calling fsync() each 100 requests.  
    Calling fsync() at the end of test, Enabled.  
    Using synchronous I/O mode  
    Doing sequential write (creation) test  
    Threads started!  
    Done.
    Operations performed:  0 Read, 655360 Write, 128 Other = 655488 Total  
    Read 0b  Written 10Gb  Total transferred 10Gb  (**1.223Gb/sec**)  
    80153.38 Requests/sec executed
    Test execution summary:  
        total time:                          8.1763s  
        total number of events:              655360  
        total time taken by event execution: 6.6005  
        per-request statistics:  
             min:                                  0.00ms  
             avg:                                  0.01ms  
             max:                                 27.13ms  
             approx.  95 percentile:               0.02ms
    Threads fairness:  
        events (avg/stddev):           655360.0000/0.00  
        execution time (avg/stddev):   6.6005/0.00

Faster sequential write than read? Something is definitely going on here.

    sysbench --test=fileio --file-total-size=10G --file-test-mode=**rndrd** --init-rng=on --max-time=60 --max-requests=0 run  
    sysbench 0.4.12:  multi-threaded system evaluation benchmark
    Operations performed:  911639 Read, 0 Write, 0 Other = 911639 Total  
    Read 13.911Gb  Written 0b  Total transferred 13.911Gb  (**237.41Mb/sec**)

and random write:

    sysbench --test=fileio --file-total-size=10G --file-test-mode=rndwr --init-rng=on --max-time=60 --max-requests=0 run  
    sysbench 0.4.12:  multi-threaded system evaluation benchmark
    Read 0b  Written 1.2466Gb  Total transferred 1.2466Gb  (**21.276Mb/sec**)

I don’t like this at all.

Consulting fdisk on the SSD partition table:

    Disk /dev/nvme0n1: 232,9 GiB, 250059350016 bytes, 488397168 sectors  
    Units: sectors of 1 * 512 = 512 bytes  
    Sector size (logical/physical): 512 bytes / 512 bytes  
    I/O size (minimum/optimal): 512 bytes / 512 bytes  
    Disklabel type: dos  
    Disk identifier: 0x37e4d57d
    Device         Boot     Start       End   Sectors  Size Id Type  
    /dev/nvme0n1p1 *         2048 455084031 455081984  217G 83 Linux  
    /dev/nvme0n1p2      455086078 488396799  33310722 15,9G  5 Extended  
    /dev/nvme0n1p5      455086080 488396799  33310720 15,9G 82 Linux swap / Solaris

**4x2TB ZFS RAIDZ2 7200rpm drives**

*   sequential write: 177.53Mb/sec

#### cross checking with gnome-disks

after reading this post

[**How to check hard disk performance**  
*How to check the performance of a hard drive (Either via terminal or GUI). The write speed. The read speed. Cache size…*askubuntu.com](https://askubuntu.com/questions/87035/how-to-check-hard-disk-performance#87036 "https://askubuntu.com/questions/87035/how-to-check-hard-disk-performance#87036")[](https://askubuntu.com/questions/87035/how-to-check-hard-disk-performance#87036)

I decided to try with another tool. This time gnome-disks and it’s benchmark utility

![](/images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*tH8xkt5gl1xnf50kAPzG5Q.png)

Now this is looking a LOT better!

As a frame of reference the single HDD

![](/images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*2TsKslBeepELHhRMZ-22cQ.png)

#### Cross checking with iozone

I found [this article](https://www.linux.com/news/iozone-filesystem-performance-benchmarking) which seems to really know what it’s talking about. So I did

    apt install iozone3

and benchmarked with this tool. It’s great because it allows a wide range of tests but even the basic automatic test is more than enough for my purposes. So I ran a “quick”

    cd ~/tank  
    iozone -a > ../Desktop/iozone_auto_HDD.txt  
    cd ~/Desktop  
    iozone -a > ../Desktop/iozone_auto_SSD.txt

Then I adjusted the gnu3d.dem file that came with iozone like this:

    outfile(n) = sprintf("%s/%s.png size 1920,1080", word(dirs,n), word(dirs,n))  
    ...  
    set terminal png size 1920,1080

The first change is a one line change, the second one needs to be applied for all i. A complete version is available [here on Gist.github.com](https://gist.github.com/pascalwhoop/0d63a79701668b092693515a1e4f5595)

SSD WRITE

![](/images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*JJXorpA-RRCERyogn3xc4g.png)

<figcaption>SSD WRITE</figcaption>

HDD WRITE

![](/images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*siS6dWmi9nYaEWTrjKvWaQ.png)

<figcaption>HDD WRITE</figcaption>

SSD READ

![](/images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*7lCr3FSmfT6nmugMXLwooQ.png)

<figcaption>SSD READ</figcaption>

HDD READ

![](/images/MacOS-to-Ubuntu-Part-7--File-System-setup-and-I-O-benchmark/1*4W2Zfiikq-cco_8dteOT9A.png)

<figcaption>HDD READ</figcaption>

Okay, this is getting out of hand. I will write another post with a detailed investigation using iozone and increasing the file size to 16GB to avoid caching being the reason for high performance reads. Stay tuned.

* * *

Please keep reading. Just pick a part

[**Getting started on Ubuntu Mate as a long time MacOS user**  
*Work in progress. This index post will be expanded with subposts as they are finished by me.*medium.com](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98 "https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98")[](https://medium.com/@pascal.brokmeier/getting-started-on-ubuntu-mate-as-a-long-time-macos-user-99e630b05c98)
