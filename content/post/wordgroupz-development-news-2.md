+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2010-08-11T17:52:37Z
description = ""
draft = false
slug = "wordgroupz-development-news-2"
tags = ["rtnpro"]
title = "wordgroupz development news"

+++


Finally got time to write this post.

<div>I’ve been a lot busy lately. Had to do some changes in wordgroupz.</div><div>I was working on the output display format for wordgroupz, i.e, how to show the definitions stored in the wordgroupz database. I created a policy that whenever a word is added, wordgroupz will fetch its definition from the default “Wordnet” dictionary. Then one has the option to search for more of the word’s details on wiktionary from within wordgroupz. Once the wiktionary page is viewed online, the wiktionary data is saved in the database.</div><div>I have implemented a tabbed design for viewing the details of the word: one for wordnet and the other for wiktionary, till now. I have to expand it for webster as well. A few extra lines of code will do that. The rest remains the same.</div><div>Below are the screenshots of the new tabbed view:</div><div>![](http://rtnpro.fedorapeople.org/images/wordnet_view.png)</div><div><span style="text-decoration:underline;">Wordnet output</span></div><div>![](http://rtnpro.fedorapeople.org/images/wik_view.png)</div><div><span style="text-decoration:underline;">Wiktionary Output View</span></div><div><span style="text-decoration:underline;">  
</span></div><div>Then, I started working on the games frontend. I took inspiration from kwordquiz. I put the flash card and the multiple-choice-question in a tabbed view. I am also recording the number of correct responses given along with the attempts made on a particular word in both the games : flash-card and MCQ. This will help the user to identify the words that are difficult to remember for him. Thus, he can pay more attention on those words. Below are the screenshots for the games module.</div><div>![](http://rtnpro.fedorapeople.org/images/flash_card.png)</div><div>Flash-Card</div><div>![](http://rtnpro.fedorapeople.org/images/mcq.png)</div><div>MCQ</div><div>Have to improve the GUI for games. The functional code is almost done. Now, remains beautification.</div><div>I request readers of this blog to give their kind suggestions to make the UI better.</div><div>To test the games module, you can do :</div><div>$git clone git://gitorious.org/wordgroupz/wordgroupz.git</div><div>$cd wordgroupz</div><div>$python wordgroupz.py</div><div>Note: Now you can add some words to wordgroupz</div><div>Note: run the games module</div><div>$python games.py</div><div><span style="text-decoration:underline;">  
</span></div>The post is brought to you by [lekhonee-gnome](http://fedorahosted.org/lekhonee) v0.9

