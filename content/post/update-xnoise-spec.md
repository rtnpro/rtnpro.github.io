+++
author = "Ratnadeep Debnath"
categories = ["koji scratch build", "package", "package review", "ratnadeep debnath", "rpm", "rsync", "update", "vala", "xnoise", "rtnpro"]
date = 2010-05-01T02:51:27Z
description = ""
draft = false
slug = "update-xnoise-spec"
tags = ["koji scratch build", "package", "package review", "ratnadeep debnath", "rpm", "rsync", "update", "vala", "xnoise", "rtnpro"]
title = "Update xnoise.spec"

+++


I got a nice and long review for my first package of xnoise :). You can read the review at

[https://bugzilla.redhat.com/show_bug.cgi?id=586433](https://bugzilla.redhat.com/show_bug.cgi?id=586433)

I made the suggested changes –

> 4c4  
>  < Summary:        A media player for gtk+  
>  —  
>  > Summary:        A media player for GTK+  
>  6c6  
>  < License:        GPLv2  
>  —  
>  > License:        GPLv2+  
>  12c12,14  
>  < Source0:        xnoise-0.1.2.tar.gz  
>  —  
>  > #   make dist  
>  > #to generate xnoise-0.1.2.tar.bz2  
>  > Source0:        xnoise-%{version}.tar.bz2  
>  15,16c17,20  
>  < BuildArch:      i686  
>  < BuildRequires:  vala, vala-devel, intltool, libtool, autogen, automake >= 1.11, gnome-common, gtk2-devel, sqlite-devel, taglib-devel, unique-devel, gstreamer-devel, gstreamer-plugins-base-devel, gettext, desktop-file-utils  
>  —  
>  > #BuildArch:      noarch  
>  > BuildRequires:  intltool, libtool, autogen, automake >= 1.11  
>  > BuildRequires:  gnome-common, gtk2-devel, sqlite-devel, taglib-devel  
>  > BuildRequires:  unique-devel, gstreamer-devel, gstreamer-plugins-base-devel, gettext, desktop-file-utils  
>  18c22  
>  < Requires:   gstreamer, gstreamer-plugins-base, gtk2, gettext  
>  —  
>  > #Requires:   gstreamer, gstreamer-plugins-base, gtk2  
>  21,32c25,27  
>  < Xnoise is a media player for gtk+  
>  < Xnoise is written in vala  
>  < Xnoise can play every kind of audio/video data that gstreamer can handle  
>  <  
>  < #%package devel  
>  < #Summary:    Development files for %{name}  
>  < #Group:      Development/Libraries  
>  < #License:    GPLv2  
>  < #Requires:   %{name} = %{version}-%{release}  
>  < #Requires:   pkgconfig  
>  < #%description devel  
>  < #Development packages for Xnoise  
>  —  
>  > Xnoise is a media player written in vala for GTK+.  
>  > Xnoise can play every kind of audio/video data that gstreamer can handle.  
>  > Xnoise uses a tracklist centric design and uses a hierarchical tree structure media browser along with plugin interface. Xnoise is always running in a single instance,so that music files that are associated with it, will always be added to the tracklist instead of starting a new instance.  
>  67,70c62  
>  < %{_libdir}/pkgconfig/xnoise-1.0.pc  
>  < #%files devel  
>  < #%{_libdir}/pkgconfig/xnoise-1.0.pc  
>  < #%{_libdir}/xnoise  
>  —  
>  > %exclude %{_libdir}/pkgconfig/xnoise-1.0.pc  
>  74,75c66,69  
>  < * Tue Apr 27 2010 rtnpro <rtnpro@gmail.com> 0.1.2  
>  < – RPM package xnoise  
>  —  
>  > * Sat May 1 2010 rtnpro <rtnpro@gmail.com> 0.1.2-2  
>  > – xnoise.spec file cleanup and some edits  
>  > * Tue Apr 27 2010 rtnpro <rtnpro@gmail.com> 0.1.2-1  
>  > – initial RPM package xnoise

Submitted a scratch build at

[https://koji.fedoraproject.org/koji/taskinfo?taskID=2152364](https://koji.fedoraproject.org/koji/taskinfo?taskID=2152364)

It got successfully packages this time for i686, ppc, x86_64 archs

Finally I updated the spec and SRPM file at rtnpro.dgplug.org via rsync.

Updated files at :

[http://rtnpro.dgplug.org/Packages/xnoise/xnoise-0.1.2-1.fc12.src.rpm](http://rtnpro.dgplug.org/Packages/xnoise/xnoise-0.1.2-1.fc12.src.rpm)

[http://rtnpro.dgplug.org/Packages/xnoise/xnoise.spec](http://rtnpro.dgplug.org/Packages/xnoise/xnoise.spec)

