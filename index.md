---
title       : Predicting Miles Per Gallon 
subtitle    : 
author      : Tanveer Ansari
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Predicting Gas Mileage of Cars

* Use the mtcars dataset in R {base} package
* Fit a linear model to explain variation in mpg
* Use the model for predictions

--- 

## Model and variables used  

Let us fit a linear regression model to the mtcars dataset in package base  


I used wt, hp, qsec and disp variables for predicting mpg.

Abbreviation | Explanation
----------------------|------------
mpg |  Miles/(US) gallon
wt | Weight in thousands of lbs 
hp | Gross Horsepower
qsec | Time taken to travel a quarter mile
disp | Engine Displacement in cubic inches  


Variable selection builds upon my [Regression Models Course Project](https://s3.amazonaws.com/coursera-uploads/user-643a38d96a27064318cb87b3/973532/asst-4/c32a7db0eb8f11e497299d1b8b45aece.pdf)

---

## R code


```r
library(dplyr); 
cars<-select(mtcars, mpg,disp,hp,wt,qsec)
fit<-lm(mpg~disp+hp+wt+qsec, data=cars)
#summary(fit)$coefficients
```
Use the mean column values of cars to create a prediction input x


```r
x<-as.data.frame(t(colMeans(cars)))
x$mpg<-NULL
x$mpg <- predict(fit, newdata = x)
x
```

```
##       disp       hp      wt     qsec      mpg
## 1 230.7219 146.6875 3.21725 17.84875 20.09062
```

---

## Shiny Application

The shiny application takes hp and wt values from user input and uses them to predict mpg  

```r
# server.R

shinyServer(function(input, output) {
  mpgPrediction<-reactive({
    # Use hp and wt from user's slider values
    x$hp<-input$hp
    x$wt<-input$wt
    #Predict mpg
    x$mpg<-predict(fit, newdata=x)
    return(x$mpg)
    })
  
  # Set the value of output element "mpg"" to predicted mpg
  output$mpg<-renderPrint(mpgPrediction())
  })
```

It then displays the calculated mpg on ui.R


---




