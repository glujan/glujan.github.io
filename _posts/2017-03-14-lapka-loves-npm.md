---
title:  "Łapka loves (?) npm"
date:   2017-03-14 13:30:00 +0100
categories: GN17 lapka update vue
---

I've finally decided to do that step and move from my old school JS workflow to
a ~~trendy~~ modern one - which means I need to install _npm_, _webpack_,
_babel_ and who knows what else. Most of that stuff was unfamiliar to me so
I've tried to understand the whole process of creating a _Node_ project by
doing it myself... but I failed. What went wrong?

## Setting up a node

Installing and setting up _node_ was an easy task. I've used [nvm] which is
conceptually pretty similar to [pyenv] and the latter tool I'm using for quite
a time now. And then the troubles started.

I must admit _Webpack_ (and it's plugins) is a really weird tool to use. Maybe
it's me being silly, I'm not sure at this anymore. My opinion on it is:
_too good to leave, too bad to stay_. And the _staying_ part I definitely owe
to _vue-cli_ which generated all the wicked configuration files and a project
structure for me. I've made some minor changes in a `package.json` but was
afraid to tweak it even more. Now I finally understand how an elderly person
unfamiliar with using a computer may feel like when in need to perform a little
bit more harder operation.

_Npm_'s errors are very verbose but at the same time are giving a little of
information about the cause of an error. For instance: accidentally I tried to
run a second _node_ development server and from logs I couldn't read that the
failure is due to a port being already in use, I only was noticed that it is
*probably* not a problem with _node_ but my package. _Touché_.

After I finally created a node project I was able to move one of my Vue
components from a regular JS and a HTML template to a separate `.vue` file.
That was not a bad experience at all. Vue compiler is by far much more helpful
than _node_ logs. Or maybe that was some of _webpack_ plugin? Dunno. Anyway,
it was very supportive.

As a summary I can only write one sentence: JavaScript ecosystem feels to be
overcomplicated and/or overengineered **for me**. Me, who was mostly using
_jQuery_ (with plugins stored inside `vendor` directory) for years and didn't
face any major problems with it. I'm really looking forward to see what my
point would be after working with _node_ for some time.

[pyenv]: https://github.com/yyuu/pyenv
[nvm]: https://github.com/creationix/nvm


## Where are unit tests?

The sad point about transforming my UI to a node project is it reminded me that
JavaScript is also a code which needs to be tested. My original idea was to
write tests just after configuring webpack and other stuff - like Travis -
but it took me so long I had to postpone my rather amibitous plan. To my
surprise, [codecov.io] recognized I'm now using _node_ in _Łapka_ without any
additional configuration and reported a significant drop in a coverage from
100% down to 82%. Well, I should have seen that coming.

I will fix it in future pushes, I promise.

[codecov.io]: https://codecov.io/github/glujan/lapka
