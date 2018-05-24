---
title: Docker Workflow
date: 2016-06-06 00:00:00 -06:00
published: false
---

## Code workflow using docker

Docker is the best things to happen to code development since git. It allows you to compartmentalize all the dependencies of your code, and run things natively. It also lets you write your code in an environment that can replicate production almost- if not exactly, like production. There are other benefits, but those are the primary ones. So lets get to it. 

This tutorial is going to be catered to mac users, since that is what I use. Most of the information here can be transcribed pretty easily to other systems though. I go over docker beta, albeit loosely. If you just want to get going and use the VM install [Docker Toolbox](https://docs.docker.com/toolbox/overview/) and skip ahead past the install steps. 

## Install Docker

Docker is now in beta for a native installation that allows you to run docker without the need for a VM. (If you want to go the VM route, you can with dockertoolbox, which is a convenient way to install everything at once. There are tutorials for that everywhere and is not covered in this tutorial.)

Simply download docker and run. Checkout their [blog post](https://blog.docker.com/2016/03/docker-for-mac-windows-beta/) for more information.

## Install Docker Machine

Docker Machine requires Docker binary. This isn't necessary with the Docker beta program. The beta program will give you more information about how to get around that though. [Here](https://docs.docker.com/machine/install-machine/) is the information for installing docker machine.

## Install Docker Compose

Lastly we need docker compose. [Go install](https://docs.docker.com/compose/install/) that and move on to the next step.

## Setup your application

This is the fun part, and there are a million ways to do it. I'll go over the basics, and possibly some other tutorials in the future on specific environments. In this particular example I will use a PHP/Laravel application, but it is similar for any language/environment.

### Docker Compose setup. 

In the root of your application create a new file and name it `docker-compose.yml'. 

Here is a simple example of the file.

```yml
files:
  image: dylanlindgren/docker-laravel-data
  volumes:
    - .:/data/www/
    - log:/data/logs
  privileged: true

php:
  image: dylanlindgren/docker-laravel-phpfpm
  volumes_from:
    - data
  privileged: true
  environment:
    - APP_ENV=local

web:
  image: dylanlindgren/docker-laravel-nginx
  volumes_from:
    - data
  links:
    - php:fpm
  ports:
    - "80:80"
  privileged: true
```





