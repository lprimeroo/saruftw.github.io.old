---
layout: post
title: Python Booster Pack! - Part 2
description: Boost your Python skills with these "hidden in plain sight" tips and hacks.
author: Sarthak Munshi
tags: python tips tricks booster clever hacks hacker python3 python2
finished: true
---

I'm back with a new part in the series where I expose amazing and little known features, tips and tricks within the Python programming language. This part is GIF-free (_please don't hurt me_) and comes straight to the point. Let's just jump right into it.

Ahh! In case you want to finish up reading the Part 1 first, you can find it <a href="https://saru.science/tech/2018/01/24/python-booster-pack.html">here</a>.

### Avoid data structure initializing functions

Have a look at the snippet below and think about what does it tell you?

```py
>>> from timeit import timeit

>>> timeit("x = dict()", number=10000000)
1.3071075379999968

>>> timeit("x = {}", number=10000000)
0.37984672199999636

>>> timeit("x = list()", number=10000000)
1.0812364029999912

>>> timeit("x = []", number=10000000)
0.285002046999999
```

In this snippet, we timed the data structure initializing functions such as `dict` and `list` with their typical symbolic initializers (_idk what to call them_) and ran them 10000000 times. We observe that using the symbolic initializers give a 4x to 5x increase in initialization speeds. So, the next time you review or write code, make sure you point this out.

### Better function aliases

In this section, we explore `functools.partial`. This utility enables us to create a new version of a function with one or more arguments already filled in. In order to convert an integer in a different number system to decimal, we use `int` with the `base` argument. Writing them as functions would give us the following snippet.

```py
def from_binary(x):
  return int(x, base=2)

def from_octal(x):
  return int(x, base=8)

def from_hex(x):
  return int(x, base=16)

print(from_binary('10010')) # prints 18
print(from_octal('34472')) # prints 14650
print(from_hex('7e8c9')) # prints 518345
```

However if we use `partial`, we can basically achieve the same functionality in a more terse and elegant way.

```py
from functools import partial

from_binary = partial(int, base=2)
from_octal = partial(int, base=8)
from_hex = partial(int, base=16)

print(from_binary('10010')) # prints 18
print(from_octal('34472')) # prints 14650
print(from_hex('7e8c9')) # prints 518345
```

Although this example may seem trivial to some extent, `partial` is quite useful during API design.

### Heard of for..else?

The snippet mentioned below finds out whether an odd number exists within a list. It uses a boolean `flag` to achieve this task. Can you find a way to achieve this without utilizing any sort of flag or without making the code more complex? Think hard!
```py
flag = False

for i in [2, 3, 4, 5, 6]:
  if i % 2 != 0:
    flag = True
    break

if flag:
  print("An odd number exists in the array.")
else:
  print("An odd number does not exist in the array.")
```

I don't know if you were able to came up with something or not, but an elegant solution to approaching this is to use the `for..else` syntax. The `else` construct executes only when the loop finishes uninterrupted. Calling a `break` within the `for` loop ignores the `else` and jumps out.

```py
for i in [2, 2, 4, 4, 6]:
  if i % 2 != 0:
    print("An odd number exists in the array.")
    break
else:
  print("An odd number does not exist in the array.")
  ```

We have elegantly avoided the flag and our code looks a lot cleaner. Another block of code to point out to during those code reviews.
### Manage your contexts

There will exist situations in your coding journey, where you'll need to implement some kind of functionality that executes before and after your logic.

These situations may include closing a file after you're done reading it, releasing a lock or making the thread release resources after the task is done. There are multiple such use cases.

Python's `with` keyword is a solution proposed to this problem.

```py
with open('filename.txt', r) as f:
  ...
```

The snippet mentioned above automatically closes the file after the logic present inside the `with` block is over. The term _context manager_ is used to refer to such coding pattern. We have multiple ways of achieving this within Python.

The first method is just a normal class but with `__enter__` and `__exit__` dunder methods. In order to understand the flow, try executing this code.

```py
class Demo:
  def __init__(self):
    print("__init__")

  def __enter__(self):
    print("__enter__")
    return "whatever __enter__ returns"

  def __exit__(self, *args):
    print("__exit__")

with Demo() as x:
  print(x)
  print("hello")

## Output
__init__
__enter__
whatever __enter__ returns
hello
__exit__
```

