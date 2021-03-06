---
layout: post
title:  Python Packaging is in a Pickle
---

This post is my attempt to summarize my learnings about the basics of how to manage [python](https://www.python.org/) packages with different tools and commands. I've spent a while collecting this information, hopefully it helps a few people searching for the same thing via google too.

The tl;dr of this post is: _Python packaging is a bit of a mess._ 

![standards](https://imgs.xkcd.com/comics/standards.png "There's always a relevant XKCD comic.")

The three most popular solutions I have found for managing python packages are as follows:

## Pip

[Pip](https://pypi.org/project/pip/) is the default way to install modules in python and one you've probably used before as it comes by default in most modern installations. The basic usage of pip is along the lines of `pip install {module name}`. The main issue with pip is that it installs _globally_ and not locally inside of the folder you invoked the command from like say, [npm.](https://www.npmjs.com/) This means you can encounter dependency problems where two packages need different versions of the same package but as it's installed globally, you end up in a bit of a pickle.

A [requirements file](https://pip.readthedocs.io/en/1.1/requirements.html) can be used with pip for loose requirements, such as _"I need some version of pygame"_ and is used by pip. It tends to be used as a _dev requirements_ file that is shared between developers of a package. You can simply create a file named `requirements.txt` in the directory of your choosing with content like so:

```
BeautifulSoup==3.2.0
Django==1.3
Fabric==1.2.0
```

Then install those requirements (again, globally) with the `pip install -r requirements.txt` command.

Another way to share pip dependencies is with a [setup.py](https://docs.python.org/3/distutils/setupscript.html) file. This is a more involved requirements installation mechanism and is required to deploy a python module to the [python package index](https://pypi.org/). This is the place you'd want to put your python code if you wanted other people to be able to `pip install` it. `setup.py` is what actually gets used to install requirements whenever you run `pip install {module name}`. Both this and requirement.txt files tend to get used in a python project so developers can install dependencies slightly differently to how end users do.

## Venv

[Venv](https://docs.python.org/3/library/venv.html) allows you to create _virtual environments_ and works around the global pip install issues by creating an environment inside a directory that you can install packages directly into. It can be a bit unwieldy however. It comes with modern python installations by default. You can run it with the following command: `python -m venv {folder path}`. To exit a running venv session you can usually just use the `deactivate` command.

## Pipenv
[Pipenv](https://github.com/pypa/pipenv) is an abstraction layer around venv and uses it and pip under the hood to extend pip. Instead of `pip install` you'd run `pipenv install` etc. It installs the dependencies to a folder inside your user directory. It greatly simplifies things at the cost of abstraction as you are less involved with how `venv` is being invoked. One thing to note is that while `venv` and `pip` are usually included with python, `pipenv` is not! Pipenv has richer package metadata and dependency tree data than a requirements.txt file. It is good at installing dev dependencies as well as your main dependencies. You can consider using it to be similar to `npm install` in a way.

## Summary

The reason there isn't one single default tool to manage python packaging appears to be because python is a mature, open source language. It does not have direct oversight from a single large corporation like Microsoft has over [C#](https://docs.microsoft.com/en-us/dotnet/csharp/) or Google has over [Go](https://golang.org/). Several groups have developed different methods to try to solve the packaging problem but there is no one single accepted way of doing things.

I myself prefer just to use pip with a requirements.txt file if required but it's probably worth looking to `venv` at the very least if you have a large deployment environment to manage.

_PS: To turn python applications into exes/binaries, use the [pyinstaller](https://www.pyinstaller.org/) module. It's very easy to make  binaries from tons of python code, even for windowed apps! This means you can avoid all the pain described above if your deployment environment allows you to share and run binaries. It does bundle the entire python runtime alongside your package, so it might not be the most efficient solution depending on your use case._
