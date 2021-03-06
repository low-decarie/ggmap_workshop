<style>
.small-code pre code {
  font-size: 1em;
}
</style>


ggmap
========================================================
author: Etienne Low-Décarie



Creating maps
===
class: small-code


```r
if(!require(ggmap)) {install.packages("ggmap")}
require(ggmap)
if(!require(maptools)) {install.packages("maptools")}
require(maptools)
```

Basic world map
===
class: small-code


```r
map <- ggplot()+borders("world", 
                        colour="gray50",
                        fill="gray50")
print(map)
```

![plot of chunk unnamed-chunk-2](ggmap-figure/unnamed-chunk-2-1.png)

Resource
===

Cheatsheet:
https://www.nceas.ucsb.edu/~frazier/RSpatialGuides/ggmap/ggmapCheatsheet.pdf

<iframe src="https://www.nceas.ucsb.edu/~frazier/RSpatialGuides/ggmap/ggmapCheatsheet.pdf" width="1000" height="800">
  <p>Your browser does not support iframes.</p>
</iframe>


Pretty world map
===

bounding box lowerleftlon, lowerleftlat, upperrightlon, upperrightlat (a little glitchy for google maps)


```r
myLocation <- with(coord, c(min(LON),
                            min(LAT), 
                            max(LON), 
                            max(LAT)))
```


```r
myLocation <- c(-179,-89,179,89)
myMap <- get_map(location=myLocation, 
                 source="stamen", 
                 maptype="watercolor",
                 crop=FALSE) 

myMap <- ggmap(myMap)
print(myMap)
```

![plot of chunk unnamed-chunk-4](ggmap-figure/unnamed-chunk-4-1.png)


Load geo-referenced coral bleaching data
===

Coral bleaching data from ReefBase http://www.reefbase.org

```r
coord <- read.csv("./Data/CoralBleaching_cleaned.csv")
```

Extract bleaching events


```r
coord <- coord[coord$BLEACHING_SEVERITY %in% c("Low","Medium","HIGH"),]
coord$BLEACHING_SEVERITY <- factor(coord$BLEACHING_SEVERITY, levels=levels(coord$BLEACHING_SEVERITY)[c(2,3,1)])
```


Add the data to the map
===


```r
map <- map+geom_point(data=coord,aes(x=LON,
                                     y=LAT,
                                     colour=BLEACHING_SEVERITY),
                      alpha=0.5)
print(map)
```

![plot of chunk unnamed-chunk-7](ggmap-figure/unnamed-chunk-7-1.png)

Fine tuning: Scales
===


```r
map <- map+
  scale_colour_manual(values = c("Low"="yellow",
                                 "Medium"="orange",
                                 "HIGH"="red"))+
  guides(colour = guide_legend(override.aes = list(alpha = 1))) #+
  #facet_grid(YEAR~.)

print(map)
```

![plot of chunk unnamed-chunk-8](ggmap-figure/unnamed-chunk-8-1.png)

Fine tuning: Facets
===


```r
print(map+facet_wrap(~YEAR))
```

![plot of chunk unnamed-chunk-9](ggmap-figure/unnamed-chunk-9-1.png)


Exercise 1
===

Create a coordinate file with the location of your house and the location of the house of your friends

Produce maps from the data in 
Henry P, Le Lay G, Goudet J, Guisan A, Jahodová S, Besnard G (2009) Reduced genetic diversity, increased isolation and multiple introductions of invasive giant hogweed in the western Swiss Alps. Molecular Ecology 18: 2819-2831. 
[paper](http://dx.doi.org/10.1111/j.1365-294X.2009.04237.x)
[data](http://datadryad.org/resource/doi:10.5061/dryad.1237)


<div class="centered">

<script src="countdown.js" type="text/javascript"></script>
<script type="application/javascript">
var myCountdown1 = new Countdown({
    							time: 300, 
									width:150, 
									height:80, 
									rangeHi:"minute"	// <- no comma on last item!
									});

</script>

</div>
