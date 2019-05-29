---
title: "Plastic Waste"
author: "Ifeoma Egbogah"
date: "5/29/2019"
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
library(countrycode)
library(patchwork)
library(cartogram)
library(tmap)
library(tmaptools)

theme_set(theme_light())

coast_vs_waste <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-21/coastal-population-vs-mismanaged-plastic.csv")
```

```
## Parsed with column specification:
## cols(
##   Entity = col_character(),
##   Code = col_character(),
##   Year = col_double(),
##   `Mismanaged plastic waste (tonnes)` = col_double(),
##   `Coastal population` = col_double(),
##   `Total population (Gapminder)` = col_double()
## )
```

```r
mismanaged_vs_gdp <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-21/per-capita-mismanaged-plastic-waste-vs-gdp-per-capita.csv")
```

```
## Parsed with column specification:
## cols(
##   Entity = col_character(),
##   Code = col_character(),
##   Year = col_double(),
##   `Per capita mismanaged plastic waste (kilograms per person per day)` = col_double(),
##   `GDP per capita, PPP (constant 2011 international $) (Rate)` = col_double(),
##   `Total population (Gapminder)` = col_double()
## )
```

```r
waste_vs_gdp <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-05-21/per-capita-plastic-waste-vs-gdp-per-capita.csv")
```

```
## Parsed with column specification:
## cols(
##   Entity = col_character(),
##   Code = col_character(),
##   Year = col_double(),
##   `Per capita plastic waste (kilograms per person per day)` = col_double(),
##   `GDP per capita, PPP (constant 2011 international $) (constant 2011 international $)` = col_double(),
##   `Total population (Gapminder)` = col_double()
## )
```

#Making A Cartogram

```r
data("World")

mismanaged2010 <- mismanaged_vs_gdp%>%
  left_join(coast_vs_waste)
```

```
## Joining, by = c("Entity", "Code", "Year", "Total population (Gapminder)")
```

```r
mismanaged2010 <- mismanaged2010%>%
  filter(!is.na(`Coastal population`))

write.csv(mismanaged2010, "mismanaged.csv")
  
mismanaged <- read.csv("mismanaged.csv", stringsAsFactors = TRUE)

World<- World%>%
  left_join(., mismanaged,  by= c("iso_a3" = "Code"))
```

```
## Warning: Column `iso_a3`/`Code` joining factors with different levels,
## coercing to character vector
```

```r
pop<- cartogram_cont(World, "Coastal.population")
```

```
## Warning in cartogram_cont.sf(World, "Coastal.population"): NA not allowed
## in weight vector. Features will be removed from Shape.
```

```
## Mean size error for iteration 1: 5.96135666412582
```

```
## Mean size error for iteration 2: 4.5367091433226
```

```
## Mean size error for iteration 3: 3.65188064184146
```

```
## Mean size error for iteration 4: 3.03685463783067
```

```
## Mean size error for iteration 5: 2.58025978647357
```

```
## Mean size error for iteration 6: 2.26682560089486
```

```
## Mean size error for iteration 7: 2.032648986634
```

```
## Mean size error for iteration 8: 1.96378418534996
```

```
## Mean size error for iteration 9: 1.81722084957751
```

```
## Mean size error for iteration 10: 3.45575272162814
```

```
## Mean size error for iteration 11: 1.78088645917166
```

```
## Mean size error for iteration 12: 1.51950949159299
```

```
## Mean size error for iteration 13: 1.40906616739079
```

```
## Mean size error for iteration 14: 1.37200359975342
```

```
## Mean size error for iteration 15: 1.37202091921589
```

```r
cart <- tm_shape(pop)+
  tm_polygons("Mismanaged.plastic.waste..tonnes.", n=6, style = "jenks", palette = "viridis", n=20, title= "Mismanged Plastic Waste(tonnes)")+
  tm_text("Entity", size = "Coastal.population", legend.size.show = FALSE)+
  tm_style("beaver", bg.color = "lightblue", space.color = "gray90")+
  tm_layout(main.title = "Cartogram of Coastal Population, 2010", title.size = 0.7, title ="Colour shows plastic waste that is not properly managed by a given country (2010)",frame = FALSE, legend.position = c("left", "bottom"))+
  tm_credits("Source: Our World in Data | Visualisation: Ifeoma Egbogah")

cart
```

![](Plastic_Waste_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

```r
#By absolute quantity China is the largest contributor of mismanaged waste but however its relative contribution (i.e kg per person per day) is not as much as Sri Lanka, Malaysia, Thailand, Egypt, Vanuatu, South Africa, Syria or Trinidad.

