# Reproducible Research: Peer Assessment 1
Jose Buraschi  
March 15, 2015  


#Introduction#
It is now possible to collect a large amount of data about personal movement using activity monitoring devices such as a Fitbit, Nike Fuelband, or Jawbone Up. These type of devices are part of the “quantified self” movement – a group of enthusiasts who take measurements about themselves regularly to improve their health, to find patterns in their behavior, or because they are tech geeks. But these data remain under-utilized both because the raw data are hard to obtain and there is a lack of statistical methods and software for processing and interpreting the data.

This assignment makes use of data from a personal activity monitoring device. This device collects data at 5 minute intervals through out the day. The data consists of two months of data from an anonymous individual collected during the months of October and November, 2012 and include the number of steps taken in 5 minute intervals each day.

##Loading and preprocessing the data##
After cloning the repo or downloading the repo files and installing them in your machine, you will want to set the working directory in R or RStudio before running this scripts in this Mark Down file.  

###Set Working Directory###
Use the following code to set the working directory:


```r
scriptWorkingDirectory = "/Users/user1/RepData_PeerAssessment1"
scriptWorkingDirectory = "/Users/jaburaschi/DataScience/Coursera/DS-ReproducibleResearch/Project1/RepData_PeerAssessment1"

setwd(scriptWorkingDirectory)
```

###Loading Activity Monitor Data###
The personal activity monitoring device data is included in the root directory of this github repo. Note that the dataset is compressed as a zip file and can be read into R using the read.csv function as follows without first uncompressing the file:


```r
data <- read.csv(  unz("activity.zip", "activity.csv")
                 , nrows=17570
                 , stringsAsFactors=F
                 , colClasses=c("numeric", "Date", "numeric")
                 , col.names = c("steps","date_recorded","interval")
                 , header=F
                 , skip = 1
                 , na.strings = "NA"
                 , quote="\""
                 , blank.lines.skip = TRUE
                 , sep=",")
```

###Quick Look at the Contents###
Once the file has been loaded into R, it is helpful to examine the contents using the structure and summary functions.  Here are the results immediately below:


```r
str(data)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps        : num  NA NA NA NA NA NA NA NA NA NA ...
##  $ date_recorded: Date, format: "2012-10-01" "2012-10-01" ...
##  $ interval     : num  0 5 10 15 20 25 30 35 40 45 ...
```

```r
summary(data)
```

```
##      steps        date_recorded           interval     
##  Min.   :  0.00   Min.   :2012-10-01   Min.   :   0.0  
##  1st Qu.:  0.00   1st Qu.:2012-10-16   1st Qu.: 588.8  
##  Median :  0.00   Median :2012-10-31   Median :1177.5  
##  Mean   : 37.38   Mean   :2012-10-31   Mean   :1177.5  
##  3rd Qu.: 12.00   3rd Qu.:2012-11-15   3rd Qu.:1766.2  
##  Max.   :806.00   Max.   :2012-11-30   Max.   :2355.0  
##  NA's   :2304
```

##What is mean total number of steps taken per day?##
While calculating the mean total number of steps taken per day note that we are going to ignore missing values.  Below is the code to calculate the total number of steps taken per day and the contents of the resulting data frame.


```r
# Calculate Steps Taken Per Day and Assign Column Names to the Result
stepsPerDay <- aggregate(data$steps, by=list(data$date_recorded), FUN=sum, na.rm = TRUE)
colnames(stepsPerDay) <- c("date_recorded","total_steps")
stepsPerDay
```

