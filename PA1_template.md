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

##### 8 A strategy for filling in all of the missing values in the dataset and a new dataset that is equal to the original dataset but with the missing data filled in

```r
stepsorig <- activity$steps
stepsorig[is.na(stepsorig)] <- mediansteps
activity_plus <- cbind(activity, stepsorig)
```

The NA values from steps have been replaced for the median

##### 9 Histogram of the total number of steps taken each day from the new dataset

```r
hist(x = activity_plus$stepsorig, breaks = 50, main = "total number of steps taken each day", 
    xlab = "Steps", col = "red")
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


##### 10 New mean of number of steps taken per day

```r
meansteps2 <- mean(activity_plus$stepsorig, na.rm = T)
```

The mean of steps taken per day is 32.48 steps.

##### 11 New median of number of steps taken per day

```r
mediansteps2 <- median(activity_plus$stepsorig, na.rm = T)
```

The mean of steps taken per day is 0 steps.

##### 12 New factor variable in the dataset with two levels - "weekday" and "weekend" indicating whether a given date is a weekday or weekend day

```r
Sys.setlocale("LC_TIME", "English")
```

```
## [1] "English_United States.1252"
```

```r
day <- weekdays(activity_plus$date, abbreviate = T)
day[day %in% c("Mon", "Tue", "Wed", "Thu", "Fri")] <- "weekday"
day[day %in% c("Sat", "Sun")] <- "weekend"
actbyday <- cbind(activity_plus, day)
```


##### 13 Panel plot containing a time series plot of the 5-minute interval and the average number of steps taken, averaged across all weekday days or weekend days

```r
actbyweekday <- subset(actbyday, subset = actbyday$day == "weekday")
mean_steps_by_weekday <- as.numeric(lapply(split(x = actbyweekday$stepsorig, 
    f = actbyweekday$interval), mean))
actbyweekend <- subset(actbyday, subset = actbyday$day == "weekend")
mean_steps_by_weekend <- as.numeric(lapply(split(x = actbyweekend$stepsorig, 
    f = actbyweekday$interval), mean))
```

```
## Warning: data length is not a multiple of split variable
```

```r
par(mfrow = c(2, 1))
plot(mean_steps_by_weekday, type = "l", xlab = "5 minutes interval", ylab = "Mean of steps across weekdays", 
    main = "Average number of steps by 5 minutes interval")
plot(mean_steps_by_weekend, type = "l", xlab = "5 minutes interval", ylab = "Mean of steps across weekend", 
    main = "Average number of steps by 5 minutes interval")
```

![plot of chunk unnamed-chunk-13](figure/unnamed-chunk-13.png) 