If you notice the flow carefully, you might observe that `x` is whatever `__enter__` returns and the order of execution is `__init__`, `__enter__` , code inside `with` and lastly `__exit__`. This way we can insert all the code we want to execute before our logic inside `__enter__` and likewise for all the code we want to execute after our logic inside `__exit__`.

Second method of achieving this is also quite useful when we don't want the overhead of a class or have minimal things to do before and after our logic. In that case, we might as well use functions. Just add the `@contextmanager` decorator to your function and anything before the `yield` statement will behave like the `__enter__` dunder and anything after `yield` will behave like the `__exit__` dunder.

```py
from contextlib import contextmanager

@contextmanager
def Demo():
  print("__enter__")
  yield "hello"
  print("__exit__")

with Demo() as x:
  print(x)

## Output
__enter__
hello
__exit__
```

Notice how `x` here stores the value returned by `yield`.

The third way of achieving this focuses on generalizing this feature. If you're repeating your before and after logic a number of times, it's wise to not repeat yourself and use `ContextDecorator`.
In the snippet below, we write a class similar to the one mentioned in the first method and make it inherit from the `ContextDecorator` class.

```py
from contextlib import ContextDecorator

class Demo(ContextDecorator):
  def __enter__(self):
    print("__enter__")

  def __exit__(self, *args):
    print("__exit__")

@Demo()
def DemoFunction():
  print("hello")

DemoFunction()
```

Using this class as a decorator over any function, will enable us to execute our before and after logics multiple times over. This way, we write once and keep on decorating all the functions we want this utility on. Notice that the `with` keyword is not used in this method.

### Generator Expressions

IPython's `%timeit` magic is quite useful to discover what's best for the speed and elegance of our code. Basically, all we're doing is finding all the even numbers up to 1000 and squaring them. And we're doing this using 2 techniques. The first is a generator expression and the second is a standard list comprehension. Later we iterate over them, and print the values.

```py
>> %timeit it = (x**2 for x in range(1000) if x % 2 == 0)
642 ns ± 18.1 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)

>> %timeit non_it = [x**2 for x in range(1000) if x % 2 == 0]
228 µs ± 2.95 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)

>> %timeit for el in it: print(el) #ignoring the output
44.9 ns ± 0.769 ns per loop (mean ± std. dev. of 7 runs, 10000000 loops each)

>> %timeit for el in non_it: print(el) #ignoringx the output
3.77 ms ± 167 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```

The generator expressions are faster by a factor of  10<sup>3</sup> and 10<sup>6</sup> when it comes to the expression and iteration respectively. Generators are an amazing concept and replacing your list comprehensions with them can provide immense speed benefits. In most cases, just replace `[]` with `()` wherever you notice a list comprehension generating a series   (¯\\\_(ツ)\_/¯).

### Invoke class routines using strings

This is interesting. Let's walk through an example to explain this. Suppose, you have a class as below and it has 4 private members (of course using the infamous double-underscore technique). You also have getter functions written for it. Now, since the member variables are private, we cannot certainly access them using `object.__member`. However, accessing them using the getter function is straightforward (`object.getter_for_member()`).

```py
class Demo():
  def __init__(self):
    self.__name = 'John Doe'
    self.__age = 18
    self.__gender = 'Male'
    self.__location = 'EU'

  def name(self):
    return self.__name

  def age(self):
    return self.__age

  def gender(self):
    return self.__gender

  def location(self):
    return self.__location

d = Demo()
print(d.__name) # fails
print(d.name()) # returns the name
```

However, what do we do if we're given the attributes we want to access in the form of strings? We use `getattr`!

`getattr` takes the first argument as the the class object and second as the member name as a string.

In the snippet below, notice how we've cleverly called all the getter functions.

```py
attrs = ['name', 'age', 'gender', 'location']
d = Demo()
for attr in attrs:
	print(getattr(d, attr)())
```

This is a nice handy feature that allows us to maintain attributes in string format and not care about them while executing.

That's it for the Part 2 folks. I'm always on the hunt for these sort of things. Until next time, Adios!
