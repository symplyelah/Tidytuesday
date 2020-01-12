---
title: "Philadelphia Parking Violations"
author: "Ifeoma Egbogah"
date: "12/16/2019"
output: 
  html_document: 
    keep_md: yes
editor_options: 
  chunk_output_type: console
---




```r
library(colorspace)
library(tidyverse)
```

```
## -- Attaching packages ----------------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.2.1     v purrr   0.3.3
## v tibble  2.1.3     v dplyr   0.8.3
## v tidyr   1.0.0     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.4.0
```

```
## -- Conflicts -------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(sf)
```

```
## Linking to GEOS 3.6.1, GDAL 2.2.3, PROJ 4.9.3
```

```r
library(viridis)
```

```
## Loading required package: viridisLite
```

```r
library(ggthemes)
library(extrafont)
```

```
## Registering fonts with R
```

```r
font_import()
```

```
## Importing fonts may take a few minutes, depending on the number of fonts and the speed of the system.
## Continue? [y/n]
```

```
## Exiting.
```

```r
loadfonts(device = "win")
```

```
## Arial Black already registered with windowsFonts().
```

```
## Arial already registered with windowsFonts().
```

```
## Bahnschrift already registered with windowsFonts().
```

```
## Calibri already registered with windowsFonts().
```

```
## Calibri Light already registered with windowsFonts().
```

```
## Cambria already registered with windowsFonts().
```

```
## Candara already registered with windowsFonts().
```

```
## Candara Light already registered with windowsFonts().
```

```
## Comic Sans MS already registered with windowsFonts().
```

```
## Consolas already registered with windowsFonts().
```

```
## Constantia already registered with windowsFonts().
```

```
## Corbel already registered with windowsFonts().
```

```
## Corbel Light already registered with windowsFonts().
```

```
## Courier New already registered with windowsFonts().
```

```
## Ebrima already registered with windowsFonts().
```

```
## Franklin Gothic Medium already registered with windowsFonts().
```

```
## Gabriola already registered with windowsFonts().
```

```
## Gadugi already registered with windowsFonts().
```

```
## Georgia already registered with windowsFonts().
```

```
## HoloLens MDL2 Assets already registered with windowsFonts().
```

```
## HP Simplified already registered with windowsFonts().
```

```
## HP Simplified Light already registered with windowsFonts().
```

```
## Impact already registered with windowsFonts().
```

```
## Ink Free already registered with windowsFonts().
```

```
## Javanese Text already registered with windowsFonts().
```

```
## Leelawadee UI already registered with windowsFonts().
```

```
## Leelawadee UI Semilight already registered with windowsFonts().
```

```
## Lucida Console already registered with windowsFonts().
```

```
## Lucida Sans Unicode already registered with windowsFonts().
```

```
## Malgun Gothic already registered with windowsFonts().
```

```
## Malgun Gothic Semilight already registered with windowsFonts().
```

```
## Marlett already registered with windowsFonts().
```

```
## Microsoft Himalaya already registered with windowsFonts().
```

```
## Microsoft Yi Baiti already registered with windowsFonts().
```

```
## Microsoft New Tai Lue already registered with windowsFonts().
```

```
## Microsoft PhagsPa already registered with windowsFonts().
```

```
## Microsoft Sans Serif already registered with windowsFonts().
```

```
## Microsoft Tai Le already registered with windowsFonts().
```

```
## Mongolian Baiti already registered with windowsFonts().
```

```
## MV Boli already registered with windowsFonts().
```

```
## Myanmar Text already registered with windowsFonts().
```

```
## Nirmala UI already registered with windowsFonts().
```

```
## Nirmala UI Semilight already registered with windowsFonts().
```

```
## Palatino Linotype already registered with windowsFonts().
```

```
## Segoe MDL2 Assets already registered with windowsFonts().
```

```
## Segoe Print already registered with windowsFonts().
```

```
## Segoe Script already registered with windowsFonts().
```

```
## Segoe UI already registered with windowsFonts().
```

