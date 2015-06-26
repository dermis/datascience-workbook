# Quiz

1. What is produced at the end of this snippet of R code?
 ```
 set.seed(1)
 rpois(5, 2)
 ```
 * *A vector with the numbers 1, 1, 2, 4, 1*
 * *Because the `set.seed()' function is used, `rpois()' will always output the same vector in this code.*

2. What R function can be used to generate standard Normal random variables?
 - *rnorm*
 - *Functions beginning with the `r' prefix are used to simulate random variates.*
 - *Standard probability distributions in R have a set of four functions that can be used to simulate variates, evaluate the density, evaluate the cumulative density, and evaluate the quantile function.*

3.  When simulating data, why is using the set.seed() function important?
 * *It ensures that the sequence of random numbers starts in a specific place and is therefore reproducible.*

4. Which function can be used to evaluate the inverse cumulative distribution function for the Poisson distribution?
 * *qpois*
 * *Probability distribution functions beginning with the `q' prefix are used to evaluate the quantile (inverse cumulative distribution) function.*

5. What does the following code do?
 ```
 set.seed(10)
 x <- rep(0:1, each = 5)
 e <- rnorm(10, 0, 20)
 y <- 0.5 + 2 * x + e
 ```
 * *Generate data from a Normal linear model*

6. What R function can be used to generate Binomial random variables?
 * *rbinom*

7. What aspect of the R runtime does the profiler keep track of when an R expression is evaluated?
 * *the function call stack*

8. Consider the following R code
 ```
 library(datasets)
 Rprof()
 fit <- lm(y ~ x1 + x2)
 Rprof(NULL)
 ```
(Assume that y, x1, and x2 are present in the workspace.) Without running the code, what percentage of the run time is spent in the 'lm' function, based on the 'by.total' method of normalization shown in 'summaryRprof()'?
 * *100%*
 * *When using `by.total' normalization, the top-level function (in this case, `lm()') always takes 100% of the time.*

9. When using 'system.time()', what is the user time?
 * *It is the time spent by the CPU evaluating an expression*

10. If a computer has more than one available processor and R is able to take advantage of that, then which of the following is true when using 'system.time()'?
 * *elapsed time may be smaller than user time*


# Programming Assignment 3: Hospital Quality

**best.R**

```
best <- function(state, outcome) {

  ## read outcome data
  data.outcome <- read.csv("outcome-of-care-measures.csv", colClasses = "character")[,c(2,7,11,17,23)]

  ## check that state and outcome are valid
  if(!state %in% data.outcome$State) { stop("invalid state") }

  if(outcome == "heart attack") {
    data.outcome.col <- data.outcome[,c(1,2,3)]
  } else if(outcome == "heart failure") {
    data.outcome.col <- data.outcome[,c(1,2,4)]
  } else if(outcome == "pneumonia") {
    data.outcome.col <- data.outcome[,c(1,2,5)]
  } else {
    stop("invalid outcome")
  }

  ## return hospital name in that state with lowest 30-day death rate
  data.outcome.state <- data.outcome.col[data.outcome.col$State == state,]
  data.outcome.state[,3] <- suppressWarnings(as.numeric(data.outcome.state[,3]))

  data.outcome.clean <- data.outcome.state[complete.cases(data.outcome.state),]

  result <- data.outcome.clean[order(data.outcome.clean[,3], data.outcome.clean$Hospital.Name),]

  vector.result <- as.vector(result$Hospital.Name)

  return(vector.result[1])

}
```

**rankhospital.R**

```
## rankhospital.R

rankhospital <- function(state, outcome, num = "best") {

  ## Read outcome data
  data <- read.csv("outcome-of-care-measures.csv", colClasses = "character") [,c(2,7,11,17,23)]

  ## Check that state are valid
  if(!state %in% data$State) { stop("invalid state") }

  ## Check that outcome are valid
  if(outcome == "heart attack") {
    data <- data[,c(1,2,3)]  
  } else if(outcome == "heart failure") {
    data <- data[,c(1,2,4)]
  } else if(outcome == "pneumonia") {
    data <- data[,c(1,2,5)]
  } else {
    stop("invalid outcome")
  }

  ## Rename long col name
  names(data)[3] <- "Deaths"

  ## filter by state
  data.state <- data[data$State == state,]

  ## cleaning Deaths col
  data.state$Deaths <- suppressWarnings(as.numeric(data.state$Deaths))

  # Remove NA's
  data.clean <- data.state[complete.cases(data.state$Deaths),]

  ## Sorting by deaths
  data.sorted <- data.clean[order(data.clean$Deaths, data.clean$Hospital.Name),]

  ## Convert to vector
  data.vector <- as.vector(data.sorted$Hospital.Name)

  ## Check that num are valid and return result
  if(!is.numeric(num)) {
    if(num == "best") {
      return(data.vector[1])
    } else if(num == "worst") {
      return(data.vector[length(data.vector)])
    } else {
      return("NA")
    }
  } else {
    return(data.vector[num])
  }

}
```

**rankall.R**

```
## rankall.R

rankall <- function(outcome, num = "best") {

  ## Read outcome data
  data <- read.csv("outcome-of-care-measures.csv", colClasses = "character") [,c(2,7,11,17,23)]

  ## Check that outcome are valid
  if(outcome == "heart attack") {
    data <- data[,c(1,2,3)]  
  } else if(outcome == "heart failure") {
    data <- data[,c(1,2,4)]
  } else if(outcome == "pneumonia") {
    data <- data[,c(1,2,5)]
  } else {
    stop("invalid outcome")
  }

  ## Rename long col name
  names(data)[3] <- "Deaths"

  ## Remove Na's
  data$Deaths <- suppressWarnings(as.numeric(data$Deaths))
  data.clean <- data[complete.cases(data$Deaths),]

  ## Group by states
  data.grouped <- split(data.clean, data.clean$State)

  ## Get result
  result <- lapply(data.grouped, function(dg, num) {

    ## Sort by deaths and hospital name
    dg <- dg[order(dg$Deaths, dg$Hospital.Name),]

    ## Check that num are valid and return result
    if(!is.numeric(num)) {
      if(num == "best") {
        return(dg$Hospital.Name[1])
      } else if(num == "worst") {
        return(dg$Hospital.Name[nrow(dg)])
      } else {
        return("NA")
      }
    } else {
      return(dg$Hospital.Name[num])
    }

  }, num)

  return(data.frame(hospital=unlist(result),state=names(result)))

}
```
