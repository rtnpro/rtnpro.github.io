+++
author = "Ratnadeep Debnath"
categories = ["BeautifulSoup", "Fedora", "linux", "python", "ratnadeep debnath", "pywebkitgtk", "urllib", "wiktionary", "wordgroupz", "wordnet", "rtnpro"]
date = 2010-07-23T20:15:54Z
description = ""
draft = false
slug = "wordgroupz-development-news"
tags = ["BeautifulSoup", "Fedora", "linux", "python", "ratnadeep debnath", "pywebkitgtk", "urllib", "wiktionary", "wordgroupz", "wordnet", "rtnpro"]
title = "wordgroupz development news"

+++


For some time now, I have been working on wordgroupz. Since version 0.2, I have added some new features.

The new features include dictionary support: online webster dictionary from dict.org and offline wordnet. For implementing the online dictionary support, I used the server interface of [dict.org](http://www.dict.org). For offline, I used the dictionary databases of the wordnet application and the python-nltk library.

I also included wiktionary support this time. I used to find it tedious opening a notebook, browser to search wikipedia/wiktionary, copy-paste notes. That’s why I decided to get them all together under one hood – wordgroupz. I include a gtkNotebook : 1st page for showing, editing details of the selected word and the 2nd page for browsing through wiktionary and downloading pronunciations if available. I had to study urllib2, BeautifulStone, pywebkitgtk for the purpose.

I also worked on the new interface, added some custom buttons to the interface. I studied gtkNotebook, gtkToolbar, gtkStockImage, etc. I also improved some logic for the interface. Now, wordgroupz allows to add words without any groups and such words are put in an “no-category” group. Now, there is a “details” field for group-words. Groups can be deleted. I have to write the code to move the words to the ‘no-category’ group once its parent group is deleted.

I released wordgroupz version 0.3b today. I have to work on machine generated pronunciation for words whose wiktionary pronunciation are not available. I have to design the games and quizzes for wordgroupz. Lots of stuff to do.

Please feel free to try wordgroupz and suggest any improvements to be done.

Details of how to get wordgroupz was given on my previous [post](http://ratnadeepdebnath.wordpress.com/2010/07/23/wordgroupz-v-0-3-beta-released/).

