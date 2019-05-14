---
title: "UNESCO"
author: "Ifeoma Egbogah"
date: "5/14/2019"
output: 
  html_document: 
    keep_md: yes
---

##Analysising the global student to teacher ratio dataset from UNESCO

```r
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
library(patchwork)
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
library(countrycode)

theme_set(theme_light())

unesco <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-07/student_teacher_ratio.csv")
```

```
## Parsed with column specification:
## cols(
##   edulit_ind = col_character(),
##   indicator = col_character(),
##   country_code = col_character(),
##   country = col_character(),
##   year = col_double(),
##   student_ratio = col_double(),
##   flag_codes = col_character(),
##   flags = col_character()
## )
```

##Changing the name of the country Cote d'Ivoire to Ivory Coast

```r
IV <-unesco%>%
  filter(country == "Côte d'Ivoire")%>%
  mutate(country = paste0("Ivory Coast"))
  
unesco2 <- unesco%>%
  filter(country != "Côte d'Ivoire")%>%
  bind_rows(IV)
```

##Calculating the change in student/teacher ratio for primary education between 2012 and 2015

```r
#Subset data/calculating change in student to teacher ratio

unesco_spread <- unesco2%>%
  mutate(continent = countrycode(country_code, "iso3c", "continent"))%>%
  mutate(region = countrycode(country_code, "iso3c", "region"))%>%
  filter(!is.na(region))%>%
  filter(!is.na(continent))%>%
  filter(indicator == c("Primary Education"))%>%
  mutate(year = paste0("Y", year))%>%
  spread(year, student_ratio)%>%
  mutate(current=Y2015,
         change =Y2015 - Y2012)
```

```
## Warning in countrycode(country_code, "iso3c", "continent"): Some values were not matched unambiguously: 40030, 40041, 40042, 40043, 40044, 40330, 40334, 40344, 40500, 40505, 40510, 40515, 40520, 40525, 40530, 40535, 40540, 40550, 40603, 40606, 40611, 40612, 40613, 40614, 40616, 40617, 40618, 40619, 40620, 40630, 40640, 40642, 40650, 40656, 40675
```

```
## Warning in countrycode(country_code, "iso3c", "region"): Some values were not matched unambiguously: 40030, 40041, 40042, 40043, 40044, 40330, 40334, 40344, 40500, 40505, 40510, 40515, 40520, 40525, 40530, 40535, 40540, 40550, 40603, 40606, 40611, 40612, 40613, 40614, 40616, 40617, 40618, 40619, 40620, 40630, 40640, 40642, 40650, 40656, 40675
```

```r
#Visualization

  plot_region<- function(data) {
    
    ggplot(data, aes(x= current, y = change, group= region, colour=continent))+
    geom_point(show.legend = FALSE)+
    geom_hline(aes(yintercept=0), show.legend = FALSE)+
  facet_wrap(~region)+
    geom_text(aes(label=country), vjust = 1, hjust = 1, check_overlap = TRUE, show.legend = FALSE)+
    scale_y_continuous(limits = c(-20, 20))+
    expand_limits(x=0)+
    labs(x="Student/Teacher ratio", y="Change")
  
}

by_region<- unesco_spread%>%
  group_by(current, change, region, continent)%>%
  split(.$continent)%>%
  purrr::map(plot_region)


  
plot_continent<-function(data) {
  
  ggplot(data, aes(current, change, ratio, colour=continent))+
  geom_point(show.legend = FALSE)+
  geom_hline(aes(yintercept = 0), show.legend = FALSE)+
  facet_wrap(~continent, ncol=1)+
  geom_text(aes(label=country), vjust = 1, hjust = 1, check_overlap = TRUE, show.legend = FALSE)+
    scale_y_continuous(limits = c(-20, 20))+
    expand_limits(x=0)+
    labs(x="Student/Teacher ratio", y="Change")
  
}


by_continent<-unesco_spread%>%
  group_by(current, change, continent)%>%
  split(.$continent)%>%
  purrr:: map(plot_continent)


cont <-wrap_plots(by_continent) + plot_annotation(title="Change in Student/Teacher Ratio For Primary Education Between 2012 and 2015", 
caption = "Data:UNESCO Institute Of Statistic| Graphics: Ifeoma Egbogah")

reg <-wrap_plots(by_region)+ plot_annotation(title="Change in Student/Teacher Ratio For Primary Education Between 2012 and 2015", 
caption = "Data:UNESCO Institute Of Statistic| Graphics: Ifeoma Egbogah")


ggsave("unesco.png", cont, width = 16, height =10)
```

```
## Warning: Removed 20 rows containing missing values (geom_point).
```

```
## Warning: Removed 20 rows containing missing values (geom_text).
```

```
## Warning: Removed 15 rows containing missing values (geom_point).
```

```
## Warning: Removed 15 rows containing missing values (geom_text).
```

```
## Warning: Removed 19 rows containing missing values (geom_point).
```

```
## Warning: Removed 19 rows containing missing values (geom_text).
```

```
## Warning: Removed 17 rows containing missing values (geom_point).
```

```
## Warning: Removed 17 rows containing missing values (geom_text).
```

```
## Warning: Removed 9 rows containing missing values (geom_point).
```

```
## Warning: Removed 9 rows containing missing values (geom_text).
```

```r
ggsave("unesco2.png", reg, width = 16, height =10)
```

```
## Warning: Removed 20 rows containing missing values (geom_point).
```

```
## Warning: Removed 20 rows containing missing values (geom_text).
```

```
## Warning: Removed 15 rows containing missing values (geom_point).
```

```
## Warning: Removed 15 rows containing missing values (geom_text).
```

```
## Warning: Removed 19 rows containing missing values (geom_point).
```

```
## Warning: Removed 19 rows containing missing values (geom_text).
```

```
## Warning: Removed 17 rows containing missing values (geom_point).
```

```
## Warning: Removed 17 rows containing missing values (geom_text).
```

```
## Warning: Removed 9 rows containing missing values (geom_point).
```

```
## Warning: Removed 9 rows containing missing values (geom_text).
```

