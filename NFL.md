---
title: "NFL"
author: "Ifeoma Egbogah"
date: "2/11/2020"
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
## -- Attaching packages ---------------------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.0.9000     v purrr   0.3.3     
## v tibble  2.1.3          v dplyr   0.8.4     
## v tidyr   1.0.0          v stringr 1.4.0     
## v readr   1.3.1          v forcats 0.4.0
```

```
## -- Conflicts ------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(grid)
library(scales)
```

```
## 
## Attaching package: 'scales'
```

```
## The following object is masked from 'package:purrr':
## 
##     discard
```

```
## The following object is masked from 'package:readr':
## 
##     col_factor
```

```r
library(RCurl)
```

```
## Loading required package: bitops
```

```
## 
## Attaching package: 'RCurl'
```

```
## The following object is masked from 'package:tidyr':
## 
##     complete
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

```r
library(ggimage)
```

```
## 
## Attaching package: 'ggimage'
```

```
## The following object is masked from 'package:cowplot':
## 
##     theme_nothing
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


##Data

```r
attendance <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-02-04/attendance.csv')
```

```
## Parsed with column specification:
## cols(
##   team = col_character(),
##   team_name = col_character(),
##   year = col_double(),
##   total = col_double(),
##   home = col_double(),
##   away = col_double(),
##   week = col_double(),
##   weekly_attendance = col_double()
## )
```

```r
standings <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-02-04/standings.csv')
```

```
## Parsed with column specification:
## cols(
##   team = col_character(),
##   team_name = col_character(),
##   year = col_double(),
##   wins = col_double(),
##   loss = col_double(),
##   points_for = col_double(),
##   points_against = col_double(),
##   points_differential = col_double(),
##   margin_of_victory = col_double(),
##   strength_of_schedule = col_double(),
##   simple_rating = col_double(),
##   offensive_ranking = col_double(),
##   defensive_ranking = col_double(),
##   playoffs = col_character(),
##   sb_winner = col_character()
## )
```

```r
url_logo <- getURL("https://raw.githubusercontent.com/statsbylopez/BlogPosts/master/nfl_teamlogos.csv")

logo <- read.csv(text = url_logo)

attendance <- attendance%>%
  unite("team2", team:team_name, sep = " ", remove = FALSE)

standings <- standings%>%
  unite("team2", team:team_name, sep = " ", remove = FALSE)
```



##Ranking Of Home and Away Attendance

```r
attendance2 <- attendance%>%
  group_by(team2)%>%
  summarise(total2 = sum(total),
            total_home = sum(home)/10000000,
            total_away = sum(away)/10000000)%>%
  mutate(rank_away = rank(-total_away, ties.method = "min"),
         rank_home = rank(-total_home, ties.method = "min"),
         sq_rank = (rank_home^2) + (rank_away^2)/2,
         rank_order = rank(sq_rank, ties.method = "min"),
         team2 = fct_reorder(str_to_title(team2), rank_order),
         total_away = -total_away)


attendance2 <- attendance2%>%
  left_join(logo, by = c("team2" = "team"))
```

```
## Warning: Column `team2`/`team` joining factors with different levels, coercing
## to character vector
```

```r
attend_rank <- attendance2%>%
  mutate(team2 = fct_reorder(str_to_title(team2), rank_order))%>%
  ggplot(aes(x = team2))+
    geom_col(aes(y = total_home), fill = "darkred", colour = "white")+
    geom_text(aes(y = 8, 
                label = round(total_home, 1), 
            colour = if_else(total_home > 13, "white", "darkred")),
            size = 3,
            fontface = "bold",
            show.legend = FALSE)+
    geom_col(aes(y = total_away), fill = "grey85", colour = "white")+
    geom_text(aes(y = -6,
                label = round(total_away*-1, 1),
            colour = if_else(total_away < -14, "white", "grey45")),
            fontface = "bold",
            size = 3)+
    geom_text(aes(y = -30, x = 30), hjust = 0.5, vjust = 0, label = "HOME&", size = 7, colour = "darkred", family = "Tempus Sans ITC")+
    geom_text(aes(y = -25, x = 20), hjust = 0.4, vjust = 0, label = "AWAY", size = 7, colour = "grey45", family = "Tempus Sans ITC")+
  geom_image(aes(y = if_else(total_home >= 13, 23.5, 12), x = team2, image = url), size = 0.055, data = attendance2)+
    scale_y_continuous(limits = c(-30, 30), expand = c(0, 0))+
    scale_colour_identity()+
    coord_polar()+
    labs(title = "Ranking of Home and Away NFL Stadium Attendance",
         subtitle = "Not much variation in individual team's home and away attendance")+
    theme_nothing()+
   theme(axis.text.x = element_blank(),
        panel.grid = element_blank(),
        axis.title = element_blank(),
        axis.ticks = element_blank(),
        panel.border = element_blank(),
        plot.title = element_text(family = "Poor Richard", face = "bold", size = 25, hjust = 0.5),
        plot.subtitle = element_text(family = "Tempus Sans ITC"),
        plot.caption = element_text(family = "Tempus Sans ITC"))


