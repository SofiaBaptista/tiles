# My personal webpage

should actually be at <https://SofiaBaptista.github.io/tiles/>. 

---
title: "Soupppppp's First Day at the New Barnyard"
author: "Sofia Baptista"
date: "Last updated on `r Sys.Date()`"
output:
  html_document:
    toc: true
    toc_depth: 2
    toc_float: true
    df_print: kable
---

<!--
Comments in HTML are like this! 
-->

```{r, include = FALSE}
library(shiny)
library(tidyverse)
library(lubridate)
library(sp)
```

Hello, Website!

# Vis?

```{r, echo = FALSE}
load("gps.RData")
gps
require(rgdal)
shape_A <- readOGR(dsn = "Abila.shp", layer = "Abila")
ui <- fluidPage(
    
    # Application title
    titlePanel("Geolocation plot"),
    # Sidebar with a slider input for number of bins 
    sidebarLayout(
        sidebarPanel(
            numericInput("id",
                        "Id:",
                        min = min(gps$id),
                        max = max(gps$id),
                        value = 35),
            sliderInput("Timestamp",
                        "Timestamp",
                        min= min(gps$Timestamp),
                        max = max(gps$Timestamp),
                        value= range(gps$Timestamp, gps$Timestamp)) #fix so lower bound is actually lower
            
        ),
        # Show a plot of the generated distribution
        mainPanel(
           plotOutput("distPlot")
        )
    )
)
# Define server logic required to draw a histogram
server <- function(input, output) {
    output$distPlot <- renderPlot({
        gps_filtered <- gps %>% filter(id == input$id, Timestamp < input$Timestamp)
        # generate bins based on input$bins from ui.R
        ggplot(gps_filtered, aes(x=long, y=lat))+
            coord_quickmap()+
            geom_point() +
            labs(title= paste("This is Id number",input$id )) +
            xlim(24.844, 24.906) +
            ylim(36.047, 36.091) +
        geom_polygon(data = shape_A, aes(x = long, y = lat, group = group), colour = "black", fill = NA)
    })
}
# Run the application 
shinyApp(ui = ui, server = server)
```


# My second section

```{r}
head(mtcars)
```
