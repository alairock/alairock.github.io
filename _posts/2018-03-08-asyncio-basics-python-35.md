---
published: false
---
# Asyncio basics in python

Python 3.5 brought with it asyncio. An event loop based paradigm previously available as a library but now it is built in as a standard library. There are other async libraries out there, but I am going to focus on the one built in to Python. 

## Creating an event loop

```
import asyncio

loop = asyncio.get_event_loop()
loop.run_until_complete(some_func())
```

This is the basic example to get you up and running. This is the core of what is happening. Starting an event loop and running some function on top of it. Some frameworks abstract this from you and handle it on the bootstrap of the application layer (aiohttp, for example). 

## The async function

```
async def some_func():
    pass
```

Just like defining a regular function, except we add the `async` keyword. This not only let's you know the function is asynchrounous, but it also let's the interpreter know. The interpreter wraps up the function inside a [`coroutine`](https://docs.python.org/3/library/asyncio-task.html#coroutines) and then is handle in various ways. This is the crux of what this post is about. 

In our first example we started an event loop and ran our function inside `run_until_complete()` That will run your function until all synchrounous and non-synchrounous calls are complete. I like to think of this step as an instantiation of our asynchrounous paradigm. 