---
title: "Procedural City Generation"
description: "I worked with fellow CS major Alex Bullard to develop a program to procedurally generate cities. We researched various topics in computational geometry and computer graphics to make generated cities realistic and interesting."
layout: projects
order: 12
permalink: /projects/procedural-city-generation/
---

We developed an application for procedurally generating the commercial districts of cities.

![voronoi diagram](voronoi.png)
We use Voronoi diagrams with randomly placed control points to generate the main roads and city districts.

![city blocks](blocks.png)
These districts are further subdivided using regularly spaced control points to create city blocks and lots.

![buildings](buildings.png)
We generate buildings with a number of randomized parameters including building height, size, window width, height and density. We control average building height with a two dimensional Gaussian function that produces buildings with higher heights in specified downtown regions.

![final city](final.png)
Finally, pre-generated city elements like stop signs and stoplights are added. Our application produces fully explorable cities using OpenGL with realistic layouts and buildings.
