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

Converting it to a "tidy" data frame:


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
