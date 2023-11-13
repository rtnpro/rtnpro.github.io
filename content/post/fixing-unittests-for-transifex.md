+++
author = "Ratnadeep Debnath"
categories = ["ratnadeep debnath", "transifex", "rtnpro"]
date = 2011-06-11T10:26:35Z
description = ""
draft = false
slug = "fixing-unittests-for-transifex"
tags = ["ratnadeep debnath", "transifex", "rtnpro"]
title = "Fixing unittests for transifex"

+++


Last week, I have been working on fixing the existing unittests for Transifex. The Transifex codebase is constantly being updated. So, some of the previous test cases failed. Before this, I had already written a unittest or two for some of the codes that I contributed to Transifex. But I did not study the Django unittest framework deeply for that.

This time, it was different. I took my time to go through the Django documentation on testing. And then I started to work on fixing the existing tests. To avoid confusion, I ran test on each component separately. Only a few were containing errors like projects, lotte, release, resources, projects, txcommon, suggestions, charts, etc.

I started working on one component at a time. After fixing it, I committed and pushed my changes. Then I proceeded to the next one. In between, I was always in touch with diegobz, kbairak, messas. They helped a lot. I once ran into a bug due to haystack module. I reported this to diegobz. He fixed the issue with haystack quickly.

There was another instance when a particular test was showing errors in my box but not for anyone else. I struggled with it for sometime. Then, kbairak told me that the Transifex code is now compatible with Django-1.2.5, but will become compatible with Django-1.3 soon. And here I was running Django-1.3. kbairak fixed some code to make it compatible with Django-1.3.

The Transifex upstream ROCKS!!!

