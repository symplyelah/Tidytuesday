---
title: "You Can Make It in R"
author: "Ifeoma Egbogah"
date: "12/16/2019"
output: 
  html_document: 
    keep_md: yes
editor_options: 
  chunk_output_type: console
---





```r
library(ggthemes)
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
library(dslabs)
library(colorspace)
theme_set(theme_light())
```


##Replicating New York Times Plot of Regent Scores

```r
nyc_regents <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-12-10/nyc_regents.csv")
```

```
## Parsed with column specification:
## cols(
##   score = col_double(),
##   integrated_algebra = col_double(),
##   global_history = col_double(),
##   living_environment = col_double(),
##   english = col_double(),
##   us_history = col_double()
## )
```



```r
mytext <- expression(paste(bold("Scraping By ")))

nyc <- nyc_regents%>%
  mutate(total = rowSums(nyc_regents[, -1], na.rm = TRUE))%>%
  ggplot(aes(score, total))+
  geom_col(fill = "#C4843C")+
  annotate("rect", xmin = 65, xmax = 99, ymin = 0, ymax = 34000, alpha = 0.2)+
  annotate("text", x = 0, y = 11000, label = "2010 Regents scores on\nthe five most common tests*", hjust = 0, size = 4)+
   geom_segment(aes(x = 66, y = 28500, xend = 65.5, yend = 28500), arrow = arrow(length = unit(0.015, "npc"), type = "closed"))+
  annotate("text", x = 67, y = 27950, size = 3, label = "MINIMUM\nREGENTS DIPLOMA\nSCORE", hjust = 0)+
  annotate("text", x = 0, y = 34000, label = mytext, size = 9, hjust = 0)+
  annotate("text", x = 0, y = 27100, label = "In New York, many students receive Regent exam scores\njust above the passing threshold than below. Not all scores\nare possible on the test(for example it is impossible to receive\na 64 on the algebra test, but the pattern raises question\nabout how teachers are grading exams", hjust = 0, size = 5)+
  scale_x_continuous(breaks = seq(5, 99, 5), expand = c(0,0))+
  scale_y_continuous(position = "right", labels = scales::comma_format())+
  theme_hc()+
  theme(axis.title.y.right = element_text(angle = 0, vjust = 0.81),
        axis.title.x.bottom = element_text(size = 8, hjust = 0),
         plot.margin = unit(c(1,1,1,1), "cm"),
        plot.caption = element_text(size = 7))+
  labs(y = "tests",
       x = "*Algebra, Global History, Biology, English and US History",
       caption = "Data: DS Labs Package | Visualization: Ifeoma Egbogah")
  

ggsave("nyc.jpeg", nyc, width = 20, height = 8)
```

```
## Warning: Removed 1 rows containing missing values (position_stack).
```

```
## Warning in is.na(x): is.na() applied to non-(list or vector) of type
## 'expression'
```


##Replicating Election Forecast By Fivethirthyeight

```r
library(sf)
```

```
## Linking to GEOS 3.6.1, GDAL 2.2.3, PROJ 4.9.3
```

```r
library(acs)
```

```
## Loading required package: XML
```

```
## 
## Attaching package: 'acs'
```

```
## The following object is masked from 'package:dplyr':
## 
##     combine
```

```
## The following object is masked from 'package:base':
## 
##     apply
```

```r
library(spData)
```

