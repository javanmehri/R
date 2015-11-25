# Managing Data Frames with dplyr

This package is specifically designed to help you work with data frames.

## dplyr Properties

- The first argument is a data frame.

- The subsequent arguments describe what to do with it, and you can refer to column in the data frame directly without using the $ operator (just use the names).

- The result is a new data frame.

- Data frame must be properly formatted and annotated for this to all be useful.

```r
> install.packages("dplyr")
> library(dplyr)

> chicago <- readRDS(“chicago.rds")
> dim(chicago)
[1] 6940    8

> str(chicago)
'data.frame':	6940 obs. of  8 variables:
 $ city      : chr  "chic" "chic" "chic" "chic" ...
 $ tmpd      : num  31.5 33 33 29 32 40 34.5 29 26.5 32.5 ...
 $ dptp      : num  31.5 29.9 27.4 28.6 28.9 ...
 $ date      : Date, format: "1987-01-01" "1987-01-02" "1987-01-03" "1987-01-04" ...
 $ pm25tmean2: num  NA NA NA NA NA NA NA NA NA NA ...
 $ pm10tmean2: num  34 NA 34.2 47 NA ...
 $ o3tmean2  : num  4.25 3.3 3.33 4.38 4.75 ...
 $ no2tmean2 : num  20 23.2 23.8 30.4 30.3 ...

> names(chicago)
[1] "city"       "tmpd"       "dptp"      
[4] "date"       "pm25tmean2" "pm10tmean2"
[7] "o3tmean2"   "no2tmean2" 

```

## Selecting

```r
> head(select(chicago, city:dptp))
  city tmpd   dptp
1 chic 31.5 31.500
2 chic 33.0 29.875
3 chic 33.0 27.375
4 chic 29.0 28.625
5 chic 32.0 28.875
6 chic 40.0 35.125

> head(select(chicago, -(city:dptp)))
        date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 1987-01-01         NA   34.00000 4.250000  19.98810
2 1987-01-02         NA         NA 3.304348  23.19099
3 1987-01-03         NA   34.16667 3.333333  23.81548
4 1987-01-04         NA   47.00000 4.375000  30.43452
5 1987-01-05         NA         NA 4.750000  30.33333
6 1987-01-06         NA   48.00000 5.833333  25.77233
```
The command Shows all the columns but not city to dptp columns.

In order to do this in regular way we have to find the index for where city column is and you have to find the index for where dptp column is and then you need to take the negative of those indices.

```r
> i <- match("city", names(chicago))
> i
[1] 1
> j <- match("dptp", names(chicago))
> j
[1] 3

> head(chicago[,-(i:j)])
        date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 1987-01-01         NA   34.00000 4.250000  19.98810
2 1987-01-02         NA         NA 3.304348  23.19099
3 1987-01-03         NA   34.16667 3.333333  23.81548
4 1987-01-04         NA   47.00000 4.375000  30.43452
5 1987-01-05         NA         NA 4.750000  30.33333
6 1987-01-06         NA   48.00000 5.833333  25.77233
```

## filter Command

Used to subset rows based on conditions

```r
> chic.f <- filter(chicago, pm25tmean2 > 30)
> head(chic.f, 10)
   city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2 no2tmean2
1  chic   23 21.9 1998-01-17      38.10   32.46154  3.180556  25.30000
2  chic   28 25.8 1998-01-23      33.95   38.69231  1.750000  29.37630
3  chic   55 51.3 1998-04-30      39.40   34.00000 10.786232  25.31310
4  chic   59 53.7 1998-05-01      35.40   28.50000 14.295125  31.42905
5  chic   57 52.0 1998-05-02      33.30   35.00000 20.662879  26.79861
6  chic   57 56.0 1998-05-07      32.10   34.50000 24.270422  33.99167
7  chic   75 65.8 1998-05-15      56.50   91.00000 38.573007  29.03261
8  chic   61 59.0 1998-06-09      33.80   26.00000 17.890810  25.49668
9  chic   73 60.3 1998-07-13      30.30   64.50000 37.018865  37.93056
10 chic   78 67.1 1998-07-14      41.40   75.00000 40.080902  32.59054

> chic.f <- filter(chicago, pm25tmean2 > 30 & tmpd > 80)
> head(chic.f, 10)
   city tmpd dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1  chic   81 71.2 1998-08-23    39.6000       59.0 45.86364  14.32639
2  chic   81 70.4 1998-09-06    31.5000       50.5 50.66250  20.31250
3  chic   82 72.2 2001-07-20    32.3000       58.5 33.00380  33.67500
4  chic   84 72.9 2001-08-01    43.7000       81.5 45.17736  27.44239
5  chic   85 72.6 2001-08-08    38.8375       70.0 37.98047  27.62743
6  chic   84 72.6 2001-08-09    38.2000       66.0 36.73245  26.46742
7  chic   82 67.4 2002-06-20    33.0000       80.5 47.42673  30.76703
8  chic   82 63.5 2002-06-23    42.5000       65.0 54.88043  30.03913
9  chic   81 70.4 2002-07-08    33.1000       64.0 45.34969  27.67857
10 chic   82 66.2 2002-07-18    38.8500       72.5 44.98045  26.06905
```

