+++
author = "Ratnadeep Debnath"
categories = ["bcrec", "dgplug", "Fedora", "mirror", "ratnadeep debnath", "rtnpro"]
date = 2009-11-02T16:34:58Z
description = ""
draft = false
slug = "fedora-11-offline-mirror-setup-at-bcrec"
tags = ["bcrec", "dgplug", "Fedora", "mirror", "ratnadeep debnath", "rtnpro"]
title = "Fedora 11 offline mirror setup at BCREC"

+++


Saturday, 31 October, 2009 I set up an offline Fedora 11 mirror in BCREC. I used the fedx makefile for the purpose of syncing the repository from my pocket HDD to the mirror PC ( an HP dual core system with Fedora 10 installed in it). I then setup a dhcpd server in the machine with the help of gdhcpd. I wrote a how to use the service and put it on the Desktop. I also wrote a fedora-local.repo which could be used by the clients to configure their machines to use the service. This should enable newbies without internet connection to get packages and softwares of Fedora 11.

