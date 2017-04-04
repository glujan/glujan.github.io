---
title:  "Inherit or compose?"
date:   2017-04-04 14:40:00 +0100
categories: GN17 lapka update python
---

Inheritance and composition are one of the most concepts in object-oriented
programming. Where the former one attempts to share a behaviour across
different classes, the latter is mostly think of as a way to combine simple
objects (or data types) in order to build a more complex ones. At the first
glance they may seem to be designed for different purposes. But are they?


## Inheritance

I bet most, if not all, programmers know what inheritance is. For the sake of
completeness a quick example in Python:

```
class Parent:
    def some_method(self):
        pass

class Child(Parent):
    def other_method(self):
        pass
```

`Child` _inherited_ `some_method` from `Parent` class, so instance of it can
run both `some_method` and `other_method`. We passed behaviour without a need
to copy and paste any code, which is nice.

This, of course, can lead to more advanced design patterns like [template
method], which uses inheritance in order to define a skeleton of an algorithm
and expect from child classes to add some flesh to it :-) An example:

```
class Skeleton:
    def do_the_stuff(self, arg1, other_arg):
        # maybe do some stuff before
        self._some_part_of_job()
        # can do more stuf after as well

    def _some_part_of_job(self):
        raise NotImplementedError

class Flesh(Skeleten):
    def _some_part_of_job(self):
        pass
        
```

Now, as you can see, `Flesh` is using invariant steps, defined in its base
class, `Skeleton`. It only implements different behaviour in
`_some_part_of_job` method, but overall algorithm is still the same. `Skeleton`
might provide a default implementation but it's not a necessity. Of course one
class may define more template methods, if needed.

I use template methods in [Łapka] for fetching data from shelters. Thanks to
that in each concrete class I only had to implement _one_ method and set up
_three_ class fields. You can browse sources [here](https://github.com/glujan/lapka/blob/743f9e0a69143ada1cea58c22fcddfdd79f832cd/lapka/fetch.py).

As you can see I fulfill my goal to not repeat myself. But did
I keep it simple? My code is DRY, but will I get a KISS for this?

(Explanation for this bad pun: one of software development principles is _DRY_
which stands for _Don't Repeat Yourself_, another one is _KISS_ which reads
_Keep It Simple, Stupid_)

[template method]: https://en.wikipedia.org/wiki/Template_method_pattern


## Composition

As described before, composition is used to create more complex objects or data
types from simpler ones. Some basic example:

```
class Animal:
    def __init__(self):
        self.name = ''
        self.age = 0
```

As you can see in the example, `Animal` class consist of two fields: `name` and
`age`. So composition in OOP is something you do all the time. Booooring. Let's
move to something more interesting, shall we?

We can compose objects to share behaviour as well. It would be called
delegation in such scenario and here's how you implement this:

```
class DBProvider:
    def __init__(self):
        # connect to a database or whatever

    def find(self, uid):
        return 

class User:
    provider = DBProvider()

    def find(self, uid):
        data = self.provider.find('user', uid)
        return User(**data)
```

This looks pretty similar to the inheritance. So why I've chosen this approach
to implement database operations in [Łapka]? Well, separation of concerns was
my concern.

See, there may be much more data providers then a database one - maybe a cache,
file or even a remote service. They have _nothing_ in common, when it comes to
a implementation. They have common interface, of course, but here's where
similarity ends. Underlying code will be completely different, so inheritance
doesn't look natural for me here.

And there is another problem. What if my application will need to use multiple
persistence implementation at once? Should I create multiple classes of User,
each inheriting from different base class? This sounds just lame. It's better
to use composition and delegate persistence to instance of another class. Plus,
in unit tests I can substitute a real provider with a dummy one to make my
tests run faster.


[Łapka]: https://github.com/glujan/lapka

## Conclusion?

Just use what seems to be a better fit for a problem you're trying to solve.
Inheritance is not always the answer.
