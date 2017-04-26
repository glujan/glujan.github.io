---
title:  "Higher order functions and how they function"
date:   2017-04-26 11:45:00 +0100
categories: GN17 python functional
---

If you are into a fantasy books or games you probably know what a vampire is.
And just like a higher (order) vampire is somewhat a regular vampire but on
steroids, higher (order) function mostly resembles its regular cousin and still
does the same things (like ~~drinking blood~~ transforming an input to an
output in a certain way), it also has more capabilities and can be used more
sophisticatedly. At least one of:

 * passing an another function as an argument, or
 * returning a function (instead of a "normal" value)

is required to became a higher order function.

But what is it for? Let's start from the first option, as it seems to be
easier to understand.

## Passing a function as an argument

It is pretty clear how to pass an argument to a function - you can use either a
value literal or a variable which stores some data. You see, in some
programming languages, _functions' names_ are no different then, let's say, a
variable storing an int. For instance, in Python:

```
>>> i = 0
>>> type(i)
<type 'int'>
>>> type(len)  # len is a function, isn't it?
<built-in function len>
```

A type of variable `i` is `int` and type of a function `len` is, well, 
`built-in function len`.

It is easy to imagine what a programmer can achieve by passing a function as a
parameter - a change of behaviour. Have you ever sorted a list in Python?
Hopefully, the answer is _yes_. It's trivial when you need to sort a list of
numerics or texts, like integers or strings. But what if you have to sort a
list of dictionaries or you own defined type?

```
def age_key(item):
    return item['age']

characters = [
    {'name': 'Anakin', 'age': 42},
    {'name': 'Luke', 'age': 19},
    # pretend we have a lot of other random names and ages here
]
by_age = sorted(characters, key=age_key)
```

In example above you can see that the `age_key` function is used as an argument
for built-in `sorted` function. Pretty simple and easy to understand, right?
You only need to define a function and bam, you list is sorted. But _do you_?

When a function you need is a simple, single expression, you may consider
using lambda expression instead. Some people hate it, though, as it is less
readable than defining a separate function and using its name as a parameter.
Think of lambdas as unnamed functions. You _can_ assign a lambda to a variable
but this is not advised. If you still want to do this... just create a regular
function. This is what you need.

```
# now the key is lambda expression
by_age = sorted(characters, key=lambda item: item['age'])
```

## Returning a function

Consider yet another implementation of sorting a list:

```
import operator

by_age = sorted(l, key=operator.itemgetter('age'))
```

As you know from previous examples, a `key` parameter needs to be a function.
But here I am calling `itemgetter` function from `operator` module. How it can
be?

And the answer is obvious - just have a look at this section title :-) Wanna
see a implementation of this sorcery? [Here you go], Grzegorz delivers.  By the
way - this is the actual CPython's code. Don't be afraid to _use the
~~force~~ source, Luke_, it usually easy to read.

[Here you go]: https://github.com/python/cpython/blob/a1fc949b5ab8911a803eee691e6eea55cec43eeb/Lib/operator.py#L265-L294

For a beginner the implementation may not be really straightforward so let's
rewrite it to not use a class but a good, old function, shall we?

```
def my_itemgetter(item, *items):
    # this is mostly a content of __init__
    if not items:
        def func(obj):
            return obj[item]
        _call = func
    else:
        items = (item,) + items
        def func(obj):
            return tuple(obj[i] for i in items)
        _call = func

    # and here goes sth similar to __call__ content
    return _call

# works pretty much the same as `operator.itemgetter`
by_age = sorted(l, key=my_itemgetter('age'))
```

I hope you can see that our implementation of `itemgetter` just returns a
function defined in its own body. It can have one or another form, depending on
how user calls `my_itemgetter` - with just one or more arguments. This stuff
works thanks to a closure but this is a topic for yet another post.

In Python, higher order functions are heavily used in (at least) one more area:
decorators. Decorators are just a functions which take a function as an
argument, define an inner function which may do some computation before and/or
after the original function is called and return this inner function. They
fulfill both requirements to be a higher order function. Confusing?

```
import functools

def my_decorator(func):              # take a function as an argument,
    @functools.wraps(func)
    def inner(*args, **kwargs):      # define an inner function
        do_stuff_before()            # may do some computation before,
        res = func(*args, **kwargs)  # run original function,
        do_stuff_after()             # may do some computation after
        return res                   # return original (or altered) result
    return inner                     # return that inner function

@my_decorator
def this_needs_decoration():
    pass    
```

This is funny how you should use `functools.wraps` decorator  while defining
your own decorator :-) Why? Because of important [reasons]: it will persist
`func`'s original docstring, function name and more. Check out [docs] for more
information.

[reasons]: http://stackoverflow.com/a/43056600/1236827
[docs]: https://docs.python.org/3/library/functools.html#functools.wraps