```
## To access larger datasets in this package, install the spDataLarge
## package with: `install.packages('spDataLarge',
## repos='https://nowosad.github.io/drat/', type='source')`
```

```r
library(cowplot)
```

```
## 
## ********************************************************
```

```
## Note: As of version 1.0.0, cowplot does not change the
```

```
##   default ggplot2 theme anymore. To recover the previous
```

```
##   behavior, execute:
##   theme_set(theme_cowplot())
```

```
## ********************************************************
```

```
## 
## Attaching package: 'cowplot'
```

```
## The following object is masked from 'package:ggthemes':
## 
##     theme_map
```

```r
library(biscale)

data("fips.state") #state code

data("us_states") #map of united states
data("hawaii") 
data("alaska")
```



```r
##Preparing the data
poll_data <- read_csv("C:/Users/Egbogah/Desktop/poll.csv")
```

```
## Parsed with column specification:
## cols(
##   state = col_character(),
##   clinton = col_double(),
##   trump = col_double()
## )
```

```r
us_map <- st_as_sf(us_states)
hawaii_map <- st_as_sf(hawaii)
alaska_map <- st_as_sf(alaska)

us_map <- st_transform(us_map, 2163)
hawaii_map <- st_transform(hawaii_map, 2163)
alaska_map <- st_transform(alaska_map, 2163)


us_map2 <- us_map%>% #adding hawaii and alaska to the us map
  rbind(., hawaii_map)%>%
  rbind(., alaska_map)


poll_data2 <- poll_data%>% #adding the state name by code
  left_join(., fips.state, by = c("state" ="STUSAB"))

us_map3 <- us_map2%>%
  left_join(., poll_data2, by = c("NAME" ="STATE_NAME"))


us_map4 <- bi_class(us_map3, x = clinton, y = trump, dim = 3) #using biscale package

us_map5 <- us_map4%>%
  filter(NAME != "Alaska")%>%
  filter(NAME != "Hawaii")

hawaii_map2 <- us_map4%>%
  filter(NAME == "Hawaii")


alaska_map2 <- us_map4%>%
  filter(NAME == "Alaska")


##Choosing the bivariate colour for the map

bivariate_color_scale <- tibble(
  "2 - 2" = "#3F2949",
  "1 - 2" = "tomato", # trump
  "2 - 1" = "deepskyblue2", # clinton
  "1 - 1" = "#D1E5F0"  
) %>%
  gather("group", "fill")


##Class intervals

us_map6 <- us_map5%>%
  replace_na(list(clinton = 0, trump = 0))%>%
  mutate(ratio_clinton = cut(clinton,
                         breaks = unique(quantile(clinton, probs = seq(0, 1, length.out = 3))), include.lowest = TRUE),
        ratio_trump = cut(trump,
                             breaks = unique(quantile(trump, probs = seq(0, 1, length.out = 3))), include.lowest = TRUE),
         
         group = paste(as.numeric(ratio_clinton), "-",
                       as.numeric(ratio_trump)))%>%
  left_join(bivariate_color_scale, by = "group")


##Selecting some states to hightlight

nc <- us_map6%>%
  filter(state == "NC")

nh <- us_map6%>%
  filter(state == "NH")

nv <- us_map6%>%
  filter(state == "NV")

az <- us_map6%>%
  filter(state == "AZ")

ga <- us_map6%>%
  filter(state == "GA")

ia <- us_map6%>%
  filter(state == "IA")

oh <- us_map6%>%
  filter(state == "OH")

mn <- us_map6%>%
  filter(state == "MN")

wi <- us_map6%>%
  filter(state == "WI")

pa <- us_map6%>%
  filter(state == "PA")

oh <- us_map6%>%
  filter(state == "OH")

fl <- us_map6%>%
  filter(state == "FL")

va <- us_map6%>%
  filter(state == "VA")

nc <- us_map6%>%
  filter(state == "NC")

mi <- us_map6%>%
  filter(state == "MI")

fl <- us_map6%>%
  filter(state == "FL")

co <- us_map6%>%
  filter(state == "CO")


##Plotting the map

us_map7 <- ggplot(data = us_map6)+
  geom_sf(aes(fill = fill),  size = 0.1)+
  scale_fill_identity()+
  geom_sf(data = us_map6, fill = "transparent", colour = "white", size = 0.4)+
  geom_sf(data = nc, fill = "#D1E5F0", colour = "black", size = 1)+
  geom_sf(data = nh, fill = "#D1E5F0", colour = "white")+
  geom_sf(data = nv, fill = "#D1E5F0", colour = "black", size = 1)+
  geom_sf(data = mn, fill = "deepskyblue2", colour = "black", size = 1)+
  geom_sf(data = co, fill = "deepskyblue2", colour = "black", size = 1)+
  geom_sf(data = wi, fill = "deepskyblue2", colour = "black", size = 1)+
  geom_sf(data = pa, fill = "deepskyblue2", colour = "black", size = 1)+
  geom_sf(data = oh, fill = "lightsalmon", colour = "black", size = 1, alpha = 1)+
  geom_sf(data = va, fill = "deepskyblue2", colour = "black", size = 1)+
  geom_sf(data = mi, fill = "deepskyblue2", colour = "black", size = 1)+
  geom_sf(data = az, fill = "lightsalmon", colour = "white", alpha = 1)+
  geom_sf(data = ga, fill = "lightsalmon", colour = "white", alpha = 1)+
  geom_sf(data = ia, fill = "lightsalmon", colour = "white", alpha = 1)+
  geom_sf(data = fl, fill = "#D1E5F0", colour = "black", size = 1)+
  geom_sf_text(data = us_map5, aes(label = state), colour = "black", size = 3, check_overlap = TRUE)+
  annotate("rect", xmin = -2031905, xmax = 1145963, ymin =  800000, ymax =  902000, colour = "deepskyblue", fill = "deepskyblue")+
  annotate("rect", xmin = 1145963, xmax = 2511000, ymin =  800000, ymax =  902000, colour = "tomato", fill = "tomato")+
  annotate("text", label = "Hillary Clinton", colour = "black", y = 1202000, x = -1000000)+
  annotate("text", label = "Donald Trump", colour = "black", y = 1202000, x = 1140000)+
  annotate("text", label = expression(paste(bold("71.4%"))), size = 9, colour = "deepskyblue2", y = 1002000, x = -1010000)+
  annotate("text", label = expression(paste(bold("28.6%"))), size = 9, colour = "tomato", y = 1002000, x = 1140000)+
    theme_map()+
  theme(plot.title = element_text(face = "bold"))+
  labs(title = "Chance of winning")



##Plotting the legend

key <- data.frame(trump = c(.5, .6, .7, .8, .9),
                  clinton = c(.5, .6, .7, .8, .9))

trump <- key%>%
  ggplot()+
  geom_tile(aes(x = trump, y = 0.5, fill = trump), show.legend = F)+
  scale_fill_gradient(low = "bisque", high = "tomato")+
  theme_light()+
  theme(axis.title.y = element_text(angle = 0, vjust = 0.5),
        axis.title.x = element_blank(),
        panel.border = element_blank(),
        panel.grid = element_blank(),
        axis.text = element_blank(),
        axis.ticks = element_blank())+
    labs(y = "TRUMP")



clinton <- key%>%
  ggplot()+
  geom_tile(aes(x = clinton, y = 0.5, fill = clinton), show.legend = F)+
  scale_fill_gradient(low = "#D1E5F0", high = "deepskyblue")+
  scale_x_continuous(position = "top", label = scales::percent_format(), breaks = seq(.5, .9, .1))+ 
  theme_light()+
  theme(axis.title.y = element_text(angle = 0, vjust = 0.5),
        axis.title.x.top = element_blank(),
        panel.border = element_blank(),
        panel.grid = element_blank(),
        axis.text.y = element_blank(),
        axis.text.x.bottom = element_blank(),
        axis.ticks = element_blank())+
  labs(y = "CLINTON")


poll_map <-  ggdraw() +
  draw_plot(us_map7, -0.12, 0.01, 1, 1)+
  draw_plot(clinton, 0.81, 0.05, 0.4, 0.5, width = 0.8, height = 0.26)+
  draw_plot(trump, 0.81, 0, 0.4, 0.5, width = 0.8, height = 0.2)
```

```
## Warning in is.na(x): is.na() applied to non-(list or vector) of type
## 'expression'

## Warning in is.na(x): is.na() applied to non-(list or vector) of type
## 'expression'
```

```r
ggsave("poll2.jpeg", poll_map, width = 20, height = 8)
```

