---
title: "Moore's Law"
author: "Ifeoma Egbogah"
date: "9/11/2019"
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
## -- Attaching packages --------------------------------------------------- tidyverse 1.2.1 --
```

```
## v ggplot2 3.2.0     v purrr   0.3.2
## v tibble  2.1.3     v dplyr   0.8.1
## v tidyr   0.8.3     v stringr 1.4.0
## v readr   1.3.1     v forcats 0.4.0
```

```
## -- Conflicts ------------------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(ggthemes)
library(gameofthrones)
library(extrafont)
```

```
## Registering fonts with R
```

```r
loadfonts()
```

```
## Arial Black already registered with pdfFonts().
```

```
## Arial already registered with pdfFonts().
```

```
## Bahnschrift already registered with pdfFonts().
```

```
## Calibri already registered with pdfFonts().
```

```
## Calibri Light already registered with pdfFonts().
```

```
## No regular (non-bold, non-italic) version of Cambria. Skipping setup for this font.
```

```
## Candara already registered with pdfFonts().
```

```
## Comic Sans MS already registered with pdfFonts().
```

```
## Consolas already registered with pdfFonts().
```

```
## Constantia already registered with pdfFonts().
```

```
## Corbel already registered with pdfFonts().
```

```
## Courier New already registered with pdfFonts().
```

```
## Ebrima already registered with pdfFonts().
```

```
## Franklin Gothic Medium already registered with pdfFonts().
```

```
## Gabriola already registered with pdfFonts().
```

```
## Gadugi already registered with pdfFonts().
```

```
## Georgia already registered with pdfFonts().
```

```
## HoloLens MDL2 Assets already registered with pdfFonts().
```

```
## More than one version of regular/bold/italic found for HP Simplified. Skipping setup for this font.
```

```
## HP Simplified Light already registered with pdfFonts().
```

```
## Impact already registered with pdfFonts().
```

```
## Ink Free already registered with pdfFonts().
```

```
## Javanese Text already registered with pdfFonts().
```

```
## Leelawadee UI already registered with pdfFonts().
```

```
## Leelawadee UI Semilight already registered with pdfFonts().
```

```
## Lucida Console already registered with pdfFonts().
```

```
## Lucida Sans Unicode already registered with pdfFonts().
```

```
## Malgun Gothic already registered with pdfFonts().
```

```
## Malgun Gothic Semilight already registered with pdfFonts().
```

```
## Marlett already registered with pdfFonts().
```

```
## Microsoft Himalaya already registered with pdfFonts().
```

```
## Microsoft Yi Baiti already registered with pdfFonts().
```

```
## Microsoft New Tai Lue already registered with pdfFonts().
```

```
## Microsoft PhagsPa already registered with pdfFonts().
```

```
## Microsoft Sans Serif already registered with pdfFonts().
```

```
## Microsoft Tai Le already registered with pdfFonts().
```

```
## Mongolian Baiti already registered with pdfFonts().
```

```
## MV Boli already registered with pdfFonts().
```

```
## Myanmar Text already registered with pdfFonts().
```

```
## Nirmala UI already registered with pdfFonts().
```

```
## Nirmala UI Semilight already registered with pdfFonts().
```

```
## Palatino Linotype already registered with pdfFonts().
```

```
## Segoe MDL2 Assets already registered with pdfFonts().
```

```
## Segoe Print already registered with pdfFonts().
```

```
## Segoe Script already registered with pdfFonts().
```

```
## Segoe UI already registered with pdfFonts().
```

```
## Segoe UI Light already registered with pdfFonts().
```

```
## Segoe UI Semibold already registered with pdfFonts().
```

```
## Segoe UI Semilight already registered with pdfFonts().
```

```
## Segoe UI Black already registered with pdfFonts().
```

```
## Segoe UI Emoji already registered with pdfFonts().
```

```
## Segoe UI Historic already registered with pdfFonts().
```

```
## Segoe UI Symbol already registered with pdfFonts().
```

```
## SimSun-ExtB already registered with pdfFonts().
```

```
## Sylfaen already registered with pdfFonts().
```

```
## Symbol already registered with pdfFonts().
```

```
## Tahoma already registered with pdfFonts().
```

```
## Times New Roman already registered with pdfFonts().
```

```
## Trebuchet MS already registered with pdfFonts().
```

```
## Verdana already registered with pdfFonts().
```

```
## Webdings already registered with pdfFonts().
```

```
## Wingdings already registered with pdfFonts().
```

```r
#font_import(device = "win")


