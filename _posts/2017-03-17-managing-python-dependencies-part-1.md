---
title:  "Managing Python dependencies - installation"
date:   2017-03-17 11:00:00 +0100
categories: GN17 python tools
---

In the first part of this mini-series I would like to explain how an user can:

 * install a Python package,
 * pin version of dependency,
 * save dependencies for later.

You can also browse other posts from this mini-series:

 * Part 1: Installation (current)
 * [Part 2: Venvs]({{ site.baseurl }}{% post_url 2017-03-24-managing-python-dependencies-part-2 %})

## Installing packages 

Python's default package manager is called _pip_ which replaced a previous one,
_easy\_install_. Basic usage of _pip_ is simple: `pip install this`, `pip
uninstall that` is most of what you will do on a daily routine. But don't be
disappointed - _pip_'s commands are flexible thanks to a number of switches and
options available.

One of the most common switches is, from my experience, `--requirement` (or
`-r` for shortcut). It allows an user to install packages referenced in a
_requirements file_. _Pip_ allows `-r` option to be used multiple times so
there might (and probably - should) be distinct files for running a project and
developing (or testing) it. _Pip_ will install _wheel_ releases, if available,
by default. _Wheels_ are Python's prebuild packages, thanks to them you don't
need to compile a plugin written in C on your dev machine or a server. To learn
more about _wheels_ go to [packaging.python.org](https://packaging.python.org/installing/#source-distributions-vs-wheels).


## Version pinning and requirements file

In the previous section I've mentioned _requirements file_. How to prepare one
and what is its format?

It is just a regular text file, usually with a `.txt` extension, which list
packages names you want to install. For instance such a requirements file:

```
aiohttp
aiohttp_jinja2
```

will tell _pip_ to install the latest versions of _aiohttp_ and
_aiohttp\_jinja2_.

That probably is not what you want to do - it's safer to specify an exact
version of a package (aka _version pinning_) to protect from your dependencies
breaking changes, which may happen in the future. For instance at the time of
writing, _aiohttp_ is available in _2.0.0rc1_ so without version pinning
[Łapka] might not run when installed after _aiohttp 2.0.0_ release. Here's how
to pin your dependencies:

```
aiohttp==1.3.3
aiohttp_jinja2==0.13.0
``` 

[Łapka]: https://github.com/glujan/lapka

But let's say I'm developing some library. I shouldn't be so specific about
dependencies version - my library can run on a variety of releases. As an
example let's say I'm developing a _aiohttp 1.2.0+_-only plugin but not for
_2.0.0_ and future releases. How do I tell this in a requirements file?
Something like this would do:

```
aiohttp>=1.2,<=2.0
```

The same syntax for version pinning may be used in a command line, like:

```
$ pip install "aiohttp>=1.2,<2.0"
```

Mind extra quotes - in a shell environment some symbols (like `=`, `<` or `>`)
might have some syntax meaning - so it is essential to escape them.


## Saving dependencies

Isn't this section already covered? Partially it is - you can just run _pip_
and point to it your _requirements.txt_. But what about _dependencies of
dependencies_?

Let's have a look on a another _pip_ command, `freeze`. Running it will list
all packages you currently have installed in your environment - including
dependencies of your dependencies. Now you can see, that after installing
_aiohttp_ there are a few other libraries attached to the list, like _chardet_
or _yarl_. What if one of those packages future release will be incompatible or
buggy? It may break your application, especially if you are not using continues
integration! We need to add them to our requirements file as well and we need
to do it quickly! Now it looks like this:

```
aiohttp==1.3.3
aiohttp-jinja2==0.13.0
async-timeout==1.2.0
chardet==2.3.0
Jinja2==2.9.5
MarkupSafe==1.0
multidict==2.1.4
yarl==0.9.8
```

But… do we need to?

Adding all of the output from `pip freeze` to our requirements file will cause
upgrading packages to be a pain. You probably want to install a security
patches to all packages and do it in a convenient way, don't you?

Luckily, there is a better approach - using constraints file. This allows to
keep your _requirements.txt_ (very) simple thanks to storing all version
information in a separate file.

So, requirements file now only contains:

```
aiohttp
aiohttp_jinja2
```

and newly created _constraints.txt_ now includes:

```
iohttp==1.3.3
aiohttp-jinja2==0.13.0
async-timeout==1.2.0
chardet==2.3.0
Jinja2==2.9.5
MarkupSafe==1.0
multidict==2.1.4
yarl==0.9.8
```

To install correct version of all packages just run:

```
$ pip install -r requirements.txt -c constraints.txt
```

Upgrading your packages is also super simple. Just run:

```
$ pip install --upgrade -r requirements.txt  # or just upgrade a single package
```

and after verifying everything works just fine with updated dependencies (ie by
running tests), generate constraints again:

```
$ pip freeze > constraints.txt
```

Don't forget to include both _requirements.txt_ and _constraints.txt_ in your
repository.

Please mind that if you install any additional packages it will be included
(with its dependencies) into your constraints. How to prevent this from
happening will be explained in the second part of this mini-series so stay
tuned!

If you wish to learn more about _pip_ itself, an excellent place would be [its
rich documentation](https://pip.pypa.io/). It includes an user guide, full
commands, options and switches reference and many explanatory examples.
