---
tags:
  - php
  - ' laravel'
  - ' lumen'
  - ' tutorial'
layout: post
title: 'Quick Laravel/Lumen install with Docker!'
category: Laravel
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

These containers aren't _just_ good for installing laravel apps. You can also use them for development.

```bash
$ cd site-name # or service-name if using lumen
$ docker run -v `pwd`:/var/www alairock/lumen 'php artisan serve'
```

Just don't forget to link to your database, nginx, or whatever other containers you are needing to integrate with.