```
## Segoe UI Light already registered with windowsFonts().
```

```
## Segoe UI Semibold already registered with windowsFonts().
```

```
## Segoe UI Semilight already registered with windowsFonts().
```

```
## Segoe UI Black already registered with windowsFonts().
```

```
## Segoe UI Emoji already registered with windowsFonts().
```

```
## Segoe UI Historic already registered with windowsFonts().
```

```
## Segoe UI Symbol already registered with windowsFonts().
```

```
## SimSun-ExtB already registered with windowsFonts().
```

```
## Sylfaen already registered with windowsFonts().
```

```
## Symbol already registered with windowsFonts().
```

```
## Tahoma already registered with windowsFonts().
```

```
## Times New Roman already registered with windowsFonts().
```

```
## Trebuchet MS already registered with windowsFonts().
```

```
## Verdana already registered with windowsFonts().
```

```
## Webdings already registered with windowsFonts().
```

```
## Wingdings already registered with windowsFonts().
```

```
## Agency FB already registered with windowsFonts().
```

```
## Algerian already registered with windowsFonts().
```

```
## Arial Narrow already registered with windowsFonts().
```

```
## Arial Rounded MT Bold already registered with windowsFonts().
```

```
## Baskerville Old Face already registered with windowsFonts().
```

```
## Bauhaus 93 already registered with windowsFonts().
```

```
## Bell MT already registered with windowsFonts().
```

```
## Berlin Sans FB already registered with windowsFonts().
```

```
## Berlin Sans FB Demi already registered with windowsFonts().
```

```
## Bernard MT Condensed already registered with windowsFonts().
```

```
## Blackadder ITC already registered with windowsFonts().
```

```
## Bodoni MT already registered with windowsFonts().
```

```
## Bodoni MT Black already registered with windowsFonts().
```

```
## Bodoni MT Condensed already registered with windowsFonts().
```

```
## Bodoni MT Poster Compressed already registered with windowsFonts().
```

```
## Book Antiqua already registered with windowsFonts().
```

```
## Bookman Old Style already registered with windowsFonts().
```

```
## Bookshelf Symbol 7 already registered with windowsFonts().
```

```
## Bradley Hand ITC already registered with windowsFonts().
```

```
## Britannic Bold already registered with windowsFonts().
```

```
## Broadway already registered with windowsFonts().
```

```
## Brush Script MT already registered with windowsFonts().
```

```
## Californian FB already registered with windowsFonts().
```

```
## Calisto MT already registered with windowsFonts().
```

```
## Castellar already registered with windowsFonts().
```

```
## Centaur already registered with windowsFonts().
```

```
## Century already registered with windowsFonts().
```

```
## Century Gothic already registered with windowsFonts().
```

```
## Century Schoolbook already registered with windowsFonts().
```

```
## Chiller already registered with windowsFonts().
```

```
## Colonna MT already registered with windowsFonts().
```

```
## Cooper Black already registered with windowsFonts().
```

```
## Copperplate Gothic Bold already registered with windowsFonts().
```

```
## Copperplate Gothic Light already registered with windowsFonts().
```

```
## Curlz MT already registered with windowsFonts().
```

```
## Edwardian Script ITC already registered with windowsFonts().
```

```
## Elephant already registered with windowsFonts().
```

```
## Engravers MT already registered with windowsFonts().
```

```
## Eras Bold ITC already registered with windowsFonts().
```

```
## Eras Demi ITC already registered with windowsFonts().
```

```
## Eras Light ITC already registered with windowsFonts().
```

```
## Eras Medium ITC already registered with windowsFonts().
```

```
## Felix Titling already registered with windowsFonts().
```

```
## Footlight MT Light already registered with windowsFonts().
```

```
## Forte already registered with windowsFonts().
```

```
## Franklin Gothic Book already registered with windowsFonts().
```

```
## Franklin Gothic Demi already registered with windowsFonts().
```

