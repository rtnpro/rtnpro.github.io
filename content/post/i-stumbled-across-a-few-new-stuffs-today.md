+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2008-12-29T19:53:50Z
description = ""
draft = false
slug = "i-stumbled-across-a-few-new-stuffs-today"
tags = ["rtnpro"]
title = "I stumbled across a few new stuffs today..."

+++


To find the PCI ID of a particular device use the <tt>lspci</tt> command. depending on the version and usage of the <tt>lspci</tt> command various output is shown. In Fedora, I use the following to identify the PCI ID (Note: the <tt>grep</tt> is added to filter the output):

$ /sbin/lspci -nn | grep 'VGA|NV' 01:00.0 VGA compatible controller [0300]: nVidia Corporation NV34 [GeForce FX 5200] [10de:<span style="color:#ff0000;">**0322**</span>] (rev a1)

Use the PCI ID and look it up under [Appendix A. Supported NVIDIA GPU Products](http://us.download.nvidia.com/XFree86/Linux-x86/177.82/README/appendix-a.html) in any

Nvidia driver documentation (also provided on the Nvidia website).

In this example the PCI ID is <tt>0322</tt>. According to *Appendix A*, this ID corresponds to the **

*Legacy version (173.14.xx series) driver*.

### Installation Using RPMFusion

To install the <tt>nvidia</tt> driver using RPMFusion and YUM.

**1. First** Install the repository configuration files for YUM. Run the following

(enter ‘root’ password when prompted):

[rtnpro@xps ~]$ su -c 'rpm -ivh http://download1.rpmfusion.org/free/fedora/rpmfusion -free-release-stable.noarch.rpm http://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-stable.noarch.rpm' [rtnpro@xps ~]$ su -c 'rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-rpmfusion-*'

**Note:** It is very important to know which driver your hardware requires. Typically many users will have newer hardware and not have to worry about this issue. However users with older hardware who install a newer driver may have non functional X-servers.

- - - - - -

