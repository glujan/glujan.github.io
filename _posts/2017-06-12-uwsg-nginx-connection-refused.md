---
title:  "Nginx + uWSGI + Docker = Connection refused"
date:   2017-06-12 22:00:00 +0100
categories: python devops docker
---

I'm working on a cool project and want to deploy it using Docker Swarm. I've
created _Dockerfile_ for each service, configured them using _docker-compose.yaml_, 
set up a private registry and pushed there images build on CI server. So far so
good.

Crutial part of my _docker-compose.yml_ is attached below. uWSGI was
configured, as adviced on the docs, to use socket file for lower latency so I
needed a named volume to share that file between `web` and `nginx` services.


```
version: "3"
services:
    web:
        build: coolproject
        image: coolproject/web
        volumes:
            - uwsgivol:/var/run/coolproject/
         networks:
            - net
        command: ["uwsgi", "--ini", "uwsgi.ini"]
    nginx:
        build:
            context: docker
            dockerfile: nginx.dockerfile
        image: coolproject/nginx
        ports:
            - "80:80"
            - "443:443"
        networks:
            - net
        depends_on:
            - web
        volumes:
            - uwsgivol:/var/run/coolproject/
networks:
    net:
        driver: overlay
volumes:
    uwsgivol:
```

First iteration of deployment of the project was both sucessful and very basic.
There was only one agent on Docker Swarm cluster, similarly as in my dev
environemnt (where I do not use Swarm mode). This had one huge drawback, though
and it became clear when Continous Delivery started to update my dockerized
application: it was not possible to gently update the project. I had to stop
the app and then deploy it again with newer images. The reason for that is
pretty straightforward: while using one node there was no agent with ports 80
and 443 available so Nginx couldn't be updated with no downtime. Docker Swarm
firstly spins containers with updated services and after that it substitutes
older ones.

No big deal. I've set up two more agents and thought CD was ready to rock. 
Unfortunately most of the time CD service was failing jobs during deployment.
I've started to debug on a ~production~ staging environment and found
interesting behaviour.

Nginx's logs were saying that a worker service is not able to read from uWSGI
socket file. Permissions were right but still, for some reason, it was unable
to properly serve my app.

After some investigation I've finally understood a flaw in my configuration.
When Swarm delegated _web_ and _nginx_ services on the same node, everything
was working just fine. The issue was occuring when it decided to use different
nodes for those two services. See, default volume driver can't handle
connections between remote agents so Swarm is creating two volumes, one on each
agent. Now this is clear why Nginx couldn't read _uwsgi.sock_: it was simply
not there or, even worse, it was there but not currently in use. _Web_ service
might be deployed on that agent before so socket file is still existing in that
volume but not currently in use by uWSGI.

As far as I understood there are two ways of dealing with that problem. First
is to use a custom volume driver, which is able to work across different
agents. Second was to ensure _nginx_ and _web_ services are always scheduled to
run on the same agent.

The latter seemed to be much simpler for me so I've chose this solution. Fix
happen to be just two lines in Compose file, inside _nginx_ service definition:

```
# yadayada
    nginx:
        environment:
            - "affinity:container==*coolproject_web*"
# yadayada
```

This is a Docker filter which makes _nginx_ service to be scheduled on the same
agent as _web_ from _coolproject_. I would probably need to rethink this
technique once the app will require scaling but for now it works perfecly fine.
There are a few kinds of filters and you can learn more about them [here].

[here]: https://docs.docker.com/swarm/scheduler/filter/
