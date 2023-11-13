+++
author = "Ratnadeep Debnath"
categories = ["askbot", "django", "django testing", "Fedora", "fudcon", "fudconin11", "pym", "security exploits", "transifex", "rtnpro"]
date = 2011-11-07T19:44:14Z
description = ""
draft = false
slug = "me-fudcon-india-2011"
tags = ["askbot", "django", "django testing", "Fedora", "fudcon", "fudconin11", "pym", "security exploits", "transifex", "rtnpro"]
title = "Me & FUDCON India 2011"

+++


[![](http://127.0.0.1:8080/wordpress/wp-content/uploads/2011/11/button.png "Button")](http://127.0.0.1:8080/wordpress/wp-content/uploads/2011/11/button.png)

I arrived in Pune for FUDCON on November 2, 2011. On November 3, 2011, I had an opportunity to visit the Red Hat office in the city and hang around with Fedora community members. I learnt more about the mechanism and importance of localization from Runa Bhattacharjee. In my free time, I also helped with some FUDCON related work. I  was able to meet Jared Smith, Joerg Simon and Robert Scheck for the first time in my life, talk with them. I couldn’t ask for anything better.

**Day 1: November 4, 2011**

Day 1 began with a speech from the Director of COEP followed by the keynote speech by Fedora’s Project Leader Jared Smith. Jared explained to people the various aspects of Fedora and how Fedora takes the lead in pushing the limits of FOSS development. Following that was my talk on Transifex. The audience included people mainly from the L10n domain. There were some mishaps during the talk. First, my Dell XPS laptop wasn’t able to use the projector; second, I had to use another netbook which I wasn’t used to; third, there were some power issues, and the netbook turned off and finally, there was power cut for a few minutes. What a chain of mishaps! I had to resort to reading the slides from my phone during the power cut. Finally, I used Kishan Goyal’s laptop to continue with the presentation. In  my talk, I explained the current features and upcoming features of Transifex. I explained it with a use case, starting from registering to advanced usage of [www.transifex.net](www.transifex.net). The audience appreciated our upcoming market place idea and the Translation Memory feature. I also got feature request for having a global glossary for a particular language in the language page. It was really nice that the audience were so actively communicating during the session. I also told how to start contributing to the Transifex project and shared my experience working on Transifex so far: from a contributor to an employee.

After the lunch, I attended [Heherson Pagcaliwagan](http://fudcon.in/users/azneita)‘s session on “Fedora web of trust” and got more insight into the use of GPG keys. We did a small workshop with Heherson on how to get introduced to each other, verify identity, share GPG key and sign it. Heherson also showed us how to encrypt mails using GPG key. Then I attended Joerg Simon’s talk on Fedora Security Lab and OSSTMM. Kital showed us a variety of security tools that can be found in the FSL, and mentioned others that need to be packaged. I must try out the tools in the FSL now. They are so cool.

Then I went back to the speaker’s lounge and started writing some code on Transifex. It was a great first day at FUDCON for me.

**Day 2: November 5, 2011**

The Second day of FUDCON began with Harish Pillay speaking about the community. Unfortunately, I was not able to turn up during the keynote, as I had to re create my slides on my talk on “Testing your Django app”. Because I had accidentally, deleted the folder which contained the slides. After I was done, I hurried to attend Arun Sag’s talk on “Creating web apps using Django”. I liked Arun’s way of presenting things to the newbies in a very lucid way. He used his classic Blog example for this.

Next was my talk on “Test your Django app”. As during Day 1, the projector did not work with my laptop, so I used Arun’s laptop for the purpose. I explained why tests are necessary, different testing frameworks in Django (doctest, unittest). Then I went forward to explain how to write simple unittests. For this, I wrote some tests for Arun’s blog example and used it so that the audience could relate things with the previous session taken by Arun. I showed some simple test cases and ran the tests. Then, I introduced the Django Test Client and spoke about its importance and features. After explaining things about the Test Client, I showed the relevant code and ran the tests again. Finally, I explained about coverage: what is it? why is it required? How to use coverage? I again ran the tests I had run before, but this time I ran them with coverage and explained how to read the coverage report. I haven’t been too happy with this session of mine. Now, when I think of it, for students who just got introduced to Django, the session on Django testing might have been asking too much. Anyway, the students now at least know something called Django testing exists. So, when they need it, they can learn.

After having lunch, I attended Siddhesh’s talk on “Security Exploits”. During the session, I could not but think that why I did not have someone like Siddhesh teaching me OS in my college. He was awesome. Back in my college days, I had tried reading about security exploits, but I did not get far. But during this session, thanks to Siddhesh’s explanations and my earlier reading, I have gained a better understanding of security exploits, especially stack smash attack, overwriting nearby entities in data region by string overflow, etc.

Following were two lightning talks on 1) How to deal with kernel panic? 2) Running external commands from Postgres. Yogesh in his talk on “kernel Panic” showed how to collect relevant data when there is a kernel panic. This data can be used for creating useful bug reports or for fixing the bug itself.

After attending Siddhesh’s talk on autotools, I joined mether’s talk on ask.fedoraproject.org or askbot. Mether discussed its roadmap and mentioned various feature requests. I picked up to implement a few of the feature requests. After the session, we had a group photo session of the almost all the people involved in FUDCON. Then all the speakers, volunteers and organizers went for the FUDPUB. I enjoyed a lot at the FUDPUB. I spoke with the community members, danced with them, drank Mirinda and ate some delicious food. It was just awesome.

**Day 3: November 6, 2011**

It’s the hackfest day. I decided to run a Transifex Testathon. I pitched the topic on stage and invited people to join me. I helped some of my friends install and setup transifex in their machines and showed them how to run tests. I started writing new tests for the watches addon in Transifex. I came across a chain of undiscovered bugs while doing so. It took me some time to write a proper test case for the watches and accordingly fix the bugs in the code. I also helped Kushal to get him logged into his Transifex account and creating a Transifex project for “Python for you and me”. Jared Smith, set up the tx client for “Python for you and me” and now PYM is hosted at [https://www.transifex.net/projects/p/pym/](https://www.transifex.net/projects/p/pym/) for localization. Shreyank, Vaidik and I had discussions on the roadmap for Dorrie. I setup Dorrie on my machine and played with it for some time. I decided to write tests for Dorrie the coming weekend.

Then, at the end of Day 3, in the auditorium, a cake was cut to celebrate this FUDCON along with quite a few photo shoots. The FUDCON organizing group and the volunteers from COEP did a great job to make this event go on smoothly.

FUDCON India 2011 is the first ever FUDCON in India and the largest FUDCON in terms of the number of participants. Apart from learning new stuffs and hacking, FUDCON provided a great platform for Fedora contributors to meet with each other and make new friends. It is also a nice experience to work with people whom I had known only in the IRC until now. I am carrying sweet memories of this FUDCON with me. These memories will help me focus more on contributing to open source and be a better contributor.

