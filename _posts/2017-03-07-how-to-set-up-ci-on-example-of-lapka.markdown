---
title:  "How to set up a CI on example of Łapka"
date:   2017-03-07 17:00:00 +0100
categories: GN17 python lapka devops
---

Recently I have written [a post] about what CI is and how it can help in
managing a project. Now I would like to show how easly anybody can start using
it.

[a post]: {{ site.baseurl }}{% post_url 2017-03-04-a-point-or-two-about-ci %}


## Choosing a CI service

GitHub offers integrations with remote services and among them there are
[plenty of CI solutions]. As I am using it as a repository for [Łapka] I've
decided to choose one of available options. Setting up a CI on an own server is
quite a big effort and many (or maybe even all?) CI services integrated with
GitHub are free for open source projects.

My choice was [Travis] because it offers not only Python support but also
integration with a [Tox] which is a Python tool for automation of test
activities. Thanks to Tox I can run tests on my dev machine and on Travis
without writing any additional code.

[plenty of CI solutions]: https://github.com/integrations/feature/continuous-integration


## Setting up [Travis]

### 1. Turn on the integration on GitHub

This is pretty straightforward: just go to the [Travis integration page](https://github.com/integrations/travis-ci)
on GitHub and click that big green button which says "Add to GiHub". A simple
to use wizard will guide through a whole process.


### 2. Configure

Configuring [Travis] is simple, at least a basic one. An user only needs to add
`.travis.yaml` (mind the dot at the beginning of that name!) file to the root
of the repository which should use continuous integration and fill it with some
required settings.

For instance [Łapka] has such a config file ([link to a commit]):

```yaml
sudo: false
language: python
cache: pip
python:
  - "3.6"
install: pip install tox-travis
script: tox
after_success:
  - codecov
```

Let me now explain what each line means:

 * `sudo` - whether or not use _root_ privileges. If not, build is run in a
   Docker container,
 * `language` - this is pretty self explanatory - which language a project is
   written in,
 * `cache` - cache installed dependencies, not to reinstall them every build so
   buld times should be reduced. There are caches for a few other languages as
   well. For Python I've set it to _pip_ as it's a Python's package manager,
 * `python` - Travis supports a thing called build matrix, which allows an user
   to build package against different environments - in this case a version of
   a language but there are more options (a database, for instance). I am using
   [Tox] for this. This option is, of course, Python-specific, there are
   similar for other languages,
 * `install` - dependencies required by a project to run a build. Again, I'm
   using Tox for it so I only need to install its integration with Travis,
 * `script` - basically a program which needs to be run on Travis, like
   a test runner, a linter and so on. Nobody will be surprised if I tell I'm 
   using Tox for that ;-) User can specify many commands here,
 * `after_success` - what Travis should do after a successful build. In my case
   I want an another service integrated with GitHub, [Codecov], to measure a
   code coverage.

There are some other configuration options which I have no need to use right
now. To learn more about them, and Travis in general, check out its [docs].

[link to a commit]: https://github.com/glujan/lapka/commit/0b400c37822df956508aa5e6395a0ecd9bb36b69
[Codecov]: http://codecov.io
[docs]: https://docs.travis-ci.com/user/customizing-the-build/


### 3. Maintain

That's it, Travis is set up. Pretty simple, nah? Now a team just need to be
conscientious about writing new (and maintaining old) tests.

One more thing - how to check if a build succeeded or failed? It's even easier
than getting started with CI - Travis will build any _pull request_ submitted
to a project and put the build status in _pull request_. This, with GitHub
possibility to prevent merging a _pull request_ with a failed build, is a
pretty robust tool. Travis behaviour can also be changed to run a build on
every push.


[Łapka]: https://github.com/glujan/lapka
[Travis]: http://travis-ci.org/
[Tox]: https://tox.readthedocs.io
