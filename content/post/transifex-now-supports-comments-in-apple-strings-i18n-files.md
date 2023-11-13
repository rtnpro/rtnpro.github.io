+++
author = "Ratnadeep Debnath"
categories = ["apple .strings", "comment", "i18n", "localization", "ratnadeep debnath", "Strings", "transifex", "apple", "rtnpro"]
date = 2012-01-13T10:18:37Z
description = ""
draft = false
slug = "transifex-now-supports-comments-in-apple-strings-i18n-files"
tags = ["apple .strings", "comment", "i18n", "localization", "ratnadeep debnath", "Strings", "transifex", "apple", "rtnpro"]
title = "#Transifex now supports comments in Apple .strings i18n files"

+++


#Transifex now supports comments in Apple .strings i18n files. Only /* foo */ style comment in the line preceding the key value pair in the source file is saved as a comment for the key. The example below will explain this in a better way:

[sourcecode]  
 /*Comment for key1*/  
 "key1" = "value 1";

/* This comment will not be  
 included in key2*/

/* comment for  
 key2*/  
 "key2" = "value 2";

/* this comment will not be included in key3*/

"key3" = "value 3";  
 [/sourcecode]  
 Well, I’m pretty sure that the above snippet explains which comments from source Apple .strings file are saved by Transifex. You can see the comment for a source string in its “Details” section in Lotte.

<figure class="wp-caption alignnone" id="attachment_384" style="width: 593px;">[![](http://127.0.0.1:8080/wordpress/wp-content/uploads/2012/01/download.png "AppleStringsComments")](http://127.0.0.1:8080/wordpress/wp-content/uploads/2012/01/download.png)<figcaption class="wp-caption-text">Comment for a source string imported from a source Apple .strings file</figcaption></figure>

