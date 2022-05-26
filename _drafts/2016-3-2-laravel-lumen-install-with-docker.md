---
title: Quick Laravel/Lumen install with Docker!
date: 2016-03-02 00:00:00 -07:00
categories:
- Laravel
tags:
- php
- laravel
- lumen
- tutorial
layout: post
---

The default installer that comes with Laravel is great, but unfortunately that means you have to install dependencies of PHP and Laravel/Lumen first, before you can get started working on your application. This is not longer the case, so long as you [use Docker](https://docs.docker.com/mac/step_one/ "use Docker"). And you [should](https://www.docker.com/use-cases "should").

All you need to do is:

## Laravel

```bash
docker run -v `pwd`:/var/www alairock/laravel 'laravel new site-name'
```

## Lumen

```bash
docker run -v `pwd`:/var/www alairock/lumen 'laravel new service-name'
```

## Bonus Round!

These containers aren't _just_ good for installing laravel apps. You can also use them for development. (Remember, you need to add `--host=0.0.0.0` in order to make sure that external connections to the artisan server are allowed)

```bash
$ cd site-name # or service-name if using lumen
$ docker run -v `pwd`:/var/www -p "8080:8000" alairock/lumen 'php artisan serve --host=0.0.0.0'
```

Just don't forget to link to your database, nginx, or whatever other containers you are needing to integrate with.
