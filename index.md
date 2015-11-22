---
title       : Tooth Growth Data Inference
subtitle    : Demonstration the effect of doses of Vitamin C and Orange Juice in the lenght of odontobasts (teeth) in each of 10 guinea pigs.
author      : Alexandre Barros (AlexFBar) - november/2015
job         : Data Scientist
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## The Idea

The idea of my application is simple like the steps below:

1. I choose a result of a Project class from Coursera to make a simple appication
2. The Peer Assessment Part 2 of Data Inference course was choosen
3. The project conclusion is a boxplot with different dosages in the dataset
4. The parameter of my application can be the dosage
5. The other parameter to transform a dinamic boxplot is the scale of y-axis

--- .class #id 

## The Dataset

The Dataset can be load

```r
data("ToothGrowth")
```

The Structure of Dataset

```r
str(ToothGrowth)
```

```
## 'data.frame':	60 obs. of  3 variables:
##  $ len : num  4.2 11.5 7.3 5.8 6.4 10 11.2 11.2 5.2 7 ...
##  $ supp: Factor w/ 2 levels "OJ","VC": 2 2 2 2 2 2 2 2 2 2 ...
##  $ dose: num  0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 0.5 ...
```

--- .class #id 

## The Application (ui.R)


```r
shinyUI(fluidPage(
  # Application title
  # Application documentation
  sidebarLayout(
    sidebarPanel(
      radioButtons("dose", "Dosage of supplements:",
        list("0.5 mg" = 0.5,
             "1.0 mg" = 1.0,
             "2.0 mg" = 2.0)),
        sliderInput("theethrange", "Scale range of Y-axis (Tooth Lenght):", 
         min=0, max=50, value=c(0,50),step=1)
    ),
    mainPanel(
      plotOutput("distPlot")
      #Conclusion
  )
)))
```

--- .class #id 

## The Application (server.R)


```r
library(shiny)
library(ggplot2)
data("ToothGrowth")

shinyServer(function(input, output) {
        
        output$distPlot <- renderPlot({
                doses <- input$dose
                ggplot(ToothGrowth[ToothGrowth$dose==doses,],
                        aes(x=factor(supp), y=len, colour=factor(supp)))+
                        facet_grid(.~dose)+
                        geom_boxplot()+
                        coord_cartesian(ylim = input$theethrange)+
                        ggtitle("Exploratory from Tooth Growth by Supplement")+
                        labs(y="Tooth Length", x="Supplement")
        })
        
})
```

--- .class #id 

## Result

[https://alexfbar.shinyapps.io/app02](https://alexfbar.shinyapps.io/app02)

![](fig01.jpg)


