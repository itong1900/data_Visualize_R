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

Andre\_iguodala shot-chart
--------------------------

    andre_iguodala_shot_chart <- ggplot(data = andre) + 
      annotation_custom(court_image, -250, 250, -50, 420) + 
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: andre iguodala (2016 season)' ) + 
      theme_minimal()

    andre_iguodala_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-5-1.png)

Draymond\_Green shot-chart
--------------------------

    Draymond_Green_shot_chart <- ggplot(data = green) + 
      annotation_custom(court_image, -250, 250, -50, 420) + 
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: Draymond_Green (2016 season)' ) + 
      theme_minimal()
    Draymond_Green_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-6-1.png)

Kevin Durant shot-chart
-----------------------

    kevin_durant_shot_chart <- ggplot(data = durant) + 
      annotation_custom(court_image, -250, 250, -50, 420) + 
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: kevin_durant (2016 season)' ) + 
      theme_minimal()
    kevin_durant_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-7-1.png)

Klay Thompson Shot-chart
------------------------

    klay_shot_chart <- ggplot(data = klay) + 
      annotation_custom(court_image, -250, 250, -50, 420) +
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: Klay Thompson (2016 season)' ) + 
      theme_minimal()
    klay_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-8-1.png)

stephen Curry shot-chart
------------------------

    stephen_curry_shot_chart <- ggplot(data = curry) + 
      annotation_custom(court_image, -250, 250, -50, 420) + 
      geom_point(aes(x = x, y = y, color = shot_made_flag)) + 
      ylim(-50, 420) + ggtitle( 'Shot Chart: stephen_curry (2016 season)' ) + 
      theme_minimal()

    stephen_curry_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-9-1.png)

    gsw <- rbind(AI,SC,KD,DG,KT)

    gsw_shot_chart <- ggplot(data = gsw)+
      annotation_custom(court_image,-250,250,-50,420)+
      geom_point(aes(x = x, y = y, color = shot_made_flag)) +
      ylim(-50, 420) + ggtitle( 'Shot Chart: gsw (2016 season)' ) + 
      theme_minimal() + facet_wrap(~name)
    gsw_shot_chart

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-10-1.png)

Graphic displaying goal percentage VS shot distance
---------------------------------------------------

    dat <- gsw %>%
      group_by(shot_distance)%>%
      summarise(total_dista_shot = length(shot_made_flag),
                made_dista_shot = sum(shot_made_flag =='made shot'),
                perc2 = made_dista_shot/total_dista_shot)
    ggplot(data = dat, aes(x = shot_distance, y = perc2)) + geom_point()

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-11-1.png)

Total number of shots by minute of occurrence
---------------------------------------------

    gsw1 <- gsw %>%
             group_by(name,minute)%>%
            select(shot_made_flag,name,minute)%>%
            summarise(shot_this_minute = length(shot_made_flag))

    ggplot(data = gsw1, aes(x = minute, y = shot_this_minute)) + 
      geom_rect(xmin = 0, xmax =12,ymin = 0 , ymax = 60 ,fill = 'grey', alpha = 0.02)+
      geom_rect(xmin = 24, xmax =36,ymin = 0 , ymax = 60 ,fill = 'grey', alpha = 0.02)+
      geom_point(colour = "red") + 
      geom_path(colour = "blue") +
      theme_minimal() + 
      scale_x_continuous(breaks = seq(0,48,4)) + facet_wrap(~name)

![](NBA_data_visualize_files/figure-markdown_strict/unnamed-chunk-12-1.png)