```
## Franklin Gothic Demi Cond already registered with windowsFonts().
```

```
## Franklin Gothic Heavy already registered with windowsFonts().
```

```
## Franklin Gothic Medium Cond already registered with windowsFonts().
```

```
## Freestyle Script already registered with windowsFonts().
```

```
## French Script MT already registered with windowsFonts().
```

```
## Garamond already registered with windowsFonts().
```

```
## Gigi already registered with windowsFonts().
```

```
## Gill Sans Ultra Bold already registered with windowsFonts().
```

```
## Gill Sans Ultra Bold Condensed already registered with windowsFonts().
```

```
## Gill Sans MT already registered with windowsFonts().
```

```
## Gill Sans MT Condensed already registered with windowsFonts().
```

```
## Gill Sans MT Ext Condensed Bold already registered with windowsFonts().
```

```
## Gloucester MT Extra Condensed already registered with windowsFonts().
```

```
## Goudy Old Style already registered with windowsFonts().
```

```
## Goudy Stout already registered with windowsFonts().
```

```
## Haettenschweiler already registered with windowsFonts().
```

```
## Harlow Solid Italic already registered with windowsFonts().
```

```
## Harrington already registered with windowsFonts().
```

```
## High Tower Text already registered with windowsFonts().
```

```
## Imprint MT Shadow already registered with windowsFonts().
```

```
## Informal Roman already registered with windowsFonts().
```

```
## Jokerman already registered with windowsFonts().
```

```
## Juice ITC already registered with windowsFonts().
```

```
## Kristen ITC already registered with windowsFonts().
```

```
## Kunstler Script already registered with windowsFonts().
```

```
## Wide Latin already registered with windowsFonts().
```

```
## Leelawadee already registered with windowsFonts().
```

```
## Lucida Bright already registered with windowsFonts().
```

```
## Lucida Calligraphy already registered with windowsFonts().
```

```
## Lucida Fax already registered with windowsFonts().
```

```
## Lucida Handwriting already registered with windowsFonts().
```

```
## Lucida Sans already registered with windowsFonts().
```

```
## Lucida Sans Typewriter already registered with windowsFonts().
```

```
## Magneto already registered with windowsFonts().
```

```
## Maiandra GD already registered with windowsFonts().
```

```
## Matura MT Script Capitals already registered with windowsFonts().
```

```
## Microsoft Uighur already registered with windowsFonts().
```

```
## Mistral already registered with windowsFonts().
```

```
## Modern No. 20 already registered with windowsFonts().
```

```
## Monotype Corsiva already registered with windowsFonts().
```

```
## MS Outlook already registered with windowsFonts().
```

```
## MS Reference Sans Serif already registered with windowsFonts().
```

```
## MS Reference Specialty already registered with windowsFonts().
```

```
## MT Extra already registered with windowsFonts().
```

```
## Niagara Engraved already registered with windowsFonts().
```

```
## Niagara Solid already registered with windowsFonts().
```

```
## OCR A Extended already registered with windowsFonts().
```

```
## Old English Text MT already registered with windowsFonts().
```

```
## Onyx already registered with windowsFonts().
```

```
## Palace Script MT already registered with windowsFonts().
```

```
## Papyrus already registered with windowsFonts().
```

```
## Parchment already registered with windowsFonts().
```

```
## Perpetua already registered with windowsFonts().
```

```
## Perpetua Titling MT already registered with windowsFonts().
```

```
## Playbill already registered with windowsFonts().
```

```
## Poor Richard already registered with windowsFonts().
```

```
## Pristina already registered with windowsFonts().
```

```
## Rage Italic already registered with windowsFonts().
```

```
## Ravie already registered with windowsFonts().
```

```
## Rockwell already registered with windowsFonts().
```

```
## Rockwell Condensed already registered with windowsFonts().
```

```
## Rockwell Extra Bold already registered with windowsFonts().
```

```
## Script MT Bold already registered with windowsFonts().
```

```
## Showcard Gothic already registered with windowsFonts().
```

