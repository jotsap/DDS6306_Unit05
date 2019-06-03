---
title: "LiveSession05"
author: "Jeremy Otsap"
date: "June 2, 2019"
output: 
  html_document:
    keep_md: true
---



## Question 1

2016 Child names


```r
# import yob2016 file
y2016 <- read.table("data/yob2016.txt", header = F, sep = ";", col.names = c("FirstName","Gender","Occurances"))

# dimensions & structure of dataframe
str(y2016)
```

```
## 'data.frame':	32869 obs. of  3 variables:
##  $ FirstName : Factor w/ 30295 levels "Aaban","Aabha",..: 9317 22546 3770 26409 12019 20596 6185 339 9298 11222 ...
##  $ Gender    : Factor w/ 2 levels "F","M": 1 1 1 1 1 1 1 1 1 1 ...
##  $ Occurances: int  19414 19246 16237 16070 14722 14366 13030 11699 10926 10733 ...
```

```r
dim(y2016)
```

```
## [1] 32869     3
```

```r
#preview
y2016[1:10,]
```

```
##    FirstName Gender Occurances
## 1       Emma      F      19414
## 2     Olivia      F      19246
## 3        Ava      F      16237
## 4     Sophia      F      16070
## 5   Isabella      F      14722
## 6        Mia      F      14366
## 7  Charlotte      F      13030
## 8    Abigail      F      11699
## 9      Emily      F      10926
## 10    Harper      F      10733
```


Finding the problem entry with "yyy" at the end


```r
# Locating problem entry with 'yyy'
y2016[grep('yyy', y2016$FirstName),]
```

```
##     FirstName Gender Occurances
## 212  Fionayyy      F       1547
```


Remove duplicate row from data set


```r
slice(y2016, -212) -> y2016
y2016[205:215,]
```

```
##     FirstName Gender Occurances
## 205    Hayden      F       1569
## 206     Alana      F       1564
## 207   Rebecca      F       1563
## 208  Michelle      F       1559
## 209    Eloise      F       1554
## 210      Lila      F       1549
## 211     Fiona      F       1547
## 212    Callie      F       1531
## 213     Lucia      F       1511
## 214    Angela      F       1503
## 215    Marley      F       1491
```

```r
#sorting data frame by FirstName
y2016[order(y2016$FirstName),] -> y2016
```



## Question 2

For the last 10 child names of 2015 they all are male and all have a total of 5 occurances. Also looks like its already in alphabetical order?


```r
# import yob2015 file
y2015 <- read.table("data/yob2015.txt", header = F, sep = ",", col.names = c("FirstName","Gender","Occurances"))

#last 10 rows
tail(y2015, 10)
```

```
##       FirstName Gender Occurances
## 33054      Ziyu      M          5
## 33055      Zoel      M          5
## 33056     Zohar      M          5
## 33057    Zolton      M          5
## 33058      Zyah      M          5
## 33059    Zykell      M          5
## 33060    Zyking      M          5
## 33061     Zykir      M          5
## 33062     Zyrus      M          5
## 33063      Zyus      M          5
```

```r
#sorting 2015 data frame
y2015[order(y2015$FirstName),] -> y2015
```


Merging 2015 and 2016 childname data frames


```r
# using merge to emulate outer join
# only names picked in 2015 AND 2016 will be in final dataset
merge(y2015, y2016, by = c("FirstName","Gender")) -> final

colnames(final) <- c("Name","Gender","Occurances2015","Occurances2016")
```


## Question 3

Data summary of merged data sets showing most top 10 most popular names across 2015 and 2016


```r
# create Total column
final$Total <- final$Occurances2015 + final$Occurances2016

# sorting by largest Total
final[order(final$Total, decreasing = T),] -> finaltotal

head(finaltotal, 10)
```

```
##           Name Gender Occurances2015 Occurances2016 Total
## 8290      Emma      F          20415          19414 39829
## 19886   Olivia      F          19638          19246 38884
## 19594     Noah      M          19594          19015 38609
## 16114     Liam      M          18330          18138 36468
## 23273   Sophia      F          17381          16070 33451
## 3252       Ava      F          16340          16237 32577
## 17715    Mason      M          16591          15192 31783
## 25241  William      M          15863          15668 31531
## 10993    Jacob      M          15914          14416 30330
## 10682 Isabella      F          15574          14722 30296
```

Top 10 girl names


```r
# all girl names
finaltotal[finaltotal$Gender == 'F',] -> finalgirl
head(finalgirl[,c(1,5)],10)
```

```
##            Name Total
## 8290       Emma 39829
## 19886    Olivia 38884
## 23273    Sophia 33451
## 3252        Ava 32577
## 10682  Isabella 30296
## 18247       Mia 29237
## 5493  Charlotte 24411
## 277     Abigail 24070
## 8273      Emily 22692
## 9980     Harper 21016
```

```r
# Export to CSV
finalgirl[,c(1,5)] -> finalgirlfile
write.csv(finalgirlfile, file = "data/finalgirl.csv", col.names = T)
```

```
## Warning in write.csv(finalgirlfile, file = "data/finalgirl.csv", col.names
## = T): attempt to set 'col.names' ignored
```


## Question 4

####GitHub Address  
####https://github.com/jotsap/DDS6306_Unit05












