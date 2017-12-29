---
layout: post
title: Handy Download Script - Plant Village Dataset
description: A nifty little script that can be used to download the Plant Village dataset.
author: Sarthak Munshi
tags: machine-learning dataset plants plant-village scipt python
finished: true
---

Plant Village Images is an open access database of 80,000+ images of healthy and diseased crops. It is a tremendously generous effort by Penn State University and EPFL.
In order to download the images, you need to download 82 **csv** files segregated based on the plants with the diseases they are infected with. These files, then have the links to the actual images.
Well, the dataset is ~40GB in size. Imagine scraping 82 files where the total downloaded data amounts to ~40GB.
Moreover, the files are also infused with certain metadata which needs to be cleaned before you can scrape them.

You also need to sign up and create an account on the website for achieving all this. All this causes a little pain given the noble intentions behind the effort.
To make this dataset more accessible, I have pre-downloaded all the **csv** files, cleaned them off their metadata and written a tiny script to make scraping them much easier and interactive.

You can download the entire 1.7MB package <a href="https://drive.google.com/file/d/0BwrR3ZPLVYhkWmxSQTdHY3NPbU0/view">here</a>.

The package contains - **data_csv** and **download.py**. Run **download.py** from within the directory. Cheers.
