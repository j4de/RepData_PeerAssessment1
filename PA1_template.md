# Reproducible Research: Peer Assessment 1

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

## Introduction

It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the "quantified self" movement -- a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

## Tasks
- Loading and preprocessing the data
- What is the mean total number of steps taken per day?
- What is the average daily activity pattern?
- Imputing missing values
- Are there differences in activity patterns between weekdays and weekends?

:

## Loading and preprocessing the data

```r
setwd("~/Github/RepData_PeerAssessment1")
## Load the activity.csv file into memory
act <- read.csv( "activity.csv", header=T)
## Transform the date from a Factor variable to a Date variable
act$date <- as.Date(act$date,"%Y-%m-%d")
```


## What is mean total number of steps taken per day?

```r
## remove 'na' rows from our dataset by subsetting
act_nonas <- act[!is.na(act$steps),]
## get total number to steps per day for histogram puposes
act_totsteps <- aggregate(steps ~ date, data=act_nonas, FUN="sum")
hist(act_totsteps$steps, xlab="Steps", main="Total number of steps taken each day")
```

![plot of chunk unnamed-chunk-2](./PA1_template_files/figure-html/unnamed-chunk-2.png) 

```r
## get mean number of steps per day
paste( "Mean number of steps per day:",mean(act_totsteps$steps))
```

```
## [1] "Mean number of steps per day: 10766.1886792453"
```

```r
## get median number of steps per day
paste( "Median number of steps per day:",median(act_totsteps$steps))
```

```
## [1] "Median number of steps per day: 10765"
```

## What is the average daily activity pattern?

```r
## get average number of steps taken across all days for each interval
act_interval <- aggregate(steps ~ interval, data=act_nonas, FUN="mean")
## show plot of avg number of steps taken per interval
plot(act_interval$interval, act_interval$steps, type="l", main="Average number of Steps taken per Interval", xlab="Steps", ylab="Interval")
```

![plot of chunk unnamed-chunk-3](./PA1_template_files/figure-html/unnamed-chunk-3.png) 

```r
## which 5-min interval on average across all the days contains the maximum number of steps
paste( "Interval containing max steps across all the days:",act_interval[act_interval$steps==max(act_interval$steps),"interval"])
```

```
## [1] "Interval containing max steps across all the days: 835"
```

## Imputing missing values
1. Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
missingvaluecount <- nrow(act) - nrow(act[!is.na(act$steps),])
paste("Total number of missing values (rows with NAs):", missingvaluecount)
```

```
## [1] "Total number of missing values (rows with NAs): 2304"
```

2. Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.

```r
## Fill in the missing values with the mean of the relevant 5-minute interval
## note that the end resulting steps column will be of type num (not int) due to the mean value
act$interval <- as.factor(act$interval)
act_interval$interval <- as.factor(act_interval$interval)
act_merged <- merge( x=act, y=act_interval, by.x="interval", by.y="interval", all.x=TRUE )
## steps.x is the old steps value or NA, steps.y is the mean value
## we only need the y value if x == NA
act_merged$steps <- ifelse(is.na(act_merged$steps.x), act_merged$steps.y, act_merged$steps.x)
```

3. Create a new dataset that is equal to the original dataset but with the missing data filled in.

```r
## we can now drop the x and y values as we have a correct steps column value
drops <- c("steps.x","steps.y")
act <- act_merged[,!(names(act_merged) %in% drops)]
```

4. Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?

```r
## remove 'na' rows from our dataset by subsetting
act_nonas <- act[!is.na(act$steps),]
## get total number to steps per day for histogram puposes
act_totsteps <- aggregate(steps ~ date, data=act_nonas, FUN="sum")
hist(act_totsteps$steps, xlab="Steps", main="Total number of steps taken each day")
```

![plot of chunk unnamed-chunk-7](./PA1_template_files/figure-html/unnamed-chunk-7.png) 

```r
## get mean number of steps per day
paste( "Mean number of steps per day:",mean(act_totsteps$steps))
```

```
## [1] "Mean number of steps per day: 10766.1886792453"
```

```r
## get median number of steps per day
paste( "Median number of steps per day:",median(act_totsteps$steps))
```

```
## [1] "Median number of steps per day: 10766.1886792453"
```

As you can see by imputing the data both the mean and median are now equal whereas with the 
previously non-imputed data the median was slightly lower, the mean has not changed.

Also note that the 'Frequency' axis has increased from 25 to 35 to account for the extra imputed
data.

## Are there differences in activity patterns between weekdays and weekends?

![plot of chunk unnamed-chunk-8](./PA1_template_files/figure-html/unnamed-chunk-8.png) 

Visually there would appear to be more activity on average at the weekends than during the weekdays and this is found to be true if we calculate the averages themselves:-


```
## [1] "Mean weekday steps: 35.6105811786629"
```

```
## [1] "Mean weekend steps: 42.366401336478"
```
