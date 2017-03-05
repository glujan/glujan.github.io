---
title:  "A point or two about CI"
date:   2017-03-04 10:00:00 +0100
categories: gn17 devops
---

After a few weeks - or maybe even months - of whole team's hard work, after
countless hours of coding and some unforgotten moments of ~~swearing~~
debugging, there it is - a shiny and new version of your tremendously
magnificent application. It is ready to be shipped to unwilling to wait even
a day more clients.

Except it's not.

Since previous release that little feature nobody cares about is not always 
working as intended, one of developers is struggling to integrate his changes 
with the rest of the project and the other says he thought API should return 
different result on his call thus his task is not ready for the release.

To avoid such or similar situations, people who gathered mysterious wisdom
while studying dark secrets of failed projects came up with an idea to actually
_test_ whether or not developer creating that sweet little _Pull
request_ (s)he's so proud of, won't break anything in the project.

Ladies and gentlemen, introducing a _continuous integration_! 


## Continuous integration 

The idea here is simple - before accepting any change to project's codebase,
run whole test suit and merge if and only if all tests passes.

Developers' responsibility now is not only to deliver a new feature but also
provide tests which will cover it. If a change is a bugfix rather then a new
piece of code, test which proves a defect exists should be created before
attempting to fix it and has to turn from red to green after a patch is
written.

That's it. Pretty simple, isn't it?

The biggest problem to start using CI might be a lack of test suit. If a
project has a lot of lines of code, it may be hard (for developers) to cover
existing code with tests. It's not a good excuse, though. Laziness is never a
good excuse.

And how to start using CI if you really want to? There are many services which
offer a CI service for free for open source projects and charge for private
repositories only - [Travis](https://travis-ci.org/), [CodeShip](https://codeship.com/),
or [CircleCI](https://circleci.com/) to name a few. If you have concerns about
giving an acces to your code to a third party company there are plenty of
solutions which you may run on your own server - like [Jenkins](https://jenkins.io/),
[Drone](https://github.com/drone/drone), [GoCD](https://www.gocd.io/) and many,
many more. And they tend to be open source, which is nice.