##Legend
legend_col<- data.frame(Away = 10, ##chose any value to create a tile
                        Home = 10)

legend_away <- legend_col%>%
  ggplot(aes(Away, Home))+
  geom_tile(fill = "grey85")+
  scale_y_continuous(sec.axis = dup_axis())+
  geom_text(aes(Away, Home), label = "Away", family = "Tempus Sans ITC", fontface = "bold", colour = "grey45")+
  theme(axis.text.x = element_blank(),
        axis.text.y = element_blank(),
        axis.title.y.left = element_blank(),
        axis.title.y.right = element_text(angle = 0, vjust = 0.5, colour = "grey45", face = "bold"),
        panel.grid = element_blank(),
        axis.ticks = element_blank(),
        panel.border = element_blank())+
  labs(y = "",
       x = " ")


legend_home <- legend_col%>%
  ggplot(aes(Away, Home))+
  geom_tile(fill = "darkred")+
  scale_y_continuous(sec.axis = dup_axis())+
  geom_text(aes(Away, Home), label = "Home", family = "Tempus Sans ITC", fontface = "bold", colour = "red")+
  theme(axis.text.x = element_blank(),
        axis.text.y = element_blank(),
        axis.title.y.left = element_blank(),
        axis.title.y.right = element_text(angle = 0, vjust = 0.5, colour = "darkred", face = "bold"),
        panel.grid = element_blank(),
        axis.ticks = element_blank(),
        panel.border = element_blank())+
  labs(y = " ",
       x = " ")


plot1 <- ggdraw() +
  draw_plot(attend_rank, 0, 0, 1, 1)+
  draw_plot(legend_home, 0.9, 0.7, 0.1, 0.09)+
  draw_plot(legend_away, 0.9, 0.6335, 0.1, 0.09)
```

```
## Warning: Removed 2 rows containing missing values (geom_image).
```


##Empirical Bayes Ranking of NFL Teams

```r
standings2 <- standings%>%
  group_by(team2)%>%
  summarise(total_wins = sum(wins),
            total_loss = sum(loss))%>%
  mutate(rank_wins = rank(-total_wins, ties.method = "min"),
         rank_loss = rank(total_loss, ties.method = "min"),
         sq_rank = (rank_wins^2) + (rank_loss^2)/2,
         rank_order = rank(sq_rank, ties.method = "min"),
         total = total_wins + total_loss,
         ratio = total_wins/(total_wins + total_loss))

shape_est <- MASS::fitdistr(standings2$ratio, dbeta,
                    start = list(shape1 = 1, shape2 = 1.09))

alpha0 <- shape_est$estimate[1]
beta0 <- shape_est$estimate[2]


standings2 <- standings2%>%
  mutate(eb_estimate = (total_wins + alpha0)/(total + alpha0 + beta0),
         total_loss_neg = -total_loss)%>%
         left_join(logo, by = c("team2" = "team"))