```
##    date_recorded total_steps
## 1     2012-10-01           0
## 2     2012-10-02         126
## 3     2012-10-03       11352
## 4     2012-10-04       12116
## 5     2012-10-05       13294
## 6     2012-10-06       15420
## 7     2012-10-07       11015
## 8     2012-10-08           0
## 9     2012-10-09       12811
## 10    2012-10-10        9900
## 11    2012-10-11       10304
## 12    2012-10-12       17382
## 13    2012-10-13       12426
## 14    2012-10-14       15098
## 15    2012-10-15       10139
## 16    2012-10-16       15084
## 17    2012-10-17       13452
## 18    2012-10-18       10056
## 19    2012-10-19       11829
## 20    2012-10-20       10395
## 21    2012-10-21        8821
## 22    2012-10-22       13460
## 23    2012-10-23        8918
## 24    2012-10-24        8355
## 25    2012-10-25        2492
## 26    2012-10-26        6778
## 27    2012-10-27       10119
## 28    2012-10-28       11458
## 29    2012-10-29        5018
## 30    2012-10-30        9819
## 31    2012-10-31       15414
## 32    2012-11-01           0
## 33    2012-11-02       10600
## 34    2012-11-03       10571
## 35    2012-11-04           0
## 36    2012-11-05       10439
## 37    2012-11-06        8334
## 38    2012-11-07       12883
## 39    2012-11-08        3219
## 40    2012-11-09           0
## 41    2012-11-10           0
## 42    2012-11-11       12608
## 43    2012-11-12       10765
## 44    2012-11-13        7336
## 45    2012-11-14           0
## 46    2012-11-15          41
## 47    2012-11-16        5441
## 48    2012-11-17       14339
## 49    2012-11-18       15110
## 50    2012-11-19        8841
## 51    2012-11-20        4472
## 52    2012-11-21       12787
## 53    2012-11-22       20427
## 54    2012-11-23       21194
## 55    2012-11-24       14478
## 56    2012-11-25       11834
## 57    2012-11-26       11162
## 58    2012-11-27       13646
## 59    2012-11-28       10183
## 60    2012-11-29        7047
## 61    2012-11-30           0
```

###Histogram of Total Steps per Day###

```r
# Calculate Steps Taken Per Day and Assign Column Names to the Result
hist(  stepsPerDay$total_steps
     , main="Histogram of Total Steps per Day"
     , xlab = "Steps per Day"
     , ylab = "Frequency of Total Steps per Day"
     , col = "Blue"
     , density = 15
     , border = "Black"
     , breaks = 10)
```

![](PA1_template_files/figure-html/unnamed-chunk-5-1.png) 

###Mean Steps Taken per Day###
The mean steps taken per day is calculated immediately below:

```r
mean(stepsPerDay$total_steps)
```

```
## [1] 9354.23
```

###Median Steps Taken per Day###
The median steps taken per day is calculated immediately below:

```r
median(stepsPerDay$total_steps)
```

```
## [1] 10395
```


##What is the average daily activity pattern?##
A plot of the average steps taken by daily time interval can help explore the daily activity patterns. 


```r
meanStepsPerInterval <- aggregate(data$steps, by=list(data$interval), FUN=mean, na.rm = TRUE)
colnames(meanStepsPerInterval) <- c("interval","average_steps")
plot(   x=meanStepsPerInterval$interval
     , y=meanStepsPerInterval$average_steps,type="l"
     , xlab = "5 minute interval"
     , ylab = "Average Steps per Interval"
     , main = "Average Daily Pattern"
     )
```

![](PA1_template_files/figure-html/unnamed-chunk-8-1.png) 

###Max Number of Steps Interval###
Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?

The following calculations provide the interval and maximum average number of steps

```r
maxIntervalRow = meanStepsPerInterval[ meanStepsPerInterval$average_steps==max(meanStepsPerInterval$average_steps),]
```

The 5 minute interval with the maximum number of average steps taken per day is listed immediately below:

```r
maxIntervalRow$interval
```

```
## [1] 835
```

The maximum number of average steps taken per day is listed immediately below:

```r
maxIntervalRow$average_steps
```

```
## [1] 206.1698
```

##Imputing missing values##

Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)

The number of rows with NA value for the number of steps taken in that time interval is provided immediately below:

```r
sum(is.na(data$steps))
```

```
## [1] 2304
```

###Replace Missing Values###
To replace the missing values we will leverage the average steps taken per day interval from the previous problem and we will update the NA values with these averages matching the data by interval immediately below:

```r
#Calculate mean steps per day by interval
dataNoNAs <- data[!is.na(data$steps),]

dataNAVals <- merge(x = data[is.na(data$steps),],y = meanStepsPerInterval, by="interval",all.x = TRUE)
dataNAVals = dataNAVals[,c("average_steps","date_recorded","interval")]
colnames(dataNAVals) = c("steps","date_recorded","interval")

#Create a new data set replacing NAs with the mean of the non-NA intervals
dataNew = rbind(dataNoNAs, dataNAVals)
str(dataNew)
```

```
## 'data.frame':	17568 obs. of  3 variables:
##  $ steps        : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ date_recorded: Date, format: "2012-10-02" "2012-10-02" ...
##  $ interval     : num  0 5 10 15 20 25 30 35 40 45 ...
```

Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. 


```r
# Calculate Steps Taken Per Day and Assign Column Names to the Result
stepsPerDay <- aggregate(dataNew$steps, by=list(dataNew$date_recorded), FUN=sum, na.rm = TRUE)
colnames(stepsPerDay) <- c("date_recorded","total_steps")
stepsPerDay
```

