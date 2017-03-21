---
title:  "Authentication with Google Sign-In"
date:   2017-03-21 17:20:00 +0100
categories: GN17 lapka update
---

Storing passwords is a great responsibility. There are a plenty of rules which need to be
followed and even implementing them won't make an application ~~bullet~~ hack proof.
I don't want to deal with password restoration, new accounts confirmation etc. while
developing [Łapka], so I've decided to take an easier approach and rely on a third-party
authentication system - the one [delivered](https://developers.google.com/identity/sign-in/web/)
by Google.

The main reason for doing this is an ease of implementation. Basically, I only need to add
one JavaScript library, a new HTML element (which will be rendered as a button to sign-in)
and a backend authentication to be sure an user gave me a valid token - which is a single
_HTTP POST_ request.

It is both safer and easier for an user as well. There are great chances that necessary
Google account already exists and in the future it should be trivial to plug yet another
service - probably Facebook, as adaptation may be even bigger than Google accounts.

There is also a good information related to [Łapka]'s web framework - _aiohttp_. The new,
backwards incompatible version, has been released. Among cleaning up API, authors have made
another important change - they've rewrote some low-level aspects of their framework and now
switching to [uvloop] will result in a huge performance improvement - up to 90%, at least
according to [release notes](https://github.com/aio-libs/aiohttp/releases/tag/2.0.0). You can
expect a post about switching to _uvloop_ in a future.

Thanks to the test suit present in _Łapka_ and a `1.x` to `2.0` migration guide, porting was
a piece of cake and my project is already using the most recent _aiohttp_ version.

[Łapka]: https://github.com/glujan/lapka
[uvloop]: http://uvloop.readthedocs.io/