```
## Snap ITC already registered with windowsFonts().
```

```
## Stencil already registered with windowsFonts().
```

```
## Tempus Sans ITC already registered with windowsFonts().
```

```
## Tw Cen MT already registered with windowsFonts().
```

```
## Tw Cen MT Condensed already registered with windowsFonts().
```

```
## Tw Cen MT Condensed Extra Bold already registered with windowsFonts().
```

```
## Viner Hand ITC already registered with windowsFonts().
```

```
## Vivaldi already registered with windowsFonts().
```

```
## Vladimir Script already registered with windowsFonts().
```

```
## Wingdings 2 already registered with windowsFonts().
```

```
## Wingdings 3 already registered with windowsFonts().
```



```r
tickets <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-12-03/tickets.csv")
```

```
## Parsed with column specification:
## cols(
##   violation_desc = col_character(),
##   issue_datetime = col_datetime(format = ""),
##   fine = col_double(),
##   issuing_agency = col_character(),
##   lat = col_double(),
##   lon = col_double(),
##   zip_code = col_double()
## )
```

```r
##Shapefile of Philadelphia (map, river snd watershed) from www.opendataphilly.org

philly <- read_sf("C:/Users/Egbogah/Desktop/Philadelphia/Zoning_PreAug2012/Zoning_PreAug2012.shp")

philly <- st_transform(philly, crs = 4326)

philly_watershed <- read_sf("C:/Users/Egbogah/Desktop/Philadelphia/Hydrographic_Features_Poly/Hydrographic_Features_Poly.shp")

philly_watershed <- st_transform(philly_watershed, crs = 4326)

philly_river <- read_sf("C:/Users/Egbogah/Desktop/Philadelphia/Philly_areawater/tl_2013_42101_areawater.shp")

philly_river <- st_transform(philly_river, crs = 4326)
```



```r
watershed <- philly_watershed%>%
  st_crop(xmin = -75.30, xmax = -74.95, ymin = 39.80, ymax = 40.05)
```

```
## although coordinates are longitude/latitude, st_intersection assumes that they are planar
```

```
## Warning: attribute variables are assumed to be spatially constant throughout all
## geometries
```

```r
water <- philly_watershed%>%
  st_intersection(., philly)
```

```
## although coordinates are longitude/latitude, st_intersection assumes that they are planar
```

```
## Warning: attribute variables are assumed to be spatially constant throughout all
## geometries
```

```r
tick <- tickets%>%
  filter(lon >= -75.3)%>%
  filter(lat >= 39.8)%>%
  ggplot()+
  geom_sf(data = philly_river, colour = darken("skyblue"), fill = "skyblue")+
  geom_sf(data = watershed, colour = darken("skyblue"), fill = "skyblue")+
  geom_sf(data = philly, fill = "transparent", colour = "grey65")+
  geom_bin2d(aes(lon, lat), binwidth = 0.001, alpha = 0.7)+
  scale_fill_viridis_c(option = "plasma", alpha = 0.7, 
                       guide = guide_colourbar(direction = "horizontal",
                                               title.position = "top",
                                               title.hjust = 0.5,
                                               barheight = unit(2, units = "mm"),
                                               barwidth = unit(50, units = "mm")))+
  theme_light()+
  theme(axis.title = element_blank(),
        axis.text = element_blank(),
        axis.line = element_blank(),
        panel.border = element_blank(),
        axis.ticks = element_blank(),
        panel.grid.minor = element_blank(),
        panel.grid.major = element_blank(),
        legend.position = "bottom",
        plot.title = element_text(family = "Rage Italic", size = 22, hjust = 0.5),
        plot.caption = element_text(family = "Poor Richard", size = 10))+
  labs(fill = "Density",
       title = "Density of Traffic Violation in Philadelphia, USA in 2017",
       caption = "Data: Open Philly Data | Visualization: Ifeoma Egbogah")


ggsave("ticket2.jpeg", tick, width = 10, height = 10)
```

