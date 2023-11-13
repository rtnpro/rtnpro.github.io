+++
author = "Ratnadeep Debnath"
categories = ["python", "ratnadeep debnath", "pywebkitgtk", "urllib", "wiktionary", "wordgroupz", "wordnet", "rtnpro"]
date = 2010-08-29T10:58:06Z
description = ""
draft = false
slug = "wordgroupz-development-news-5"
tags = ["python", "ratnadeep debnath", "pywebkitgtk", "urllib", "wiktionary", "wordgroupz", "wordnet", "rtnpro"]
title = "wordgroupz development news"

+++


Lately, I worked on fine tuning the code for wordgroupz. I fixed some bugs that I was aware of, like unable to parse data retrieved from wiktionary pages which did not have any ‘Contents’ field, error launching games when no words in db, some bugs in webster view, etc. Now, I have added a dialog which will show a message saying ‘Not enough data’ if the number of words in the db are not enough for playing games. I have also added support for parsing data from wiktionary pages which have no ‘Contents’ field. Also made some fixes in the webster view.

I today updated the RPM for wordgroupz to wordgroupz-0.3b-4.fc13.noarch.rpm

You can install the latest version of wordgroupz as follows:

1) download [rtnpro.repo](http://rtnpro.fedorapeople.org/rtnpro.repo) into /etc/yum.repos.d/

2) then as root, do :

yum install wordgroupz

You can also get the source code from [http://gitorious.org/wordgroupz/](http://gitorious.org/wordgroupz/)

Please test wordgroupz, and feel free to drop in your suggestions.

