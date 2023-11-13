+++
author = "Ratnadeep Debnath"
categories = ["'/dev/root'", "(/dev/dm-1)", "boot", "could not find", "device", "failure", "Fedora", "2.6.27.19-170.2.35", "filesystem", "kernel", "linux", "lvm", "mount", "ratnadeep", "resume", "unable access", "VolGroup00/LogVol0", "rtnpro"]
date = 2009-03-11T07:36:45Z
description = ""
draft = false
slug = "resolved-kernel-boot-problem-d"
tags = ["'/dev/root'", "(/dev/dm-1)", "boot", "could not find", "device", "failure", "Fedora", "2.6.27.19-170.2.35", "filesystem", "kernel", "linux", "lvm", "mount", "ratnadeep", "resume", "unable access", "VolGroup00/LogVol0", "rtnpro"]
title = "Resolved Kernel boot problem :D"

+++


I am using Fedora 10 and I was running the kernel 2.6.27.15-170.2.24.fc10.i686 and I was happy until I did a ‘yum update’. My kernel got updated to kernel 2.6.27.19-170.2.35.fc10.i686, and then the system won’t boot the latest kernel. While booting it gave a mesage :

> Unable to access resume device (/dev/dm-1)
> 
> mount : could not find filesystem ‘/dev/root’

I wondered what had happened? But, the old kernel 2.6.27.15-170.2.24.fc10.i686 seemed to work perfectly. I became fanatic to resolve this issue. I did some googling, and finally came to know that the issue was with /etc/fstab and my Fedora 10 being installed on LVM. Here is my previous fstab details :

> # /etc/fstab: static file system information.  
>  #  
>  # <file system> <mount point>   <type>  <options>       <dump>  <pass>
> 
> tmpfs    /dev/shm    tmpfs    defaults    0    0  
>  devpts    /dev/pts    devpts    gid=5,mode=620    0    0  
>  sysfs    /sys    sysfs    defaults    0    0  
>  proc    /proc    proc    defaults    0    0  
>  /dev/dm-0    /    ext3    defaults    1    1  
>  #Entry for /dev/sda7 :  
>  UUID=3bb42530-5757-4f5a-9c10-16580ee6994a    /boot    ext3    defaults    1    2  
>  #Entry for /dev/sda5 :  
>  UUID=08563C60563C50A4    /media/Personal_Data    ntfs-3g    defaults,locale=en_US.UTF-8    0    0  
>  #Entry for /dev/sda2 :  
>  UUID=FCB60995B6095196    /media/c:    ntfs-3g    defaults,locale=en_US.UTF-8    0    0  
>  /dev/dm-1    swap    swap    defaults    0    0

But there were no /dev/dm-0 and /dev/dm-1 in /dev/ folder. The solution was to replace dm-0 by VolGroup00/LogVol00 and dm-1 by VolGroup00/LogVol01. Actually in LVM, the root is denoted by /dev/VolGroup00/LogVol00 and swap by /dev/VolGroup00/LogVol01. I made the necessary changes. Now, /etc/fstab looks like this:

NOTE : For the following steps, root access will be required, do ‘su -‘ first.

> # /etc/fstab: static file system information.  
>  #  
>  # <file system> <mount point>   <type>  <options>       <dump>  <pass>
> 
> tmpfs    /dev/shm    tmpfs    defaults    0    0  
>  devpts    /dev/pts    devpts    gid=5,mode=620    0    0  
>  sysfs    /sys    sysfs    defaults    0    0  
>  proc    /proc    proc    defaults    0    0  
>  /dev/VolGroup00/LogVol00    /    ext3    defaults    1    1  
>  #Entry for /dev/sda7 :  
>  UUID=3bb42530-5757-4f5a-9c10-16580ee6994a    /boot    ext3    defaults    1    2  
>  #Entry for /dev/sda5 :  
>  UUID=08563C60563C50A4    /media/Personal_Data    ntfs-3g    defaults,locale=en_US.UTF-8    0    0  
>  #Entry for /dev/sda2 :  
>  UUID=FCB60995B6095196    /media/c:    ntfs-3g    defaults,locale=en_US.UTF-8    0    0  
>  /dev/VolGroup00/LogVol01    swap    swap    defaults    0    0

Now I just needed to create initial ramdisk images for preloading modules. So I did :

> mkinitrd -f -v /boot/initrd-2.6.27.19-170.2.35.fc10.i686.img 2.6.27.19-170.2.35.fc10.i686
> 
> and also for the previous kernel
> 
> mkinitrd -f -v /boot/initrd-2.6.27.15-170.2.24.fc10.i686.img 2.6.27.15-170.2.24.fc10.i686

Then I did a reboot, and booting with the kernels (2.6.27.19-170.2.35 and 2.6.27.15-170.2.24) were as smooth as before.

That was all.

Thank you

Regards

rtnpro