## arrange Function

Used to reorder the rows of a data frame based on the values of a column

```r
> chicago <- arrange(chicago, date)
> head(chicago)
  city tmpd   dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
1 chic 31.5 31.500 1987-01-01         NA   34.00000 4.250000  19.98810
2 chic 33.0 29.875 1987-01-02         NA         NA 3.304348  23.19099
3 chic 33.0 27.375 1987-01-03         NA   34.16667 3.333333  23.81548
4 chic 29.0 28.625 1987-01-04         NA   47.00000 4.375000  30.43452
5 chic 32.0 28.875 1987-01-05         NA         NA 4.750000  30.33333
6 chic 40.0 35.125 1987-01-06         NA   48.00000 5.833333  25.77233

> tail(chicago)
     city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2 no2tmean2
6935 chic   35 29.6 2005-12-26    8.40000        8.5 14.041667  16.81944
6936 chic   40 33.6 2005-12-27   23.56000       27.0  4.468750  23.50000
6937 chic   37 34.5 2005-12-28   17.75000       27.5  3.260417  19.28563
6938 chic   35 29.4 2005-12-29    7.45000       23.5  6.794837  19.97222
6939 chic   36 31.0 2005-12-30   15.05714       19.2  3.034420  22.80556
6940 chic   35 30.1 2005-12-31   15.00000       23.5  2.531250  13.25000

> chicago <- arrange(chicago, desc(date))
> head(chicago)
  city tmpd dptp       date pm25tmean2 pm10tmean2  o3tmean2 no2tmean2
1 chic   35 30.1 2005-12-31   15.00000       23.5  2.531250  13.25000
2 chic   36 31.0 2005-12-30   15.05714       19.2  3.034420  22.80556
3 chic   35 29.4 2005-12-29    7.45000       23.5  6.794837  19.97222
4 chic   37 34.5 2005-12-28   17.75000       27.5  3.260417  19.28563
5 chic   40 33.6 2005-12-27   23.56000       27.0  4.468750  23.50000
6 chic   35 29.6 2005-12-26    8.40000        8.5 14.041667  16.81944

> tail(chicago)
     city tmpd   dptp       date pm25tmean2 pm10tmean2 o3tmean2 no2tmean2
6935 chic 40.0 35.125 1987-01-06         NA   48.00000 5.833333  25.77233
6936 chic 32.0 28.875 1987-01-05         NA         NA 4.750000  30.33333
6937 chic 29.0 28.625 1987-01-04         NA   47.00000 4.375000  30.43452
6938 chic 33.0 27.375 1987-01-03         NA   34.16667 3.333333  23.81548
6939 chic 33.0 29.875 1987-01-02         NA         NA 3.304348  23.19099
6940 chic 31.5 31.500 1987-01-01         NA   34.00000 4.250000  19.98810 

```

## rename Function

Suppose we want to change the name of “pm25tmean2” to “pm25” and also “dptp” to “dewpoint”

```r
> chicago <- rename(chicago, pm25=pm25tmean2, dewpoint=dptp)
> head(chicago)
  city tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2 no2tmean2
1 chic   35     30.1 2005-12-31 15.00000       23.5  2.531250  13.25000
2 chic   36     31.0 2005-12-30 15.05714       19.2  3.034420  22.80556
3 chic   35     29.4 2005-12-29  7.45000       23.5  6.794837  19.97222
4 chic   37     34.5 2005-12-28 17.75000       27.5  3.260417  19.28563
5 chic   40     33.6 2005-12-27 23.56000       27.0  4.468750  23.50000
6 chic   35     29.6 2005-12-26  8.40000        8.5 14.041667  16.81944

```

