+++
author = "Ratnadeep Debnath"
categories = ["extract", "format string", "i18n", "python", "ratnadeep debnath", "regex", "replacement fiel", "replacement field", "transifex", "validator", "_formatter_parser", "rtnpro"]
date = 2012-09-02T07:29:55Z
description = ""
draft = false
slug = "validate-python-string-translation-in-transifex"
tags = ["extract", "format string", "i18n", "python", "ratnadeep debnath", "regex", "replacement fiel", "replacement field", "transifex", "validator", "_formatter_parser", "rtnpro"]
title = "Validate Python string translation in Transifex"

+++


Transifex already supported validating translations of old styled Python strings, e.g.,

[sourcecode language=”python”]  
 "A sample string with a %(keyword)s argument." % {‘keyword': ‘key word’}  
 [/sourcecode]

The validation is done by checking if all the positional and keyword arguments are present in the translation string and the translation string does not contain any extra argument which is not in the source string. You can have a look at the validator code [here](https://github.com/transifex/transifex/blob/devel/transifex/resources/formats/validators.py#L232).

However, the existing validator is not able to check for replacement fields in new style [Python format strings](http://docs.python.org/library/string.html#format-examples), e.g.

[sourcecode language=”python”]  
 "This is a sample string with different replacement fields: {} {1} {foo["bar"]:^30}".format(  
 "arg0", "arg1", foo={"bar":"a kwarg"})  
 [/sourcecode]

I tried to devise a regex to extract the replacement fields in the Python format string based on the grammar defined [here](http://docs.python.org/library/string.html#format-string-syntax).

[sourcecode language=”python”]  
 # Regex to find format specifiers in a Python string

import re

field_name = ‘(?P<field_name>(?P<arg_name>w+|d+){0,1}’  
 ‘(?:(?P<attribute_name>.w+)|’  
 ‘(?P<element_index>[(?:d+|(?:[^]]+))]))*)’  
 conversion = ‘(?P<conversion>r|s)’  
 align = ‘(?:(?P<fill>[^}{]?)(?P<align>[<>^=]))’  
 sign = ‘(?P<sign>[+- ])’  
 width = ‘(?P<width>d+)’  
 precision = ‘(?P<precision>d+)’  
 type_ = ‘(?P<type_>[bcdeEfFgGnosxX%])’  
 format_spec = ”  
 ‘(?P<format_spec>’  
 ‘%(align)s{0,1}’  
 ‘%(sign)s{0,1}#?0?’  
 ‘%(width)s{0,1},?’  
 ‘(?:.%(precision)s){0,1}’  
 ‘%(type)s{0,1}’  
 ‘)’ % {  
 ‘align': align,  
 ‘sign': sign,  
 ‘width': width,  
 ‘precision': precision,  
 ‘type': type_  
 }  
 replacement_field = ”  
 ‘{‘  
 ‘(?:’  
 ‘%(field_name)s{0,1}’  
 ‘(?:!%(conversion)s){0,1}’  
 ‘(?::%(format_spec)s){0,1}’  
 ‘)’  
 ‘}’ % {  
 ‘field_name': field_name,  
 ‘conversion': conversion,  
 ‘format_spec': format_spec  
 }

printf_re = re.compile(  
 ‘(?:’ + replacement_field + ‘|’  
 ‘%((?:(?P<ord>d+)$|((?P<key>w+)))?(?P<fullvar>[+#-]*(?:d+)?’  
 ‘(?:.d+)?(hh|h|l|ll)?(?P<type>[w%])))’  
 ‘)’  
 )  
 [/sourcecode]

Well, with the above, I was able to parse almost all the cases discussed [here](http://docs.python.org/library/string.html#format-examples) except for this one:

[sourcecode language=”python”]  
 import datetime  
 d = datetime.datetime(2010, 7, 4, 12, 15, 58)  
 s = ‘{:%Y-%m-%d %H:%M:%S}’.format(d)  
 [/sourcecode]

I was not sure how I could fit the above case to my regex. After some discussions in #python on IRC, I found some limitations of regular expressions and that it is not Turing complete. People suggested me to use some parser tools.

I, being a strong supporter of “Never re invent the wheel”, gave another shot to find some existing solution and lucky I was to come across **_formatter_parser()** of a Python string object.  It correctly found all replacement fields in python format strings properly and returned  an iterable of tuples (*literal_text*, *field_name*, *format_spec*, *conversion*). All I needed then was to convert this info to a list of replacement fields in a format string. A simple script below would is all that I needed to extract replacement fields in a format string in Python:

[sourcecode language=”python”]  
 replacement_fields = []  
 s = "{foo:^+30f} bar {0} foo {} {time:%Y-%m-%d %H:%M:%S}"

for literal_text, field_name, format_spec, conversion in  
 s._formatter_parser():  
 if field_name is not None:  
 replacement_field = field_name  
 if conversion is not None:  
 replacement_field += ‘!’ + conversion  
 if format_spec:  
 replacement_field += ‘:’ + format_spec  
 replacement_field = ‘{‘ + replacement_field + ‘}’  
 replacement_fields.append(replacement_field)  
 print replacement_fields  
 ["{foo:^+30f}", "{0}", "{}", "{time:%Y-%m-%d %H:%M:%S}"]

[/sourcecode]

That’s all. Simple and easy, isn’t it?

