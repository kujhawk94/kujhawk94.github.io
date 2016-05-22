---
layout: post
title:  "Replacing disks on ReadyNAS 2100 - SOLVED!"
date:   2015-06-03
categories: linux
redirect_from: "/archives/357"
---

The time came to upgrade the storage space on the [ReadyNAS 2100][1] at Jen's office.  Checking the Netgear's [Hardware Compatibility List (HCL)][2] indicated WD's Red 3TB drives ([WD30EFRX][3]) to be a compatible choice.  Once they arrived, per the manual, I removed one drive and replaced it with the new 3TB drive.  The Frontview software seemed to recognize the situation and said it would start rebuilding the XRAID-2 volume on the new drive.  But the rebuild never started.

![][4]  

Many searches for the keywords involved resulted in some consensus regarding three settings on Frontview that could keep the rebuilding process from starting.  Bott's Thoughts summarized the settings:

> Login to Frontview and check to see if any apply and temporarily change the setting to what is listed below.  
> * **Frontview &gt; System &gt; Performance &gt; Enable Journaling**  
> * **Frontview &gt; System &gt; Power &gt; Disable disk spin-down**  
> * **Frontview &gt; Volumes &gt; Volume Settings &gt; Snapshot** &gt; Delete any active snapshots &amp; turn off snapshot schedule  
>   
> After changing the settings go into <strong>Frontview &gt; System &gt; Shutdown &gt; Check and Fix Quotas on next boot</strong> and expansion should begin.  

However the expansion did not begin.  I want to note these settings, however, as they may play a role in the following steps actually working.  Once connected via ssh to the ReadyNAS command these steps successfully started the disk rebuild/resync.  It should go without saying that many things could go wrong…make sure you have appropriate backups!

Print the valid partition on a disk that is still part of the volume as a basis for a new partition table on the new disk.

    :~# sgdisk -p /dev/sdc
    Disk /dev/sdc: 5860533168 sectors, 2.7 TiB
    Logical sector size: 512 bytes
    Disk identifier (GUID): D7090D89-9206-4577-9DFB-E520853EB20D
    Partition table holds up to 128 entries
    First usable sector is 34, last usable sector is 1953525134
    Partitions will be aligned on 64-sector boundaries
    Total free space is 4092 sectors (2.0 MiB)
    
    Number  Start (sector)    End (sector)  Size       Code  Name
       1              64         8388671   4.0 GiB     FD00  
       2         8388672         9437247   512.0 MiB   FD00  
       3         9437248      1953521072   927.0 GiB   FD00

Copy the good partition table to the new drive and randomize the GUID.  NB: In the following example, /dev/sdd is the new blank disk and /dev/sdc is the live disk already part of the RAID volume. ***The destination drive is on the left and comes first.***

    :~# sgdisk -R/dev/sdd /dev/sdc
    :~# sgdisk -Gg /dev/sdd

Determine which partitions are associated with which RAID volumes. Note how /dev/md0 has the first partition 
on each disk, /dev/md1 the second and /dev/md2 the third. In the next step, do the same for the new drive.

    :~# mdadm -D --scan -v
    ARRAY /dev/md/0 level=raid1 num-devices=4 metadata=1.2 name=A021B7C0FC6C:0     UUID=863c5ab4:67edbad8:3283f4d5:bdec405e
       devices=/dev/sda1,/dev/sdb1,/dev/sdc1,/dev/sdd1
    ARRAY /dev/md/1 level=raid6 num-devices=4 metadata=1.2 name=A021B7C0FC6C:1     UUID=ce54fed4:cc96e28f:e9c67342:57a75196
       devices=/dev/sda2,/dev/sdb2,/dev/sdc2,/dev/sdd2
    ARRAY /dev/md/2 level=raid5 num-devices=4 metadata=1.2 name=A021B7C0FC6C:2     UUID=bb41d992:349d5fd2:2e28c67f:af1e6c2d
       devices=/dev/sda3,/dev/sdb3,/dev/sdc3,/dev/sdd3

Add the new disk's partitions to the existing RAID volumes.

    :~# mdadm --add /dev/md2 /dev/sdd3
    mdadm: added /dev/sdd3
    :~# mdadm --add /dev/md0 /dev/sdd1
    mdadm: added /dev/sdd1
    :~# mdadm --add /dev/md1 /dev/sdd2
    mdadm: added /dev/sdd2

Adding the new disk partitions triggers the resync (or rebuild) process on the new drive. Monitor the progress of the rebuild via Frontview or via CLI with the following:

    :~# watch cat /proc/mdstat

## Netgear support (or lack thereof)

Early on in the process, I saw many references suggesting contacting Netgear support.  With a compromised disk array, I hoped for a quick resolution of the problem.  As our ReadyNAS was almost four years old, telephone support was not an inexpensive option.  I did create a trouble ticket and attempt to connect to tech support via live chat without success.  Perhaps more patience was required?  My previous experiences with Netgear support had been positive, so I would probably still consider future purchases from them, but I will more closely consider support options in the future.

**2015-06-05**:  A quick update on service options.  According to email conversation with the Netgear support staff, we'd have to pay for a year of service, $380, due to the legacy status of the device.

## References
* [What To Do When Expansion Doesn't Start][5]
* [Rebuild Netgear ReadyNAS RAID][6]
* [How to Clone Partition Tables][7]

[1]:http://www.support.netgear.com/product/RNRX4000%2b%2428ReadyNAS%2b2100%2429
[2]:http://kb.netgear.com/app/answers/detail/a_id/20686
[3]:http://www.amazon.com/gp/product/B008JJLW4M
[4]:/images/replacing-readynas2100-disks/Napkin-06-03-15-10.24.24-PM.png
[5]:http://home.bott.ca/webserver/?p=550
[6]:http://cobaltfish.com/techie/521-readynas-raid-rebuild
[7]:http://ram.kossboss.com/clone-partition-tables-gpt-mbr-sfdisk-sgdisk/
