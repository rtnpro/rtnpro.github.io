+++
author = "Ratnadeep Debnath"
categories = ["c", "comment", "parse", "ratnadeep debnath", "regex", "style", "rtnpro"]
date = 2011-10-27T11:47:27Z
description = ""
draft = false
slug = "regex-pattern-for-c-style-comments"
tags = ["c", "comment", "parse", "ratnadeep debnath", "regex", "style", "rtnpro"]
title = "Regex pattern for c style comments"

+++


Today, I am going to discuss my attempts to parse c style comments.

For example,

**`//This is a comment<br></br>`**

**```
/***This is also<br></br>
*** a comment ***/```
**

Initially, I came up with a regex for /*…*/ style comments :  
`<strong>/*.**/</strong>`  
 Well, the above expression was not able to parse comments like:  
```
<br></br>
/*** This is a comment ***/<br></br>```

I googled and came across [http://ostermiller.org/findcomment.html](http://ostermiller.org/findcomment.html) where I found the regex:  
`<strong>/*(.|[rn])*?*/</strong>`  
 This was able to match comments like the above one. But it’d also match the following /*…*/ comments which are not really comments:  
```
<br></br>
s = "This is a string: /* with a comment */";<br></br>
//comment1 /*<br></br>
foo();<br></br>
//comment2 */<br></br>```

I then worked on a regex for //… style comments: `//[^n]*n`

Then I combined the two regexes by or and my regex pattern becomes:

> `<strong>//[^n]*n|/*(.|[rn])*?*/</strong>`

Now, this pattern is able to search for both: //… and /*…*/ style comments and avoid matches for patterns like:  
```
<br></br>
//comment1 /*<br></br>
foo();<br></br>
//comment2 */```

One caveat that remains is the /*…*/ pattern in  
`s = "This is a string: /* with a comment */";`  
 getting matched. If any one has a work around this issue, please comment.

I hope this helps.

