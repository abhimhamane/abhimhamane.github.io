---
layout: post
title: 'Closer look at the Altimetric Datasets'
date: 2024-06-21
permalink: /blog/closer-look-altimetry/
tags:
  - IITK
  - Thesis
  - Altimetry
---

Let's take a look at different datasets with help of some Visualizations. The two datasets used are - 
1. AVISO GDR 
2. ALES OpenADB coastal retracked from Technical University of Munich (TUM)

These are nothing new or noval ways to visualize. This might even seem very trivial to many, but this is my effort to document my learnings and develop as a researcher.

### 1. Data Availability (Missing Cycles) across cycles and tracks

The red represents missing cycles whereas blue represents data availability.

![envisat available](/images/altimetry-MSR-thesis/envisat-data-availability.png)

![Jason-1 available](/images/altimetry-MSR-thesis/jason-1data-availability.png)

SARAL ALES OpenADB data
![Saral available](/images/altimetry-MSR-thesis/saral-data-availability.png)

### 2. Waveform contamination and distance to coast

The waveforms were retrieved from the Sensor record from AVISO.

<img src="/images/altimetry-MSR-thesis/altimetry_waveform_different_regions.svg" alt="Waveform contamination" width="400"/>


In the standard AVISO GDR data processing the coastal regions are masked. Whereas the Sea Surface Height (SSH) info can be retrieved to certain extent using coastal retracking algorithms. Precisely can be observed in the ALES OpenADB data.

<img src="/images/altimetry-MSR-thesis/aviso-ales-distance2coast.png" alt="Waveform contamination" width="250"/>


### 3. Spatial and Temporal resolution tradeoff

The tracks of SARAL are more closely spaced compared to the Jason-2 tracks. SARAL has a repeat cycle of ~35 days whereas Jason-2 has of ~11 days.

<img src="/images/altimetry-MSR-thesis/tracks.png" alt="Waveform contamination" width="250"/>


### 4. Repeat Passes

The repeat passes do not allign exactly, which is expected due to orbital peturbations and other practical limitaion involved with satellite geodesy.

<img src="/images/altimetry-MSR-thesis/changes-cycles-tracks.png" alt="Waveform contamination" width="550"/>


### 5. Closer look at the repeat passes

SARAL mission has pretty scattered accquisation points, scattered both along the accending and descending tracks.

<img src="/images/altimetry-MSR-thesis/saral-cycles-tracks.png" alt="Waveform contamination" width="350"/>

Curiously, for Jason-2 the accqisations are pretty well clustered along descending track and not so well along the ascending track. I could not find any references which explained this observations.

<img src="/images/altimetry-MSR-thesis/jason-cycles-cluster.png" alt="Waveform contamination" width="350"/>

### 6. Let's find the Cross-Over points

The cross-over was identified by determining the minimum distance b/w pairs of ascending and descending track accquisations over a small window.

<img src="/images/altimetry-MSR-thesis/track-intersection-1.png" alt="Waveform contamination" width="350"/>

The traingle represent the two cross-over points, not exactly coinciding with each other.

<img src="/images/altimetry-MSR-thesis/track-intersection-2.png" alt="Waveform contamination" width="350"/>

Following the same approach for other cycles we get the following variations in cross-over points.

<img src="/images/altimetry-MSR-thesis/track-intersection-3.png" alt="Waveform contamination" width="350"/>