## mutate Function

Used to transform existing variables or to create new variables

```r
> chicago <- mutate(chicago, pm25detrend=pm25-mean(pm25, na.rm=TRUE))
> head(chicago)
  city tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2 no2tmean2 pm25detrend
1 chic   35     30.1 2005-12-31 15.00000       23.5  2.531250  13.25000   -1.230958
2 chic   36     31.0 2005-12-30 15.05714       19.2  3.034420  22.80556   -1.173815
3 chic   35     29.4 2005-12-29  7.45000       23.5  6.794837  19.97222   -8.780958
4 chic   37     34.5 2005-12-28 17.75000       27.5  3.260417  19.28563    1.519042
5 chic   40     33.6 2005-12-27 23.56000       27.0  4.468750  23.50000    7.329042
6 chic   35     29.6 2005-12-26  8.40000        8.5 14.041667  16.81944   -7.830958

```

## group_by Function

Allows you to split a data frame according to certain categorical variables.

In this example, at first, we create a temperature category variable which will indicate whether a given day was hot or cold, depending on whether the temperature was over 80 degrees or not.

```r
 > chicago <- mutate(chicago, tempcat = factor(1*(tmpd>80), labels=c(“cold","hot")))

> head(chicago)
  city tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2 no2tmean2 pm25detrend tempcat
1 chic   35     30.1 2005-12-31 15.00000       23.5  2.531250  13.25000   -1.230958    cold
2 chic   36     31.0 2005-12-30 15.05714       19.2  3.034420  22.80556   -1.173815    cold
3 chic   35     29.4 2005-12-29  7.45000       23.5  6.794837  19.97222   -8.780958    cold
4 chic   37     34.5 2005-12-28 17.75000       27.5  3.260417  19.28563    1.519042    cold
5 chic   40     33.6 2005-12-27 23.56000       27.0  4.468750  23.50000    7.329042    cold
6 chic   35     29.6 2005-12-26  8.40000        8.5 14.041667  16.81944   -7.830958    cold

> hotcold <- group_by(chicago, tempcat)
> hotcold
Source: local data frame [6,940 x 10]
Groups: tempcat [3]

    city  tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2 no2tmean2 pm25detrend tempcat
   (chr) (dbl)    (dbl)     (date)    (dbl)      (dbl)     (dbl)     (dbl)       (dbl)  (fctr)
1   chic    35     30.1 2005-12-31 15.00000       23.5  2.531250  13.25000   -1.230958    cold
2   chic    36     31.0 2005-12-30 15.05714       19.2  3.034420  22.80556   -1.173815    cold
3   chic    35     29.4 2005-12-29  7.45000       23.5  6.794837  19.97222   -8.780958    cold
4   chic    37     34.5 2005-12-28 17.75000       27.5  3.260417  19.28563    1.519042    cold
5   chic    40     33.6 2005-12-27 23.56000       27.0  4.468750  23.50000    7.329042    cold
6   chic    35     29.6 2005-12-26  8.40000        8.5 14.041667  16.81944   -7.830958    cold
7   chic    35     32.1 2005-12-25  6.70000        8.0 14.354167  13.79167   -9.530958    cold
8   chic    37     35.2 2005-12-24 30.77143       25.2  1.770833  31.98611   14.540471    cold
9   chic    41     32.6 2005-12-23 32.90000       34.5  6.906250  29.08333   16.669042    cold
10  chic    22     23.3 2005-12-22 36.65000       42.5  5.385417  33.73026   20.419042    cold

```

Now suppose I want to know what is the mean pm25 for both hot and cold days. What’s the maximum ozone for hot and cold days, and  what is the median nitrogen dioxide or no2 for both hot and cold days.

