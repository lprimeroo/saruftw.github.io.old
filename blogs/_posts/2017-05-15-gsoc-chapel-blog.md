---
layout: post
title: GSoC 2017 Blog Series - Crypto module for Chapel
description: Logging the progress of my GSoC project.
author: Sarthak Munshi
tags: summer-of-code gsoc chapel crypto module google
finished: true
---

<p align="center">
  <img alt="Google Summer of Code" src="https://musescore.org/sites/musescore.org/files/Capture%20d%27e%CC%81cran%202016-03-01%2009.48.11.png"/>
</p>

<p align="center">
  <img alt="Chapel" src="https://upload.wikimedia.org/wikipedia/en/c/c0/Cray_Chapel_Logo.png"/>
</p>
<br />

I'm quite thrilled this summer as I would be working with Cray's Chapel language under the Google Summer of Code program. My prerogative this summer would be to arm Chapel with its own Cryptography library which would efficiently utilize the parallelism Chapel offers. I would be using <u><a href="https://www.openssl.org">OpenSSL</a></u> for this purpose.

I would be adding more detailed content to this blog as my coding period progresses. Signing off the preface to this blog post with the link to my <u><a href="https://drive.google.com/file/d/0BwrR3ZPLVYhkNUpMcU9jMUQ0VE0/view?usp=sharing">proposal</a></u>.

### Community Bonding Period

I worked on a couple of bug fixes and features during this period. This phase helped me to get a hang of how things behave in the Chapel ecosystem. Didn't do much in this period due my University examinations and contributed to a tiny feature.

* Worked on the Path module feature. [<a href="https://github.com/chapel-lang/chapel/pull/6200/files">PR</a>]
* Tested the <a href="https://github.com/chapel-lang/chapel/tree/master/tools/c2chapel">c2chapel</a> script on various native crypto libraries.
* Prepared a design CHIP. [<a href="https://github.com/chapel-lang/chapel/blob/master/doc/rst/developer/chips/21.rst">PR</a>]

### Coding Period

* Starting creating foreign function interfaces over OpenSSL's EVP API.
* Reported a compiler bug. [<a href="https://github.com/chapel-lang/chapel/issues/6483">Issue</a>]
