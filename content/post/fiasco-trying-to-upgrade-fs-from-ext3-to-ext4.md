+++
author = "Ratnadeep Debnath"
categories = ["error", "ext3", "ext4", "Fedora", "filesystem", "lvm", "ratnadeep", "upgrade", "vgchange", "rtnpro"]
date = 2009-04-14T18:44:22Z
description = ""
draft = false
slug = "fiasco-trying-to-upgrade-fs-from-ext3-to-ext4"
tags = ["error", "ext3", "ext4", "Fedora", "filesystem", "lvm", "ratnadeep", "upgrade", "vgchange", "rtnpro"]
title = "Fiasco trying to upgrade fs from ext3 to ext4"

+++


It was afternoon when I tried my hands on trying to upgrade my filesysten from ext3 to ext4. I referred to :

> http://ext4.wiki.kernel.org/index.php/Ext4_Howto#Converting_an_ext3_filesystem_to_ext4

As mentioned… I did the following :

`<code style="white-space:nowrap;color:#495988;background-color:white;"># tune2fs -O extents,uninit_bg,dir_index /dev/<em>VolGroup00/LogVol00</em>`

`<code style="white-space:nowrap;color:#495988;background-color:white;"># e2fsck -fD /dev/<em>VolGroup00/LogVol00</em>`

*After quite some time, the procedure finally completed. I was so delighted that I did a sytem reboot to find that my system won’t boot. The reason, I didn’t update my kernel ramdisk image, and it tried to mount the / partition as ext3 and failed as the partition has already become ext4. What I forgot to do was :*

> *#mv /boot/initrd-‘uname -r’.img /boot/initrd-‘uname -r’.img.bak*

*// this is to keep a backup of the existing initrd image*

> *#mkinitrd -v –with=ext4 /boot/initrd-‘uname -r’.img ‘uname -r’*

*Also the following line in /etc/fstab :*

*UUID=fd296dfc-e7b3-4dc9-adf9-0038631d9c1f /                       ext3    defaults        1 1*

*needed to be updated as :*

*UUID=fd296dfc-e7b3-4dc9-adf9-0038631d9c1f /                       ext4    defaults        1 1  
*

*But, I was a bit hasty. I tried to correct this issue. I booted my system from the F11-Beta installed in my pocket hard drive. I learnt how to mount an LVM partition:*

> *#vgchange -ay*
> 
> *#mount /dev/VolGroup00/LogVol00 <mount point>*

*I tried updating the initrd images in the mounted LVM partition to find that the kernel modules for the particular kernel version of the initrd image was not found. As F11-Beta was running on a newer kernel. Tried to update the initrd images on other similar system as mine, copied the respective files in /boot and pasted it in my /boot folder and I tried to boot. It didn’t boot and gave an error message saying :*

> *cannot mount /dev/root to /sysroot*

*Any suggestions?  
*