```r
> summarize(hotcold, pm25=mean(pm25), o3=max(o3tmean2), no2=median(no2tmean2))
Source: local data frame [3 x 4]

  tempcat    pm25        o3      no2
   (fctr)   (dbl)     (dbl)    (dbl)
1    cold      NA 66.587500 24.54924
2     hot      NA 62.969656 24.93870
3      NA 47.7375  9.416667 37.44444

For the missing values:

> summarize(hotcold, pm25=mean(pm25, na.rm=TRUE), o3=max(o3tmean2), no2=median(no2tmean2))
Source: local data frame [3 x 4]

  tempcat     pm25        o3      no2
   (fctr)    (dbl)     (dbl)    (dbl)
1    cold 15.97807 66.587500 24.54924
2     hot 26.48118 62.969656 24.93870
3      NA 47.73750  9.416667 37.44444

> chicago <- mutate(chicago, year=as.POSIXlt(date)$year + 1900)
> head(chicago)
  city tmpd dewpoint       date     pm25 pm10tmean2  o3tmean2 no2tmean2 pm25detrend tempcat year
1 chic   35     30.1 2005-12-31 15.00000       23.5  2.531250  13.25000   -1.230958    cold 2005
2 chic   36     31.0 2005-12-30 15.05714       19.2  3.034420  22.80556   -1.173815    cold 2005
3 chic   35     29.4 2005-12-29  7.45000       23.5  6.794837  19.97222   -8.780958    cold 2005
4 chic   37     34.5 2005-12-28 17.75000       27.5  3.260417  19.28563    1.519042    cold 2005
5 chic   40     33.6 2005-12-27 23.56000       27.0  4.468750  23.50000    7.329042    cold 2005
6 chic   35     29.6 2005-12-26  8.40000        8.5 14.041667  16.81944   -7.830958    cold 2005

> years <- group_by(chicago, year)
> summarize(years, pm25=mean(pm25, na.rm=TRUE), o3=max(o3tmean2), no2=median(no2tmean2))
Source: local data frame [19 x 4]

    year     pm25       o3      no2
   (dbl)    (dbl)    (dbl)    (dbl)
1   1987      NaN 62.96966 23.49369
2   1988      NaN 61.67708 24.52296
3   1989      NaN 59.72727 26.14062
4   1990      NaN 52.22917 22.59583
5   1991      NaN 63.10417 21.38194
6   1992      NaN 50.82870 24.78921
7   1993      NaN 44.30093 25.76993
8   1994      NaN 52.17844 28.47500
9   1995      NaN 66.58750 27.26042
10  1996      NaN 58.39583 26.38715
11  1997      NaN 56.54167 25.48143
12  1998 18.26467 50.66250 24.58649
13  1999 18.49646 57.48864 24.66667
14  2000 16.93806 55.76103 23.46082
15  2001 16.92632 51.81984 25.06522
16  2002 15.27335 54.88043 22.73750
17  2003 15.23183 56.16608 24.62500
18  2004 14.62864 44.48240 23.39130
19  2005 16.18556 58.84126 22.62387

```

## Pipeline Operators %>%

Allows you to chain different operations together. The idea is that you take a data set you feed through a pipeline of operations to create a new data set.

Suppose I want to take chicago data set and I want to mutate to create a month variable because I want to create a summary of each of the pollutant variables by month. Then I want to take output of mutate and then group by it according to this month variable. And then I want to take the output of group by and then run it through summarize.

If I use pipeline operator I don’t have to specify the data frame as the first argument.


```r
> chicago %>% mutate(month=as.POSIXlt(date)$mon+1) %>% group_by(month) %>% summarize(pm25=mean(pm25, na.rm=TRUE), o3=max(o3tmean2), no2=median(no2tmean2))
Source: local data frame [12 x 4]

   month     pm25       o3      no2
   (dbl)    (dbl)    (dbl)    (dbl)
1      1 17.76996 28.22222 25.35417
2      2 20.37513 37.37500 26.78034
3      3 17.40818 39.05000 26.76984
4      4 13.85879 47.94907 25.03125
5      5 14.07420 52.75000 24.22222
6      6 15.86461 66.58750 25.01140
7      7 16.57087 59.54167 22.38442
8      8 16.93380 53.96701 22.98333
9      9 15.91279 57.48864 24.47917
10    10 14.23557 47.09275 24.15217
11    11 15.15794 29.45833 23.56537
12    12 17.52221 27.70833 24.45773

```
Once you learn the dplyr “grammar” there are a few additional benefits
dplyr can work with other data frame “backends”
data.table for large fast tables
SQL interface for relational databases via the DBI package

Credit: Jeff Leek, Assistant Professor of Biostatistics at the Johns Hopkins Bloomberg School of Public Health