```

```
## Warning: Column `team2`/`team` joining character vector and factor, coercing
## into character vector
```

```r
bayes_rank <- standings2%>%
  mutate(team2 = fct_reorder(team2, eb_estimate, .desc = TRUE))%>%
  ggplot(aes(team2))+
  geom_col(aes(y = total_wins), fill = "steelblue")+
  geom_text(data = standings2, aes(y = if_else(total_wins >= 50, 60, 68), 
                label = total_wins, 
            colour = if_else(total_wins >= 50, "white", "steelblue")),
            size = 3,
            fontface = "bold",
            show.legend = FALSE)+
  geom_col(aes(y = total_loss_neg), fill = "grey85")+
  geom_text(aes(y = if_else(total_loss_neg < -27, -60, -40), 
                label = total_loss, 
            colour = if_else(total_loss_neg < -27, "white", "grey45")),
            size = 3,
            fontface = "bold",
            show.legend = FALSE)+
  geom_image(aes(y = if_else(total_wins >= 182, 258, 205), x = team2, image = url), size = 0.055, data = standings2)+
  geom_text(aes(y = -250, x = 28), hjust = 0.25, vjust = 0, label = "WINS&", size = 7, colour = "steelblue", family = "Tempus Sans ITC")+
    geom_text(aes(y = -200, x = 22), hjust = 0.07, vjust = 0, label = "LOSSES", size = 7, colour = "grey45", family = "Tempus Sans ITC")+
  scale_y_continuous(limits = c(-270, 320))+
  scale_colour_identity()+
  coord_polar()+
  labs(title = "Empirical Bayes Ranking of Teams in the NFL",
       subtitle = "No logo for San Diego Chargers (wins = 136, losses = 136, empiral bayes = 0.5) and St.Louis Rams (wins = 107, losses = 148, empirical bayes = 0.42)",
       caption = "Source: Pro Football Reference | Visualization: @negbogah")+
  theme_nothing()+
  theme(axis.text.x = element_blank(),
        panel.grid = element_blank(),
        axis.title = element_blank(),
        axis.ticks = element_blank(),
        panel.border = element_blank(),
        plot.title = element_text(family = "Poor Richard", face = "bold", size = 25, hjust = 0.5),
        plot.subtitle = element_text(family = "Tempus Sans ITC"),
        plot.caption = element_text(family = "Tempus Sans ITC"))


##Legend
legend_col2<- data.frame(Wins = 10,
                        Loss = 10)

legend_loss <- legend_col2%>%
  ggplot(aes(Wins, Loss))+
  geom_tile(fill = "grey85")+
  scale_y_continuous(sec.axis = dup_axis())+
  geom_text(aes(Wins, Loss), label = "Loss", family = "Tempus Sans ITC", fontface = "bold", colour = "white")+
  theme(axis.text.x = element_blank(),
        axis.text.y = element_blank(),
        axis.title.y.left = element_blank(),
        axis.title.y.right = element_text(angle = 0, vjust = 0.5, colour = "grey45", face = "bold"),
        panel.grid = element_blank(),
        axis.ticks = element_blank(),
        panel.border = element_blank())+
  labs(y = "",
       x = " ")


legend_wins <- legend_col2%>%
  ggplot(aes(Wins, Loss))+
  geom_tile(fill = "steelblue")+
  scale_y_continuous(sec.axis = dup_axis())+
  geom_text(aes(Wins, Loss), label = "Wins", family = "Tempus Sans ITC", fontface = "bold", colour = "white")+
  theme(axis.text.x = element_blank(),
        axis.text.y = element_blank(),
        axis.title.y.left = element_blank(),
        axis.title.y.right = element_text(angle = 0, vjust = 0.5, colour = "steelblue", face = "bold"),
        panel.grid = element_blank(),
        axis.ticks = element_blank(),
        panel.border = element_blank())+
  labs(y = " ",
       x = " ")


plot2 <- ggdraw() +
  draw_plot(bayes_rank, 0, 0, 1, 1)+
  draw_plot(legend_wins, 0.9, 0.7, 0.1, 0.09)+
  draw_plot(legend_loss, 0.9, 0.6335, 0.1, 0.09)
```

```
## Warning: Removed 2 rows containing missing values (geom_image).
```

```r
nfl_plot <- plot_grid(plot1, plot2, ncol = 2)

#ggsave("~NFL/nfl_plot.png", nfl_plot, width = 24, height = 16)
```