waste<- cartogram_cont(World, "Mismanaged.plastic.waste..tonnes.")
```

```
## Warning in cartogram_cont.sf(World, "Mismanaged.plastic.waste..tonnes."):
## NA not allowed in weight vector. Features will be removed from Shape.
```

```
## Mean size error for iteration 1: 8.49114439858092
```

```
## Mean size error for iteration 2: 7.39783544864386
```

```
## Mean size error for iteration 3: 6.54029800795259
```

```
## Mean size error for iteration 4: 5.81889530590688
```

```
## Mean size error for iteration 5: 5.1790061496559
```

```
## Mean size error for iteration 6: 4.60623064857242
```

```
## Mean size error for iteration 7: 4.11093907211164
```

```
## Mean size error for iteration 8: 3.68946836266015
```

```
## Mean size error for iteration 9: 3.34746207120233
```

```
## Mean size error for iteration 10: 3.09361210415155
```

```
## Mean size error for iteration 11: 3.44857091304242
```

```
## Mean size error for iteration 12: 2.90182918496688
```

```
## Mean size error for iteration 13: 2.56974587994786
```

```
## Mean size error for iteration 14: 2.4168041180021
```

```
## Mean size error for iteration 15: 2.25952651080385
```

```r
cart1 <- tm_shape(waste)+
  tm_polygons("Per.capita.mismanaged.plastic.waste..kilograms.per.person.per.day.", n=6, style = "jenks", palette = "viridis", n=10, title= "Plastic waste per person per day")+
  tm_text("Entity", size = "Mismanaged.plastic.waste..tonnes.", legend.size.show = FALSE)+
  tm_style("beaver", bg.color = "lightblue", space.color = "gray90")+
  tm_layout(main.title = "Cartogram of Mismanaged Plastic Waste (tonnes)", title.size = 0.6, title ="By absolute quantity, China is the largest contributor of mismanaged waste but its relative contribution (kg per person per day) is low",  frame = FALSE, legend.position = c("left", "bottom"))+
  tm_credits("Source: Our World in Data | Visualisation: Ifeoma Egbogah")

cart1
```

![](Plastic_Waste_files/figure-html/unnamed-chunk-2-2.png)<!-- -->

```r
GDP<- cartogram_cont(World, "GDP.per.capita..PPP..constant.2011.international.....Rate.")
```

```
## Warning in cartogram_cont.sf(World, "GDP.per.capita..PPP..constant.
## 2011.international.....Rate."): NA not allowed in weight vector. Features
## will be removed from Shape.
```

```
## Mean size error for iteration 1: 17.5212301717016
```

```
## Mean size error for iteration 2: 11.3899072914541
```

```
## Mean size error for iteration 3: 7.68652541910705
```

```
## Mean size error for iteration 4: 5.54083875851991
```

```
## Mean size error for iteration 5: 4.29483888220633
```

```
## Mean size error for iteration 6: 3.53477554012764
```

```
## Mean size error for iteration 7: 3.0456113853112
```

```
## Mean size error for iteration 8: 2.71352113131319
```

```
## Mean size error for iteration 9: 2.48169024029841
```

```
## Mean size error for iteration 10: 2.31476992477852
```

```
## Mean size error for iteration 11: 2.19113185511479
```

```
## Mean size error for iteration 12: 2.09866014768019
```

```
## Mean size error for iteration 13: 2.0283231297781
```

```
## Mean size error for iteration 14: 1.96922467658105
```

```
## Mean size error for iteration 15: 1.90458412788161
```

```r
cart2 <- tm_shape(GDP)+
  tm_polygons("Mismanaged.plastic.waste..tonnes.", n=6, style = "jenks", palette = "viridis", n=20, title= "Mismanged Plastic Waste(tonnes)")+
  tm_text("Entity", size = "GDP.per.capita..PPP..constant.2011.international.....Rate.", legend.size.show = FALSE)+
  tm_style("beaver", bg.color = "lightblue", space.color = "gray90")+
  tm_layout(main.title = "Cartogram of GDP per capita (2010)", title.size = 0.7, title = "Countries with high GDP have low mismanaged plastic waste",  frame = FALSE, legend.position = c("left", "bottom"))+
  tm_credits("Source: Our World in Data | Visualisation: Ifeoma Egbogah")

cart2
```

![](Plastic_Waste_files/figure-html/unnamed-chunk-2-3.png)<!-- -->

```r
tmap_save(cart, "cart.png", width = 16, height = 10)
```

```
## Map saved to C:\Users\Egbogah\Desktop\Tidytuesday\Tidytuesday\cart.png
```

```
## Resolution: 4800 by 3000 pixels
```

```
## Size: 16 by 10 inches (300 dpi)
```

```r
tmap_save(cart1, "cart1.png", width = 16, height = 10)
```

```
## Map saved to C:\Users\Egbogah\Desktop\Tidytuesday\Tidytuesday\cart1.png
```

```
## Resolution: 4800 by 3000 pixels
```

```
## Size: 16 by 10 inches (300 dpi)
```

```r
tmap_save(cart2, "cart2.png", width = 16, height =10)
```

```
## Map saved to C:\Users\Egbogah\Desktop\Tidytuesday\Tidytuesday\cart2.png
```

```
## Resolution: 4800 by 3000 pixels
```

```
## Size: 16 by 10 inches (300 dpi)
```

