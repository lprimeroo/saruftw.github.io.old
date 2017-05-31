---
layout: post
title: GSoC 2017 Blog Series - Crypto module for Chapel
description: Logging the progress of my GSoC project.
author: Sarthak Munshi
category: logs
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

I'm quite thrilled this summer as I would be working with Cray's Chapel language under the Google Summer of Code program. My prerogative this summer would be to arm Chapel with its own Cryptography library which would efficiently utilize the parallelism Chapel offers. I would be using <u><a href="http://www.libtom.net/LibTomCrypt/">libtomcrypt</a></u> for this purpose.

I would be adding more content to this blog as my coding period progresses. Signing off the preface to this blog post with the link to my <u><a href="https://storage.googleapis.com/summerofcode-prod.appspot.com/gsoc/core_project/doc/4894007705468928_1491160173_GSoC17-Chapel-SarthakMunshi-ProjectProposal.pdf?Expires=1494501331&GoogleAccessId=summerofcode-prod%40appspot.gserviceaccount.com&Signature=WRkAxB9bc9O5R5%2ByjFUfoVzYi7bFuTOMXoJWfMBplT%2Fll1QSlmQmS8taR96sNq6QbP5Ksya5ZeuG5DwYbPad06%2BbKsQjfWf1%2Fl3o67KZxLxYBOtlaptL4Ah5ePzT6zP3S1Xkos2UuHHQym5eQUPTk5yEuxbhvaLtqDY4Ekjxwuq0XwTgn1abuPmJS3SLxNLZM%2FAtqbHdFu1S%2F8%2FvsT0RQLkL3vQJXMcJkUWEPpsAAxhqNtpQCgker7MhPdTfAJ0AWz2s5HsrspIlR%2BITeemSGW4C2R6Iy3CuysaPphqzuSzx5CaGZ0qZMXFOjobWqZFa9AipVfnuIy0uan6NUJj7lg%3D%3D">proposal</a></u>.

### Community Bonding Period

I worked on a couple of bug fixes and features during this period. This phase helped me to get a hang of how things behave in the Chapel ecosystem. Didn't do much in this period due my University examinations and contributed to a tiny feature.

* Worked on the Path module feature. [<a href="https://github.com/chapel-lang/chapel/pull/6200/files">PR</a>]
* Tested the c2chpl script on various native crypto libraries.

### Coding Period

