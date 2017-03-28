---
title:  "To ORM or not to ORM"
date:   2017-03-28 16:30:00 +0100
categories: GN17 lapka python db update
---


## What ORM is

_ORM_ stands for _Object-relational mapping_ and is a very handy technique
which allow to query a database in an object-oriented style. Instead of writing
a (No)SQL query and then using data directly or converting to a well-defined
format manually, an user can operate on objects native to a project's
programming language and chosen ORM library will generate and run all queries
needed to create, read, update or delete data (see: [CRUD]) on your database.

[CRUD]: https://en.wikipedia.org/wiki/Create,_read,_update_and_delete

Thanks to that technology creating a database schema is no much different than
creating an ordinary class. There are a few libraries which provides ORM in
Python and the most notable are [SQLAlchemy] and [Django ORM]. Lesser known
projects are [Peewee], [Pony] and [SQLObject]. The functionality and
compatibility with database engines may vary so choose your ORM library with
caution. Using _SQLAlchemy_ would be my first choice, especially when a
project is not using _Django_.

[SQLAlchemy]: http://www.sqlalchemy.org/
[Django ORM]: https://docs.djangoproject.com/en/dev/topics/db/models/
[Peewee]: https://peewee.readthedocs.io
[Pony]: https://ponyorm.com/
[SQLObject]: http://sqlobject.org/

To see how ORM library can save development time please have a look at an
example below using _Django ORM_. Here is the model:

```
from django.db import models

class Animal(models.Model):
    name = models.CharField(max_length=30)
    age = models.PositiveSmallIntegerField()
```

which will generate a database table with ~~two~~ three columns: _name_, _age_
and _id_, which _Django ORM_ adds by default. The model would roughly translate
to such a query to create a table:

```
CREATE TABLE myapp_animal (
    "id" SERIAL NOT NULL PRIMARY KEY,
    "name" VARCHAR(30) NOT NULL,
    "age" SMALLINT UNSIGNED NOT NULL
);
```

And an example query which would search for all animals under the age of 3 with
name starting with "R":

```
Animal.objets.filter(age__lt=3, name__startswith='R')
```

which would generate a query similar to this one:

```
SELECT * FROM myapp_animal
  WHERE age < 3 AND name LIKE 'R%' ;
```

Using more complicated queries is, of course, supported as well (like `JOIN`s).


## What ORM is not

It's not a silver bullet :-) Queries are generated so sometimes an user could
prepare an alternative which would run much faster. Also, since ORM calls to a
database are much simpler, developers may not feel a cost (in time) of doing
them. Instead of preparing an efficient query to pull data from a DB, they may
easily make a number of queries instead. This can be reduced but some
discipline and self control among devs will be needed. Devs should know _SQL_,
otherwise yet another abstraction layer in an application will cause a
confusion in case of any problems. 

_ORMs_ are trying to be universal so it means that using a very specific
database functionality might be not possible with a chosen library. Keep this
in mind and better check if the feature you are interested in is supported.

Nonetheless, _ORM_ libraries are great. Just remember to use them responsibly.


## Database in Łapka

As mentioned before, I will use Postgres in [Łapka]. As I usually work with
_ORMs_ I decided to act differently this time and rely on my SQL knowledge. I
hope it will allow me to better understand how architecture of such solution
should look like and, maybe, learn something new regarding Postgres itself. For
sure I want to try a `JSONB` type, which is available since version 9.4
released back in 2014. For NoSQL storage I usually use MongoDB but after
reading some articles explaining why Postgres should be my document database
(like [this](https://blog.jetbrains.com/pycharm/2017/03/interview-with-jim-fulton-for-why-postgres-should-be-your-document-database-webinar/) 
or [that](http://www.aptuz.com/blog/is-postgres-nosql-database-better-than-mongodb/) one), 
I've decided to give it a shot.

[Łapka]: https://github.com/glujan/lapka
