---
title: "Wildlife Impact"
author: "Ifeoma Egbogah"
date: "8/1/2019"
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
## -- Attaching packages -------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.1     v purrr   0.3.2
## v tibble  2.1.1     v dplyr   0.8.1
## v tidyr   0.8.3     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.4.0
```

```
## -- Conflicts ----------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(ggalluvial)
library(lubridate)
```

```
## 
## Attaching package: 'lubridate'
```

```
## The following object is masked from 'package:base':
## 
##     date
```

```r
library(ggthemes)
library(drlib)
library(gameofthrones)
library(sf)
```

```
## Linking to GEOS 3.6.1, GDAL 2.2.3, PROJ 4.9.3
```

```r
library(maps)
```

```
## 
## Attaching package: 'maps'
```

```
## The following object is masked from 'package:purrr':
## 
##     map
```

```r
library(extrafont)
```

```
## Registering fonts with R
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

```r
wildlife_impacts <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-07-23/wildlife_impacts.csv")
```

```
## Parsed with column specification:
## cols(
##   .default = col_character(),
##   incident_date = col_datetime(format = ""),
##   num_engs = col_double(),
##   incident_month = col_double(),
##   incident_year = col_double(),
##   time = col_double(),
##   height = col_double(),
##   speed = col_double(),
##   cost_repairs_infl_adj = col_double()
## )
```

```
## See spec(...) for full column specifications.
```



```r
wildlife <- wildlife_impacts%>%
  filter(species != "Unknown bird - medium")%>%
  filter(species != "Unknown bird - small")%>%
  filter(species != "Unknown bird - large")%>%
  filter(species != "Unknown bird")%>%
  filter(!is.na(time_of_day))%>%
  filter(!is.na(height))%>%
  filter(!is.na(speed))%>%
  mutate(level_height = cut(height, breaks = unique(quantile(height, probs = seq(0, 1, length.out = 5))), include.lowest = TRUE))%>%
  count(species = fct_lump(species, 20), level_height, sort = TRUE)%>%
  mutate(species = fct_reorder(species, n))%>%
  ggplot(aes(species, n))+
  geom_col(aes(fill = level_height), width = .6)+
  coord_flip()+
  scale_fill_got_d(option = "Tully")+
  labs(y = "Wildlife Count",
       x = "WIldlife",
       title = "Top 20 Wildlife Struck By Airplane in North America",
       subtitle = "Showing height of impact",
       caption = "Source: | Visualization: Ifeoma Egbogah",
       fill = "Height")+
  theme_solarized_2()+
  theme(plot.title = element_text(family = "Nirmala UI Semilight", colour = "darkred"),
        plot.subtitle = element_text(family = "Nirmala UI Semilight"),
        axis.title = element_text(family = "Segoe UI Historic"),
        axis.text.y = element_text(family = "Segoe UI Historic"),
        legend.text = element_text(family = "Nirmala UI Semilight"),
        legend.title = element_text(family = "Nirmala UI Semilight"),
        plot.caption = element_text(colour = "darkred"))


wildlife1 <- wildlife_impacts%>%
  filter(!is.na(time_of_day))%>%
  group_by(incident_year, time_of_day)%>%
  summarise(time_total = n())%>%
  ungroup()%>%
  group_by(incident_year)%>%
  mutate(day_total = sum(time_total),
         pct = time_total/day_total)%>%
  ggplot(aes(incident_year, time_total))+
  geom_col(aes(fill = time_of_day))+
  theme_solarized()+
  scale_fill_got_d(option = "Tully")+
  scale_x_continuous(breaks = seq(1990, 2019, 5))+
  labs(x = "Year",
       y = "Total",
       fill = "Time of Day",
       title = " Aircraft Strike On Wildlife In North America From 1990 - 2018",
       subtitle = "Catergorised by impact on the time of day",
       caption = "Source: FAA | Visualization: Ifeoma Egbogah")


wildlife2 <- wildlife_impacts%>%
  filter(species != "Unknown bird - medium")%>%
  filter(species != "Unknown bird - small")%>%
  filter(species != "Unknown bird - large")%>%
  filter(species != "Unknown bird")%>%
  filter(!is.na(time_of_day))%>%
  filter(!is.na(height))%>%
  filter(!is.na(sky))%>%
  mutate(level_height = cut(height, breaks = unique(quantile(height, probs = seq(0, 1, length.out = 6))), include.lowest = TRUE))%>%
  mutate(level = case_when(level_height == "[0,30]" ~ "Low",
                           level_height == "(30,500]" ~ "Mid",
                           level_height == "(500,2e+04]" ~ "High"))%>%
  group_by(time_of_day, level, sky)%>%
  summarise(total = n())%>%
  ggplot(aes(y = total, axis1 = time_of_day, axis2 = sky))+
  geom_alluvium(aes(fill = level), width = 0.02)+
  geom_stratum(width = 0.2)+
  geom_text(stat = "stratum", label.strata = TRUE)+
  scale_fill_got_d(option = "Daenerys", labels = c("High (>500)", "Low (<30)", "Mid (30 - 500)"))+
  scale_x_discrete(limits = c("Time of Day", "Sky"), expand = c(0.05, 0.05))+
  theme_light()+
  labs(y = "Total",
       title = "Aircraft Impact On Wildlife in North America",
       subtitle = "Most aircraft strike happen during the day at heights <30 while strikes at night occur at heights >500",
       caption = "Source: FAA | Visualization: Ifeoma Egbogah",
       fill = "Height")+
  theme(plot.title = element_text(family = "Nirmala UI Semilight"),
        plot.subtitle = element_text(family = "Nirmala UI Semilight"),
        axis.title = element_text(family = "Segoe UI Historic"),
        axis.text.y = element_text(family = "Segoe UI Historic"),
        legend.text = element_text(family = "Nirmala UI Semilight"),
        legend.title = element_text(family = "Nirmala UI Semilight"))


wildlife3 <- wildlife_impacts%>%
  filter(species != "Unknown bird - medium")%>%
  filter(species != "Unknown bird - small")%>%
  filter(species != "Unknown bird - large")%>%
  filter(species != "Unknown bird")%>%
  filter(species %in% c( "Brazilian free-tailed bat",  "Microbats", "Coyote", "Eastern red bat", "Red fox", "White-tailed deer", "Striped skunk", "Raccoon", "American alligator"))%>%
  filter(!is.na(time_of_day))%>%
  group_by(species, time_of_day)%>%
  summarise(total = n())%>%
  arrange(desc(total))%>%
  ggplot(aes(y = total, axis1 = species, axis2 = time_of_day))+
  geom_alluvium(aes(fill = time_of_day))+
  geom_stratum()+
  geom_label(stat = "stratum", label.strata = TRUE)+
  scale_fill_got_d(option = "Greyjoy")+
  scale_x_discrete(limits = c("Wildlife", "Time of Day"), expand = c(.05, .05))+
  theme_light()+
  theme(plot.subtitle = element_text(size = 8))+
  labs(y = "Total",
    title = "Aircraft Strike on Some Non-avaian Wildlife",
       subtitle = "Most strikes occur at night compared to avaian strikes which happen during the day",
       fill = "Time of Day",
       caption = "Source: FAA | Visualization: Ifeoma Egbogah")
  

wildlife4 <- wildlife_impacts%>%
  filter(species != "Unknown bird - medium")%>%
  filter(species != "Unknown bird - small")%>%
  filter(species != "Unknown bird - large")%>%
  filter(species != "Unknown bird")%>%
  filter(species %in% c("Gulls", "European starling", "Sparrows", "Blackbirds", "Morning dove", "Rock pigeon", "Barn swallow", "Hawks", "American robin", "Killdeer"))%>%
  filter(!is.na(time_of_day))%>%
  group_by(species, time_of_day)%>%
  summarise(total = n())%>%
  arrange(desc(total))%>%
  ggplot(aes(y = total, axis1 = species, axis2 = time_of_day))+
  geom_alluvium(aes(fill = time_of_day))+
  geom_stratum()+
  geom_label(stat = "stratum", label.strata = TRUE)+
  scale_fill_got_d(option = "Greyjoy")+
  scale_x_discrete(limits = c("Birds", "Time of Day"), expand = c(.05, .05))+
  theme_light()+
  theme(plot.subtitle = element_text(size = 8.5))+
  labs(y= "Total",
    title = "Aircraft Strike on Birds",
       subtitle = "Most strikes occur during the day compared to non-avaian strikes which happen at night", 
       fill = "Time of Day",
       caption = "Source: FAA | Visualization: Ifeoma Egbogah")


ggsave("C:/Users/Egbogah/Desktop/wildlife.png", wildlife, width = 8, height = 8)
ggsave("C:/Users/Egbogah/Desktop/wildlife1.png", wildlife1, width = 8, height = 8)
ggsave("C:/Users/Egbogah/Desktop/wildlife2.png", wildlife2, width = 8, height = 8)
ggsave("C:/Users/Egbogah/Desktop/wildlife3.png", wildlife3, width = 8, height = 8)
ggsave("C:/Users/Egbogah/Desktop/wildlife4.png", wildlife4, width = 8, height = 8)
```


