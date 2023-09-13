---
title: Publishing a Python Package
date:  2023-09-13 08:25:06 +0800
categories: [Technology, Python]
tags: [pypi, pip, python]     # TAG names should always be lowercase
---

Firstly, I am not a Python guy. I had got into dependencies and version conflicts in my previous MacBook Pro without using [Miniconda](https://docs.conda.io/projects/miniconda/en/latest/), it is a nightmare and even blocking me from developing any piece of Python code. Also, the convention of Python documentation is definitely not my cup of tea, i.e., density text, dark grey theme, full of `>>>` and `...`. Even the language itself, I prefer `?:` much much more then `... if ... else ...`.

Therefore, I need to learn the way to appreciate Python and get familiar to its syntax. To start with, I try to publish some meaningful packages, i.e., [hk-bus-eta](https://pypi.org/project/hk-bus-crawling) and [cantonese-romanisation](https://pypi.org/project/cantonese-romanisation/). Taking the package _cantonese-romanisation_ as an example, below is a walk through on publishing it to pypi.org, a.k.a. enabling others to install it via `pip install cantonese-romanisation`. 

## 0. TLDR;

Referencing as the project [here](https://github.com/chunlaw/cantonese-romanisation/tree/pypi), and run below code should be good enough.

```sh
# pull the project and prepare the environment
$ git clone --single-branch --branch pypi https://github.com/chunlaw/cantonese-romanisation
$ conda create --name pypi python=3.7
$ conda activate pypi
$ pip install twine
$ cd cantonese-romanisation

# publish the project;
$ python setup.py sdist bdist_wheel
$ twine upload dist/*
```

## 1. Environment preparation
To avoid messing up the development environment, it is always a good practise to create a separate development environmet.

```sh
# create the environment
$ conda create --name pypi python=3.7

# activate the environment
$ conda activate pypi

# install twine for uploading package to pypi
$ pip install twine
```

The core tool [setuptools](https://pypi.org/project/setuptools/) should be included in the environment, and hence the environment is good to go.

## 2. Directory structure

Below directory structure depicts the files required.

```sh
.
├── setup.py        # required
├── README.md       # description to be used in pypi.org
├── MANIFEST.in     # optional, required for special features
├── test.py         # optional, just for testing the package before publish
└── cantoroman/     # the pacakge name to be used in Python, only lower case and underscroe are valid
  ├── __init__.py   # package entry point
  └── canto.py      # package code
```

## 3. Construct `setup.py`

The `setup.py` comprises the metadata of the pacakge. Most of them is initutive and the `name` defined here will be used for `pip install <name>`.

It is important to note that all built-in packages, such as [re](https://docs.python.org/3/library/re.html), [datetime](https://docs.python.org/3/library/datetime.html), [os](https://docs.python.org/3/library/os.html), must be excluded in `install_requires`. If there is third-party packages should be stated in `install_requires`, state it with version, e.g., `beautifulsoup4>=4.12`.

Another fancy part is the `classifiers`, the available set of classifiers is listed [here](https://pypi.org/classifiers/).

```python
from setuptools import setup, find_packages
import codecs
import os

here = os.path.abspath(os.path.dirname(__file__))

with codecs.open(os.path.join(here, "README.md"), encoding="utf-8") as fh:
    long_description = "\n" + fh.read()

VERSION = '1.0.5'
DESCRIPTION = 'Library for mapping Chinese character to Hong Kong Government Cantonese Romanisation, Pingyam (Yale or LSHK)'

# Setting up
setup(
    name="cantonese-romanisation",
    version=VERSION,
    author="Chun Law (chunalw)",
    author_email="<chunlaw@rocketmail.com.com>",
    description=DESCRIPTION,
    long_description_content_type="text/markdown",
    long_description=long_description,
    packages=find_packages(),
    install_requires=[],
    include_package_data=True,
    keywords=['pinyin', 'cantonese', 'pingyam', 'hong kong government cantonese romanisation', 'romanisation', 'romanization'],
    classifiers=[
        "Development Status :: 5 - Production/Stable",
        "Intended Audience :: Developers",
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: Unix",
        "Operating System :: MacOS :: MacOS X",
        "Operating System :: Microsoft :: Windows",
    ]
)
```

## 4. Code `__init__.py` and `canto.py`

The content of `__init__.py` is as simple as below. The string `.canto` refers to the `canto.py` in the same directory, and the `Cantonese` is the python class defined in `canto.py`.

```python
# __init__.py
from .canto import Cantonese
```

The content of `canto.py` is as below.

```python
class Cantonese:
  def say_hello(self):
    print("Hello")
    return

```

## 5. Publishing

Assuming your pypi account is ready and you have setup the API token correctly, you may run the below commands.

```sh
$ python setup.py sdist bdist_wheel   # packaging
$ twine upload dist/*                 # publishing
```

## 6. OPTIONAL - Testing

The `testing.py` is used to examine the correctness of the package. Content is as below,

```python
from cantoroman import Cantonese  
# cantoroman is the directory name
# Cantonese is the class defined in canto.py and exported from __init__.py

cantonese = Cantonese()
cantonese.say_hello()
```

## 7. OPTIONAL - Prepare the optional `MANIFEST.in`

In the actual repository, there is a set of _json_ files to be used in `canto.py`. To correctly load them into the pacakge, we need to use `pkgutil.get_data(__name__, "<relative path inside the directory cantoroman>")`, and it is important to add a command into `MANIFEST.in` to include these _json_ files in the packaging process. The content of `MANIFEST.in` is as below, other commands could be found [here](https://packaging.python.org/en/latest/guides/using-manifest-in/).

```
recursive-include cantoroman *
```

Your package is now available in pypi.org, and other developers can now install it via `pip install cantonese-romanisation`. And you may test it as below,

```sh
$ pip install cantonese-romanisation
$ python
>>> from cantoroman import Cantonese
>>> cantonese = Cantonese()
>>> cantonese.say_hello()
Hello
```

That's all. Have fun.
