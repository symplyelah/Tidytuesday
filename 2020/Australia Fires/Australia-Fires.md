---
title: "Australia Fires"
author: "Ifeoma Egbogah"
date: "1/12/2020"
output: 
  ioslides_presentation: 
    keep_md: yes
editor_options: 
  chunk_output_type: console
---



##Load Packages

```
## Warning: package 'here' was built under R version 3.6.2
```

```
## here() starts at C:/Users/Egbogah/Desktop/Tidytuesday/Australia Fires/Australia Fires
```

```
## -- Attaching packages ---------------------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.0.9000     v purrr   0.3.3     
## v tibble  2.1.3          v dplyr   0.8.3     
## v tidyr   1.0.0          v stringr 1.4.0     
## v readr   1.3.1          v forcats 0.4.0
```

```
## -- Conflicts ------------------------------------------------------- tidyverse_conflicts() --
## x readr::col_factor() masks scales::col_factor()
## x purrr::discard()    masks scales::discard()
## x dplyr::filter()     masks stats::filter()
## x dplyr::lag()        masks stats::lag()
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:here':
## 
##     here
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```
## Registering fonts with R
```

```
## Importing fonts may take a few minutes, depending on the number of fonts and the speed of the system.
## Continue? [y/n]
```

```
## Exiting.
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

##Get the Data

```
## Parsed with column specification:
## cols(
##   station_code = col_character(),
##   city_name = col_character(),
##   year = col_double(),
##   month = col_character(),
##   day = col_character(),
##   rainfall = col_double(),
##   period = col_double(),
##   quality = col_character(),
##   lat = col_double(),
##   long = col_double(),
##   station_name = col_character()
## )
```

```
## Parsed with column specification:
## cols(
##   city_name = col_character(),
##   date = col_date(format = ""),
##   temperature = col_double(),
##   temp_type = col_character(),
##   site_name = col_character()
## )
```

```
## Parsed with column specification:
## cols(
##   latitude = col_double(),
##   longitude = col_double(),
##   brightness = col_double(),
##   scan = col_double(),
##   track = col_double(),
##   acq_date = col_date(format = ""),
##   acq_time = col_character(),
##   satellite = col_character(),
##   confidence = col_double(),
##   version = col_character(),
##   bright_t31 = col_double(),
##   frp = col_double(),
##   daynight = col_character()
## )
```

```
## Reading layer `majorIncidents' from data source `http://www.rfs.nsw.gov.au/feeds/majorIncidents.json' using driver `GeoJSON'
## Simple feature collection with 114 features and 7 fields
## geometry type:  GEOMETRY
## dimension:      XY
## bbox:           xmin: 144.1628 ymin: -37.81881 xmax: 153.4047 ymax: -28.19746
## epsg (SRID):    4326
## proj4string:    +proj=longlat +datum=WGS84 +no_defs
```




