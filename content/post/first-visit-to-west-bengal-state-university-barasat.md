+++
author = "Ratnadeep Debnath"
categories = ["barasat", "Blade server", "chandana", "Fedora", "FOSS", "hp", "harsh", "kishan", "Liebert UPS", "linux", "meejan", "opencomms web card", "indradg", "ratnadeep debnath", "rpmfusion", "rsync", "screen", "ssh", "stephdg", "wbsu", "rtnpro"]
date = 2009-07-13T18:50:52Z
description = ""
draft = false
slug = "first-visit-to-west-bengal-state-university-barasat"
tags = ["barasat", "Blade server", "chandana", "Fedora", "FOSS", "hp", "harsh", "kishan", "Liebert UPS", "linux", "meejan", "opencomms web card", "indradg", "ratnadeep debnath", "rpmfusion", "rsync", "screen", "ssh", "stephdg", "wbsu", "rtnpro"]
title = "First visit to West Bengal State University, Barasat ..."

+++


Today, 13th July, 2009, we ( me , i.e., Ratnadeep Debnath aka rtnpro, Kishan Goyal, Meejanur Rahaman, Harsh Verma, Chandana Boral) led by Indranil Das Gupta and Stephanie Das Gupta, went to West Bengal State University at Barasat, Kolkata. We started at around 10:30 AM from Ruby Hospital, and after a long journey ( changing two buses, then riding on a van, with  a few drizzles  on the way), we finally reached Barasat University at around 12:30 PM.

We were then taken to the server room of Barasat University. Indradg got us introduced to the wiring and connections in the server room,  the big UPS, the batteries being charged by the UPS, the mechanism for providing back up during power cuts, switching between the two ACs periodically and finally the HP Blade Server.

The UPS is a Liebert GXT-MT 6KVA UPS with a OpenComms Web Card. The OpenComms Web Card delivers SNMP (Simple Network Management Protocol) and Web support to the UPS in which it is installed. Then we were acquainted with the network configuration of the University Network ( wifi + lined connection).

Next job was to download the Fedora 11 repository. We brought in another computer, placed it in place of the server and installed Fedora 10 in it and started working. We were given the required bandwidth and my job was to start the rsync. But it didn’t go well with me. I was confused with how and where to start. I did rsync locally in earlier instances and not over the internet. I started with the man page. Then googled about “rsync”. Found some good documentation at [http://fedoraproject.org/wiki/Infrastructure/Mirroring](http://fedoraproject.org/wiki/Infrastructure/Mirroring). It mainly dealt with setting up Fedora mirrors and how to enable rsync in them. But what I needed was just use rsync to pull Fedora 11 repository. It was quite some time, I was still stuck. Got some suggestions on configuring rsyncd.conf file, I was again redirected to the above link. Then we had some lunch and break.

I again sat working on it. Again went through the rsync man page. I found that,

> $rsync [options] [source]

gives a directory listing of the source directory. Chose rsync://ftp.riken.jp/fedora as the source. Found its directory structure using

> $rsync -auvr rsync://ftp.riken.jp/fedora

I created the directory structure in my current folder ( here it was /home/$USER/f11_repo/). Now I started pulling Fedora 11 i386 release packages in the following way

> $rsync -auvr rsync://ftp.riken.jp/linux/fedora/releases/11/Everything/i386/os/ /home/$USER/f11_repo/linux/fedora/releases/11/Everything/i386/os/

and it started syncing the source and destination directories.

Then we also started downloading the rpmfusion_free repositories for Fedora 11 using wget. Indradg wrote a shell script to automate the download of rpmfusion/free repository using wget. We also used screen to run the two processes in different screens so that they can be monitored remotely.

> $screen -R [screen name]

In the mean time, others prepared some “Do not disturb, work in progress” labels and put them on the running computer, the server room door, etc. to make sure that no one messes with the running computer.

It was around 5:30 PM that we finally packed up. We got into the University Bus and reached Ruby Hospital at around 7:30 PM.

It was a long long day :).

