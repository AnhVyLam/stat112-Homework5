---
title: 'Weekly Exercises #5'
author: "Vy Lam"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(gardenR)       # for Lisa's garden data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(gifski)        # need the library for creating gifs but don't need to load each time
library(shiny)         # for creating interactive apps
theme_set(theme_minimal())
```


```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
data("garden_harvest")

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels and alt text.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.
  

```r
breed_traits <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_traits.csv')
trait_description <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/trait_description.csv')
breed_rank_all <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-02-01/breed_rank.csv')
```


```r
breed_trait_graph <- breed_traits %>% 
  ggplot(aes(x = `Affectionate With Family`)) +
  geom_histogram(fill = "lightsteelblue", binwidth = .4) +
  labs(title = "Affection with Family Among Dog Breeds Affects Ratings 1-5",
       caption = "Plot created by Vy Lam, data from 'Dog Breeds Github'",
       x = NULL, y = NULL) +
  theme(
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank())

ggplotly(breed_trait_graph, tooltip = c("text", "x"))
```

```{=html}
<div id="htmlwidget-95fe0bda17fcee86a4c6" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-95fe0bda17fcee86a4c6">{"x":{"data":[{"orientation":"v","width":[0.4,0.4,0.4,0.4,0.4,0.4,0.4,0.4,0.4,0.4,0.4,0.4,0.399999999999999],"base":[0,0,0,0,0,0,0,0,0,0,0,0,0],"x":[0,0.4,0.8,1.2,1.6,2,2.4,2.8,3.2,3.6,4,4.4,4.8],"y":[1,0,1,0,0,0,0,32,0,0,29,0,132],"text":["Affectionate With Family: 0.0","Affectionate With Family: 0.4","Affectionate With Family: 0.8","Affectionate With Family: 1.2","Affectionate With Family: 1.6","Affectionate With Family: 2.0","Affectionate With Family: 2.4","Affectionate With Family: 2.8","Affectionate With Family: 3.2","Affectionate With Family: 3.6","Affectionate With Family: 4.0","Affectionate With Family: 4.4","Affectionate With Family: 4.8"],"type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(176,196,222,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":28.4931506849315},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Affection with Family Among Dog Breeds Affects Ratings 1-5","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-0.46,5.26],"tickmode":"array","ticktext":["0","1","2","3","4","5"],"tickvals":[0,1,2,3,4,5],"categoryorder":"array","categoryarray":["0","1","2","3","4","5"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-6.6,138.6],"tickmode":"array","ticktext":["0","50","100"],"tickvals":[0,50,100],"categoryorder":"array","categoryarray":["0","50","100"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"58f5653076aa":{"x":{},"type":"bar"}},"cur_data":"58f5653076aa","visdat":{"58f5653076aa":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```


```r
stations <- readr::read_csv('https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2022/2022-03-01/stations.csv')
```


```r
stations_graph <- stations %>% 
  ggplot(aes(x = FUEL_TYPE_CODE)) +
  geom_bar(fill = "lightsteelblue") +
  labs(title = "Total Usage of Different Types of Fuel" , 
  caption = "Plot created by Vy Lam, data from 'USDOT'",
  x = NULL, y = NULL)+
  theme(
    panel.grid.major = element_blank(),
    panel.grid.minor = element_blank())

ggplotly(stations_graph, tooltip = c("text", "x"))
```

```{=html}
<div id="htmlwidget-62fea9ce7c4bec3c29b1" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-62fea9ce7c4bec3c29b1">{"x":{"data":[{"orientation":"v","width":[0.9,0.9,0.9,0.9,0.9,0.9,0.9],"base":[0,0,0,0,0,0,0],"x":[1,2,3,4,5,6,7],"y":[712,1541,4377,50163,144,150,2840],"text":["FUEL_TYPE_CODE: BD","FUEL_TYPE_CODE: CNG","FUEL_TYPE_CODE: E85","FUEL_TYPE_CODE: ELEC","FUEL_TYPE_CODE: HY","FUEL_TYPE_CODE: LNG","FUEL_TYPE_CODE: LPG"],"type":"bar","textposition":"none","marker":{"autocolorscale":false,"color":"rgba(176,196,222,1)","line":{"width":1.88976377952756,"color":"transparent"}},"showlegend":false,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":25.5707762557078,"l":40.1826484018265},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Total Usage of Different Types of Fuel","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[0.4,7.6],"tickmode":"array","ticktext":["BD","CNG","E85","ELEC","HY","LNG","LPG"],"tickvals":[1,2,3,4,5,6,7],"categoryorder":"array","categoryarray":["BD","CNG","E85","ELEC","HY","LNG","LPG"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"y","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-2508.15,52671.15],"tickmode":"array","ticktext":["0","10000","20000","30000","40000","50000"],"tickvals":[0,10000,20000,30000,40000,50000],"categoryorder":"array","categoryarray":["0","10000","20000","30000","40000","50000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":false,"gridcolor":null,"gridwidth":0,"zeroline":false,"anchor":"x","title":{"text":"","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":false,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895}},"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","modeBarButtonsToAdd":["hoverclosest","hovercompare"],"showSendToCloud":false},"source":"A","attrs":{"58f5490d7dff":{"x":{},"type":"bar"}},"cur_data":"58f5490d7dff","visdat":{"58f5490d7dff":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script>
```
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).

