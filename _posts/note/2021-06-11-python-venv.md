---
layout: post
title: Python Virtual Environment
categories: Python
description: 
keywords: python, virtual environment
---


# Virtual Environments for Python

Sometimes we want a new, clean environment for testing python codes, 
and we want the host environment not been interfered with. 
Using virtual environments could meet the requirements.

This page is for quick startup, it is recommend to 
read the [Official Documentation](https://docs.python.org/3/library/venv.html) first, 
especially the ```Note``` part, which contains some explanations for ```venv```.

# Creating New Venv

Start from Python 3.6, the build in ```venv``` has replaced ```pyvenv```.

Run command:
```bash
python3 -m venv /path/to/new/virtual/environment
```
for generate a new virtual environment. 

# Activate and Deactivate

For bash/zsh:
```bash
source /path/to/new/virtual/environment/bin/activate
```

Other systems could be found at Official Documentation. Meanwhile,

> You can deactivate a virtual environment by typing “deactivate” in your shell.

then the virtual environment has been deactivated and return to the host environment.

# Miscellaneous

## first-time start

It seems some systems build-in Python will bootstrap ```pip``` with old version 
and provide no ```wheel``` as well.

use
```bash
pip install pip -U
pip install wheel
```
to update ```pip``` version and install ```wheel```. 

Notice that the source of ```pip``` in virtual environment is the same as the host environment. If the installation step takes too much time on downloading, consider change another ```PyPI``` mirror station.

## remove venv

Just remove directory ```/path/to/new/virtual/environment```, painless once and this ```venv``` is gone for good. Remember to backup needed files first.


[homepage](/)
