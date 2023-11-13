+++
author = "Ratnadeep Debnath"
categories = ["i18n", "l10n", "nplural", "plural", "plural form", "ratnadeep debnath", "turkish", "transifex", "rtnpro"]
date = 2012-03-11T06:23:09Z
description = ""
draft = false
slug = "dear-turkish-translators"
tags = ["i18n", "l10n", "nplural", "plural", "plural form", "ratnadeep debnath", "turkish", "transifex", "rtnpro"]
title = "Dear Turkish translators"

+++


Transifex usually defines plural rules for languages according to [http://unicode.org/repos/cldr-tmp/trunk/diff/supplemental/language_plural_rules.html](http://unicode.org/repos/cldr-tmp/trunk/diff/supplemental/language_plural_rules.html). So, the plural rule for Turkish language in Transifex is **other →** ***everything***. However, lately there has been some requests that the Turkish language should have two plural forms:

> nplurals=2; plural=(n>1)

The requests have been with reference to [http://translate.sourceforge.net/wiki/l10n/pluralforms](http://translate.sourceforge.net/wiki/l10n/pluralforms).

Here is a quote from a user at [https://bitbucket.org/indifex/transifex/issue/26/turkish-plural-forms](https://bitbucket.org/indifex/transifex/issue/26/turkish-plural-forms):

> Turkish behaves like Akan for example. The rule should be:
> 
> One: 0, 1 Other: 2-999
> 
> It is only when including a count that there are no plural forms. For example:
> 
> “You posted a photo”, “You posted several photos”
> 
> is correct in Turkish, as is:
> 
> “You posted 1 photo”, “You posted 6 photo”.

So, dear Turkish translators, please share your opinion on this issue. This will help a lot to resolve this issue at Transifex and fix plural translations in Turkish language.

