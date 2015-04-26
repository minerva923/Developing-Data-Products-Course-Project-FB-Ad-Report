---
title       : Developing Data Products Course Project
subtitle    : Facebook Ad Report
framework   : io2012   # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Goal

Nowadays, social campaigns are important to digital marketers. 
Aiming to save lots of times from day by day pivot table, 
this app helps us to quickly identify common metrics on Facebook 
advertising campaigns. 

Please noticed that all data have been preprocessed to protect client privacy.

--- .class #id 

## DataTable & Widgets
DataTable is chosen over other charts because it's easy to sort and copy the needed data. 
There are two sections which are performance and ROI. In the performance section,
one can see common metrics such as impressions, clicks, likes, and so on. In addition,
the dataset is able to group by variables from campaigns to date. 

Widgets in the sidebar panel is apply to performance only, because information 
in ROI itself is sufficient in my experience. Through the checkboxs, one is able 
to dicide which variables he/she would like to show, which is convenient when you 
just want to observe few.

---

## Data processing - I
Package *dplyr* is heavily used in the app. It does a great help to group, summarize data, and create new variables. For instance, following codes let me group a variable, average wanted variables, and create new variable based on variable just created:

```r
suppressPackageStartupMessages(library(dplyr))
dt <- tbl_df(mtcars) %>% 
            group_by(cyl) %>%
            summarise(mpg=mean(mpg), wt=mean(wt)) %>%
            mutate(v1 = "test", v2=wt^2)
dt
```

```
## Source: local data frame [3 x 5]
## 
##   cyl      mpg       wt   v1        v2
## 1   4 26.66364 2.285727 test  5.224549
## 2   6 19.74286 3.117143 test  9.716580
## 3   8 15.10000 3.999214 test 15.993715
```

---

## Data processing - II
Besides *dplyr*, *grep* also helps a lot, since one can create useful variables to measure more indexes which won't know via regular report. For example:

```r
        color <- vector("character", length(dt[dt$cyl]))
        color[grep('4', dt$cyl)] <- "Red"
        color[grep('6', dt$cyl)] <- "Navy"
        color[color==""] <- "Black"
        dt <- cbind(dt, color)
        dt
```

```
##   cyl      mpg       wt   v1        v2 color
## 1   4 26.66364 2.285727 test  5.224549   Red
## 2   6 19.74286 3.117143 test  9.716580  Navy
## 3   8 15.10000 3.999214 test 15.993715 Black
```

