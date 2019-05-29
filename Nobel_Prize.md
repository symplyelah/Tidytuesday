---
title: "NobelPrizeWinners"
author: "Ifeoma Egbogah"
date: "5/23/2019"
output: 
  html_document: 
    keep_md: yes
editor_options: 
  chunk_output_type: console
---




```r
library(tmap)
library(tmaptools)
library(tidyverse)
```

```
## -- Attaching packages ------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.1.1       v purrr   0.3.2  
## v tibble  2.1.1       v dplyr   0.8.0.1
## v tidyr   0.8.3       v stringr 1.4.0  
## v readr   1.3.1       v forcats 0.4.0
```

```
## -- Conflicts ---------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
theme_set(theme_light())

nobel_winners <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-14/nobel_winners.csv")
```

```
## Parsed with column specification:
## cols(
##   prize_year = col_double(),
##   category = col_character(),
##   prize = col_character(),
##   motivation = col_character(),
##   prize_share = col_character(),
##   laureate_id = col_double(),
##   laureate_type = col_character(),
##   full_name = col_character(),
##   birth_date = col_date(format = ""),
##   birth_city = col_character(),
##   birth_country = col_character(),
##   gender = col_character(),
##   organization_name = col_character(),
##   organization_city = col_character(),
##   organization_country = col_character(),
##   death_date = col_date(format = ""),
##   death_city = col_character(),
##   death_country = col_character()
## )
```

```r
#Remove duplicates of Nobel Winners
nobel_winners <- nobel_winners%>%
  arrange(full_name, prize_year, category, motivation)%>%
  distinct(full_name, prize_year, category, motivation, .keep_all = TRUE)
```

#Laureates who have won the Nobel Prize More Than Once. ICRC have won it thrice.
(Marie Curie won her Nobel Prizes in the Physics and Chemistry Categories)

```r
#Laureates who have won more than once.
multiple<-nobel_winners%>%
  separate(prize_share, c("number", "share"))%>%
 count(full_name, laureate_id, sort=TRUE)%>%
  head(6)%>%
  mutate(full_name = fct_reorder(full_name, n))%>%
  ggplot(aes(full_name, n))+
  geom_col(fill= "hotpink2")+
  coord_flip()+
  labs(title= "Laureates who have won the Nobel Prize More Than Once",
       subtitle = "Marie Curie won her Nobel Prizes in the Physics and Chemistry Categories", x="Name of Laureates", y="Number of Nobel Prizes", caption = "Source: Kaggle / Visualization: Ifeoma Egbogah")

multiple
```

![](Nobel_Prize_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
ggsave("multiple.png", multiple, width = 16, height =10)
```

#Age Distribution of Laureates By Category

```r
#Age at the time of Award
Age <- nobel_winners%>%
  mutate(age = prize_year - lubridate::year(birth_date))%>%
  filter(!is.na(age))%>%
  ggplot(aes(category, age, fill=gender))+
  geom_boxplot()+
  scale_fill_manual(values = c("hotpink","skyblue"))+
  labs(x = "Category", y= "Age of Laureates", title = "Age Distribution of Noble Prize Winners By Category and Gender", subtitle = "Malala Yousafzai (at 17yrs) is the youngest laureate (Peace) and Leonid Hurwicz (at 90yrs) is the oldest laureate (Economics)", caption = "Source: Kaggle / Visualization: Ifeoma Egbogah", fill="Gender")

Age
```

![](Nobel_Prize_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
ggsave("Age.png", Age, width = 16, height =10)
```



```r
#Separating the birth country column because old countries names were merged with new countries names

by_country<- nobel_winners%>%
  filter(!is.na(birth_country))%>%
  separate(birth_country, c("old_name", "country"), sep= "\\(", extra = "merge", fill = "right")%>%
  separate(country, c("country", NA), sep="[)]")%>%
  mutate(country = coalesce(country, old_name))

#Top and Bottom countries With Nobel Prize Wins
Top.Bottom<- by_country%>%
  filter(!is.na(country))%>%
  count(country, sort=TRUE)%>%
  arrange(desc(n))%>%
  slice(c(1:15, seq(n()- 15, n())))%>%
   mutate(country = fct_reorder(country, n))%>%
  ggplot(aes(country, n))+
  geom_point(colour="hotpink4")+
  coord_flip()+
  labs(title = "Top 15 Countries with the Most Nobel Prize Wins",
       subtitle = "Bottom 15 Countries with the least Nobel Prize Wins",
       x= "Number of Nobel Prize wins", y="Country", caption = "Source: Kaggle / Visualization: Ifeoma Egbogah")

Top.Bottom
```

![](Nobel_Prize_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

```r
ggsave("Top.Bottom.png", Top.Bottom, width = 16, height =10)
```


#Mapping the Nobel Prize Winners Based on their Country of Birth

```r
#Changing the names of some countries and getting the iso3 codes for each country to enable merging with the map data (World)

map <- by_country%>%
  mutate(country =str_replace(country, "Scotland", "United Kingdom" ))%>%
  mutate(country =str_replace(country, "Northern Ireland", "United Kingdom"))%>%
  mutate(iso3 = countrycode::countrycode(country, "country.name", "iso3c"))%>%
  mutate(country1 = countrycode::countrycode(iso3, "iso3c", "country.name"))


map2 <- map%>%
  group_by(iso3)%>%
  filter(!is.na(iso3))%>%
  summarise(Total = n())%>%
  arrange(desc(Total))


data("World")
data("land", "rivers")


map3 <- append_data(World, map2, key.data ="iso3", key.shp = "iso_a3", ignore.na = TRUE, ignore.duplicates = TRUE)
```

```
## Under coverage: 103 out of 177 shape features did not get appended data. Run under_coverage() to get the corresponding feature id numbers and key values.
```

```
## Over coverage: 2 out of 76 data records were not appended. Run over_coverage() to get the corresponding data row numbers and key values.
```

```r
winners<-tm_shape(land)+
  tm_raster("elevation", legend.show = FALSE)+
  tm_shape(rivers)+
   tm_lines("lightblue", lwd = "strokelwd", scale = 1.5, legend.lwd.show = FALSE)+
   tm_shape(World, is.master = TRUE)+
   tm_borders("grey20", lwd = .5)+
  tm_grid(projection = "longlat", labels.size = 0.4, lwd = 0.25)+
  tm_shape(map3)+
  tm_dots("Total", col="Total", palette="Spectral", n=10, scale = 1.4, legend.show = FALSE)+
  tm_style("beaver", bg.color = "lightblue", space.color = "gray90")+
  tm_layout(main.title= "Number of Nobel Prize Winners By Country", earth.boundary = TRUE, inner.margins = c(0.04, 0.04, 0.03, 0.02), title.size = 1.1, main.title.position = c("center") ,  frame = FALSE, legend.position = c("left", "bottom"))+
   tm_compass( type = "rose", position = c(0.08, 0.45), size = 3, show.labels =1)+
  tm_credits("Source: Kaggle / Visualization: Ifeoma Egbogah")

winners
```

```
## Variable "elevation" contains positive and negative values, so midpoint is set to 0. Set midpoint = NA to show the full spectrum of the color palette.
```

![](Nobel_Prize_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

```r
tmap_save(winners, "winners.png")
```

```
## Variable "elevation" contains positive and negative values, so midpoint is set to 0. Set midpoint = NA to show the full spectrum of the color palette.
```

```
## Map saved to C:\Users\Egbogah\Desktop\Tidytuesday\Tidytuesday\winners.png
```

```
## Resolution: 2100 by 1500 pixels
```

```
## Size: 6.999999 by 4.999999 inches (300 dpi)
```

