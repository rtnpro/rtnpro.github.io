+++
author = "Ratnadeep Debnath"
categories = ["hard drive", "127.0.0.1", "Maxtor", "mirror", "offline", "portable", "setup", "vsftpd", "fedora-mirror", "rtnpro"]
date = 2008-12-29T19:14:24Z
description = ""
draft = false
slug = "i-wrote-a-shell-script"
tags = ["hard drive", "127.0.0.1", "Maxtor", "mirror", "offline", "portable", "setup", "vsftpd", "fedora-mirror", "rtnpro"]
title = "I wrote a shell script..."

+++


We were facing a lot of difficulties in setting up Fedora after the installation. Reason? Poor internet connection in the locality. It was a great dissatisfaction for me and Kishan (my friend) and also for the newbies. To resolve this, Arindam Ghosh gave me a copy of “Fedora-mirror” and taught me the How-Tos to setup an offline Fedora-mirror and do the requisite installations. It was really a needed gift for us. We set-up many a system successsfully with its help.

Then I thought of trying to automate this process. So, I started writing shell scripts. My idea was simple. Take a backup of the contents of **yum.repos.d** and place the local repository files in yum.repos.d. Start the ftp server (**vsftpd**), and use the loopback (**lo**) interface which generally has the proxy **127.0.0.1**. Then provide the user with a terminal to do the requisite installations. Then exiting that terminal, turning off the ftp server and replacing the original contents of  yum.repos.d. I transferred the entire Fedora-mirror to my **Maxtor-Portable** hard drive.

I created 3 shell scripts – 1)mirror-setup.sh, 2)mirror-start.sh and 3)mirror-stop.sh.

The user jsut needs to invoke the mirror-setup.sh as root and he gets the terminal for doing installations. To exit the terminal, he just needs to type ‘exit’ and press <enter>. The scripts mirror-start.sh is for starting the ftp mirror and the mirro-stop.sh is for stopping the mirror. These two scripts are being called by the mirror-setup.sh. Below are the details of the scripts….

1)mirror-setup.sh===================================================================

> #!/bin/sh
> 
> cd /media/My storage/fedora-mirror/
> 
> sh ./mirror-start.sh  
>  echo You are now ready to install packages offline…To  quit…enter ‘exit’  
>  su –  
>  sh /media/My storage/fedora-mirror/mirror-stop.sh  
>  echo Local Fedora-mirror stopped…

2)mirror-start.sh=====================================================================

> #!/bin/sh
> 
> /etc/init.d/vsftpd start
> 
> mkdir /tmp/yum.repos.bak  
>  mv /etc/yum.repos.d/* /tmp/yum.repos.bak/
> 
> cd ~
> 
> mount –bind /media/My storage/fedora-mirror/ /var/ftp/pub/
> 
> cd /etc/yum.repos.d/
> 
> wget -ivh ftp://127.0.0.1/pub/yum/*

3)mirror-stop.sh=====================================================================

> #!/bin/sh
> 
> rm  /etc/yum.repos.d/*  
>  mv /tmp/yum.repos.bak/* /etc/yum.repos.d/
> 
> rm -r /tmp/yum.repos.bak

Any suggestions or comments on the topic are welcome.

Thank you.

