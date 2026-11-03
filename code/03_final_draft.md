# Final draft
Leo P

``` r
library(ggplot2)
library(sf)
```

    Linking to GEOS 3.13.1, GDAL 3.11.4, PROJ 9.7.0; sf_use_s2() is TRUE

``` r
Low_Wint_Run <- st_read(
  "../data/LowerWintersRun_StreamData_v3.shp")
```

    Reading layer `LowerWintersRun_StreamData_v3' from data source 
      `C:\Users\Leope\Documents\midterm\data\LowerWintersRun_StreamData_v3.shp' 
      using driver `ESRI Shapefile'
    Simple feature collection with 832 features and 37 fields
    Geometry type: MULTILINESTRING
    Dimension:     XY
    Bounding box:  xmin: 1659562 ymin: 1995743 xmax: 1667206 ymax: 2007062
    Projected CRS: USA_Contiguous_Albers_Equal_Area_Conic_USGS_version

``` r
Low_Wint_Run_data <- Low_Wint_Run|>
  dplyr::select(ID, SinuosityI, PhysProv,Slope, strOrder, Avg_WD)

StreamData <- Low_Wint_Run_data |>
  dplyr::filter(SinuosityI <= 200)

StreamData$strOrder<-as.factor(StreamData$strOrder)
# 
```

``` r
ggplot(StreamData,aes(x=strOrder,y=Slope/100))+
         geom_boxplot(fill="lightblue", outlier.alpha =0.5 )+ 
  labs(x="Stream Order", y="Slope")+
  scale_y_continuous(labels = scales::label_percent())+
  labs(title="Channel Slope Distribution by Stream Order, Lower Winter's Run Watershed, MD",y="Channel Slope", subtitle = "Channel Slope = stream elevation change over horizontal length
Stream Order = hierarchical position of stream segment in drainage network", caption = "Data generated from 2025 Chesapeake watershed hyper-resolution hydrography and 1m DEM in ArcGIS Pro")+
  theme_minimal()+
  theme(plot.subtitle = element_text(size = 9),plot.title = element_text(size=13))
```

<img
src="03_final_draft_files/figure-commonmark/Boxplot%20of%20Channel%20Slope%20by%20Stream%20Order-1.png"
data-fig-alt="Boxplots of channel slope by stream order in the Lower Winter&#39;s Run watershed where the median channel slope decreases as stream order increases. Variation in channel slope is much greater in 1st order streams than other higher orders." />

``` r
StreamData_cats <- StreamData|>
  dplyr::select(PhysProv, strOrder)
StreamData_cats_shares <-StreamData_cats|>
    dplyr::group_by(PhysProv,strOrder) |>
    dplyr::summarise(n = dplyr::n())|>
    dplyr::mutate(share = n / sum(n))
```

    `summarise()` has regrouped the output.
    ℹ Summaries were computed grouped by PhysProv and strOrder.
    ℹ Output is grouped by PhysProv.
    ℹ Use `summarise(.groups = "drop_last")` to silence this message.
    ℹ Use `summarise(.by = c(PhysProv, strOrder))` for per-operation grouping
      (`?dplyr::dplyr_by`) instead.

``` r
StreamData_cats_shares$share <- StreamData_cats_shares$share*100

StreamData_cats_shares$share <- round(StreamData_cats_shares$share,2)

StreamData_cats_shares$share <- paste0(StreamData_cats_shares$share, "%")


knitr::kable(
    StreamData_cats_shares|>
      sf::st_drop_geometry()|>
      dplyr::select(PhysProv,strOrder,share),
    align = "lcr",
    col.names = c(
        "Phys. Province &nbsp;",
        "Stream Ord.&nbsp;","Share"),
    caption = "Share of Stream Segments by Physiographic Province and Stream Order, Lower Winter's Run Watershed, MD",
    digits = 2)
```

| Phys. Province   | Stream Ord.  |  Share |
|:-----------------|:------------:|-------:|
| CoastalPlain     |      1       |  48.2% |
| CoastalPlain     |      2       | 21.17% |
| CoastalPlain     |      3       | 17.57% |
| CoastalPlain     |      4       |  1.35% |
| CoastalPlain     |      5       | 11.71% |
| Piedmont         |      1       | 50.57% |
| Piedmont         |      2       | 20.36% |
| Piedmont         |      3       | 15.27% |
| Piedmont         |      4       |  4.27% |
| Piedmont         |      5       |  9.52% |

Share of Stream Segments by Physiographic Province and Stream Order,
Lower Winter’s Run Watershed, MD

``` r
ggplot(StreamData,aes(x=Slope/100,y=SinuosityI))+
  geom_point(aes(colour=strOrder))+
  geom_smooth(colour="blue")+
  scale_colour_manual(values=rcartocolor::carto_pal(5,name ="SunsetDark"))+
  labs(y="Sinuosity Index",x="Slope",title = "Slope, Sinuosity, and Stream Order in the Lower Winters Run Watershed, MD", colour="Stream Order",caption = "Data generated from 2025 Chesapeake watershed hyper-resolution hydrography and 1m DEM in ArcGIS Pro",subtitle = "Channel Slope = stream elevation change over horizontal length
Stream Order = hierarchical position of stream segment in drainage network")+
  scale_x_continuous(labels = scales::label_percent())+
  theme_classic()+
  theme(plot.subtitle = element_text(size = 10),plot.title = element_text(size=14, legend.title=element_text(size=6),axis.title=element_text(size = 9)))
```

    Warning in element_text(size = 14, legend.title = element_text(size = 6), : `...` must be empty.
    ✖ Problematic arguments:
    • legend.title = element_text(size = 6)
    • axis.title = element_text(size = 9)

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

<img
src="03_final_draft_files/figure-commonmark/Scatterplot%20of%20channel%20slope%20vs.%20sinuosity%20index%20with%20points%20colored%20by%20stream%20order-1.png"
data-fig-alt="Scatterplot of channel slope vs. sinuosity index with points colored by stream order showing that higher order streams generally have a low channel slope and moderate sinuosity while 1st order streams generally have much higher channel slopes and a wide range of variation in channel slope and sinuosity when compared to other stream orders." />

``` r
ggplot(StreamData,aes(x=PhysProv,y=Avg_WD,fill=PhysProv))+
         geom_boxplot(outlier.alpha =0.5 )+ 
  scale_fill_brewer(palette="Dark2")+
  labs(x="Physiographic Province", y="Average W/D ratio")+
  labs(title="W/D Ratio Distribution by Physiographic Province, Lower Winter's Run Watershed, MD",y="W/D Ratio",x=NULL, subtitle = "Average W/D Ratio = average channel width divided by average channel depth in a stream segment, greater = a wider+shallower channel. 
Physiographic Province = region with similar geologic and geomorphic properties.", caption = "Data generated from 2025 Chesapeake watershed hyper-resolution hydrography and 1m DEM in ArcGIS Pro. Physiographic Data retrieved from USGS")+
  theme_minimal()+
  theme(plot.subtitle = element_text(size = 8),plot.title = element_text(size=12),plot.caption = element_text(size=7),legend.position = "none")
```

<img
src="03_final_draft_files/figure-commonmark/Boxplot%20of%20W/D%20ratio%20by%20physiographic%20province-1.png"
data-fig-alt="Boxplot of Channel Slope by Stream Order" />
