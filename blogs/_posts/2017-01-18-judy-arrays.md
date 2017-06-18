---
layout: post
title: Judy Array Gist
description: "What is a Judy Array? What does it do? How fast is it?"
author: Sarthak Munshi
tags: core-cs data-structures judy-arrays database
finished: true
---

While building a key-value for my university project, I came across this data structure called **Judy Arrays**. It was developed by Douglas Baskins while he was working at HP. Due to the dearth of academic research on this specific subject, Judy Arrays aren't quite as popular as other data structures. This article is my humble attempt to explain the complex data churning that goes underneath this data structure.

A simple Wikipedia lift will tell you that 

```
Judy array is a data structure implementing a type of associative array with high performance and low memory usage.
```

If at all it is an associative array, why on earth do we need another data structure doing the same job as AVL Trees, B-Trees or Skip lists? Let's find out.

Any living being who has written a few lines of code would be familiar with the traditional arrays which are available in most of the programming languages. A traditional array works exceptionally well due to its
1. fast lookup
2. fast insertion, and
3. fast deletion

All we need is the array's base address and an offset to access an element as all the values are stored contiguously. 

But what if a situation arises, wherein the data to be indexed is sparse in nature. The size of a traditional array is predetermined and cannot vary with sparseness in data. This is grossly memory inefficient. To better understand this, consider that your range of keys or **_Expanse_** is _1...100_. Thus, we create a traditional array of _size = 100_ so that it covers the entire expanse. Now, if we happen to fill just 10 locations in the array, it leads to a loss of 90 memory locations. Memory inefficient, isn't it? Although, a highly populated array may be an exception.

Enter Judy Arrays. In a nutshell, Judy Array is a **_trie_**. I suggest you read up about tries if you're not familiar with it. It is a very rudimentary and useful data structure and is also called a **_digital tree_** or a **_radix tree_**.


![Example of a Trie](http://odhyan.com/blog/wp-content/uploads/2010/11/trie-example.png)

Each node in a Judy Array has 256 branches. For a 32-bit expanse, that roughly translates to at most 4 lookups. Also, if the number of keys to be stored or **_population_** is quite small, Judy stores everything in the root. A well-implemented Judy Array also uses highly efficient node-compaction schemes that further reduce the size of each node. 

```
Judy Array does not use any hashing function and stores the keys in sorted order just like other tree implementations. 
```

## What makes it fast?

1. **Avoids cache-line fills :** A cache-line fill is the additional time required to do a read reference from RAM when a word is not found in cache. In today’s computers the time for a cache-line fill is in the range of _50..2000_ machine instructions. Therefore a cache-line fill should be avoided when fewer than 50 instructions can do the same job. Judy is designed to avoid cache-line fills wherever possible.

2. **Memory Efficient :** It's node-compaction and brach-compression schemes makes it well suited for semi-sequential sparse data. Some of the branch compression schemes are _linear_ and _bitmap_ but they are beyond the scope of this article. Also, Judy Arrays are left _uncompressed_ when the population is dense. 

3. **No tree balancing and dynamically growing :** Judy Array can grow or shrink as elements are added to, or removed from, the array. The memory used by Judy arrays is nearly proportional to the number of elements in the Judy array.

## When to use it?

Judy Arrays are suitable to use under the following conditions.

1. Large and sparse data sets.
2. Semi-sequential data sets.
3. Dense data sets.
4. General low latency requirement.

## Benchmarks

[Source] : <a href="https://rusty.ozlabs.org/?p=153">https://rusty.ozlabs.org/?p=153</a>

In a series of benchmarks conducted by Rusty Russell (linked above!) between Judy Arrays, Google's SparseHash and DenseHash, and DumbHash; the following results were obtained.

#### Task 1

Initial insertion of keys 0 to 49,999,999 in order, looking them up in order, looking up keys 50,000,000 to 99,999,999 in order.

![Task 1](http://oi65.tinypic.com/2j4vcb9.jpg)

#### Task 2

Delete everything and reinsert them all (in order).

![Task 2](http://oi67.tinypic.com/2z3xts3.jpg)

#### Task 3

Lookup random elements and access the elements. 

![Task 3](http://oi68.tinypic.com/fz6bti.jpg)


You can read further about these benchmarks @ <a href="https://rusty.ozlabs.org/?p=153">https://rusty.ozlabs.org/?p=153</a>
## Current Implementations

The original implementation of Judy Array was written by Douglas Baskins and his team at HP. This implementation is probably the fastest but sprawls across _20,000_ lines of code making it difficult to comprehend. However, it is available as a sweet API library. 

Link: <a href="http://sourceforge.net/projects/judy">http://sourceforge.net/projects/judy</a>

 Below is an example of the API this implementation provides.

```c
    Judy1  - maps an Index (word) to a bit
    JudyL  - maps an Index (word) to a Value (word/pointer)
    JudySL - maps an Index (null terminated string) to a Value
    JudyHS - maps an Index (array-of-bytes) of Length to a Value
```

Years later, a Google engineer, Karl Mcbrain rewrote Douglas's Judy Arrays within _1250_ lines of code. His implementation is quite popular within the FOSS community with multiple language-specific wrappers written around it. 

Link: <a href="https://code.google.com/archive/p/judyarray/">https://code.google.com/archive/p/judyarray/</a>


