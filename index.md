    my_calendar2 <- my_calendar%>%mutate(date1= as.Date(start))%>%filter(date1>="2020-04-25", summary!="", summary!='Summer Housing Request Meeting')%>%
      select(summary, start, end, length_min, length_hrs)

    ggplot(my_calendar2, aes(x=summary, y = length_min))+
      geom_bar(stat = 'identity')

    ## Don't know how to automatically pick scale for object of type difftime. Defaulting to continuous.

![](index_files/figure-markdown_strict/unnamed-chunk-2-1.png)

    my_cal3<- my_calendar2%>%filter(summary == 'Studying')%>%mutate(date = as.Date(start))%>%group_by(date)%>%summarise(daily_study_time = sum(length_hrs))

    plot2<- ggplot(my_cal3, aes(x= date, y=daily_study_time))+
      geom_line()+geom_point()+ transition_reveal(date)

    animate(plot2, 200, fps = 20,renderer = gifski_renderer("line2.gif"))

    ## Don't know how to automatically pick scale for object of type difftime. Defaulting to continuous.

![](index_files/figure-markdown_strict/unnamed-chunk-2-1.gif)

    my_cal4<- my_calendar2%>%filter(summary == 'Social Activities')%>%mutate(date = as.Date(start))%>%group_by(date)%>%summarise(daily_social_time = sum(length_hrs))
    ggplot(my_cal4, aes(x= date, y=daily_social_time, fill = as.factor(date)))+
      geom_bar(stat= 'identity', width = 0.4)+
        theme(legend.position="none")

    ## Don't know how to automatically pick scale for object of type difftime. Defaulting to continuous.

![](index_files/figure-markdown_strict/unnamed-chunk-2-3.png)

    my_cal5<- my_calendar2%>%mutate(date = as.Date(start))%>%group_by(summary,date)%>%summarise(daily_time = sum(length_hrs))%>%ungroup()
    cal6 <- my_cal5%>%spread(date, daily_time)
    cal6[is.na(cal6)]<-0
    view(cal6)
    cal7<- cal6%>%gather(key = "date", value="dailytime",-summary)

    plot <-ggplot(cal7, aes(x= summary, y= dailytime))+
      geom_bar(stat='identity') 
    anim = plot + transition_states(date, transition_length = 4, state_length = 1)+
      labs(title = 'Date : {closest_state}')
    anim

    ## Don't know how to automatically pick scale for object of type difftime. Defaulting to continuous.

    animate(anim, 200, fps = 20,  width = 1200, height = 1000, 
            renderer = gifski_renderer("ggaim2.gif"))

    ## Don't know how to automatically pick scale for object of type difftime. Defaulting to continuous.

![](index_files/figure-markdown_strict/unnamed-chunk-2-1.gif)
