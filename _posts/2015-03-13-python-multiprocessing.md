---
layout: post
title: Multi-processing in Python
author: Kalin Kiesling
time: 12-1 PM
location: 3425 Sterling Hall
location_map: http://map.wisc.edu/s/jzb4imln
category: upcoming
tags: meeting python multi-processing
---


## Attending

- Hackers with some Python familiarity.


## Kalin Kiesling

I am a first year grad student in nuclear engineering, currently 
developing software to aid in computational nuclear engineering tasks. 

## Multiprocessing in Python

Today will be a discussion of using the `multiprocessing` module from Python.
You can follow allow with what we will be doing today [here][code].

**Why might you want to use multiprocessing?**

If you have functions within a single Python file, or process, that cannot be run
at the same time, then Python's `multiprocessing` is for you. "Multiprocessing"
here means threading, so you can use this module to force functions to 
run on different processors.

Documentation for the module can be found [here][docs].

## How to Use

To use the package simply add this to the top of your python script:

`import multiprocessing`

There are many classes you can import specifically like `Pool`, `Process`, `Queue`,
`Pipe`, etc. Today we will go over the `Pool` and `Process` classes specifically.

## The Process Class

`from multiprocessing import Process`

This class will run a function `f(x)` on a single process. There are two
main chunks of code needed in the script:

1. Any function `f(x)` that you wish to run.

2. The process that calls that function.

In our example in [process_example.py][process], we will demonstrate how to 
use this class by getting the process id in a few different ways.

We will start by retrieving the current process id for the script without
launching any extra processes.

```
current = os.getpid()
print "Current process:", current
```

We should get "Current process: *x*"

Next we will create a function `get_id()` that will give us the current
process id and the parent process id.

```
def get_id():   
    pp = os.getppid()   # parent process
    p = os.getpid()     # current process
    print "parent process:", pp
    print "current process:", p
```

We will then call that function a by creating a new process.

```
p = Process(target=get_id)
p.start()
p.join()
```

We initialize the process with `p = Process(target=get_id)` where *target* 
specifies the function we wish to call on a new process. We then have to
start the process `p.start()` and bring it back to our current process
with `p.join()`. 

We should recieve that the current process is now a new number *x+1* and
the parent process has the same id as the main script *x*.
We can call many processes and see that the parent process id will remain
at *x* and the current process will continue to increase by 1.

**Notes:**

- `p.join()` should be called before starting a new process if you 
wish to retrieve your output before starting a new process otherwise a 
single join() statement is required at the end regardless of number of
processes launched and all output will be retrieved at once.

- The standard call for `Process` appears to be in the following if-statement:

```
if __name__ == '__main__':
    p = Process(target=f)
    p.start()
    p.join()
```

## The Pool Class

`from multiprocessing import Pool`

The `Pool` class is similar to `Process` except that you can control a
pool of processes. This is a good class to use if the function returns 
a value.

In the most basic case, you can create a Pool instance with no arguments
and call the function by using apply_async(). We can use our `get_id`
example from before in the same way (see [here][pool]).

```
p = Pool()
r = p.apply_async(get_id_print)
```

We can also use `Pool` if we have a function that returns a value by using
`r.get()` to retrieve the return value. For example if our det_id function 
now returns a list of ids `[pp, p]`, we can retrieve them as so:

```
p2 = Pool()
r2 = p2.apply_async(get_id_return)
vals = r2.get()
print "List of ids:", vals
print "Parent id:", vals[0]
print "Current id:", vals[1]
```

Another great use for `Pool` is its `map` which allows you to call the
function many times, each on a new process.

```
def f(x):
    r = x*2
    return r

p4 = Pool()
r4 = p4.map(f, [1,2,3]) 
print r4
```

**Notes**

- If using `get()`, only basic Python structures can be passed through 
(lists, dictionaries, arrays, etc).

## Tips and Tricks

- This does not work (to the best of my knowledge) if you call it within 
a class (such as in a `unittest` class).

- You can use this with `nosetests` very easily. Assert statements must be
made in the function that creates the process (otherwise tests that should fail 
will show as passes). `SkipTests` can be done in either the function that calls
the process or the actual function. (See this [example][test] and run as 
`nosetests test_example.py`).

## Lightning Discussions 

### Treat rotation?

We can take turns bringing in lunch-time treats.

[code]: https://github.com/kkiesling/THW_multiprocessing
[docs]: https://docs.python.org/2/library/multiprocessing.html
[process]: https://github.com/kkiesling/THW_multiprocessing/blob/master/process_example.py
[pool]: https://github.com/kkiesling/THW_multiprocessing/blob/master/pool_example.py
[test]: https://github.com/kkiesling/THW_multiprocessing/blob/master/test_example.py
