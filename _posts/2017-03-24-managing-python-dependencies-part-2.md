---
title:  "Managing Python dependencies - venvs"
date:   2017-03-24 15:30:00 +0100
categories: GN17 python tools
---

Welcome to the second part of the _Managing Python dependencies_ mini-series.

In this part I will cover how to separate incompatible dependencies across your
projects using different tools:

 * _venv_ - a built-in module,
 * _virtualenvwrapper_ - set of _venv_ module extensions,
 * _pyenv_ - a tool to manage Python versions.

You can also browse other posts from this mini-series:

 * [Part 1: Installation]({{ site.baseurl }}{% post_url 2017-03-17-managing-python-dependencies-part-1 %})
 * Part 2: Venvs (current)

Anyone who has more than one ~~Python~~ software project on a hard drive knows
the need to isolate dependencies across different projects. For instance, one
project is using `some_lib` in version _1.x_ but another uses the same library
in backwards incompatible version _2.x_. What should you do? How to separate a
conflicting dependencies? Maybe by including a specific releases of all
dependencies in each projects repository? Or use _git_'s submodules? Any of
those ideas don't sound good at all.

Fortunately, Python's rich standard library includes a tool which can be used
to manage your dependencies across all of your projects. It's named [venv] and
is available since Python 3.3. For older releases use its predecessor,
[virtualenv], which basically does the very same thing but was not included in
a standard library. Virtual environment will keep your packages, interpreter
and (optionally, on by default) system site directories isolated. If you code
in Python you want to use it.

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io


## Venv

To create one, simply run command:

```
python -m venv NAME  # Requires Python 3.3+
```

where `NAME` is how a new virtual environment should be called. As an example
I will call mine _moonlit_. The command will create a directory called
_moonlit_ with a directory structure similar to this:

```
$ tree moonlit -L 4
moonlit
├── bin
│   ├── activate
│   ├── activate.csh
│   ├── activate.fish
│   ├── easy_install
│   ├── easy_install-3.5
│   ├── pip
│   ├── pip3
│   ├── pip3.5
│   ├── python -> /home/grzegorz/.pyenv/versions/3.5.2/bin/python
│   └── python3 -> python
├── include
├── lib
│   └── python3.5
│       └── site-packages
│           ├── easy_install.py
│           ├── pip
│           ├── pip-8.1.1.dist-info
│           ├── pkg_resources
│           ├── __pycache__
│           ├── setuptools
│           └── setuptools-20.10.1.dist-info
├── lib64 -> lib
└── pyvenv.cfg
```

To activate the newly created venv user needs to source `activate` script from
_bin_ directory. Just run `source moonlit/bin/activate` in a shell and it's
ready to go. An user can now install all dependencies using a duo of `pip` and
_requirements.txt_ file. To read more about that visit [this post]. To stop
working inside a venv just type `deactivate`. Many, if not all, Python's IDE
support it (I'm talking about PyCharm, PyDev, Komodo) and text editors like
Vim, Sublime Text, Atom (and more) have a plugin to do that.

[this post]: {{ site.baseurl }}{% post_url 2017-03-17-managing-python-dependencies-part-1 %}

Please note that venv directory is not safe to move around. Once it is created
in one place it should stay there. This is the very reason why
[virtualenvwrapper] was created.

[virtualenvwrapper]: https://virtualenvwrapper.readthedocs.io


## Virtualenvwrapper

It makes managing multiple venvs easier as it stores all of them in one place
and relies on its own command, `workon NAME`, to activate them. Thanks to that
an user doesn't need to remember where did (s)he created that nasty venv.

The creation of venv is also different. Now, the command is `mkvirtualenv NAME`
and won't create venv inside a current directory but in `$HOME/.virtualenvs` by
default.

It also supports a few hooks to customize a behaviour before and after some
action (like creating, moving, deleting a venv) and allow to change to a
project's directory when activating corresponding venv. Handy tool it is, I
tell you. It's worth to read [virtualenvwrapper]'s docs to learn more (like how
to install and stuff).


## Pyenv

The latest, and the most favoured tool in my eyes. It requires a plugin to work
with venvs but it's worth it. With [pyenv] itself and [pyenv-virtualenv] plugin
user can not only create and manage venvs but also can use it with any Python
version and distribution. At the time of writing it supports multiple releases
of CPython, PyPy, IronPython, Jython, Stackless, Anaconda and Pyston. Phew. To
list all interpreters and their versions available run `pyenv install --list'.

A plugin is using built-in `venv` module when available and fallbacks to
`virtualenv` for older Python's versions. Both projects are under active
development and since I've started using them I did not face any problems. In a
fact, every project mentioned here is mature and rock-stable.

After installing _peynv_ and necessary plugin just run:

```
pyenv virtualenv VERSION VENV_NAME
```

where the `VERSION` is one of Python interpreters installed on your system or
using _pyenv_. Maybe an example would be appreciated here to clarify it.

So, let's say I want to use the latest PyPy in my project. Here's how do I
install it and create a venv:

```
pyenv install pypy-5.7.0  # or any other version desired
pyenv virtualenv pypy-5.7.0 my_project
pyenv shell my_project    # activate my_project venv
```

_Pyenv_ has a cool feature which allows it to activate custom Python version
whenever user `cd` into particular directory (or any of its subdirectories). It
works not only with custom Python interpreters but with pyenv-made venvs as
well.  Just run `pyenv local my_project` and it will create a _.python-version_
file which will be used in a future to switch to a proper virtual environment.

[pyenv]: https://github.com/pyenv/pyenv
[pyenv-virtualenv]: https://github.com/pyenv/pyenv-virtualenv
