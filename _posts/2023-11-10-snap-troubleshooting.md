---
layout: post
title: 'Troubleshooting and Understanding'
date: 2023-11-10
permalink: /blog/troubleshooting-understanding/
categories: MS
---

I had been struggling with the InSAR processing using SNAP for quite some time. There is always one cryptic message after another. The one that halted my progress was related to topographic phase removal. There was an issue with downloading the EGM96 model. I wondered why one requires a geoid model when I already have DEM for phase removal.

The issue with downloading the model was related to blocking the FTP link on IITK WiFi. I was so furious; such a trivial thing halted my progress, and I could not figure it out earlier. The issue was solved after placing the required EGM model in the necessary directory, Thanks to a helpful GitHub Issue response[1](https://github.com/johntruckenbrodt/pyroSAR/issues/98).
.

The answer is simple: InSAR works with ellipsoidal heights, and SRTM DEM (v3) is in WGS84 but with EGM08 as its datum. For the datum to be WGS84, one must add the geoid model, basics of ellipsoidal and orthometric heights. Then, I also found a dedicated function in GMTSAR[2](https://topex.ucsd.edu/gmtsar/demgen/) and custom scripts[3](https://github.com/insarwxw/run_download_DEM) on Git Hub for the same.

The one thing I learned is that troubleshooting requires patience. I had learned this before too, while debugging PySHBundle. But I forgot the lesson, going systematically; one step at a time, eliminating every possible known error source and iterating is the only solution to troubleshoot

Untill next time...




Helpful Links: <br>
[1] Github Issues - https://github.com/johntruckenbrodt/pyroSAR/issues/98 <br>
[2] GMTSAR DEM webpage - https://topex.ucsd.edu/gmtsar/demgen/ <br>
[3] Custon shell script - https://github.com/insarwxw/run_download_DEM <br>
