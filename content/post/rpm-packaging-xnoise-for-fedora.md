+++
author = "Ratnadeep Debnath"
categories = ["bugzilla", "Fedora", "gstreamer", "koji", "package review", "packaging", "ratnadeep debnath", "rpm", "xnoise", "rtnpro"]
date = 2010-04-28T04:59:49Z
description = ""
draft = false
slug = "rpm-packaging-xnoise-for-fedora"
tags = ["bugzilla", "Fedora", "gstreamer", "koji", "package review", "packaging", "ratnadeep debnath", "rpm", "xnoise", "rtnpro"]
title = "RPM Packaging xnoise for fedora"

+++


This is the first time I am packaging a software.  
 I chose xnoise. I have been using xnoise (compiled  
 from the source repository) for sometime and I like it.

**What is Xnoise?**

Xnoise can play every kind of audio/video data that gstreamer  
 can handle. Xnoise uses a tracklist centric design and uses  
 a hierarchical tree structure media browser. It also has a  
 plugin interface. Xnoise is always running in a single instance,  
 so that music files that are associated with it, will always be  
 added to the tracklist instead of starting a new instance.

Xnoise is written in vala. This means it is compiled to pure  
 Gobject/C and therfore very fast and lightweight compared  
 with other Gtk music players written in mono or python.

**License :**

The license of xnoise is GNU GPL v2 or later. There is a  
 license exeption for the distribution together with non-GPL  
 compatible gstreamer plugins.

SPEC URL : [http://rtnpro.dgplug.org/Packages/xnoise/xnoise.spec  
](http://rtnpro.dgplug.org/Packages/xnoise/xnoise.spec SRPM URL : http://rtnpro.dgplug.org/Packages/xnoise/xnoise-0.1.2-1.fc12.src.rpm Bugzilla URL : https://bugzilla.redhat.com/show_bug.cgi?id=586433 Koji scratch build URL : https://koji.fedoraproject.org/koji/taskinfo?taskID=2141814) SRPM URL :[ http://rtnpro.dgplug.org/Packages/xnoise/xnoise-0.1.2-1.fc12.src.rpm  
](http://rtnpro.dgplug.org/Packages/xnoise/xnoise-0.1.2-1.fc12.src.rpm) Bugzilla URL : [https://bugzilla.redhat.com/show_bug.cgi?id=586433  
](https://bugzilla.redhat.com/show_bug.cgi?id=586433) Koji scratch build URL :  
[https://koji.fedoraproject.org/koji/taskinfo?taskID=2141814](https://koji.fedoraproject.org/koji/taskinfo?taskID=2141814)

