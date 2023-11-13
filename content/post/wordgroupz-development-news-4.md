+++
author = "Ratnadeep Debnath"
categories = ["gtk.cellrendererprogressbar", "gtk.treeviewcolumn.set_cell_data_fun", "python", "ratnadeep debnath", "pywebkitgtk", "urllib", "wiktionary", "wordgroupz", "wordnet", "rtnpro"]
date = 2010-08-24T05:43:04Z
description = ""
draft = false
slug = "wordgroupz-development-news-4"
tags = ["gtk.cellrendererprogressbar", "gtk.treeviewcolumn.set_cell_data_fun", "python", "ratnadeep debnath", "pywebkitgtk", "urllib", "wiktionary", "wordgroupz", "wordnet", "rtnpro"]
title = "wordGroupz development News"

+++


Although, I had added  the accuracy for the words in wordGroupz in the treeview, it was not satisfactory. First, the app crashed whenever I added a new word to the database. I fixed that easily. But then, the new word would show its accuracy as 0% even before attempting to answer it. I decided to set the CellRendererProgress bar text to ‘New’ for new/not attempted words.

I got some help for IRC Nick: Juhaz at #pygtk in GimpNet. I came to know that set different properties of a cell for a particular column in a gtk.TreeView, I have to set a function in gtk.TreeViewColumn.set_cell_data_func(). I got further help from the documentation at Develp in Fedora.

Finally, after some tries, I got the code up and running. Now new words show ‘New’ and after being attempted, they show their respective accuracy.

I also wrote some code to enable migration from old wordgroupz db schema to new db schema with retention of the old db contents.

I have pushed the latest code to gitorious.

If you want to try the code, you can get the source from [http://gitorious.org/wordgroupz](http://gitorious.org/wordgroupz) . Then, you can install wordgroupz in your system as:

#python setup.py install

Then run wordgroupz : $wordgroupz

Now, I need to do a code cleanup.

