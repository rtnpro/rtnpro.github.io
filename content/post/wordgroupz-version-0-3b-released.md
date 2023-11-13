+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2010-07-22T21:43:38Z
description = ""
draft = false
slug = "wordgroupz-version-0-3b-released"
tags = ["rtnpro"]
title = "wordgroupz development news"

+++


Lately, I have been working on adding new features to **wordgroupz** like dictionary support, wiktionary support, pronunciation support. I have also made some changes to the **wordgroupz** gui optimized for the new features.

<div>**wordgroupz** v 0.2 screenshot:</div><div>![](http://rtnpro.fedorapeople.org/Screenshot-wordGroupz.png)</div><div>![](http://home/rtnpro/Pictures/Screenshot-wordGroupz.png)</div><div>The dictionary support features online dict.org webster dictionary and a offline wordnet dictionary. I used the server interface to retrieve definitions from it.</div><div>I used the nltk library to retrieve definitions from wordnet dicitionary database.</div><div>I implemented the wiktionary support using webkit, BeautifulSoup and urllib2. Pronunciations if availale on wiktionary are allowed to be downloaded.</div><div>The new interface looks like this below:</div><div>![](http://rtnpro.fedorapeople.org/Screenshot-wordGroupz-1.png)</div><div>Due to the frequent changes made in the **wordgroupz** repository at [http://gitorious.org/**wordgroupz**/**wordgroupz**/trees/master](http://gitorious.org/wordgroupz/wordgroupz/trees/master), I have not been able to update the setup.py, README, etc. files. I also added a nltk*tar.gz in the src folder, that looks weird. I will be cleaning up the repository.</div><div>For the time being, to test the code, follow the following steps:</div><div>1) download the src code</div><div>2) do not install it via setup.py</div><div>3) extract the nltk*.tar.gz and install the nltk python module from the setup.py inside nltk.</div><div>4) install pywebkitgtk</div><div>5) Now run the new interface using wordgroupz_new.py</div><div>All the previous features have not been ported to the wordgroupz_new.py, which I wrote while coding for the wiktionary support. I will updating the file soon.</div><div>Please give your valuable feedback for improvements.</div>The post is brought to you by [lekhonee-gnome](http://fedorahosted.org/lekhonee) v0.9

<div>// // </div>