cpu <- readr::read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-09-03/cpu.csv")
```

```
## Parsed with column specification:
## cols(
##   processor = col_character(),
##   transistor_count = col_double(),
##   date_of_introduction = col_double(),
##   designer = col_character(),
##   process = col_double(),
##   area = col_double()
## )
```

```r
moore_law <- cpu%>%
  filter(!is.na(transistor_count), !is.na(area))%>%
  ggplot(aes(date_of_introduction, transistor_count))+
  geom_point(aes(size = area, colour = designer))+
  geom_text(aes(label = designer), family = "Gabriola", size = 4, hjust = -0.2, vjust = -0.2, check_overlap = TRUE)+
  #geom_smooth(method = "auto", lty = 2, colour = "darkred")+
  scale_colour_got_d(option = "wildfire")+
  scale_x_continuous(breaks = seq(1970, 2020, 5), limits = c(1970, 2022))+
  scale_y_log10(labels = scales::comma)+
  theme_minimal()+
  theme(plot.title = element_text(family = "Leelawadee UI", colour = "black"),
        axis.title = element_text(family = "Microsoft Himalaya", size = 15, colour = "black"),
        plot.subtitle = element_text(family = "Microsoft Himalaya", size = 15),
        plot.caption = element_text(family = "Gabriola", size = 11))+
  guides(colour = F)+
  labs(y = "Transistor Count (Log)",
       x = "Date of Introduction",
       title = "The Number of Transistors (1970 - 2018)",
       subtitle = "Moore's Law: The Number of Transistors Doubles Approximately Every Two Years",
       size = expression(paste("Area of Chip ", (mm^2))),
       caption = "Data Source: Wikipedia | Visualization: Ifeoma Egbogah")



area_of_chip <- cpu%>%
  filter(!is.na(transistor_count), !is.na(area))%>%
  mutate(year = as.integer(date_of_introduction))%>%
  group_by(designer, year)%>%
  ggplot(aes(transistor_count, area, label = designer))+
  geom_point(aes(size = area, colour = designer), show.legend = FALSE)+
  geom_text(hjust = -0.2, vjust = -0.2, check_overlap = TRUE)+
  geom_smooth(method = "auto")+
  scale_x_continuous(labels = scales::comma)+
  scale_colour_got_d(option = "tully")+
  guides(size = F, colour = F)+
  theme_minimal()+
  theme(plot.title = element_text(family = "Leelawadee UI", colour = "black", size = 30),
        axis.title = element_text(family = "Microsoft Himalaya", size = 19, colour = "black"),
        plot.subtitle = element_text(family = "Microsoft Himalaya", size = 18),
        plot.caption = element_text(family = "Gabriola", size = 11)) +
    labs(title = "The Number of Transistors on Intergrated Circuit Chip",
       subtitle = "Processing power amongst other things is mostly dependent on the transistor count in a chip. The Transistor count also determines the size or area footprint of the chip. 
The plot shows the Area of chip over time plateaued at about 800 sq mm and a regression analysis shows no significant increase in area is likely.
Newer technologies favour smaller footprint or Area of chip while still boasting high processing power. In this bracket you have chips with transistor count between\n5 Billion - 10 Billion integrated within an area of less than 300 sq mm. 
While some older devices still use the likes of IBM processors with similar processing power and bigger footprint, newer designers go for the likes of AMD,\nQualcomm and Apple, which provides the high processing power in a much reduced area, with Qaulcomm boasting the highest processing power close to 20 Billion transistors within 400 sq mm",
       y = expression(paste("Area of Chip ", (mm^2))),
       x = "Transistor Count",
       caption = "Data Source: Wikipedia | Visualization: Ifeoma Egbogah")


ggsave("area.png", area_of_chip, height = 7, width = 15)
```

```
## `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

```r
ggsave("mooore.png", moore_law, height = 5, width = 10)
```

