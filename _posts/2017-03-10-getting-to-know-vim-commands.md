---
title:  "Getting to know Vim commands"
date:   2017-03-10 9:25:00 +0100
categories: GN17 tools app
---

As some may has noticed I'm using Vim on my daily basis - my [.vimrc], a Vim
configuration file, is not only hosted on GitHub but also being updated time to
time. I'm coding using this text editor in Python, Go, JavaScript and I've even
created all posts for this blog in Vim.

[.vimrc]: https://github.com/glujan/vimrc

## Why?

Because why not ;-)

Because it's convenient. As an user, I only need to remember one set of
commands and this knowledge allows me to work efficiently in many different
domains - think of how much writing a Markdown document has in common with
creating a Python script? Editing a code is mostly the same as editing a
regular text and moving a cursor around the document.


## Gimme examples

Let's say you want to remove everything inside brackets because that function
call needs different arguments. In a _normal_ editor you probably need to grab
a mouse, select every character inside parentheses (or use `Ctrl` + `Shift` +
arrow combination) and press `Delete` on your keyboard. In Vim just type
command `di(` while a cursor is anywhere inside brackets and text inside is
gone. The same goes for other delimiters, like braces, chevrons, quotation
marks and apostrophes.

A crucial thing is to understand how to compose a command.


## Learning commands

Getting started with Vim is pretty hard. There are even (decades old) jokes
about that:

> Q: How to generate a random string?
>
> A: Put a casual PC user in front of Vi and tell them to exit.

Or this one:

> I've been using Vi for about 2 years now, mostly because
> I can't figure out how to exit it.
 
(Note that Vim stands for _Vi IMproved_ and is based on original Vi editor,
dating back to 1976)

There is, however, a built in tutorial, called `vimtutor` which will guide a
new user through the most common commands. But what's next?

I've recently came up with a simple but really beneficial app on Google's Play
Store. It's called simply [Vim master] and is available for free (but with
ads). This is a quiz-like app with 3 difficulty settings (easy, normal and
hard) and basic questions filtering. Questions are composed from two images -
before and after running some command and an user task is to choose a correct
command from 4 possibilities.

My favourite feature, however, is that after submitting an answer, the app will
not only show the right one but also will explain what all of them mean - so in
fact I can learn all of them at the same time. Plus, I can attempt a short quiz
virtually anytime and anywhere as it only takes a few minutes to accomplish it.

Check out [Vim master] on Play Store!

[Vim master]: https://play.google.com/store/apps/details?id=develop.example.beta1139.vimmaster
