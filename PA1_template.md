---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
---


## Loading and preprocessing the data

```r
unzip("activity.zip")
activity <- read.csv("activity.csv")
```

## What is mean total number of steps taken per day?

### 1. Calculate the total number of steps taken per day.


```r
steps.by.date <- aggregate(steps ~ date, activity, sum, na.rm=TRUE)
```

### 2. Make a histogram of the total number of steps taken each day.


```r
hist(steps.by.date$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

### 3. Calculate and report the mean and median of the total number of steps taken per day.


```r
mean(steps.by.date$steps)
```

```
## [1] 10766.19
```

```r
median(steps.by.date$steps)
```

```
## [1] 10765
```

## What is the average daily activity pattern?

### 1. Make a time series plot (i.e. \color{red}{\verb|type = "l"|}type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis).


```r
steps.by.interval <- aggregate(steps~interval, 
                          data=activity, 
                          mean, na.rm = TRUE)
plot(steps ~ interval, data = steps.by.interval, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

### 2. Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
steps.by.interval[which.max(steps.by.interval$steps),]$interval
```

```
## [1] 835
```

## Imputing missing values

### 1.Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with N𝙰s).


```r
sum(is.na(activity$steps))
```

```
## [1] 2304
```

### 2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc. 


```r
gi <-function(i){
    steps.by.interval[steps.by.interval$i==i,]$steps
}
```

### 3. Create a new dataset that is equal to the original dataset but with the missing data filled in.


```r
complete <-activity
for(i in 1:nrow(complete)){
    if(is.na(complete[i,]$steps)){
        complete[i,]$steps <- gi(complete[i,]$interval)
    }
}
```

### 4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

## Are there differences in activity patterns between weekdays and weekends?

### 1. Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.


```r
totalcomplete <- aggregate(steps ~ date, data=complete, sum)
hist(totalcomplete$steps)
```

![](PA1_template_files/figure-html/unnamed-chunk-11-1.png)<!-- -->

### 2. Make a panel plot containing a time series plot (i.e. \color{red}{\verb|type = "l"|}type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.


```r
mean(totalcomplete$steps)
```

```
## [1] 10766.19
```

```r
median(totalcomplete$steps)
```

```
## [1] 10766.19
```



























