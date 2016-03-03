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
# Quick Lumen/Laravel install with Docker

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
