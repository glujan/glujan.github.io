---
title:  "High level tech stack for Łapka"
date:   2017-03-02 19:00:00 +0100
categories: GN17 asyncio python vue lapka
---

## Backend

Here the choice is simple. I had no oppurtinity (yet) to run Python's `asyncio`
code on a production. So far I've just created a bunch of simple experiments to
get familiar with its API.

Both me and `asyncio` are now ready to create (a little bit) more complex
project. I aim to use the newest version of the language possible - **Python
3.6.0**. If I came across any nasty denependencies not supporting this version
yet, I _may consider_ downgrading to 3.5 but not any older than that. More on
that later.

My favourite RDBMS is **Postgres** and this is what Łapka will use.

## Frontend

As I've mentioned in earlier post I want to try one of new & cool JS frameworks
that pops up every now and then. Looks like the biggest communities have formed
around React and Angular and its important for me to choose popular framework
in case of having any problems during development. Also, easy to read docs
would be a great plus and I would appreciate a gradual learning curve as well -
I am a lazy person after all.

Angular is splitted into two parts because of 2nd version and to be honest I
neither want to learn TypeScript nor stick to (possibly) outdated 1.x.

React on the other hand... I don't know. Licensing is strange, how can I know
I won't sue Facebook at some point in the future? And I don't like beige
websites and React's docs are beige. It clearly disqualifies this framework.

And that's why my choice happen to be **Vue.js**.
