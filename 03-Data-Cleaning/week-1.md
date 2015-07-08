# Quiz #1

## 1.
```
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv

and load the data into R. The code book, describing the variable names is here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf

How many properties are worth $1,000,000 or more?

data <- read.csv("survey.csv", colClasses="character")
col.val <- data[,37]
data[,37] <- suppressWarnings(as.numeric(data[,37]))
data.val.clean <- data[complete.cases(data$VAL),]
sum(data.val.clean$VAL >= 24)
```

## 2.
```
Use the data you loaded from Question 1. Consider the variable FES in the code book. Which of the "tidy data" principles does this variable violate?

Tidy data has one variable per column.
```

## 3.
```
Download the Excel spreadsheet on Natural Gas Aquisition Program here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FDATA.gov_NGAP.xlsx

Read rows 18-23 and columns 7-15 into R and assign the result to a variable called:
 dat
What is the value of:
 sum(dat$Zip*dat$Ext,na.rm=T)
(original data source: http://catalog.data.gov/dataset/natural-gas-acquisition-program)

install.packages("xlsx")
library(xlsx)
rowIndex <- 18:23
colIndex <- 7:15
dat <- read.xlsx("nga.xlsx", sheetIndex = 1, rowIndex = rowIndex, colIndex = colIndex)
sum(dat$Zip*dat$Ext,na.rm=T)
36534720
```

## 4.
```
Read the XML data on Baltimore restaurants from here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Frestaurants.xml

How many restaurants have zipcode 21231?

install.packages("XML")
library(XML)
library(RCurl)
xmlFile <- getURL(url)
doc <- xmlTreeParse(xmlFile, useInternalNodes = TRUE)
root.node <- xmlRoot(doc)
sum(xpathSApply(root.node,"//zipcode",xmlValue)==21231)
127
```

## 5.
```
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv

using the fread() command load the data into an R object
 DT
Which of the following is the fastest way to calculate the average value of the variable
pwgtp15
broken down by sex using the data.table package?

install.packages("data.table",dependencies = TRUE, repos="http://cran.r-project.org")
library(data.table)
DT <- fread("ss06pid.csv")

system.time(rowMeans(DT)[DT$SEX==1])+system.time(rowMeans(DT)[DT$SEX==2])
Timing stopped at: 0.621 0.021 0.642

system.time(mean(DT[DT$SEX==1,]$pwgtp15))+system.time(mean(DT[DT$SEX==2,]$pwgtp15))
user  system elapsed
0.038   0.001   0.039

system.time(mean(DT$pwgtp15,by=DT$SEX))
user  system elapsed
0.000   0.000   0.001

system.time(tapply(DT$pwgtp15,DT$SEX,mean))
user  system elapsed
0.002   0.000   0.002  

system.time(sapply(split(DT$pwgtp15,DT$SEX),mean))
user  system elapsed
0.002   0.000   0.001

system.time(DT[,mean(pwgtp15),by=SEX])
user  system elapsed
0.002   0.000   0.002
```
