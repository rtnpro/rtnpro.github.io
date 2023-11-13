+++
author = "Ratnadeep Debnath"
categories = ["django", "django-nose", "python", "ratnadeep debnath", "resuse_db=1", "transactiontestcase", "rtnpro"]
date = 2012-09-08T16:33:18Z
description = ""
draft = false
slug = "django-transactiontestcase-with-reuse_ddjango-nose"
tags = ["django", "django-nose", "python", "ratnadeep debnath", "resuse_db=1", "transactiontestcase", "rtnpro"]
title = "#Django #TransactionTestCase with REUSE_DB=1 of #django-nose"

+++


Lately, I found out that Django’s **TransactionTestCase** leaves test data in database after the test case is executed. It’s not until the next execution of _pre_setup method of a TransactionTestCase instance that the database is flushed. This is troublesome when tests are run with **Django Nose’s test runner with REUSE_DB =1.**

An easy fix to this is to customize the TransactionTestCase so that it deletes the test data on exit. I wrote a simple wrapper around Django’s TransactionTestCase and extend it to write other transaction test cases.

[sourcecode language=”python”]  
 from django.test import TransactionTestCase  
 from django.db import connections, DEFAULT_DB_ALIAS

def flushdb(cls):  
     if getattr(cls, ‘multi_db’, False):  
         databases = connections  
     else:  
         databases = [DEFAULT_DB_ALIAS]  
     for db in databases:  
         management.call_command(‘flush’, verbosity=0,  
             interactive=False, database=db)

class BaseTransactionTestCase(TransactionTestCase):  
     @classmethod  
     def tearDownClass(cls):  
         flushdb(cls)

[/sourcecode]