```r
highestJourneyTime <- small_trains %>% 
  group_by(month, year, journey_time_avg) %>% 
  filter(journey_time_avg > 320, year == "2018") %>% 
  select(year, month, journey_time_avg) %>% 
  head(highestJourneyTime, n = 5)

highestJourneyTime
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["year"],"name":[1],"type":["dbl"],"align":["right"]},{"label":["month"],"name":[2],"type":["dbl"],"align":["right"]},{"label":["journey_time_avg"],"name":[3],"type":["dbl"],"align":["right"]}],"data":[{"1":"2018","2":"2","3":"440.0118"},{"1":"2018","2":"4","3":"390.6000"},{"1":"2018","2":"1","3":"346.2487"},{"1":"2018","2":"7","3":"351.6918"},{"1":"2018","2":"8","3":"346.3310"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>

```r
highestJourneyTime %>% 
  ggplot(aes(x = journey_time_avg, y = month))+
    geom_jitter()+
    labs(title = "Average Journey Time in 2018 by Service",
         x = "",
         y = "") +
    transition_states(month)
```

<img src="05_exercises_new_files/figure-html/unnamed-chunk-5-1.gif" title="A jitter plot showing the Average journey time of by highest average journey time, at random months. " alt="A jitter plot showing the Average journey time of by highest average journey time, at random months. "  />

```r
anim_save("randomJourneyTimes.gif")
```


```r
knitr::include_graphics("randomJourneyTimes.gif")
```

![](randomJourneyTimes.gif)<!-- -->

## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. I have filtered the data to the tomatoes and find the *daily* harvest in pounds for each variety. The `complete()` function creates a row for all unique `date`/`variety` combinations. If a variety is not harvested on one of the harvest dates in the dataset, it is filled with a value of 0. 
  You should do the following:
  * For each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each variety and arranged (HINT: `fct_reorder()`) from most to least harvested weights (most on the bottom).  
  * Add animation to reveal the plot over date. Instead of having a legend, place the variety names directly on the graph (refer back to the tutorial for how to do this).


```r
daily_variety_harvest_lb <-garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  group_by(date, variety) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  ungroup() %>% 
  complete(variety, 
           date, 
           fill = list(daily_harvest_lb = 0))

vegVariety_gganim <- daily_variety_harvest_lb%>% 
  ggplot(aes(x = daily_harvest_lb, 
             y = date,
             group = variety)) +
  geom_area() +
  labs(title = "Daily harvest",
       subtitle = "Variety: {closest_state}",
       x = "",
       y = "") +
  transition_states(variety, 
                    transition_length = 2, 
                    state_length = 1) +
  exit_shrink() +
  enter_recolor(color = "lightblue") +
  exit_recolor(color = "lightblue")

animate(vegVariety_gganim, duration = 20)
```

<img src="05_exercises_new_files/figure-html/unnamed-chunk-7-1.gif" title="A visualization of stacked area plot that reveals itself over time for cumulative harvest of tomatoes varieties over time." alt="A visualization of stacked area plot that reveals itself over time for cumulative harvest of tomatoes varieties over time."  />

```r
anim_save("vegVarietyGardenCum.gif")
```


```r
knitr::include_graphics("vegVarietyGardenCum.gif")
```

![](vegVarietyGardenCum.gif)<!-- -->


## Maps, animation, and movement!

  4. Map Lisa's `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.

