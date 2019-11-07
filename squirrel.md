---
title: "NYC Squirrel Census"
author: "Ifeoma Egbogah"
date: "11/7/2019"
output: 
  html_document: 
    keep_md: yes
editor_options: 
  chunk_output_type: console
---





```r
library(tidyverse)
```

```
## -- Attaching packages ---------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.2.0     v purrr   0.3.2
## v tibble  2.1.3     v dplyr   0.8.1
## v tidyr   0.8.3     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.4.0
```

```
## -- Conflicts ------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(ggmap)
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(viridis)
```

```
## Loading required package: viridisLite
```

```r
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
## Candara Light already registered with windowsFonts().
```

```
## Corbel Light already registered with windowsFonts().
```

```r
nyc_squirrels <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-10-29/nyc_squirrels.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   long = col_double(),
##   lat = col_double(),
##   date = col_double(),
##   hectare_squirrel_number = col_double(),
##   running = col_logical(),
##   chasing = col_logical(),
##   climbing = col_logical(),
##   eating = col_logical(),
##   foraging = col_logical(),
##   kuks = col_logical(),
##   quaas = col_logical(),
##   moans = col_logical(),
##   tail_flags = col_logical(),
##   tail_twitches = col_logical(),
##   approaches = col_logical(),
##   indifferent = col_logical(),
##   runs_from = col_logical(),
##   zip_codes = col_double(),
##   community_districts = col_double(),
##   borough_boundaries = col_double()
##   # ... with 2 more columns
## )
```

```
## See spec(...) for full column specifications.
```

```r
ny.map2 <- get_map("Central Park, New York", maptype = "terrain", zoom = 14) #Get map of central park from google
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=Central%20Park,%20New%20York&zoom=14&size=640x640&scale=2&maptype=terrain&language=en-EN&key=xxx
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=Central+Park,+New+York&key=xxx
```


```r
nyc <- ggmap(ny.map2)+
  stat_density_2d(data = nyc_squirrels%>%
                  filter(!is.na(highlight_fur_color)), 
                  aes(long, lat, fill = stat(level)), 
                  alpha = 0.1, bins = 20, geom = "polygon")+
  scale_fill_viridis(option = "C")+
  facet_wrap(~ `highlight_fur_color`)+
  theme_minimal()+
  theme(axis.text = element_blank(),
        strip.text.x = element_text(size = 13))+
  labs(x = " ",
       y = " ",
       title = "Geographic Density of Squirrels in Central Park, New York",
       subtitle = "The geographic density is faceted based on the highlight colour of the squirrels' fur",
       caption = "Data: NYC Squirrel Census | Visualization: Ifeoma Egbogah",
       fill = "Level")


nyc2 <- ggmap(ny.map2)+
  stat_density_2d(data = nyc_squirrels%>%
                  filter(!is.na(primary_fur_color)), 
                  aes(long, lat, fill = stat(level)), 
                  alpha = 0.1, bins = 20, geom = "polygon")+
  scale_fill_viridis(option = "C")+
  facet_wrap(~ `primary_fur_color`)+
  theme_minimal()+
  theme(axis.text = element_blank(),
        strip.text.x = element_text(size = 13))+
  labs(x = " ",
       y = " ",
       title = "Geographic Density of Squirrels in Central Park, New York",
       subtitle = "The geographic density is faceted based on the squirrels' primary fur colour",
       caption = "Data: NYC Squirrel Census | Visualization: Ifeoma Egbogah",
       fill = "Level")
```



```r
ggsave("map.jpeg", nyc,  width = 12, height = 12)
ggsave("map2.jpeg", nyc,  width = 12, height = 10)
```

