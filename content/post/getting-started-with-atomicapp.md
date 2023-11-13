+++
author = "Ratnadeep Debnath"
categories = ["Atomic", "Projectatomic", "nulecule", "planet", "osas", "atomicapp", "rtnpro"]
date = 2016-02-03T11:01:55Z
description = ""
draft = false
slug = "getting-started-with-atomicapp"
tags = ["Atomic", "Projectatomic", "nulecule", "planet", "osas", "atomicapp", "rtnpro"]
title = "Getting started with atomicapp"

+++


**[atomicapp](https://github.com/projectatomic/atomicapp)** is a reference implementation of the Nulecule Specification. It can be used to bootstrap container applications and to install and run them. **atomicapp** is designed to be run in a container context. Examples using this tool may be found in the [Nulecule library](https://github.com/projectatomic/nulecule-library).

If you want to know the internals of **atomicapp**, how it works, etc., or contribute to it's development, this post is for you.

### Setup

#### Install python virtualenv utils
[virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/) is a tool to create isolated Python environments. virtualenv creates a folder which contains all the necessary executables to use the packages that a Python project would need. [virtualenvwrapper](https://virtualenvwrapper.readthedocs.org/en/latest/) is a set of extensions to easily create, manage and destroy virtualenvs. I personally prefer using **virtualenvwrapper** for my Python developement work.

```
sudo dnf install -y python-virtualenvwrapper
```

Restart your shell after the above is installed.

#### Getting the code
```
git clone https://github.com/projecatomic/atomicapp.git
```

#### Setup for development

```
cd atomicapp
mkvirtualenv atomicapp
python setup.py develop
echo "alias atomicapp=~/.virtualenvs/atomicapp/bin/atomicapp" >> ~/.virtualenvs/atomicapp/bin/postactivate
echo "alias sudo='sudo '" >> ~/.virtualenvs/atomicapp/bin/postactivate
echo "unalias atomicapp && unalias sudo" >> ~/.virtualenvs/atomicapp/bin/postdeactivate
```

### Understanding the code

```
atomicapp
├── cli
│   ├── __init__.py
│   └── main.py
├── constants.py
├── __init__.py
├── nulecule
│   ├── base.py
│   ├── container.py
│   ├── exceptions.py
│   ├── __init__.py
│   ├── lib.py
│   └── main.py
├── plugin.py
├── providers
│   ├── docker.py
│   ├── external
│   ├── __init__.py
│   ├── kubernetes.py
│   ├── marathon.py
│   ├── openshift.py
│   └── README.md
├── requirements.py
└── utils.py
```

#### CLI
The entry point for **atomicapp** CLI is ``atomicapp/cli/main.py``. The CLI args and options are added in ``atomicapp.cli.main.CLI`` class. The CLI commands are handled by functions named as ``cli_<command_name>`` in ``atomicapp.cli.main``.

#### Nulecule
``atomicapp.nulecule`` is the crux of **atomicapp** as it implements the Nulecule specification. The CLI interacts with ``NuleculeManager`` in ``atomicapp.nulecule.main`` to execute various commands. **NuleculeManager** provides the entrypoints to various functionalites: ``unpack``, ``genanswers``, ``fetch``, ``run``, ``stop``, ``clean``, to manage the life cycle of a Nulecule application.

``atomicapp.nulecule.base`` implements ``Nulecule`` to represent a Nulecule application from Nulecule SPEC file, and ``NuleculeComponent`` to represent an item in the ``graph`` attribute of the Nulecule application. ``atomicapp.nulecule.base.NuleculeComponent`` interacts with the underlying providers using ``atomicapp.providers`` in it's ``deploy()`` and ``undeploy()`` methods.

#### Providers
``atomicapp.providers`` implements interfaces for **atomicapp** to interact with the different providers: ``docker``, ``kubernetes``, ``openshift``, etc. A **provider** class usually has three important methods:

- ``init``: to do initialization required by the provider
- ``run``: Run artifacts on provider
- ``stop``: Stop artifacts on provider

### Conclusion
The above should have given you a brief overview of the **atomicapp** and should be good enough to help you get started with **atomicapp** development. In case you have any query, please feel free to ping us on ``#nulecule`` on Freenode or create an issue at https://github.com/projecatomic/atomicapp/issues/.

