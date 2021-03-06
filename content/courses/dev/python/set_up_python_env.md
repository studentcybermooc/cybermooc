---
title: "Setting up your python environment"
description: "Learn how to set up a clean python environment"
date: 2018-09-19
githubIssueID: 15
tags: ["python", "environment"]
authors: {
    gmolveau: "/authors/gmolveau/"
}
draft: false
---


## Introduction

This course will teach you how to set-up a clean python environment for all your projects

### Concepts

- python3
- virtual environment
- dependency management

---

## 1. Requirements

### 1.1 Installing Python3

Make sure you've got `python3` and `pip3` installed on your system.
To check if you have it installed try running :

```bash
$ python3 --version
```

Same thing for `pip` with :

```bash
$ pip --version
```

If you encounter `command not found` then you need to install it :

- for windows, [download this executable](https://www.python.org/downloads/) and make sure to check "Add python to PATH ".

- for mac, run :
	```bash
	$ brew install python3
	```
	(if you don't have brew installed, [go there](https://brew.sh/index_fr))

- for linux, run :
	```bash
	$ sudo apt-get install python3-pip python3-dev
	```

---

### 1.2 Using pip

Pip is the dependency manager for python (like npm for nodejs, cargo for rust, composer for php [...]). It allows you to download framework and libraries. When you install a dependency with `pip install XXX --user`, it installs it only for the current user.

```bash
$ pip3 install gimgurpython --user
Collecting gimgurpython
  Using cached https://files.pythonhosted.org/packages/7a/e9/7bf364691f3a16de4b161765282c8cde4ac9924542bddea7d0c2a8aa0351/gimgurpython-0.0.4-py2.py3-none-any.whl
Requirement already satisfied: requests in /usr/lib/python3/dist-packages (from gimgurpython) (2.18.4)
Installing collected packages: gimgurpython
Successfully installed gimgurpython-0.0.4
```

To see where a certain dependency is installed, try `pip show ...` and look at the Location line.
For example : `Location: /home/gmolveau/.local/lib/python3.6/site-packages`.

```bash
$ pip3 show gimgurpython          
Name: gimgurpython
Version: 0.0.4
Summary: A fork of Official Imgur python library with OAuth2 and samples, modified as it seems not maintained anymore
Home-page: https://github.com/gmolveau/imgurpython
Author: Imgur Inc. (+ gmolveau)
Author-email: api@imgur.com
License: MIT
Location: /home/gmolveau/.local/lib/python3.XXX/site-packages
Requires: requests
Required-by: 
```

But sometimes for a project, you will have to download a specific version of a dependency, and if this dependency is already installed for another project in another version, you gonna have a bad time.

That's why we will use `virtualenv`.

---

### 1.3 Using virtualenv

Virtualenv is a tool to create isolated Python environments. (if you want to learn more about it [go there](https://virtualenv.pypa.io/en/stable/)).

This tool will allow us to create a virtual environment for each project. It means no more dependency correlation. This also means that you will be able to share your project with every dependencies easily via a file called `requirements.txt` (more on this later).

To install virtualenv, simply run :

```bash
$ pip install virtualenv
```

You can now create virtual environments, called a `venv`.

---

## 2. Let's do this

### 2.1 Setting up a venv

Now that we have everything we need :

- let's create a project :

	- linux/osx

		```bash
		$ mkdir /tmp/project
		$ cd /tmp/project
		```

	- windows

		```bash
		mkdir %TMP%\project
		cd %TMP%\project
		```

- create a venv

	```bash
	$ virtualenv venv -p python3
	```

	This command tells virtualenv to create a virtual environment, to create a folder `venv` where the virtual environment will be located, and to use python3.

- now that the venv is created, we need to enter into this venv
	
	- linux/osx
		```bash
		$ source venv/bin/activate
		```

	- windows
		```bash
		venv\Scripts\activate.bat
		```

	you should now see `(venv)` at the beginning of your shell

	![virtualenv example](/img/courses/dev/python/set_up_python_env/virtualenv.png)

---

### 2.2 Experimenting with pip and venv

Let's now install a library

```bash
(venv) $ pip install flask
```

now if you look at where flask was installed with `pip show flask` you'll see that its directly into our venv. more specifically in `venv/lib/python3.X/site-packages`. So our venv is working well :-)

![venv flask example](/img/courses/dev/python/set_up_python_env/venv_flask.png)

Now let's try to run : 

```bash
(venv) $ flask --version
```

It should return `Flask 1.x.x [...]`.

Now open another terminal and try to run :

```bash
$ flask --version
```

You should have a `command not found` in return. Flask is available **only** inside your venv. No more pollution of your entire machine now :-)

If you wan't to quit your venv, simply run :

```bash
(venv) $ deactivate
```

---

### 2.3 Listing your dependencies

If you want to share your project, and list all the dependencies necessary to build it, pip is going to help you.

In your venv, run :

```bash
(venv) $ pip freeze
```

You should see all the dependencies and versions.

![pip freeze example](/img/courses/dev/python/set_up_python_env/pip_freeze.png)

Now if you want to export this list, simply run

```bash
(venv) $ pip freeze > requirements.txt
```

You now have a practical way of sharing your dependencies :-)

Remember, you should **never** commit your `venv`. Only commit your `requirements.txt`. More about git_and_stuff in a moment.

---

### 2.4 .editorconfig

This file, `.editorconfig` was made to tell your code-editor some presets about your code.
For example in sublime text, if you write python, you'll often encounter bugs due to the use of 'tab' instead of 'spaces'. This file is here to fix this.

- .editorconfig
	```
	# .editorconfig
	# http://editorconfig.org
	root = true

	[*]
	charset = utf-8
	insert_final_newline = true
	trim_trailing_whitespace = true

	[*.py]
	indent_size = 4
	indent_style = space

	[*.md]
	trim_trailing_whitespace = false
	```

	This configuration is pretty explicit. For example, it tells your code-editor that for python files, 'space' should be used as indentation, and that a tabulation is equal to 4 spaces.


You can [find](http://lmgtfy.com/?q=.editorconfig+springboot) examples of `.editorconfig` files for many kinds of project.

If you use `sublime text` as an IDE, you will need to install a plugin. If you never installed a plugin before on `sublime text` here's a quick walkthrough.

With sublime text open, press `Control + Shift + P` (or `Command + Shift + P` on mac) : a window will popup. Type `install` and a field called `Install Package Control` will appear. Select it with the arrow keys of your keyboard and hit `Enter`.  
You might need to restart `sublime text` after the installation is finished. You can now easily manage your plugins ! 

To install `editorconfig` plugin, press `Control + Shift + P` (or `Command + Shift + P` on mac) then type `install` and select `Package Control: Install Package`. 

Sublime will then download the list of available packages.  
Then type `editorconfig`, select the package and hit `enter`. The plugin is now installed.

Now every time you open a folder in sublime text, it will search for a `.editorconfig` file to automaticaly adapt itself. Handy :-)


---

### 2.5 Versioning

First here's an example of a `.gitignore` file for a python project :

- .gitignore (yes, it's huge)
	```
	venv/
	# Byte-compiled / optimized / DLL files
	__pycache__/
	.pytest_cache/
	*.py[cod]
	*$py.class

	# C extensions
	*.so

	# Distribution / packaging
	.Python
	env/
	build/
	develop-eggs/
	dist/
	downloads/
	eggs/
	.eggs/
	lib/
	lib64/
	parts/
	sdist/
	var/
	*.egg-info/
	.installed.cfg
	*.egg
	.cache/

	# PyInstaller
	#  Usually these files are written by a python script from a template
	#  before PyInstaller builds the exe, so as to inject date/other infos into it.
	*.manifest
	*.spec

	# Installer logs
	pip-log.txt
	pip-delete-this-directory.txt

	# Unit test / coverage reports
	htmlcov/
	.tox/
	.coverage
	.coverage.*
	.cache
	nosetests.xml
	coverage.xml
	*,cover
	.hypothesis/

	# Translations
	*.mo
	*.pot

	# Django stuff:
	*.log
	local_settings.py

	# Flask stuff:
	instance/
	.webassets-cache

	# Scrapy stuff:
	.scrapy

	# Sphinx documentation
	docs/_build/

	# PyBuilder
	target/

	# IPython Notebook
	.ipynb_checkpoints

	# pyenv
	.python-version

	# celery beat schedule file
	celerybeat-schedule

	# dotenv
	.env

	# virtualenv
	venv/
	ENV/
	env/
	.flaskenv

	# Spyder project settings
	.spyderproject

	# Rope project settings
	.ropeproject

	# Mac OSX
	**/.DS_Store


	# JetBrains products
	.idea/

	# Misc
	_mailinglist
	```


This .gitignore will prevent you of commiting some useless/dangerous files (temporary, compiled, credentials [...]).

Also, a course will be soon available about git workflows to work effectively in team. (but if you [can't wait](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)).

---

## 3. Choosing your IDE

There's no good/bad/worst/attrocious IDE; choose the one you're the most effective with and that won't go into your way when you wan't to work.

BUT if you don't have an IDE yet or if you want to try a new one, I'm currently using two products :

- [sublime text](https://www.sublimetext.com/) + lots of additional packages (via package control)
- [pycharm](https://www.jetbrains.com/pycharm/)

Sublime Text is not really (natively) an IDE but with the help of additional packages, it can be.

Pycharm **is** an IDE and helps you in a **lot** of ways. You'll have to learn some shortcuts and stuff, but for professional python programming, [imho](https://www.dictionary.com/browse/imho) it's the best one.

---

## Conclusion

OKkkkkay so now you should know how to easily set-up your python environment :-)

We've seen how pip and virtualenv works and how to make a clean place when working on a new project.

---

### Summary

[TL;DR](https://en.wikipedia.org/wiki/TL;DR) ?

So a quick workflow for every new project is :

```bash
$ mkdir project; cd project
$ touch .editorconfig (then paste the config)
$ touch .gitignore (then paste the config)
$ touch README.md$

$ virtualenv venv -p python3
$ source venv/bin/activate (enter the venv)
(venv) $ pip install XXX
(venv) $ pip freeze > requirements.txt
(venv) $ pip install YYZZ
(venv) $ pip freeze > requirements.txt

(venv) $ git init
(venv) $ git add .gitignore .editorconfig requirements.txt README.md
(venv) $ git commit -m "init project"

(venv) $ deactivate (quit the venv)
```

---

### Going further

- If you don't like managing your venv, requirements.txt etc... :

	a tool was created **just for you** then and it's called [pipenv](https://pipenv.readthedocs.io/en/latest/). Here's a [french explanation](http://sametmax.com/pipenv-solution-moderne-pour-remplacer-pip-et-virtualenv/) of this tool.

- If you want to learn more about how python really works :

	- [watch this conference by James POWELL](https://www.youtube.com/watch?v=7lmCu8wz8ro)
	- [watch this other conference by Nina Zakharenko](https://www.youtube.com/watch?v=F6u5rhUQ6dU)
	- [and this one by Raymond HETTINGER](https://www.youtube.com/watch?v=wf-BqAjZb8M)
	- [RTFM#1](https://docs.python.org/3/tutorial/datastructures.html)


- and more globaly : [read this book by Kevlin HENNEY](http://shop.oreilly.com/product/9780596809492.do)

- If you want to apply your newly acquired knowledge on something useful, please follow our course on how to build an API with Flask.