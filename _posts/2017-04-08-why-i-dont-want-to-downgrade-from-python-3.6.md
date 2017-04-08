---
title:  "Why I don't want to downgrade from Python 3.6?"
date:   2017-04-08 13:30:00 +0100
categories: GN17 asyncio python lapka
---

In those early days of Łapka lifetime I've came across a pretty solid point
which can motivate my whim to not downgrade Python to any earlier vesions.
There are also another reasons why I'm so obstinated about using Python 3.6
but more on that later.

Good code requires testing.

During development of _Łapka_ I'm trying to follow TDD which requires tests to
be written before any production code. Testing Goat is looking on my hands and 
keyboard all the time and disobeying it's rules may (and potentially, will be) 
harmful for the entire project.

And here's where I came across _that filthy bug_ which took me an hour or two
out of my life.

I needed to use [unittest.mock] in order to fake making a HTTP request to a 
remote server. My code is using an [asynchronous context manager] for that and 
I was having a suspicious looking exception while mocking an object which is 
relying on this concept.

The very next day, after I gave up using `unittest.mock` module in this
scenario, I came across [this](https://bugs.python.org/issue26467) issue on
the official Python bug tracker. This is the very same issue I was facing a
dozen of hours before. Great. At least now I know it wasn't me who was silly.

Of course I've made walk around but [this](https://github.com/glujan/lapka/blob/afaf61f8e444fcc4c85ccc07a42f617d18d06a5f/tests/test_fetch.py#L113-L137)
code might have looked cleaner and be easier to write. However a bird in the
hand is worth two in the bush, as they say.

Another strange behaviour is that I can't use `path.object` function (from
`unittest.mock` module, of course) as a [decorator] because it is giving me 
some more funny exceptions. This is due to its a lack of support of coroutine 
functions, I believe.

[unittest.mock]: https://docs.python.org/3.6/library/unittest.mock.html
[asynchronous context manager]: https://docs.python.org/3.6/glossary.html#term-asynchronous-context-manager
[decorator]: https://docs.python.org/3.6/glossary.html#term-decorator