```r
bike_data <- mallorca_bike_day7 %>% 
  group_by(lat, lon)

basemap <- get_stamenmap(
    bbox = c(left = -77.3, bottom = 38.75, right = -76.8, top = 39.15), 
    maptype = "terrain-background",
    zoom = 10
)

ggmap(basemap) +
  geom_point(data = bike_data, 
             aes(x = lon, y = lat), 
             alpha=.8, shape = 17) +
  scale_color_viridis_c() +
  theme_map() +
  theme(legend.background = element_blank())

pal <- colorFactor("viridis",
                   domain = bike_data)

leaflet(data = bike_data) %>% 
  addProviderTiles(providers$Stamen.TonerHybrid) %>% 
  addCircles(lng = ~lon, 
             lat = ~lat, , 
             weight = 10, 
             opacity = 1) %>% 
  addLegend(position = "bottomleft", 
            pal = pal)
```
Comment: If I were to image that I am looking at a working map with clear visuals
and the route in the data, then I would prefer it over a static map.

  
  5. In this exercise, you get to meet Lisa's sister, Heather! She is a proud Mac grad, currently works as a Data Scientist where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files putting them in swim, bike, run order (HINT: `bind_rows()`), 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGxE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
macGrad  <- bind_rows(panama_bike,panama_run, panama_swim) %>% 
  group_by(lat, lon)

basemap <- get_stamenmap(
    bbox = c(left = -77.3, bottom = 38.75, right = -76.8, top = 39.15), 
    maptype = "terrain-background",
    zoom = 10
)

ggmap(basemap) +
  geom_point(data = macGrad, 
             aes(x = lon, y = lat), 
             alpha=.8, shape = 17) +
  scale_color_viridis_c() +
  theme_map() +
  theme(legend.background = element_blank())
```

<img src="05_exercises_new_files/figure-html/unnamed-chunk-10-1.png" title="A map based on one of Heather Lendway's triathlon races, mapping the speed and location." alt="A map based on one of Heather Lendway's triathlon races, mapping the speed and location."  />

```r
pal <- colorFactor("viridis",
                   domain = macGrad)
```

```
## Error in `stop_vctrs()`:
## ! Can't combine `lon` <double> and `time` <datetime<UTC>>.
```

```r
leaflet(data = macGrad) %>% 
  addProviderTiles(providers$Stamen.TonerHybrid) %>% 
  addCircles(lng = ~lon, 
             lat = ~lat, , 
             weight = 10, 
             opacity = 1) %>% 
  addLegend(position = "bottomleft", 
            pal = pal)
```

```
## Error in inherits(values, "formula"): argument "values" is missing, with no default
```
  
## COVID-19 data

  6. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. The code below gives the population estimates for each state and loads the `states_map` data. Here is a list of details you should include in the plot:
  
  * Put date in the subtitle.   
  * Because there are so many dates, you are going to only do the animation for the the 15th of each month. So, filter only to those dates - there are some lubridate functions that can help you do this.   
  * Use the `animate()` function to make the animation 200 frames instead of the default 100 and to pause for 10 frames on the end frame.   
  * Use `group = date` in `aes()`.   
  * Comment on what you see.  


```r
census_pop_est_2018 <- read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))

states_map <- map_data("state")


censusanim <- census_pop_est_2018%>% 
  states_map <- map_data("state")
```

```
## Error in census_pop_est_2018 %>% states_map <- map_data("state"): could not find function "%>%<-"
```

```r
  labs(title = "Cumulative COVID-19 Cases",
       subtitle = "Variety: {closest_state}",
       x = "",
       y = "") +
  transition_states(variety, 
                    transition_length = 2, 
                    state_length = 1) +
  exit_shrink() +
  enter_recolor(color = "lightblue") +
  exit_recolor(color = "lightblue")
```

```
## NULL
```

```r
animate(census_gganim, duration = 20)
```

```
## Error in animate(census_gganim, duration = 20): object 'census_gganim' not found
```

```r
anim_save("covid19visual.gif")
```

## Your first `shiny` app (for next week!)

  7. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. You should create a new project for the app, separate from the homework project. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' daily number of COVID cases per 100,000 over time. The x-axis will be date. You will have an input box where the user can choose which states to compare (`selectInput()`), a slider where the user can choose the date range, and a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
Put the link to your app here:   https://vlam777.shinyapps.io/exercise5Proj/
  
## GitHub link

  8. Below, provide a link to your GitHub repo with this set of Weekly Exercises. 


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
