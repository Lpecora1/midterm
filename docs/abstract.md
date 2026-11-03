---
title: Project overview
author: Your name here
date: 2026-10-01
---

This is the midterm project from my graduate course in data visualization at
UMBC. For this project, I analyzed stream geomorphology data from the Lower Winter's Run watershed drainage
network in Maryland. Each data point was a stream segment. Categorical variables were physiographic province and
stream order. Numeric variables were channel slope, sinuosity index, and average channel width/depth ratio.
Slope, sinuosity,stream order, and width/depth data were generated from the 2025 Chesapeake watershed hyper-resolution
hydrography dataset and a 1m DEM. Physiographic data, the hydrography, and DEM were retrieved from USGS.
This analysis in Lower Winter's Run demonstrated that stream geomorphology as measured by channel slope, sinuosity, and
channel width/depth, is greatly influenced by a stream's geographic setting, as represented by physiographic
province and stream order. First order streams were more numerous, exhibited higher slopes, and greater variability in
geomorphic variables when compared to higher order streams, showing that first order streams are geomorphically diverse and likely
more dynamic systems.

Reducing unnecessary text and visual clutter are key aspects of producing figures that are legible and appealing to the audience.
Catering to your audience, in this case a scientific audience, through appropriate language and style are also important considerations.
I found myself comparing different ways to represent the same data graphically, particularly when I was deciding between a single scatterplot
encoded by category with color and using facets of multiple scatterplots subset by that same category. I chose the single scatterplot because
of its less cluttered visuals and efficiency in displaying the same data with less visual information. Displaying the data and communicating its
story in the most visually efficient way is a major takeaway from class readings that I applied here. I also did this my omitting unnecessary legends, labels,
and extra information when possible and by using concise text. Using visual contrast, which I achieved with dark colors on light backgrounds, is
also key to enhancing figure readability. Knowing your data through EDA prior to visualization is an important step as well. I identified outliers, 
likely from measurement error, to remove that altered the visualization of my data. All of these decisions I had to make reinforced the idea that
many of the readings emphasized: the author of a visualization has many choices to make that affect their final output and what is communicated to the audience.
This is why ethics and planning are so important in data visualization.
