---
layout: post
title: Handy Download Script - Plant Village Dataset
description: A nifty little script that can be used to download the Plant Village dataset.
author: Sarthak Munshi
category: tutorials
tags: machine-learning dataset plants plant-village scipt python
finished: true
---

Plant Village Images is an open access database of 50,000+ images of healthy and diseased crops. It is a tremendously generous effort by Penn State University and EPFL. 
In order to download the images, you need to download 82 `csv` files segregated based on the plants with the diseases they are infected with. These `csv` files, then have the links to the actual images. 
Well, the dataset is ~40GB in size. Imagine scraping 82 `csv` files where the total downloaded data amounts to ~40GB.

Moreover, to download the `csv` files, you need to sign up and log in on their website. All this causes a bit of pain when the effort and the dataset is so fantastic.
To make this dataset more accessible, I have pre-downloaded all the `csv` files and written a tiny little script to make scraping them for image downloads much more easier and interactive.

You can download the entire 1.7MB package <a href="https://drive.google.com/file/d/0BwrR3ZPLVYhkWmxSQTdHY3NPbU0/view">here</a>.

The package contains - `data_csv` and `download.py`.

The script is mentioned below for reference purposes.

```py
import pandas as pd
from tqdm import *
import urllib
import os

class PlantVillage:
	def __init__(self):
		self.plantwise_diseases = {
			'apple': ['healthy', 'apple_scab', 'cedar_apple_rust', 'black_rot'],
			'banana': ['healthy', 'black_sigatoka_black_leaf_streak', 'banana_speckle'],
			'blueberry': ['healthy'],
			'cabbage_red_white_savoy': ['healthy', 'black_rot'],
			'cantaloupe': ['healthy'],
			'cassava_manioc': ['brown_leaf_spot', 'cassava_green_spider_mite'],
			'celery': ['early_blight_cercospora_leaf_spot_cercospora_blight'],
			'cherry_including_sour': ['healthy', 'powdery_mildew'],
			'corn_maize': ['healthy', 'cercospora_leaf_spot_gray_leaf_spot', 'common_rust', 'northern_leaf_blight'],
			'cucumber': ['healthy', 'downy_mildew'],
			'eggplant': ['healthy'],
			'gourd': ['downy_mildew'],
			'grape': ['healthy', 'black_rot', 'esca_(black_measles_or_spanish_measles)', 'leaf_blight_(isariopsis_leaf_spot)'],
			'onion': ['healthy'],
			'orange': ['huanglongbing_(citrus_greening)'],
			'peach': ['healthy', 'bacterial_spot'],
			'pepper_bell': ['healthy', 'bacterial_spot'],
			'potato': ['healthy', 'late_blight', 'early_blight'],
			'pumpkin': ['cucumber_mosaic'],
			'raspberry': ['healthy'],
			'soybean': ['healthy', 'downy_mildew', 'frogeye_leaf_spot', 'septoria_leaf_blight'],
			'squash': ['healthy', 'powdery_mildew'],
			'strawberry': ['healthy', 'leaf_scorch'],
			'tomato': ['healthy', 'bacterial_spot', 'early_blight', 'late_blight', 'septoria_leaf_spot', 'tomato_mosaic_virus', 'leaf_mold', 'target_spot', 'tomato_yellow_leaf_curl_virus'],
			'watermelon': ['healthy']
		}
		self.choice = ''

	def cli(self):
		print "============= Plant Village Dataset Download Script ================"
		print "1. Selective Download"
		print "2. Complete Download"
		first_choice = raw_input("Enter input: ")
		if first_choice == '1':
			self.cli_stage_two()
		elif first_choice == '2':
			self.get_entire_dataset()
		else:
			print "Incorrect input. Try running the script again."

	def cli_stage_two(self):
		print "============ Select Plant =============="
		index = 1
		for key in sorted(self.plantwise_diseases):
			print str(index) + '. ' + key
			index += 1
		second_choice = raw_input("Enter input: ")

		if int(second_choice) > index - 1 or int(second_choice) <= 0:
			print "Incorrect input. Try running the script again."
			return
		self.cli_stage_three(sorted(self.plantwise_diseases)[int(second_choice) - 1])

	def cli_stage_three(self, plant_name):
		print "============ Select Plant Specific Diseases =============="
		index = 1
		for diseases in self.plantwise_diseases[plant_name]:
			print str(index) + '. ' + diseases
			index += 1
		print str(index) + '. ALL OF THE ABOVE'
		third_choice = raw_input("Enter input: ")
		if int(third_choice) > index - 1 or int(third_choice) <= 0:
			print "Incorrect input. Try running the script again."
			return
		file_name = ''
		if int(third_choice) == index:
			file_name = plant_name + '_images.csv'
		else:
			file_name = plant_name + '_' + self.plantwise_diseases[plant_name][int(third_choice) - 1] + '_images.csv'
		self.get_plant_data(file_name)

	def get_plant_data(self, file_name):
		df = pd.read_csv('data_csv/' + file_name)
		folder_name = file_name.split('.')[0]
		os.makedirs(folder_name)
		for index, url in enumerate(tqdm(df['url'])):
			urllib.urlretrieve(url, folder_name + '/' + str(index) + '.jpg')

	def get_entire_dataset(self):
		print '=========== Downloading the entire dataset =============='
		files = [f for f in os.listdir('data_csv') if os.path.isfile('data_csv/' + f) and f.split('.')[1] == 'csv']
		print 'Entire Progress'
		for file_name in tqdm(files):
			df = pd.read_csv('data_csv/' + file_name)
			folder_name = file_name.split('.')[0]
			os.makedirs(folder_name)
			print '\n {} Progress'.format(folder_name)
			for index, url in enumerate(tqdm(df['url'])):
				urllib.urlretrieve(url, folder_name + '/' + str(index) + '.jpg')

if __name__ == '__main__':
	data = PlantVillage()
	data.cli()
```


To use this package:
1. Download and extract the zip file.
2. `cd PlantVillageScript`
3. `pip install tqdm pandas`
4. `python download.py`
