---
title: "Wrangling Data"
author: "Andy"
date: "16 May 2020"
output: 
  html_document: 
    keep_md: yes
---



##Data Wrangling Webinar

This is a step-through of the work done in the Data Wrangling Webinar from RStudio.

Webinar is here: <https://rstudio.com/resources/webinars/data-wrangling-with-r-and-rstudio/>

Firstly, load essential packages:


```r
library("tidyr")
library("dplyr")
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```


This is an example of tidy data. Each column is a variable to be called upon.


```r
load("storms.rdata")
storms
```

```
##     storm wind pressure       date
## 1 Alberto  110     1007 2000-08-03
## 2    Alex   45     1009 1998-07-27
## 3 Allison   65     1005 1995-06-03
## 4     Ana   40     1013 1997-06-30
## 5  Arlene   50     1010 1999-06-11
## 6  Arthur   45     1010 1996-06-17
```

Whilst this is an examploe of data that is not yet "tidy"


```r
load("cases.rdata")
cases
```

```
##   country  2011  2012  2013
## 1      FR  7000  6900  7000
## 2      DE  5800  6000  6200
## 3      US 15000 14000 13000
```

The issue is that the same variable with multiple years is split across the data frame. Really we want a 3 column dataframe - Country / Year / "n"

Converting it to a "tidy" data frame we use the *gather* function:


```r
gather(cases, "year","n",2:4)
```

```
##   country year     n
## 1      FR 2011  7000
## 2      DE 2011  5800
## 3      US 2011 15000
## 4      FR 2012  6900
## 5      DE 2012  6000
## 6      US 2012 14000
## 7      FR 2013  7000
## 8      DE 2013  6200
## 9      US 2013 13000
```

Within the gather function:  
 --- **cases** is the data frame to be transformed  
 --- **year** is the name of the new column variable, representing the column names of the original data frame  
 --- **n** is the name of the new column variable that includes all the values of the data frame  
 --- **2:4** is used to show that only columns 2 to 4 of the original data frame need to be transformed into our new data frame.
 
 Next, to move onto different issues with another dataset.
 

```r
load("pollution.rdata")
pollution
```

```
##       city  size amount
## 1 New York large     23
## 2 New York small     14
## 3   London large     22
## 4   London small     16
## 5  Beijing large    121
## 6  Beijing small     56
```
Here, the issue is that there is a duplication of variables in the "city" column which are matched to the size. We want to separate this out into 3 columns of city / large / small. In this way the data can be called easily with reference to a variable in the data frame.

Here we use the *spread* function to make it "tidy"


```r
spread(pollution, size, amount)
```

```
##       city large small
## 1  Beijing   121    56
## 2   London    22    16
## 3 New York    23    14
```

The spread function inputs are:  
---**pollution** as the data frame to transform  
---**size** as the key column to use with each factor used as a new column  
---**amount** as the associated values to be used in the dataframe  

Note that *spread* and *gather* are opposites. Applying both should return the original data frame. 

Another useful function is *separate* which creates additional columns from one original column:


```r
storms2<-separate(storms, date, c("year","month", "day"), sep = "-")
storms2
```

```
## # A tibble: 6 x 6
##   storm    wind pressure year  month day  
##   <chr>   <int>    <int> <chr> <chr> <chr>
## 1 Alberto   110     1007 2000  08    03   
## 2 Alex       45     1009 1998  07    27   
## 3 Allison    65     1005 1995  06    03   
## 4 Ana        40     1013 1997  06    30   
## 5 Arlene     50     1010 1999  06    11   
## 6 Arthur     45     1010 1996  06    17
```

The individual parts of separate are fairly 

*unite* is the opposite of *separate*


```r
storms3<-unite(storms2, "date", year, month, day, sep = "-")
storms3
```

```
## # A tibble: 6 x 4
##   storm    wind pressure date      
##   <chr>   <int>    <int> <chr>     
## 1 Alberto   110     1007 2000-08-03
## 2 Alex       45     1009 1998-07-27
## 3 Allison    65     1005 1995-06-03
## 4 Ana        40     1013 1997-06-30
## 5 Arlene     50     1010 1999-06-11
## 6 Arthur     45     1010 1996-06-17
```

