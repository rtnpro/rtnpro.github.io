+++
author = "Ratnadeep Debnath"
categories = ["bash", "Dorrie", "dummy repository", "Fedora", "ratnadeep debnath", "rsync", "rtnpro"]
date = 2012-03-14T14:48:28Z
description = ""
draft = false
slug = "create-a-dummy-fedora-repository"
tags = ["bash", "Dorrie", "dummy repository", "Fedora", "ratnadeep debnath", "rsync", "rtnpro"]
title = "Create a dummy Fedora repository (for Dorrie)"

+++


I have been fiddling with [Dorrie](https://github.com/shreyankg/Dorrie) lately. Dorrie is a web interface to create Fedora spins, remixes and installation media. But working with Dorrie requires one to have a Fedora repository at disposal if one has to build Live ISOs. I rule the possibility of downloading packages from the internet for testing. You need to have a local repository for testing it in a nice and efficient way. I tried to create a dummy Fedora repository with 0-byte .rpm files and tried to mock livecd-creator, but I failed. I forgot that livecd-creator installs the packages during the process of creating a Live ISO. Nevertheless, this attempt was not without any result. I fixed and updated Dorrie’s code to handle offline repositories.

Now, we (I and sayan) are working on another feature for Dorrie: building installation media (just like Fedora installation DVD). We are using pungi for the purpose. If my calculations are correct, then building an installation media leaves an option to use 0-byte .rpm files. But, this is not the topic of this post ;). I will talk on how to create a dummy Fedora repository.

Fedora repositories mainly have two important components for a Fedora version: releases and updates. All files in the dummy repository can’t be fake, the metadata files have to be there for real, otherwise the packages names won’t be found. I have devised a shell script for creating such a dummy repository.

[sourcecode language=”bash”]

#!/bin/bash  
 # Destination directory  
 DESTDIR="dummy_repo/"

# RSYNC URL  
 URL="rsync://fedora.c3sl.ufpr.br/fedora/linux/releases/16/"

#file extensions to be touched  
 FILE_EXTENSIONS_PATTERN=".*.rpm$|.*.iso$|.*.img$"

#patterns to exclude from rsync  
 EXCLUDE_PATTERN="–exclude=source/ –exclude=debug/ –exclude=repoview/  
 –exclude=x86_64 –exclude=Everything/ –exclude=Live/"

#This will create only the directories and 0-byte .iso and .rpm files  
 #Since the files created will have newer timestamp, when we run  
 #rsync for real, the .iso and .rpm files won’t be downloaded  
 rsync -auvr $URL $DESTDIR –dry-run $EXCLUDE_PATTERN |  
 while read line; do  
 if [ `expr match "$line" ".*/$"` -ne 0 ]; then  
 echo $line  
 mkdir -p $DESTDIR/$line  
 elif [ `expr match "$line" "$FILE_EXTENSIONS_PATTERN"` -ne 0 ]; then  
 echo $line  
 touch $DESTDIR/$line  
 fi  
 done

#Download metadata files  
 rsync -auvr $URL $DESTDIR $EXCLUDE_PATTERN –progress

[/sourcecode]

This is just a sample code. The above code can be customized to do more versatile dummy cloning of Fedora repositories.

