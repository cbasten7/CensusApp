HW4
================
Chris Basten
3/11/2021

``` r
library(shiny)
library(maps)
library(mapproj)
counties = readRDS("census-app/data/counties.rds")
head(counties)
```

    ##              name total.pop white black hispanic asian
    ## 1 alabama,autauga     54571  77.2  19.3      2.4   0.9
    ## 2 alabama,baldwin    182265  83.5  10.9      4.4   0.7
    ## 3 alabama,barbour     27457  46.8  47.8      5.1   0.4
    ## 4    alabama,bibb     22915  75.0  22.9      1.8   0.1
    ## 5  alabama,blount     57322  88.9   2.5      8.1   0.2
    ## 6 alabama,bullock     10914  21.9  71.0      7.1   0.2

``` r
source("census-app/data/helpers.r")
percent_map(counties$white, "darkgreen", "% White")
```

![](HW4_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

``` r
ui = fluidPage( 
  titlePanel("censusVis"),
                sidebarLayout(
                sidebarPanel(
                helpText("Create demographic maps with 
    information from the 2010 US Census."),
                    
    selectInput("var", 
    label = "Choose a variable to display",
    choices = c("Percent White", "Percent Black",
                "Percent Hispanic", "Percent Asian"),
                selected = "Percent White"),
                    
          sliderInput("range", 
          label = "Range of interest:",
          min = 0, max = 100, value = c(0, 100))
                  ),
                  
          mainPanel(plotOutput("map"))
                ))
server = 
  function(input, output) { output$map = 
    renderPlot({ data = switch(input$var, 
                "Percent White" = counties$white, 
                "Percent Black" = counties$black, 
                "Percent Hispanic" = counties$hispanic, 
                "Percent Asian" = counties$asian)
  
  color = switch(input$var, 
                 "Percent White" = "darkgreen",
                 "Percent Black" = "black",
                 "Percent Hispanic" = "darkorange",
                 "Percent Asian" = "darkviolet")
  
  legend = switch(input$var, 
                  "Percent White" = "% White",
                  "Percent Black" = "% Black",
                  "Percent Hispanic" = "% Hispanic",
                  "Percent Asian" = "% Asian")
  
  percent_map(data, color, legend, input$range[1], input$range[2])
  })
  }
shinyApp(ui, server)
```

<!--html_preserve-->

<div class="muted well" style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;">

Shiny applications not supported in static R Markdown documents

</div>

<!--/html_preserve-->
