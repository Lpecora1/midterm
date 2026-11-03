# EDA
Leo P

``` r
rm(list = ls()) #clears environment
cat("\014") #clears console
```



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

#should get data in R workspace / git, not off local directory
```

``` r
#summary and histograms


summary(Low_Wint_Run_data)
```

           ID          SinuosityI         PhysProv             Slope        
     Min.   :  1.0   Min.   :  0.0000   Length:832         Min.   : 0.0000  
     1st Qu.:208.8   1st Qu.:  0.9221   Class :character   1st Qu.: 0.9222  
     Median :416.5   Median :  1.0313   Mode  :character   Median : 2.1739  
     Mean   :416.5   Mean   :  1.2867                      Mean   : 2.8989  
     3rd Qu.:624.2   3rd Qu.:  1.1249                      3rd Qu.: 4.0484  
     Max.   :832.0   Max.   :228.9777                      Max.   :18.0056  
        strOrder         Avg_WD                  geometry  
     Min.   :1.000   Min.   : 0.000   MULTILINESTRING:832  
     1st Qu.:1.000   1st Qu.: 2.453   epsg:NA        :  0  
     Median :1.500   Median : 3.471   +proj=aea ...  :  0  
     Mean   :2.031   Mean   : 3.638                        
     3rd Qu.:3.000   3rd Qu.: 4.788                        
     Max.   :5.000   Max.   :15.943                        

``` r
ggplot(Low_Wint_Run_data,aes(x=strOrder))+
         geom_histogram()
```

    `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

![](01_eda_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
#right skewed histogram for stream order, many more 1st order streams

ggplot(Low_Wint_Run_data,aes(x=Slope))+
         geom_histogram()
```

    `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

![](01_eda_files/figure-commonmark/unnamed-chunk-1-2.png)

``` r
#right skewed histogram for slope

ggplot(Low_Wint_Run_data,aes(x=SinuosityI))+
         geom_histogram(bins = 100)
```

![](01_eda_files/figure-commonmark/unnamed-chunk-1-3.png)

``` r
#need to remove sinuosity index outlier

#getting rid of outlier here, probably is an error, value is way out of expected range
StreamData <- Low_Wint_Run_data |>
  dplyr::filter(SinuosityI <= 200)

ggplot(StreamData,aes(x=SinuosityI))+
         geom_histogram(bins = 100)
```

![](01_eda_files/figure-commonmark/unnamed-chunk-1-4.png)

``` r
#sinuosity is normally distributed


ggplot(StreamData,aes(x=Avg_WD))+
         geom_histogram()
```

    `stat_bin()` using `bins = 30`. Pick better value `binwidth`.

![](01_eda_files/figure-commonmark/unnamed-chunk-1-5.png)

``` r
#average channel width to depth ratio is also right skewed
```

``` r
#scatterplots


ggplot(StreamData,aes(x=Avg_WD,y=SinuosityI))+
  geom_point()+
  geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](01_eda_files/figure-commonmark/unnamed-chunk-2-1.png)

``` r
ggplot(StreamData,aes(x=Avg_WD,y=Slope))+
  geom_point()+
  geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](01_eda_files/figure-commonmark/unnamed-chunk-2-2.png)

``` r
#seems like a negative curvilinear trend between avg_wd and slope


ggplot(StreamData,aes(x=Slope,y=SinuosityI))+
  geom_point()+
  geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](01_eda_files/figure-commonmark/unnamed-chunk-2-3.png)

``` r
#no strong relationship between slope and sinuosity, I thought there would be a negative relationship

ggplot(StreamData,aes(x=Slope,y=SinuosityI))+
  geom_point(aes(colour=PhysProv))+
  geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](01_eda_files/figure-commonmark/unnamed-chunk-2-4.png)

``` r
#categorizing by phys prov adds more info, larger slopes in piedmont streams


ggplot(StreamData,aes(x=Slope,y=SinuosityI))+
  geom_point(aes(colour=strOrder))+
  geom_smooth()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](01_eda_files/figure-commonmark/unnamed-chunk-2-5.png)

``` r
#this is an interesting graph, higher order streams clustered near 0, much more variation in 1st order streams, still thinking about what this means

#boxplots of slope and sinuosity by stream order and phys province may show variation better
```

``` r
#boxplots


ggplot(StreamData,aes(x=PhysProv,y=Avg_WD))+
         geom_boxplot()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-3-1.png)

