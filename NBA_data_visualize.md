    library(readr)    # importing data
    library(dplyr)    # data wrangling
    library(ggplot2)  # graphics
    library(jpeg)
    library(grid)

    ## load data
    andre <-read.csv("data/andre-iguodala.csv",stringsAsFactors = FALSE)
    green <-read.csv("data/draymond-green.csv",stringsAsFactors = FALSE)
    durant <-read.csv("data/kevin-durant.csv",stringsAsFactors = FALSE)
    klay <-read.csv("data/klay-thompson.csv",stringsAsFactors = FALSE)
    curry <-read.csv("data/stephen-curry.csv",stringsAsFactors = FALSE)

    ## some configuration set up
    name<-"Andre_Iguodala"
    AI <- cbind(andre,name)

    name<- "Draymond_Green"
    DG <- cbind(green,name)

    name<-"Kevin_Durant"
    KD <- cbind(durant,name)

    name<-"Klay-Thompson"
    KT <- cbind(klay,name)

    name<-"Stephen_Curry"
    SC <-cbind(curry,name)

    AI$shot_made_flag[AI$shot_made_flag=="y"]="made shot"
    AI$shot_made_flag[AI$shot_made_flag=="n"]="missed shot"

    DG$shot_made_flag[DG$shot_made_flag=="y"]="made shot"
    DG$shot_made_flag[DG$shot_made_flag=="n"]="missed shot"

    KD$shot_made_flag[KD$shot_made_flag=="y"]="made shot"
    KD$shot_made_flag[KD$shot_made_flag=="n"]="missed shot"

    KT$shot_made_flag[KT$shot_made_flag=="y"]="made shot"
    KT$shot_made_flag[KT$shot_made_flag=="n"]="missed shot"

    SC$shot_made_flag[SC$shot_made_flag=="y"]="made shot"
    SC$shot_made_flag[SC$shot_made_flag=="n"]="missed shot"

    AI<-mutate(AI,minute=period*12-minutes_remaining)
    DG<-mutate(DG,minute=period*12-minutes_remaining)
    KD<-mutate(KD,minute=period*12-minutes_remaining)
    KT<-mutate(KT,minute=period*12-minutes_remaining)
    SC<-mutate(SC,minute=period*12-minutes_remaining)

    ## load court image
    court_file <- "images/nba-court.jpg"
    court_image <-rasterGrob(readJPEG(court_file), 
      width = unit(1, "npc"), 
      height = unit(1, "npc"))

    andre_iguodala_shot_chart <- ggplot(data = andre) + 
      annotation_custom(court_image, -250, 250, -50, 420) + 
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: andre iguodala (2016 season)' ) + 
      theme_minimal()

    andre_iguodala_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-5-1.png)

    stephen_curry_shot_chart <- ggplot(data = curry) + 
      annotation_custom(court_image, -250, 250, -50, 420) + 
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: stephen_curry (2016 season)' ) + 
      theme_minimal()

    stephen_curry_shot_chart

    ## Warning: Removed 17 rows containing missing values (geom_point).

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-6-1.png)