+++
author = "Ratnadeep Debnath"
categories = ["fad", "glade", "gtk", "kupfer", "kushal", "packaging", "Pune", "python", "python-keyring", "ratnadeep debnath", "Red Hat", "sayamindu", "siddhesh", "sqlite", "wordgroupz", "Fedora Activity Day", "rtnpro"]
date = 2010-06-01T08:46:08Z
description = ""
draft = false
slug = "fad-pune-2010"
tags = ["fad", "glade", "gtk", "kupfer", "kushal", "packaging", "Pune", "python", "python-keyring", "ratnadeep debnath", "Red Hat", "sayamindu", "siddhesh", "sqlite", "wordgroupz", "Fedora Activity Day", "rtnpro"]
title = "FAD Pune 2010"

+++


It was a great experience at the FAD in Red Hat, Pune. The FAD was conducted for two days, 29th and 30th May, 2010.

**Day 01**

Siddhesh took a session on autotools. The session was informative and interactive. I came to know how the big Makefile and Configure files are automatically generated from makefile.am and configure.ac (Makefile.in is first generated though, then the Makefile). Siddhesh used the linkc program for the purpose.

That was the only workshop. Then it was doing our own work. Everyone discussed what they will be working on. As for me, I decided to work on packaging python-keyring and kupfer and writing code for my application named **wordGroupz** (it is an app for building one’s vocabulary based on groups). My target was to get the code ready for 0.1 release of **wordGroupz**. For the first day, I made some changes in the python-keyring.spec as suggested by Ankur(FranciscoD) and Rahul (mether). Then I spent the rest of the time coding for kupfer, designed the GUI using Glade3. For programming, I used python, GTK, sqlite. By the end of the day, I managed to get  a input from the user and store it in the database. Updating in the combobox was not achieved that day.

In between, people from Bhasha Technologies came to the FAD to meet the ARM Fedora contributors who didn’t turn up. They shared some of their ideas with us over the lunch. I along with some of my friends (Rangeen) alongwith Salim decided to work on the suggested projects.

**Day 02**

I wrote a spec file for kupfer which wasn’t working for some unknown reasons. I submitted a review request for that and Ankur started reviewing it. Soon Rahul joined followed by Kushal. Kupfer uses a waf build system and the wscript was broken for kupfer. It didn’t produce any kupfer package, but was a good exercise. After that, Sayamindu gave a speech on OLPC and Sugar, which is a Fedora downstream project for the OLPC. Then, I resumed coding for **wordGroupz**. I managed getting the combobox updated on new entry. I made some changes in the glade file. I did some reading on the treeview model and got it to display the words from the database categorized into groups. I added a search facility in the app to search for words. As the day was ending, I thought to drop the displaying of word info for the 0.1 version.

At the end of the session, we reported what we achieved in the 2 days of the FAD. Then plans for future FADs were discussed. After the session, we (rtnpro, meejan, kishan, yevlempy) along with Rahul, Kushal and Salim went to Haka for dinner. Going to Haka was another story ![:)](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_smile.gif) .

It turned out to be a great day, working together throughout the day along with some fun also.

Arrangements made for our accommodation were great. Kushal took pics and recorded videos during the FAD. I hope kushal will upload them soon.

[https://fedoraproject.org/wiki/FAD_Pune_2010](https://fedoraproject.org/wiki/FAD_Pune_2010)

Currently, I am fixing some glitches in the code for wordGroupz. I have set a repository for it at

[http://gitorious.org/wordgroupz](http://gitorious.org/wordgroupz)

