---
layout: post
title: Python Booster Pack! - Part 1
description: Boost your Python skills with these "hidden in plain sight" tips and hacks.
author: Sarthak Munshi
tags: python tips tricks booster clever hacks hacker python3 python2
finished: true
---

"Python Booster Pack" is probably the most clickbait-y title you've read for a post which aims at documenting a few tips and tricks in Python. Believe me, the content is not as disappointing as the title. _kek_. In this post, I wish to explain some of the lesser known features that Python or some of its modules have to offer which may affect the way you code and debug or increase your developer productivity to some extent. _Not claiming anything though_. Some of them are hidden in plain sight and call for a _Eureka!_ moment .

<p align="center" markdown="1">
![](https://media.giphy.com/media/26ufdipQqU2lhNA4g/giphy.gif)
</p>

I'm not the one who discovered or developed any of these features. Most of them appeared on my Twitter feed at 2AM or my Reddit feed at 3AM or my friend's chat at 4AM.

<p align="center" markdown="1">
![](https://media.giphy.com/media/3o7bu7LCmYp0x6T97y/giphy.gif)
</p>

Let's begin!

### Introducing `lru_cache()`

```py
from functools import lru_cache

@lru_cache(maxsize=None)
def fibonacci_with_lru(n):
    return n if n in [0, 1] else (fibonacci_with_lru(n - 1) + fibonacci_with_lru(n - 2))

def fibonacci(n):
    return n if n in [0, 1] else (fibonacci(n - 1) + fibonacci(n - 2))
```

Go on. Give the code mentioned above a whirl in your Jupyter notebook.

<p align="center" markdown="1">
![](http://oi65.tinypic.com/vgshw5.jpg)
</p>

The `lru_cache` decorator in the `functools` module maintains a dictionary in a least recently used fashion to cache or _auto-memoize_ the repeated calls. The `maxsize` argument in the decorator decides the max cache size (usually in the powers of 2). It has another argument called `typed` which, if set to `True`, treats `3` and `3.0` as two separate numbers.

In the snippet above, you can notice a speed up of almost 10<sup>-3</sup> even though the input increased by 10x. This is not always advisable and certainly not a replacement for a faster non-recursive approach. It's certainly cool and helps occasionally during prototyping and has it's own use cases such as fetching files, etc.



### `binding.pry` in Python? Hmmm.

I've seen an umpteen number of Ruby practitioners express their undying love for `binding.pry`. It's basically a debugging routine that helps you debug line by line and check various variable or object values at each stage. Well, it also exists in Python (`pip install ipdb`)!

Consider the example of a Binary Search routine that we want to examine from within.

```py
import ipdb; debug = ipdb.set_trace

def binary_search(needle, haystack):
    left, right = 0, len(haystack) - 1
    while left <= right:
        mid = left + (right - left) // 2
        debug();
        if haystack[mid] == needle:
            return True
        elif haystack[mid] > needle:
            right = mid - 1
        else:
            left = mid + 1
    return False

binary_search(2, [1, 2, 3, 4])
binary_search(5, [1, 2, 3, 4])
```

Observe, how we've sneakily placed the `debug()` routine after the middle index deciding stage of the algorithm. Execute this code and you get something like the image below.

<p align="center" markdown="1">
![](http://oi67.tinypic.com/w9i8p5.jpg)
</p>

The program pauses at the `debug()` call and enables code introspection. Keep in mind that `n` is used to move to the next line and `c` is used to continue through the debugger.
You could also try multiple other debuggers. However, `ipdb` uses a nice IPython shell with syntax highlighting and tab-completion which makes it my favourite.



### String Concatenation Puzzle

<p align="center" markdown="1">
![](http://oi64.tinypic.com/200a1iu.jpg)
</p>

Have a look at the code above. The code tries to concatenate three strings together and assign it back to the first string. Both statements essentially do the same thing but one runs almost 30 times faster compared to the other.

This happens because in the second method `s1` is not destroyed while calculating the complete string.

<p align="center" markdown="1">
![](https://media.giphy.com/media/SW3PNayoSGXao/giphy.gif)
</p>




### Magic cells in Jupyter Notebook

Jupyter notebook is my favourite software ever. Period. Magic lies within it. Don't believe me? I'll prove it.

Fire up your Python 3 Jupyter notebooks and ~~type~~ repeat after me.

##### I can run Bash scripts in it

<p align="center" markdown="1">
![](http://oi65.tinypic.com/1zqexpe.jpg)
</p>

##### I can render Latex in it

<p align="center" markdown="1">
![](http://oi68.tinypic.com/iy3jo4.jpg)
</p>

##### I can run client-side Javascript in it

<p align="center" markdown="1">
![](http://oi67.tinypic.com/vesykg.jpg)
</p>

##### I can run Python 2 in a Python 3 notebook

<p align="center" markdown="1">
![](http://oi68.tinypic.com/2cnv62u.jpg)
</p>

##### I can run Ruby in it
<p align="center" markdown="1">
![](http://oi65.tinypic.com/28i6xqt.jpg)
</p>

##### I can render SVGs in it
<p align="center" markdown="1">
![](http://oi67.tinypic.com/1ibe5k.jpg)
</p>

Time for the master spell?

#### I can also upload directly to dpaste (pastebin)
<p align="center" markdown="1">
![](http://oi68.tinypic.com/n6stqx.jpg)
</p>

Lots and lots of fire emoji!

### Introducing the `LineProfiler`

Ever wondered why your routine takes so long in spite of writing an efficient piece of code? Spending hours trying to figure out which part of your routine is eating up all the time may be injurious to the hair on your head.

Anyway, if you're on Python 2.7, start with a

```sh
pip install line_profiler
```

Python 3 users need to follow,

```sh
pip3 install Cython
git clone https://github.com/rkern/line_profiler
cd line_profiler; sudo python3 setup.py install
```

Let's take an arbitrary mathematical function as an example. We import the `line_profiler` and `atexit` modules for our use and place the `@profile` decorator over the function we want to profile.

```py
import numpy as np
import line_profiler
from atexit import register

profile = line_profiler.LineProfiler()
register(profile.print_stats)

@profile
def math_function(n):
    x = np.random.normal(size=n)
    y = np.power(x, 4)
    z = np.sqrt(y)
    return np.sum(z)

math_function(7123681)
```

Run your script and voila!

<p align="center" markdown="1">
![](http://oi64.tinypic.com/10xx6k7.jpg)
</p>

It gives you a line by line breakdown of how many hits each line gets (greater than 1 in case of a loop), total time, time per hit and percentage time taken with respect to the entire function.

<p align="center" markdown="1">
![](https://media.giphy.com/media/aLdiZJmmx4OVW/giphy.gif)
</p>



### Save plots as GIFs

Animations play a significant role in conveying the behaviour of the data points. Sharing them as GIFs on social networks is even cooler.

The code below generates a Sine curve animation in `matplotlib`.

<p align="center" markdown="1">
![](http://oi63.tinypic.com/1jaet3.jpg)
</p>

We require the `imagemagick` module to save it as a GIF and the `save` routine by `matplotlib` takes care of the rest.

```py
anim = animation.FuncAnimation(fig, sine, frames=len(X), interval=50)
anim.save('./animation.gif', writer='imagemagick', fps=60)
Image(url='./animation.gif') # to view
```

### Images == Numbers

Since an image is nothing but a collection of numbers, we can use a very handy lambda function to generate an image from a matrix of numbers. This lambda is quite fast and lightweight and gets the job done. This is also immensely helpful while prototyping or introspecting the dataset.

```py
import numpy as np
from PIL import Image

get_image = lambda x: Image.fromarray(np.uint8(255 * (x - x.min()) / x.ptp()))

get_image(np.random.rand(200, 200))
```

In the snippet above, we pass a random 200 x 200 matrix to the `get_image` routine and get an ~~noise~~ image.

<p align="center" markdown="1">
![](http://oi67.tinypic.com/34yxze1.jpg)
</p>

### Better Python REPL

Most of the Pythonistas may be familiar with this. For those who aren't, run

```sh
pip install ptpython
```

<a href="https://github.com/jonathanslenders/ptpython">Ptpython</a> is a replacement for the traditional Python REPL. It boasts of autocompletion, syntax highlighting, mouse support, multi-line editing and auto-tabbing. What a package!

<p align="center" markdown="1">
![](http://oi64.tinypic.com/r6wi9k.jpg)
</p>
<br />
<p align="center" markdown="1">
![](http://oi63.tinypic.com/95s8pd.jpg)
</p>

You could also supercharge your game by setting it as an alias for your specific Python distribution.

### Cython is <3

We often encounter scripts that perform a simple task but crunch a lot of data. Gaining any sort of speed up is a real pain at times. Let's see how we can achieve this to some extent, starting with a simple Python program.

```py
n = 100000000
arr = [0.0] * n

for i in range(n):
    arr[i] = i % 3

print(arr[:10])
```

Time taken:

```sh
13.26s user 0.31s system 99% cpu 13.636 total
```

Using modifications inspired from Cython, let's convert this program to something like the one below (also change the file extension to `.pyx`),

```py
from array import array

cdef int n = 100000000
cdef object arr = array('d', [0.0]) * n
cdef double[:] mv = arr

cdef int i
for i in range(n):
    mv[i] = i % 3

print(arr[:10])
```

Execute it using,

```sh
pip install Cython
cythonize -b -i filename.pyx
python -c "import <filename>"
```

Time taken:
```sh
0.53s user 0.31s system 86% cpu 0.866 total
```

Holy smokes! That's almost 25 times faster than our non-cython code.

<p align="center" markdown="1">
![](https://media.giphy.com/media/3XR0chfiSTtAI/giphy.gif)
</p>

This happens because Cython creates a native binary that is linked at runtime. I've heard about a few use cases where it gives a speed up of almost 100x. If a few type declarations and avoiding Python's lucid syntax doesn't bother you much, this is a great option for computing heavy scripts.

### Named Tuple

This is one of the lesser known data structures in the `collections` module. Its working is explained in the snippet below.

```py
from collections import namedtuple
Candidate = namedtuple('Candidate', 'name age gender')
candidate_1 = Candidate(name='John Doe', age=35, gender='M')

# Output
# >>> candidate_1
# Candidate(name='John Doe', age=35, gender='M')
# >>> candidate_1.name
# 'John Doe'
# >>> candidate_1.age
# 35
# >>> candidate_1.gender
# 'M'
```

`namedtuple` seems like a great replacement for people who want a `struct` like functionality in Python and want to avoid classes in order to maintain basic records.

<br />
This wraps up the Part 1 of the "Python Booster Pack". I'll be on the hunt for more interesting stuff and hopefully there will be a Part 2. Until then, Adios!
