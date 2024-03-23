[![Python package](https://github.com/Jylpah/queutils/actions/workflows/python-package.yml/badge.svg)](https://github.com/Jylpah/queutils/actions/workflows/python-package.yml)  [![codecov](https://codecov.io/gh/Jylpah/queutils/graph/badge.svg?token=rMKdbfHOFs)](https://codecov.io/gh/Jylpah/queutils)

# Queutils

Queutils *[Queue Utils]* is a package if handy Python queue classes:
- **[AsyncQueue](asyncqueue.md)** - `async` wrapper for `queue.Queue`
- **[IterableQueue](iterablequeue.md)** - `AsyncIterable` queue
- **FileQueue** - builds a queue of filenames from input


# AsyncQueue

[`AsyncQueue`](asyncqueue.md) is a async wrapper for non-async `queue.Queue`. It can be used to create 
an `asyncio.Queue` compatible out of a (non-async) `multiprocessing.Queue`. This is handy to have `async` code running in `multiprocessing` processes and yet be able to communicate with the parent via (non-async) managed `multiprocessing.Queue` queue. 

## Features 

- `asyncio.Queue` compatible
- `queue.Queue` support
- `multiprocessing.Queue` support


# IterableQueue

[`IterableQueue`](iterablequeue.md) is an `asyncio.Queue` subclass that is `AsyncIterable[T]` i.e. it can be 
iterated in `async for` loop. `IterableQueue` terminates automatically when the queue has been filled and emptied. 
    
## Features

- `asyncio.Queue` interface, `_nowait()` methods are experimental
- `AsyncIterable` support: `async for item in queue:`
- Automatic termination of the consumers with `QueueDone` exception when the queue has been emptied 
- Producers must be registered with `add_producer()` and they must notify the queue
  with `finish()` once they have finished adding items 
- Countable interface to count number of items task_done() through `count` property
- Countable property can be disabled with count_items=False. This is useful when you
    want to sum the count of multiple IterableQueues 

# FileQueue

`FileQueue` searches files and directories given as input and builds an `IterableQueue[pathlib.Path]` queue of the matching files found. 

## Features

- Input can be given both as `str` and `pathlib.Path`
- Support both exclusive and inclusive filtering 
- `case_sensitive: bool = True` param to control case sensitivity (use of `fnmatch` or `fnmatchcase`)
  