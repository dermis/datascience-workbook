# Week #1

```
## What is the value of Ozone in the 47th row?

data[47,c("Ozone")]
```

```
## How many missing values are in the Ozone column of this data frame?

miss <- is.na(dataset[, "Ozone"])
sum(miss)
```

```
## What is the mean of the Ozone column in this dataset? Exclude missing values (coded as NA) from this calculation.

mean(data$Ozone[!is.na(data$Ozone)])

mean(dataset[, "Ozone"], na.rm = TRUE)
```

```
## Extract the subset of rows of the data frame where Ozone values are above 31 and Temp values are above 90. What is the mean of Solar.R in this subset?

good <- complete.cases(data$Ozone, data$Solar.R, data$Temp)
mean(data$Solar.R[good & data$Ozone > 31 & data$Temp > 90])
```

```
## What is the mean of "Temp" when "Month" is equal to 6?

good <- complete.cases(data$Month, data$Temp)
mean(data$Temp[good & data$Month == 6])
```
