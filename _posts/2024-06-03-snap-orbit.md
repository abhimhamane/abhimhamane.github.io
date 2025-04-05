---
layout: post
title: 'Orbit Corrections in SNAP'
date: 2024-06-03
categories: MS
permalink: /blog/orbit-correction-snap/
---

There's a pretty common issue while performing orbit correction in ESA-SNAP. There's a simple way to pre-download the required orbit files:
1. All the files one at a time (bulk)
2. As per the requirement.

The required orbit files need to be placed in appropriate folder `.snap/auxdata/Orbits/Sentinel-1/POEORB/S1A/2019/04/19`. 

**NOTE** - **`POEORB` stands for precise orbits, `RESORB` stands for restituted orbits. And depending on the satellite vehical `S1A` and `S1B` can be used. The files can be organized in the following structure - `year`, `month` and `day`. Individual files for each day can be placed in their respective folder.**


### 1. Bulk download (Orbit data till date) 

**NOTE** **Precise orbit data is delayed by around 2 weeks.**

The bulk data can be downloaded from the very well maintained GitHub repo by [Alexey Pechnikov (PyGMTSAR maintainer)](https://github.com/AlexeyPechnikov/S1orbits). Simply clone or download the repo and place the orbits folder at `.snap/auxdata/Orbits/Sentinel-1/POEORB/`.



### 2. Individual download 

Head to the `https://step.esa.int/auxdata/orbits/` in order to download the required orbits depending on your required scene.

One can follow the automated approach using Python scripts. A crude script can be found to bulk download, this can be simply modified to download specific files too.

```py
"""This script is to fix the issues with Sentinel-1 Precise Orbit download
issue faced in SNAP.
"""
import requests
from bs4 import BeautifulSoup
import wget
import os
from tqdm import tqdm

# get a list of all the updated 
url = "https://step.esa.int/auxdata/orbits/Sentinel-1/POEORB/S1A/"

response = requests.get(url)
soup = BeautifulSoup(response.content, "html.parser")
years_links = [link['href'] for link in soup.find_all('a', href=True)][1:]
print(years_links)

download_root = "../.snap/auxdata/Orbits/Sentinel-1/POEORB/S1A/"

for year in years_links:
    yearly_url = url + year
    yearly_response = requests.get(yearly_url)
    yearly_parser = BeautifulSoup(yearly_response.content, "html.parser")
    monthly_links = [link['href'] for link in yearly_parser.find_all('a', href=True)][1:]
    print(monthly_links)

    for month in tqdm(monthly_links):
        monthly_url = yearly_url + month
        monthly_response =  requests.get(monthly_url)
        monthly_parser = BeautifulSoup(monthly_response.content, "html.parser")
        orbit_links = [link['href'] for link in monthly_parser.find_all('a', href=True)]

        # structure of download folder as per year/month
        download_folder = download_root + year
        if os.path.exists(download_folder):
            pass
        else:
            os.mkdir(download_folder)
        
        download_folder = download_folder + month
        if os.path.exists(download_folder):
            pass
        else:
            os.mkdir(download_folder)
        
        # download the file
        os.chdir(download_folder)
        for link in tqdm(orbit_links):
            if link.endswith('.EOF.zip'):
                file_url = monthly_url + link
                filename = link.split('/')[-1]
                wget.download(file_url)
                print(f"Downloaded {filename}")
        break
    break
```

### References
- STEP forum - https://forum.step.esa.int/
- ChatGPT for web scrapping
- PyGMTSAR - https://github.com/AlexeyPechnikov/pygmtsar