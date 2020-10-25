---
layout: post
title:  "Common Issues With Attaching Huge Capacity Hard Disks To Your Raspberry Pi"
excerpt: "This article discusses common pitfalls which can happen when trying to setup a Huge Capacity HDD on Rpi"
date:   2020-10-26 00:00:00 +0530
author: Pavan Kalyan Damalapati
tags: [rpi]
---


### Introduction
There are many reasons to attach a huge hard disk to the Raspberry Pi, whether you want to setup a Media server for all of your movies and music or whether a torrent machine (or both). A Hard disk with a huge capacity makes a lot of sense in these cases.
I recently bought a 4TB hard disk (nick named **Brick**) and wanted it to be attached to my RPI for downloads and also to stream those downloads across my home network. The following are some hoops I had to jump through to get it working compared to a smaller capacity HDD.



### Concern 1: Power Requirements
As soon as I connected my hard disk, I started hearing clicking sounds every few seconds. I thought it might be faulty hardware, but it was actually the drive trying to start up but failing due to lack of power.
#### There are a few things that can be done in this case
- Use an external power supply for the HDD. Usually big capacity HDDs come with the requirement of an external power supply. But my 4TB WD HDD (Brick) didn't.
- Unplug all other unnecessary appliances. The power is shared among the different USB ports. This did the trick for me. I previously had a 64GB and 1TB hard disk attached. removing them, helped my brick to startup.
- Ensure that the power Adapter is a hight quality one with a high amperage. The one I used provided 2A.



### Concern 2: HDD Automounting
Normally to make an external HDD mount, you need to understand it's UUID and add a line into the `/etc/fstab` file. This tells the system how and where to mount the HDD on bootup.
But when I added the line to the fstab file and rebooted the system. It showed that the **Brick** was not mounted. I rechecked the fstab and it was correct.
This is when I thought of [**dmesg**](https://man7.org/linux/man-pages/man1/dmesg.1.html). **dmesg** is a tool which outputs the kernel buffer. This is useful for diagnostic purposes.
I simply ran 
~~~
dmesg
~~~
and I received the following output

~~~
[    5.680612] sd 0:0:0:0: [sda] Spinning up disk...
[    5.681206] scsi 0:0:0:1: Enclosure         WD       SES Device       4008 PQ: 0 ANSI: 6
[    6.717053] ......ready
[   11.917626] sd 0:0:0:0: [sda] Very big device. Trying to use READ CAPACITY(16).
[   11.918011] sd 0:0:0:0: [sda] 7813969920 512-byte logical blocks: (4.00 TB/3.64 TiB)
~~~
Notice the time difference from when it starts up the disk and when it figures out the size of the HDD. seems like around 4-5 seconds. This made me suspicious that maybe the system does not mount it because, it takes too long with such a big drive.
I looked for a solution and found [this](https://www.raspberrypi.org/forums/viewtopic.php?t=111664).
The solution was to add the line 'rootdelay=5' to `/boot/cmdline.txt`.
After adding that and rebooting. It worked! (This may not be the best way to do this)

### Concern 3: HDD Speed and RPI Performance
As I was trying to download a large file. I noticed it was failing and my Rpi started to hang.
The lag was so bad that simply typing ls in the `/media/pi/brick` folder seemed to **brick** my system.

I used **htop** to understand what was happening
There was one process which seemed to consume a lot of cpu cycles.
**mkfuse.exfat**
This was the software used to mount and handle **exfat** file system. The brick was formatted as **exfat**.
I realized this was unusable and decided to format it to **ext4** using gparted. 

This time it was a world of difference, the rpi no longer got hung and the downloads were working. But there is a downside to using ext4. It cannot be easily used with other OSs like Windows or Mac OS.
I anyway planned to stream everything over the network, so this wasn't a big deal.
If interoperability is important, then the HDD can be formatted as **NTFS**. This would allow it to be used with Windows as well. This is much slower than ext4, but from what I have seen it generally performs better than **exfat** on my Rpi.


### End Note
Using a 4TB hard disk on a Raspberry Pi, is considerably different than simply attaching a USB or 500GB hard disk. Some of these issues can be handled by getting a dedicated server instead of an Raspberry Pi. There also might be other issues as well, time will tell.
