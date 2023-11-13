+++
author = "Ratnadeep Debnath"
categories = ["contribute", "coverage", "django", "django-nose", "hack", "ratnadeep debnath", "start test", "tests", "transifex", "unittest", "rtnpro"]
date = 2011-11-01T17:43:27Z
description = ""
draft = false
slug = "start-testing-transifex"
tags = ["contribute", "coverage", "django", "django-nose", "hack", "ratnadeep debnath", "start test", "tests", "transifex", "unittest", "rtnpro"]
title = "Start testing Transifex"

+++


**How do you setup Transifex?**

Here is all you need to know to setup Transifex: [http://help.transifex.net/server/install.html](http://help.transifex.net/server/install.html)

[http://fosswithme.wordpress.com/2011/10/20/setup-transifex-in-virtualenv/](http://fosswithme.wordpress.com/2011/10/20/setup-transifex-in-virtualenv/) is another good write-up on how to setup and run Transifex in virtualenv. So, I’d be building on top of that to show you how to start testing Transifex using django-nose.

**What packages will you need?**

django-nose, nose, nose-exclude, coverage

You can install the above packages using

`pip install <package_name>`

Configure Transifex settings to enable django-nose Test Runner

`cd <transifex source code's root directory>`

cd transifex/settings

cp  90-local.conf.sample 90-local.conf

open and edit 90-local.conf and add the following lines:

`INSTALLED_APPS += [`

‘django_nose’,

]

TEST_RUNNER = ‘django_nose.NoseTestSuiteRunner’

cd ..

Now, save the file. Now you are good to go.

**Start testing**

`python manage.py test <app_name><br></br>`  
 for example:

`python manage.py test resources<br></br>`  
 You can also test a particular test class like below:

`python manage.py test resources.tests:TestJavaProperties`

You can even run a particular test method:

`python manage.py test resources.tests:TestJavaProperties.test_properties_parser<br></br>`  
**Run coverage on Transifex tests**

django-nose has a plugin for coverage. So, you can run the above tests and collect coverage data.

For example:

`python manage.py test resources.tests:TestJavaProperties.test_properties_parser --with-coverage --cover-package=resources.tests.formats`

All the coverage results are saved in a .coverge file by default in the current directory. Although, running tests with coverage plugin of django-nose shows the coverage results by default. You can also see the coverage results in the usual way:

`coverage -rm`

You can also use grep along with the above command to filter the results displayed.

Well, that’s all you need to know to start testing Transifex.

**What’s next?**

Start testing transifex. If you find a test fails, try to find the reason why it failed. Read the traceback info properly. Find where the error took place. There are various reasons why a test may fail:

- Test is not updated according to updates in code
- Bug in code
- A wrong test case

and others…

You can report the issues or any bug you find at [http://trac.transifex.org/newticket.](http://trac.transifex.org/newticket) Feel free to submit a patch that fixes the issue. The patch will be reviewed by the Transifex upstream and if it is ok, it will be merged with Transifex’s code at [http://code.indifex.com/transifex/](http://code.indifex.com/transifex/).

Keep hacking ![:)](http://127.0.0.1:8080/wordpress/wp-includes/images/smilies/icon_smile.gif)

