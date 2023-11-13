+++
author = "Ratnadeep Debnath"
categories = ["rtnpro"]
date = 2010-10-15T08:36:27Z
description = ""
draft = false
slug = "worgroupz-version-0-3-1-released"
tags = ["rtnpro"]
title = "worgroupz version 0.3.1 released"

+++


Nothing new in the version 0.3.1 though. I removed the dependency on the python-nltk library. Extracted the requisite code from the python-nltk library to access only the wordnet dictionary. Also, there has been some clean ups. 

<div></div><div>Future plans:</div><div>I am thnking to get rid of the dependency on the wordnet package as it brings in some other unnecessary dependency like Tcl. I am thinking of providing the wordnet dictionary as a default plugin. It will be installed if wordnet dictionary is not already installed. Also, I am planning to incorporate a dictionary view, and some kind of dashboard to wordgroupz.</div>

