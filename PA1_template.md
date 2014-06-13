---
title: "PA1_template"
author: "jdelgastat"
date: "Monday, June 09, 2014"
output: html_document
---

##### 1 Loading the data


```r
activity <- read.table(file = "activity.csv", header = T, sep = ",", colClasses = c("numeric", 
    "Date", "factor"))
```


##### 2 Histogram of number of steps taken each day

```r
hist(x = activity$steps, breaks = 50, main = "total number of steps taken each day", 
    xlab = "Steps", col = "lightblue")
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


##### 3 Mean of number of steps taken per day

```r
meansteps <- mean(activity$steps, na.rm = T)
```

The mean of steps taken per day is 37.3826 steps.

##### 4 Median of number of steps taken per day

```r
mediansteps <- median(activity$steps, na.rm = T)
```

The median of steps taken per day is 0 steps.

##### 5 Time series plot of the 5-minute interval and the average number of steps taken, averaged across all days

```r
activity_clean <- na.omit(object = activity)
mean_steps_by_interval <- as.numeric(lapply(split(x = activity_clean$steps, 
    f = activity_clean$interval), mean))
plot(mean_steps_by_interval, type = "l", xlab = "5 minutes interval", ylab = "Mean of steps across all days", 
    main = "Average number of steps by 5 minutes interval")
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 


##### 6 Five minute interval with maximum number of steps across all the days

```r
intervals <- sort(as.numeric(levels(activity$interval)))
maxinterval <- data.frame(intervals, mean_steps_by_interval)
max <- maxinterval[(mean_steps_by_interval == max(maxinterval$mean_steps_by_interval)), 
    ]
maximo <- as.character(max[[1]])
h <- substr(x = maximo, start = 1, stop = 2)
m <- substr(x = maximo, start = 3, stop = 4)
hm <- paste(h, ":", m, sep = "")
```

The 5 minutes interval with maximum number of steps across all the days begins at 22:35.

##### 7 The total number of rows with missing values in the dataset

```r
rowsNA <- nrow(activity) - nrow(activity_clean)
```

The total number of rows with missing values in the dataset is 2304 rows.

