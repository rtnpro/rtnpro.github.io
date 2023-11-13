+++
author = "Ratnadeep Debnath"
categories = ["auto save", "javascript", "jquery", "lotte", "ratnadeep debnath", "spellcheck", "textarea.blur", "transifex", "rtnpro"]
date = 2011-07-01T05:00:04Z
description = ""
draft = false
slug = "more-fine-tuned-auto-save-feature-in-lotte-for-transifex"
tags = ["auto save", "javascript", "jquery", "lotte", "ratnadeep debnath", "spellcheck", "textarea.blur", "transifex", "rtnpro"]
title = "More fine tuned auto save feature in Lotte for Transifex"

+++


Lotte is the component of Transifex providing the web UI for the translators to translate online. Well, some time back, I made some changes in the Lotte code so that it has the auto-save feature on by default. It was running fine then and I was happy. Then I added the spellcheck feature in Lotte. Then the things started going not as expected.

Actually, the auto-save triggers when the contents of a text area has been edited and it loses focus. The first problem I faced is that the spellcheck button did not respond on the first click, but on the second. As expected, on the first click, auto-save was triggered. But this was not the case when I clicked the Undo button. First the auto-save and then the undo function would be executed. I was not able to come up with an answer to explain this. I was tinkering with the code and then accidentally I solved the problem. I just changed the order of the buttons. Initially I had – Spellcheck button , Save Button and Undo Button. Now, I have – Save button, Spellcheck button, Undo button. Weird! You can have a look at the change at [https://bitbucket.org/rtnpro/transifex/changeset/b59880cadf67](https://bitbucket.org/rtnpro/transifex/changeset/b59880cadf67) . If you have an explanation for this, please comment.

One problem gets fixed and another comes to your mind. The auto-save function was being an overhead. The function would be triggered even if I press the spellcheck button, auto translate and copy source buttons in the same row. I am still editing the same string! The way it should be is  that it should trigger when I am finished with editing the string.

So, what I did is that I moved the code saving the string from the function being called by the blur event of the textarea to a new function. Whenever a textarea being edited receives the blur event, I save the edited string, its id and a must_push flag for future use. Now, whenever any other textarea receives focus, depending on the must_push flag, the string save function is called. This was just one part of the problem I fixed. Now, I have to deal with the click events that might take place in various parts of the page and it should save the edited string from the textarea that just lost focus. My save function is already ready and it deals with the required checks on whether to save or not. I just need to call the new save function. I just browsed through the html tree, found a list of elements and bound their click event to the save function. As for the table rows, I bound the click event of all the rows other than the current row to the save function. Here is the commit implementing the new auto-save feature : [https://bitbucket.org/rtnpro/transifex/changeset/5a7c2a2e13ea](https://bitbucket.org/rtnpro/transifex/changeset/5a7c2a2e13ea)  . I had quite some jquery drill to fix these issues ![:)](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_smile.gif) .

Well, now Lotte with new auto-save feature is ready. Nice and elegant. I hope it makes up to the expectations of the translators. That’s what people strive at Transifex.