``` r
ggplot(StreamData,aes(x=PhysProv,y=Slope))+
         geom_boxplot()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-3-2.png)

``` r
#greater variation in slope among piedmont streams, greater max, and more right skewed (higher slopes) - this follows theory

ggplot(StreamData,aes(x=PhysProv,y=SinuosityI))+
         geom_boxplot()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-3-3.png)

``` r
#sinuosity similarly distributed between phys provinces

ggplot(StreamData,aes(x=strOrder,y=SinuosityI))+
         geom_boxplot()
```

    Warning: Orientation is not uniquely specified when both the x and y aesthetics are
    continuous. Picking default orientation 'x'.

    Warning: Continuous x aesthetic
    ℹ did you forget `aes(group = ...)`?

![](01_eda_files/figure-commonmark/unnamed-chunk-3-4.png)

``` r
#need to transform strOrder from numerical to categorical variable
#looked up on stack
StreamData$strOrder<-as.factor(StreamData$strOrder)

ggplot(StreamData,aes(x=strOrder,y=SinuosityI))+
         geom_boxplot()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-3-5.png)

``` r
#sinuosity slightly lower usually in 1st order streams and 1st order streams have the most low sinuosity (low value SI) segments, this follows theory. (SI = Sinuosity Index)
#distribution of SI in other order streams is similar

ggplot(StreamData,aes(x=strOrder,y=Slope))+
         geom_boxplot()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-3-6.png)

``` r
#gradually decreasing slopes as stream order gets higher. 1st order has the most variation and highest slopes. Highest order streams have lowest order slopes, this makes sense.

ggplot(StreamData,aes(x=strOrder,y=Avg_WD))+
         geom_boxplot()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-3-7.png)

``` r
#calculate % of stream segments that are each stream order and each phys province

StreamData_CatCts<-StreamData|>
  dplyr::count(PhysProv,strOrder)



ggplot(StreamData_CatCts,aes(x = PhysProv, y = strOrder)) +
    geom_point(aes(size = n)) +
    scale_size_area()
```

![](01_eda_files/figure-commonmark/unnamed-chunk-4-1.png)

``` r
#need percents, not raw count since majority of samples are piedmont
#I think dropping the geometry field 

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
ggplot(StreamData_cats_shares, aes(x = PhysProv, y = strOrder, fill = share)) +
    geom_tile() +
    rcartocolor::scale_fill_carto_c(palette = "PurpOr")
```

![](01_eda_files/figure-commonmark/unnamed-chunk-4-2.png)

``` r
#this might just be better as a table

#intend to use the following table, had to look up how to drop geometry field on google and used chatgpt to get the symbols right for my pipe 
knitr::kable(
    StreamData_cats_shares|>
      sf::st_drop_geometry()|>
      dplyr::select(PhysProv,strOrder,share),
    align = "lrr",
    col.names = c(
        "PhysProv",
        "strOrder",
        "share"
        
    ),
    caption = "Socioeconomic indicators, western MD counties, 2024"
)
```

| PhysProv     | strOrder |     share |
|:-------------|---------:|----------:|
| CoastalPlain |        1 | 0.4819820 |
| CoastalPlain |        2 | 0.2117117 |
| CoastalPlain |        3 | 0.1756757 |
| CoastalPlain |        4 | 0.0135135 |
| CoastalPlain |        5 | 0.1171171 |
| Piedmont     |        1 | 0.5057471 |
| Piedmont     |        2 | 0.2036125 |
| Piedmont     |        3 | 0.1527094 |
| Piedmont     |        4 | 0.0426929 |
| Piedmont     |        5 | 0.0952381 |

Socioeconomic indicators, western MD counties, 2024

``` r
#try heat map or stacked bars or bubbles, waffle?


ggplot(StreamData, aes(x = PhysProv, y = strOrder, fill = Slope)) +
    geom_tile() +
    rcartocolor::scale_fill_carto_c(palette = "PurpOr")
```

![](01_eda_files/figure-commonmark/unnamed-chunk-4-3.png)

``` r
ggplot(StreamData, aes(x = PhysProv, y = strOrder, fill = SinuosityI)) +
    geom_tile() +
    rcartocolor::scale_fill_carto_c(palette = "PurpOr")
```

![](01_eda_files/figure-commonmark/unnamed-chunk-4-4.png)

``` r
#tiles not really looking good, table probably better
#should I bin sinuosity and slope into categories?
```

# going to show how stream order and slope are related factors, how 1st order streams are much more numerous, and how first order streams have generally more geomorphic variation (sinuosity and slope)
