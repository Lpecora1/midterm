---
title: Technical notes
author: Leo P
date: 2026-11-02
---

(This doesn't have to be the structure you use, just some guidelines)

## Data

-Removed data point with extreme high outlier sinuosity index, likely due to some error in data
-do sequential color palette for stream order
-explore relationships between variables within subsets of the data based in stream order and phys province, ideal for faceting but needed more data points for some categories
-rounded shares of streams for better representation as percentages
-ensure that stream order variable is a factor/category data type for proper encoding in ggplot, particularly boxplots, use as.factor

## Code

-calculated share of stream segments (data points) by stream order and physiographic province using dplyr::group_by method
-used theme_minimal often to reduce visual clutter in plots without having to set many individual theme arguments
-kableExtra package extends customizability of kable for maming tables but has trouble with rendering as a markdown document
-use sf's st_read function to read in shapefiles
-when setting path to directory within same repo as working directory repo to read in data use ".." to indicate proper directory i.e.: "../data/LowerWintersRun_StreamData_v3.shp", see hint on Slack and refrence directory layout
-use ggsave to save pngs of plots but be sure to route to correct path and don't accidentally create new folder, i.e. ggsave(
  filename = "plots/pecora-slope-by-stream-ord-boxplot.png",
  plot = last_plot())
-be careful to not combine code chunks

## Potential improvements / possible next steps

-Expand analysis to entire Gunpowder-Patapsco or Chesapeake Bay watersheds
-Compare relationships of variables between different sub watersheds
-Expand analysis to include additional physiographic provinces
-Expand analysis to include additional variables representing stream geomorphology such as valley confinement
-Incorporate biotic measures of stream health into the analysis

## Other notes

-be sure to use help() to learn about packages and functions
-legend for categories not always necessary, especially when few categories (i.e. only 2 phys provinces)