##Choropleth Map

```r
data("fips.state")
data("hawaii")
data("alaska")
data("us_states")



usa <- wildlife_impacts%>%
  filter(state != "N/A")%>%
  left_join(., fips.state, by = c("state" = "STUSAB"))

usa_1 <- usa%>%
  filter(!is.na(STATE_NAME))%>%
  group_by(STATE_NAME)%>%
  summarise(total = n())

usa_states <- us_states%>%
  left_join(., usa_1, by = c("NAME" = "STATE_NAME"))

hawaii <- hawaii%>%
  left_join(., usa_1, by = c("NAME" = "STATE_NAME"))

alaska <- alaska%>%
  left_join(., usa_1, by = c("NAME" = "STATE_NAME"))

usa_states1 <- st_transform(usa_states, 2163)
hawaii1 <- st_transform(hawaii, 2163)
alaska1 <- st_transform(alaska, 2163)

map <- ggplot()+
  geom_sf(data = usa_states1, aes(fill = total))+
  geom_sf(data = alaska1, aes(fill = total))+
  geom_sf(data = hawaii1, aes(fill = total))+
  scale_fill_got(option = "Tully")+
  theme_light()+
  theme_light()+
  theme(plot.title = element_text(family = "Nirmala UI Semilight"),
        plot.subtitle = element_text(family = "Nirmala UI Semilight"),
        legend.text = element_text(family = "Nirmala UI Semilight"),
        legend.title = element_text(family = "Nirmala UI Semilight"))+
  labs(fill = "Total",
       title = "Aircraft Strike On Wildlife in North America from 1990 - 2018",
       caption = "Source: FAA | Visualization: Ifeoma Egbogah")

ggsave("C:/Users/Egbogah/Desktop/wildlife.png", map, width = 8, height = 8)
```

