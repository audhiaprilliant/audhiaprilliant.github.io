---
layout: post
title:  "Data Visualization Through Leaflet Using R"
author: audhi
categories: [ data visualization,rprogramming ]
tags: [data mining]
image: assets/images/18-0.jpg
---

### Overview
Leaflet is an open-source JavaScript library for interactive web maps. Leaflet is designed with *simplicity*, *performance* and *usability* in mind. It's *lightweight*, *simple*, and *flexible*, and is probably the most popular open-source mapping library at the moment. 

Leaflet works efficiently across all major desktop and mobile platforms, can be extended with lots of plugins, has a beautiful, easy to use and well-documented API and a simple, readable source code that is a joy to contribute to. Leaflet is developed by **Vladimir Agafonkin** (currently of *MapBox*) and other contributors.

**Interaction Features**
- Drag panning with inertia
- Scroll wheel zoom
- Pinch-zoom on mobile
- Double click zoom
- Zoom to area *(shift-drag)*
- Keyboard navigation
- Events: *click, mouseover, etc.*
- Marker dragging

### Personal Project
**Prerequisites**  
Before we begin to build the visualization, make sure we fulfil the following requirements:
- Programming language of R with several libraries, such as `leaflet`, `leaflet.extras`, and `dplyr`
- The data consists of location point (latitude and longitude) and additional information. You can easily download the sample data [**here**]({{site.baseurl}}/assets/docs/Location Data.txt)
- Good internet connection

**Explanation**  
For building a script to create a leaflet map in R is quite simple. But, if we want to build an interactive and beautiful map, of course, some script must be added. The following script will create our simple map (actually it's quite cool). Why? Because we have added some information to our marker, such as *location's name, address, longitude, latitude, supervisor,* and *students's name*, instead of only markers. That information is displayed using popup after we click the marker
```r
leaflet(data.location) %>%
  addProviderTiles(providers$OpenStreetMap) %>%
  addMarkers(lng = ~long,
             lat = ~lat,
             popup = paste(paste('<b>Office:</b>',
                                 data.location$place),
                           paste('<b>Address:</b>',
                                 data.location$address),
                           paste('<b>Lat:</b>',
                                 data.location$lat),
                           paste('<b>Long:</b>',
                                 data.location$long),
                           paste('<b>Supervisor:</b>',
                                 data.location$supervisor),
                           data.location$student1,
                           data.location$student2,
                           data.location$student3,
                           sep = '<br/>'),
             label = ~place,
             group = 'data.location')
```

Okay, let's add some features to our map. After zooming in and out each location, it's good to reset our view to default. So, we can add a reset map button. Further, we have a search feature to simplify our searching, just by typing our location's name and the algorithm will show up relevant result based on our keyword. So, just adding following script at the bottom of the previous script.
```r
addResetMapButton() %>%
addSearchFeatures(
  targetGroups = 'data.location',
  options = searchFeaturesOptions(zoom = 15,
                                  openPopup = TRUE,
                                  firstTipSubmit = TRUE,
                                  autoCollapse = TRUE,
                                  hideMarkerOnCollapse = TRUE))
```

Okay then, we add measure button and highlight for general info or title. For the measure button, as its name, it will show us the distance between two points or more. We can estimate the distance between two citis using this feature. If we meet the zigzag route, we can make it like polygon-shape. The unit of measurement must be set depending on our needs, for instance in meters. Lastly, to show up our general information, it's good to add like infobox.
```r
addMeasure(
    position = 'bottomleft',
    primaryLengthUnit = 'meters',
    primaryAreaUnit = 'sqmeters',
    activeColor = '#3D535D',
    completedColor = '#7D4479') %>%
addControl("<P><b>Masterpiece Statistics 53</b>
<br/>Search for offices/ industries<br/>in Java by name.</P>",
           position = 'topright')
```


**Recap**  
To recap our script, we can run the following script properly and voila, our map is ready to interpret and launch to production.
```r
# Load the libraries
library(leaflet)
library(leaflet.extras)
library(dplyr)

# Load the data
data.location = read.csv('/path-to-file/Location Data.txt',
                         header = TRUE,
                         sep = ',')
# Create leaflet
leaflet(data.location) %>%
  addProviderTiles(providers$OpenStreetMap) %>%
  addMarkers(lng = ~long,
             lat = ~lat,
             popup = paste(paste('<b>Office:</b>',
                                 data.location$place),
                           paste('<b>Address:</b>',
                                 data.location$address),
                           paste('<b>Lat:</b>',
                                 data.location$lat),
                           paste('<b>Long:</b>',
                                 data.location$long),
                           paste('<b>Supervisor:</b>',
                                 data.location$supervisor),
                           data.location$student1,
                           data.location$student2,
                           data.location$student3,
                           sep = '<br/>'),
             label = ~place,
             group = 'data.location') %>%
  addResetMapButton() %>%
  addSearchFeatures(
    targetGroups = 'data.location',
    options = searchFeaturesOptions(zoom = 15,
                                    openPopup = TRUE,
                                    firstTipSubmit = TRUE,
                                    autoCollapse = TRUE,
                                    hideMarkerOnCollapse = TRUE)) %>%
  addMeasure(
    position = 'bottomleft',
    primaryLengthUnit = 'meters',
    primaryAreaUnit = 'sqmeters',
    activeColor = '#3D535D',
    completedColor = '#7D4479') %>%
  addControl("<P><b>Masterpiece Statistics 53</b>
  <br/>Search for offices/ industries<br/>in Java by name.</P>",
             position = 'topright')
```

We can look at our map like the following figure
<iframe src="{{site.baseurl}}/assets/docs/Leaflet - Internship Program.html" style="width: 100%; height: 315px;" frameBorder="0"></iframe>

### Sources
<a target="_blank" href="https://maptimeboston.github.io/leaflet-intro/" class="btn btn-danger">Map Time Boston</a> <a target="_blank" href="https://leafletjs.com/index.html" class="btn btn-warning">Leaflet JS</a>
