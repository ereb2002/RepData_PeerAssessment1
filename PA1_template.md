---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---

## Loading and preprocessing the data


```r
library(ggplot2)
```

```
## Registered S3 methods overwritten by 'tibble':
##   method     from  
##   format.tbl pillar
##   print.tbl  pillar
```

```r
setwd("/home/ron/Documentos/DataScience/RepData_PeerAssessment1/")
datos <- data.frame(read.csv("activity.csv"))
```

## What is mean total number of steps taken per day?


```r
datos_filtro <- subset(datos, steps != "NA")
media_total<-mean(datos_filtro$steps)
media_total
```

```
## [1] 37.3826
```

## What is the average daily activity pattern?


```r
media_dia<-setNames(aggregate(datos$steps, list(datos$date), FUN=mean),c("Dia", "Media"))
media_dia
```

```
##           Dia      Media
## 1  2012-10-01         NA
## 2  2012-10-02  0.4375000
## 3  2012-10-03 39.4166667
## 4  2012-10-04 42.0694444
## 5  2012-10-05 46.1597222
## 6  2012-10-06 53.5416667
## 7  2012-10-07 38.2465278
## 8  2012-10-08         NA
## 9  2012-10-09 44.4826389
## 10 2012-10-10 34.3750000
## 11 2012-10-11 35.7777778
## 12 2012-10-12 60.3541667
## 13 2012-10-13 43.1458333
## 14 2012-10-14 52.4236111
## 15 2012-10-15 35.2048611
## 16 2012-10-16 52.3750000
## 17 2012-10-17 46.7083333
## 18 2012-10-18 34.9166667
## 19 2012-10-19 41.0729167
## 20 2012-10-20 36.0937500
## 21 2012-10-21 30.6284722
## 22 2012-10-22 46.7361111
## 23 2012-10-23 30.9652778
## 24 2012-10-24 29.0104167
## 25 2012-10-25  8.6527778
## 26 2012-10-26 23.5347222
## 27 2012-10-27 35.1354167
## 28 2012-10-28 39.7847222
## 29 2012-10-29 17.4236111
## 30 2012-10-30 34.0937500
## 31 2012-10-31 53.5208333
## 32 2012-11-01         NA
## 33 2012-11-02 36.8055556
## 34 2012-11-03 36.7048611
## 35 2012-11-04         NA
## 36 2012-11-05 36.2465278
## 37 2012-11-06 28.9375000
## 38 2012-11-07 44.7326389
## 39 2012-11-08 11.1770833
## 40 2012-11-09         NA
## 41 2012-11-10         NA
## 42 2012-11-11 43.7777778
## 43 2012-11-12 37.3784722
## 44 2012-11-13 25.4722222
## 45 2012-11-14         NA
## 46 2012-11-15  0.1423611
## 47 2012-11-16 18.8923611
## 48 2012-11-17 49.7881944
## 49 2012-11-18 52.4652778
## 50 2012-11-19 30.6979167
## 51 2012-11-20 15.5277778
## 52 2012-11-21 44.3993056
## 53 2012-11-22 70.9270833
## 54 2012-11-23 73.5902778
## 55 2012-11-24 50.2708333
## 56 2012-11-25 41.0902778
## 57 2012-11-26 38.7569444
## 58 2012-11-27 47.3819444
## 59 2012-11-28 35.3576389
## 60 2012-11-29 24.4687500
## 61 2012-11-30         NA
```

## Imputing missing values


```r
media_dia$Media <- ifelse(is.na(media_dia$Media), 0, media_dia$Media)
```

## Are there differences in activity patterns between weekdays and weekends?

```r
media_dia$num_dia<-as.numeric(format(as.Date(media_dia$Dia), format = '%u'))
media_dia$laboral <- floor(media_dia$num_dia / 6)
media_dia$grupo <- ifelse(media_dia$laboral == 0, "Weekday", "Weekend")
ggplot(media_dia, aes(x=Dia, y=Media, shape=grupo, color=grupo)) + geom_point()+
  scale_x_discrete(guide = guide_axis(check.overlap = TRUE))
```

![](PA1_template_files/figure-html/laboral-1.png)<!-- -->

```r
medias <- setNames(aggregate(media_dia$Media, list(media_dia$grupo), FUN=mean), c("Type", "Mean"))
medias
```

```
##      Type     Mean
## 1 Weekday 30.62623
## 2 Weekend 37.69358
```
