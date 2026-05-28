---
layout: post
title: 'Improving my TikZ skills'
date: 2026-05-28
permalink: /blog/tikz-challenge/
published: true
tags:
  - PhD
  - research
  - tools
  - LaTeX
---

I want to learn more about TikZ graphics package. I will be going throught the TikZ gallery and trying to recreate any one by myself. This should not take more than 1 hour daily (worst case).

Day 1 - Learning about help grid... this is better to visualize the grid, no more guessing the placement node

```latex
\begin{tikzpicture}
    % this is a help grid to know placement of other elememts
    \draw[help lines, style={gray!25}] grid (5, 5);
  
    % what's happening is a node at (0,0) with certain style
    % the '--' line towards another node defined using 
    % polar coordinates (angle, radius)

    \draw (0, 0) node[style={circle, fill=blue, minimum size=3pt, 
        inner sep=0pt}, label=below:{$(0,0)$}] {} -- (45:2) 
          
        node[style={circle, fill=blue, minimum size=3pt, inner sep=0pt}, label=$\pi/4$] {};
\end{tikzpicture}
```

The syntax will take some time to sink in.

Helpful content - 
1. [Unlocking LaTeX Graphics YouTube channel from Dr. Tamara Kolda](https://www.youtube.com/@UnlockingLaTeXGraphics)