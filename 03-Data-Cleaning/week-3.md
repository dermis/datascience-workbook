# Quiz

## 1.

```
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv

and load the data into R. The code book, describing the variable names is here:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf

Create a logical vector that identifies the households on greater than 10 acres who sold more than $10,000 worth of agriculture products. Assign that logical vector to the variable agricultureLogical. Apply the which() function like this to identify the rows of the data frame where the logical vector is TRUE. which(agricultureLogical) What are the first 3 values that result?
```

```
url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv"
download.file(url,destfile = "hid.csv", method = "curl")

# AGS = 6
# ACR = 3

data <- read.csv("hid.csv")
agricultureLogical <- data$ACR == 3 & data$AGS == 6
which(agricultureLogical)
125  238  262

```

## 2.

```
Using the jpeg package read in the following picture of your instructor into R

https://d396qusza40orc.cloudfront.net/getdata%2Fjeff.jpg

Use the parameter native=TRUE. What are the 30th and 80th quantiles of the resulting data? (some Linux systems may produce an answer 638 different for the 30th quantile)
```

```
install.packages("jpeg")
library("jpeg")

download.file(url, destfile = "jeff.jpg", method = "curl")
jeff <- readJPEG("jeff.jpg", native = TRUE)
quantile(jeff, probs = c(0.3,0.8))

30%       80%
-15259150 -10575416
```

## 3.

```
Load the Gross Domestic Product data for the 190 ranked countries in this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv

Load the educational data from this data set:

https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv

Match the data based on the country shortcode. How many of the IDs match? Sort the data frame in descending order by GDP rank (so United States is last). What is the 13th country in the resulting data frame?

Original data sources:
http://data.worldbank.org/data-catalog/GDP-ranking-table
http://data.worldbank.org/data-catalog/ed-stats
```

```
url.gdp <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FGDP.csv"
url.edu <- "https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FEDSTATS_Country.csv"

download.file(url.gdp, destfile = "gdp.csv", method = "curl")
download.file(url.edu, destfile = "edu.csv", method = "curl")

library(data.table)

edu <- read.csv("edu.csv")
gdp <- read.csv("gdp.csv")

data.edu <- edu[,c(1,2,3)]

data.gdp <- gdp[,c(1,2,5)]
data.gdp <- data.gdp[5:194,]
names(data.gdp) <- c("Code","Rank","Gdp")
merged.data <- merge(data.edu, data.gdp, by.x = "CountryCode", by.y = "Code", all = TRUE)

clean.data <- merged.data[complete.cases(merged.data$Rank),]
clean.data <- clean.data[complete.cases(clean.data$Long.Name),]

clean.data$Rank <- as.numeric(as.character(clean.data$Rank))
desc.clean.data <- clean.data[order(clean.data$Rank, decreasing = TRUE),]
```

## 4.

```
dt.clean.data <- data.table(clean.data)
dt.clean.data[, mean(Rank, na.rm = TRUE), by = Income.Group]
```

## 5.

```
dt.quantile <- quantile(dt.clean.data$Rank, probs = seq(0,1,0.2), na.rm = TRUE)
dt.clean.data$quantile <- cut(dt.clean.data$Rank, breaks = dt.quantile)
dt.clean.data[Income.Group == "Lower middle income", .N, by = c("Income.Group", "quantile")]

Income.Group    quantile  N
1: Lower middle income (38.6,76.2] 13
2: Lower middle income   (114,152]  9
3: Lower middle income   (152,190] 16
4: Lower middle income  (76.2,114] 11
5: Lower middle income    (1,38.6]  5

```
