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

## The real magic - `await`

Cool. We have our code starting to come together. Let's add a new async function and rename `some_func` to `run` since that is all we are going to have it do -- run our async logic.

```
import asyncio

async def speak():
    print('Hey!')

async def run():
    await speak()

loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```

You see how we have `await` before we call `speak()`? Coroutines are generators in asyncio, so we could handle them like we do generators. However python 3.5 gives us `await` as syntactic sugar around `yield from`, which should make it feel more like other languages async syntax. (btw, if you are not familiar with Python generators, that's fine. You don't need them, [but for sure read about them later](https://jeffknupp.com/blog/2013/04/07/improve-your-python-yield-and-generators-explained/))


What happens if you leave out `await`? There are times, which I will go over, that do not require you to immediately `await` your coroutine, but at some point before the execution of your code is complete, you will need to handle your coroutine. 

## Starting a function and getting back to it later. 

The first thing you might need an async pattern for is starting a job in the background, do other tasks in the front and then coming back to your original task. 

```
import asyncio

async def speak():
    print('C')
    await asyncio.sleep(3)
    return 'D'

async def run():
    will_speak = speak()
    print('A')
    print('B')
    print(await will_speak)

loop = asyncio.get_event_loop()
loop.run_until_complete(run())

```

If you run this you will see the results:
```
A
B
C
D
```

That's what we wanted right? Close. If you remember, we wanted to start our async job `speak()` and when it was done, return the results. So the first thing that should have been printed to our terminal is `C` not `A`. While the above is a good pattern if you just want to define your job(s) and then return to them later to actually perform the execution, this does not perform our task while we move on to do other things. To do that you actually need to use `asyncio.ensure_future`.

```
import asyncio

async def speak():
    print('C')
    await asyncio.sleep(3)
    return 'D'

async def run():
    will_speak = asyncio.ensure_future(speak())
    await asyncio.sleep(1)
    print('A')
    print('B')
    print(await will_speak)

loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```

Note: I added a 1 second sleep after we did `ensure_future()` That's because `will_speak` is now running in parallel with the rest of the function, and you cannot guarantee that C will print before A and B, so putting a sleep in there ensures the speak() function can finish it's task first. This is just for the example and not necessarily necessary (that's fun to say)

Cool. We have a function that branches off and does it's own thing while we continue on, and when we need it's results we will have them. 


## Order mucking

```
import asyncio


async def meow(number):
    print(f'starting {number}')
    await asyncio.sleep(1)
    print(f'stopping {number}')


async def run():
    # notice how we assign each coroutine to a variable
    a = meow(1)
    b = meow(2)
    c = meow(3)
    d = meow(4)
    e = meow(5)

    # then we can decide which order we wish our coroutines to execute
    await c
    await a
    await e
    await b
    await d


loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```

Let's start looking at how our coroutines are handled. In the above example, I am showing that you can define your work in any order. Then await for them out of order. Or out of order and then back in order. Or any combination. Great. They all run sequentially though in the order they are awaited. What about if we wanted all those to run at the same time. Well, we could `ensure_future` all the things (which is just fine). OR we can show you another way.

## Parallel runnings. 

```
import asyncio


async def meow(number):
    print(f'starting {number}')
    await asyncio.sleep(1)
    print(f'stopping {number}')


async def run():
    f = []
    for x in range(1, 6):
        f.append(meow(x))
    await asyncio.wait(f)

loop = asyncio.get_event_loop()
loop.run_until_complete(run())

```

Here we are creating a list and then adding all of our items to said list. In this case, we are just iterating over a `range()` and appending those coroutines to that list. Then with `asyncio.wait()` we can wait for all those coroutines to complete. (Don't forget to `await` the `asyncio.wait()`, a common mistake.)

One thing you should notice, in Python 3.5, 3.6 and 3.7 they all work out of order. I'm not sure why, or if/when they will resolve this. If order is important, wrap your coroutine in `ensure_future`. Free beer/tacos for anyone that can tell me why and can meet me in Salt Lake City. 

Example of order keeping parallelism.
```
async def run():
    # We create an empty list to store our coroutines in
    f = []
    for x in range(1, 6):
        f.append(asyncio.ensure_future(meow(x)))
    await asyncio.wait(f)
```

## Handling async function results

Our previous example handles the parallelism beautifully (albeit out of order), but what if you want to do something with the results of those functions, a la "scatter-gather"?

GATHER!

```
import asyncio

async def meow(number):
    await asyncio.sleep(1)
    return number

async def run():
    f = []
    for x in range(1, 6):
        f.append(asyncio.ensure_future(meow(x)))
    x = await asyncio.gather(*f)
    print(x)

loop = asyncio.get_event_loop()
loop.run_until_complete(run())

# $>: [1, 2, 3, 4, 5]
```

## That it? What else you got? 

A couple things come to mind. Look at this example. 

```
import asyncio

async def new_future(n):
    print('future', n)
    await asyncio.sleep(3)
    print('future done', n)
    return n

async def run():
    results = asyncio.ensure_future(new_future(1))
    print(results) # <Task> object returned
    print(await results) # `1` is returned
    print(results) # <Task> is returned again
    print(await results) # `1` is returned again

    results = new_future(2)
    print(results) # coroutine returned
    print(await results) # `2` is returned
    print(results) # coroutine returned again
    print(await results) # RuntimeError: cannopy reuse aalready awaited coroutine


loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```

`ensure_future` wraps up our coroutine in a `Task`. A task is more roubust than a coroutine. First a task is executed immediately when it is created, and then stores the results inside of it's object. Then, any time in the future if you call `await` on your future, you can retrieve the result, without having to re-execute the item again. A coroutine dies as soon as it is used up, and will raise an exception. 

And lastly, just have fun with it. You can do all sorts of crazy stuff with asyncio. Execute things in weird orders, or whatever makes sense for your application. 

```
import asyncio


async def meow(number):
    print(f'starting {number}')
    await asyncio.sleep(1)
    print(f'stopping {number}')


async def run():
    f = []
    for x in range(1, 6):
        f.append(meow(x))

    for x, y in enumerate(f):
        if x % 2 == 0:
            await y
    for x, y in enumerate(f):
        if x % 2 != 0:
            await y

loop = asyncio.get_event_loop()
loop.run_until_complete(run())
```

And as always. 