```
##    date_recorded total_steps
## 1     2012-10-01    10766.19
## 2     2012-10-02      126.00
## 3     2012-10-03    11352.00
## 4     2012-10-04    12116.00
## 5     2012-10-05    13294.00
## 6     2012-10-06    15420.00
## 7     2012-10-07    11015.00
## 8     2012-10-08    10766.19
## 9     2012-10-09    12811.00
## 10    2012-10-10     9900.00
## 11    2012-10-11    10304.00
## 12    2012-10-12    17382.00
## 13    2012-10-13    12426.00
## 14    2012-10-14    15098.00
## 15    2012-10-15    10139.00
## 16    2012-10-16    15084.00
## 17    2012-10-17    13452.00
## 18    2012-10-18    10056.00
## 19    2012-10-19    11829.00
## 20    2012-10-20    10395.00
## 21    2012-10-21     8821.00
## 22    2012-10-22    13460.00
## 23    2012-10-23     8918.00
## 24    2012-10-24     8355.00
## 25    2012-10-25     2492.00
## 26    2012-10-26     6778.00
## 27    2012-10-27    10119.00
## 28    2012-10-28    11458.00
## 29    2012-10-29     5018.00
## 30    2012-10-30     9819.00
## 31    2012-10-31    15414.00
## 32    2012-11-01    10766.19
## 33    2012-11-02    10600.00
## 34    2012-11-03    10571.00
## 35    2012-11-04    10766.19
## 36    2012-11-05    10439.00
## 37    2012-11-06     8334.00
## 38    2012-11-07    12883.00
## 39    2012-11-08     3219.00
## 40    2012-11-09    10766.19
## 41    2012-11-10    10766.19
## 42    2012-11-11    12608.00
## 43    2012-11-12    10765.00
## 44    2012-11-13     7336.00
## 45    2012-11-14    10766.19
## 46    2012-11-15       41.00
## 47    2012-11-16     5441.00
## 48    2012-11-17    14339.00
## 49    2012-11-18    15110.00
## 50    2012-11-19     8841.00
## 51    2012-11-20     4472.00
## 52    2012-11-21    12787.00
## 53    2012-11-22    20427.00
## 54    2012-11-23    21194.00
## 55    2012-11-24    14478.00
## 56    2012-11-25    11834.00
## 57    2012-11-26    11162.00
## 58    2012-11-27    13646.00
## 59    2012-11-28    10183.00
## 60    2012-11-29     7047.00
## 61    2012-11-30    10766.19
```

###Histogram of Total Steps per Day###

```r
# Calculate Steps Taken Per Day and Assign Column Names to the Result
hist(  stepsPerDay$total_steps
     , main="Histogram of Total Steps per Day"
     , xlab = "Steps per Day"
     , ylab = "Frequency of Total Steps per Day"
     , col = "Blue"
     , density = 15
     , border = "Black"
     , breaks = 10)
```

![](PA1_template_files/figure-html/unnamed-chunk-15-1.png) 

###Do these values differ from the estimates from the first part of the assignment? ###
Yes, the values differ from the estimates in the first part of the assignments where the rows with NA values were ignored.

###What is the impact of imputing missing data on the estimates of the total daily number of steps?###
The frequency of days with more stays has increased and now the distribution looks more gaussian.

###Mean Steps Taken per Day###
The mean steps taken per day is calculated immediately below:

```r
mean(stepsPerDay$total_steps)
```

```
## [1] 10766.19
```

###Median Steps Taken per Day###
The median steps taken per day is calculated immediately below:

```r
median(stepsPerDay$total_steps)
```

```
## [1] 10766.19
```

## Are there differences in activity patterns between weekdays and weekends?

For this part the weekdays() function may be of some help here. Use the dataset with the filled-in missing values for this part.

dataNew$dayOfWeek <- weekdays(dataNew$date_recorded)

dataNew$dayType <- ifelse(dataNew$dayOfWeek == "Saturday" | dataNew$dayOfWeek =="Sunday","Weekend", "Weekday")

Create a new factor variable in the dataset with two levels – “weekday” and “weekend” indicating whether a given date is a weekday or weekend day.

Make a panel plot containing a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all weekday days or weekend days (y-axis). See the README file in the GitHub repository to see an example of what this plot should look like using simulated data.

library(lattice) 
xyplot(mpg~wt|cyl.f*gear.f, 
    main="Scatterplots by Cylinders and Gears", 
   ylab="Miles per Gallon", xlab="Car Weight")


