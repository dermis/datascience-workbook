# Week #2

**Pollutant Mean**

```
## pollutantmean.R

pollutantmean <- function(directory, pollutant, id=1:332) {

  filelist <- list.files(directory, full.names = TRUE)

  df <- data.frame()

  for(i in id) {

    df <- rbind(df, read.csv(filelist[i]))

  }

  mean(df[,pollutant], na.rm = TRUE)
}
```

**Complete Observations**

```
## complete.R

complete <- function(directory, id=1:332) {

  filelist <- list.files(directory, full.names = TRUE)

  df <- data.frame()

  for(i in id) {

    fileData <- read.csv(filelist[i])
    cd <- subset(fileData, !is.na(fileData$sulfate) & !is.na(fileData$nitrate))

    df <- rbind(df, cbind(i,nrow(cd)))

  }

  names(df) <- c("id", "nobs")

  return(df)

}
```

**Correlation**

```
## corr.R

corr <- function(directory, threshold = 0) {

  ## 'directory' is a character vector of length 1 indicating
  ## the location of the CSV files

  ## 'threshold' is a numeric vector of length 1 indicating the
  ## number of completely observed observations (on all
  ## variables) required to compute the correlation between
  ## nitrate and sulfate; the default is 0

  ## Return a numeric vector of correlations
  ## NOTE: Do not round the result!

  ## TEST 1
  # source("corr.R")
  # source("complete.R")
  # cr <- corr("specdata", 150)
  # head(cr)
  ## [1] -0.01896 -0.14051 -0.04390 -0.06816 -0.12351 -0.07589

  ## TEST 2
  # summary(cr)
  ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  ## -0.2110 -0.0500  0.0946  0.1250  0.2680  0.7630

  ## TEST 3
  # cr <- corr("specdata", 400)
  # head(cr)
  ## [1] -0.01896 -0.04390 -0.06816 -0.07589  0.76313 -0.15783

  ## TEST 4
  # summary(cr)
  ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  ## -0.1760 -0.0311  0.1000  0.1400  0.2680  0.7630

  ## TEST 5
  # cr <- corr("specdata", 5000)
  # summary(cr)
  ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  ##

  ## TEST 6
  # length(cr)
  ## [1] 0

  ## TEST 7
  # cr <- corr("specdata")
  # summary(cr)
  ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.
  ## -1.0000 -0.0528  0.1070  0.1370  0.2780  1.0000

  ## TEST 8
  # length(cr)
  ## [1] 323


  ######################################################

  file.list <- list.files(directory, full.names = TRUE)

  nv <- c()

  for(i in 1:332) {

    data <- read.csv(file.list[i])

    complete.data <- subset(data, !is.na(data$sulfate) & !is.na(data$nitrate))
    #complete.data <- data[complete.cases(data),]

    #write.csv(complete.data, "complete.data.csv", append = TRUE)

    nrow.complete.data <- nrow(complete.data)

    if(nrow.complete.data > threshold) {
      nv <- c(nv, cor(complete.data$sulfate, complete.data$nitrate))
    }

  }

  return(nv)

}
```
