---
layout: post
title: 'Colour pallets and scientific plotting'
date: 2026-02-08
permalink: /blog/scientific-colour-pallets/
published: false
tags:
  - PhD
  - research
  - tools
---

I envy seeing creative plots in papers. The starting point to making such plots/figures/visualizations is to have a idea which can be expressed in a creative way. The journey of developing that idea is a completely different art/process but the process of expressing it can be learnt and it's better to learn it early on. It's like learning the syntax of a programming language. The most complex and elegant of all softwares were built using the same syntax.

One of the key points when it comes to visualization/figures in my field of Geodesy is of using appropriate colour pallets. There are two choices (i) default to your programing language or plotting library and (ii) a delibrate and informed choice. Like the road less traveled by, the second one makes all the difference.

I did not learn this automatically, I started reading about it sincerely after meeting with my supervisor and showing him my initial figures. He said there should be intent anc choice in the making figures. Somewhere inside I knew that, I had made those figures in haste burning the midnight oil (literally).

This is one of those things, if you start seing it once then you start finding it everywhere. After that interaction, I spent sometime reading and learning about use of colourmaps. It's complex just a small decision, it's like the 80-20 rule. A small delibrate decision can improve the quality of figure drastically. Things to remember

1. Do not use the default colourmaps (eg. jet in matlab/python)
2. Four class of colour maps - (i) Diverging, (ii) multi-sequential, (iii) cyclic and (iv) sequential
3. Three types of colour maps - (i) categorical, (ii) discrete and (iii) continuous

For most part, the names and organization is self explainatory. Please check the article in Nature Comminications by Fabio Carmerie, Grace Shephard and Philip Heron for more information. They also provide a convinent flowchart for determining the appropriate colourmap choice.


I present some improvements that I got in figures I have seen.

1. a standard spherical harmonics triangle plot

2. a standard global field plot





This is an art and will have to go through many iterations. This is just how to get something that looks intential and not default plot. One can take inspiration from observing how others do it, especially from other discplines. For instance, reading up on cartographic principles, or graphics design principles may be reqarding.

Next step...

1. Colour maps and colour pallets are just one aspect that's involved in making a good plot that communicates the data/finding

2. There are other aspects - both technical and asthetic which can help improve a figure/graph/visualization.

Having an idea that can use these basic principles to elevate the presentation and ultimately communicate the idea effectively.