Assignment One
========================================================

Data about personal movement **Fitbit**, **Nike Fuelband**, or **Jawbone Up**.

Analysis statistics.

Loading data.


```r
require(lubridate)
require(plyr)
data<- read.csv(unzip("repdata_data_activity.zip"))
summary(data)
```

```
##      steps               date          interval   
##  Min.   :  0.0   2012-10-01:  288   Min.   :   0  
##  1st Qu.:  0.0   2012-10-02:  288   1st Qu.: 589  
##  Median :  0.0   2012-10-03:  288   Median :1178  
##  Mean   : 37.4   2012-10-04:  288   Mean   :1178  
##  3rd Qu.: 12.0   2012-10-05:  288   3rd Qu.:1766  
##  Max.   :806.0   2012-10-06:  288   Max.   :2355  
##  NA's   :2304    (Other)   :15840
```

```r
data$date<-ymd(data$date)
data<-mutate(data, day=day(date))
```

**What is mean total number of steps taken per day?**


```r
require(plyr)

ttsteps<-ddply(data,.(day),summarise, t_steps=sum(steps,na.rm=T), m_steps=mean(steps,na.rm=T))

ttsteps1<-ddply(data,.(day,interval),summarise, t_steps=sum(steps,na.rm=T), m_steps=mean(steps,na.rm=T))

ttsteps2<-ddply(ttsteps1,.(interval),summarise, t_steps=sum(t_steps,na.rm=T), m_steps=mean(m_steps,na.rm=T))

mean(ttsteps$t_steps,na.rm=T)
```

```
## [1] 18407
```
Histogram

```r
hist(ttsteps$t_steps, xlab="total number of steps",main="Number of steps for day")
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 

Mean 


```r
mean(ttsteps$t_steps,na.rm=T)
```

```
## [1] 18407
```
Median

```r
median(ttsteps$t_steps,na.rm=T)
```

```
## [1] 20525
```


**What is the average daily activity pattern?**  
Make a time series plot (i.e. type = "l") of the 5-minute interval (x-axis) and the average number of steps taken, averaged across all days (y-axis)


```r
plot.ts(x=ttsteps2$interval,y=ttsteps2$m_steps,type="l",xlab="Interval",ylab="Mean steps per day",main="Average daily activity pattern")
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 

Which 5-minute interval, on average across all the days in the dataset, contains the maximum number of steps?


```r
ttsteps2[which.max(ttsteps2$m_steps),]
```

```
##     interval t_steps m_steps
## 104      835   10927   200.8
```

**Imputing missing values**

Note that there are a number of days/intervals where there are missing values (coded as NA). The presence of missing days may introduce bias into some calculations or summaries of the data.

Calculate and report the total number of missing values in the dataset (i.e. the total number of rows with NAs)


```r
summary(data)
```

```
##      steps            date               interval         day      
##  Min.   :  0.0   Min.   :2012-10-01   Min.   :   0   Min.   : 1.0  
##  1st Qu.:  0.0   1st Qu.:2012-10-16   1st Qu.: 589   1st Qu.: 8.0  
##  Median :  0.0   Median :2012-10-31   Median :1178   Median :16.0  
##  Mean   : 37.4   Mean   :2012-10-31   Mean   :1178   Mean   :15.8  
##  3rd Qu.: 12.0   3rd Qu.:2012-11-15   3rd Qu.:1766   3rd Qu.:23.0  
##  Max.   :806.0   Max.   :2012-11-30   Max.   :2355   Max.   :31.0  
##  NA's   :2304
```

Devise a strategy for filling in all of the missing values in the dataset. The strategy does not need to be sophisticated. For example, you could use the mean/median for that day, or the mean for that 5-minute interval, etc.


Create a new dataset that is equal to the original dataset but with the missing data filled in.


Make a histogram of the total number of steps taken each day and Calculate and report the mean and median total number of steps taken per day. Do these values differ from the estimates from the first part of the assignment? What is the impact of imputing missing data on the estimates of the total daily number of steps?.


