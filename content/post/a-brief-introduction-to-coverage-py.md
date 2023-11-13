+++
author = "Ratnadeep Debnath"
categories = ["coverage", "coverage.py", "django", "python", "ratnadeep debnath", "test", "rtnpro"]
date = 2011-09-22T16:07:25Z
description = ""
draft = false
slug = "a-brief-introduction-to-coverage-py"
tags = ["coverage", "coverage.py", "django", "python", "ratnadeep debnath", "test", "rtnpro"]
title = "A brief introduction to coverage.py"

+++


Coverage.py is a tool for measuring code coverage of Python programs. It monitors your program, noting which parts of the code have been executed, then analyzes the source to identify code that could have been executed but was not.

Coverage measurement is typically used to gauge the effectiveness of tests. It can show which parts of your code are being exercised by tests, and which are not.

Getting started:

1. Install coverage:

- pip install coverage
- easy_install coverage

2. Use coverage to run your program and gather data:

<tt>$ coverage run my_program.py arg1 arg2  
 blah blah ..your program's output.. blah blah</tt>

3. Generate reports with coverage:

$coverage -rm

<tt>Name                      Stmts   Miss  Cover   Missing  
 -------------------------------------------------------  
 my_program                   20      4    80%   33-35, 39  
 my_other_module              56      6    89%   17-23  
 -------------------------------------------------------  
 TOTAL                        76      10    87%</tt>

4. You can also use coverage to generate reports in other presentation oriented formats like HTML:

$coverage html

You can also use coverage.py with Django. You can run your Django tests along with coverage to check which codes in your app have been tested by your tests. With the coverage data, you can write new tests to test the codes which have not been tested so far by your tests. For example:

- $coverage -e        //This deletes previous coverage data
- $coverage -x manange.py test foo_app.FooTest.foo_method        //Execute manage.py from coverage to collect coverage data
- $coverage -rm | grep ‘foo_app’      // to filter the report to show the coverage of foo_app
- coverage run –include=”*foo_app*” –omit=”*tests* manage.py test foo_app        //This will include *foo_app* pattern and omit *tests* pattern from your coverage report.

You can also write custom Test Runners using the coverage API to measure code coverage in a more controlled manner. You can find more detailed documentation about coverage [here](http://nedbatchelder.com/code/coverage/).

