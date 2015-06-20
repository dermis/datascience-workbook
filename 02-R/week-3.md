# Week #3


### Q1

Take a look at the 'iris' dataset that comes with R. The data can be loaded with the code:

```
library(datasets)
data(iris)
```

There will be an object called 'iris' in your workspace. In this dataset, what is the mean of 'Sepal.Length' for the species virginica? (Please only enter the numeric result and nothing else.)

```
mean(iris[iris$Species == "virginica",]$Sepal.Length)
```

**= 6.588**


### Q3

Load the 'mtcars' dataset in R with the following code

```
library(datasets)
data(mtcars)
```

How can one calculate the average miles per gallon (mpg) by number of cylinders in the car (cyl)?

```
tapply(mtcars$mpg, mtcars$cyl, mean)
```


### Q4

Continuing with the 'mtcars' dataset from the previous Question, what is the absolute difference between the average horsepower of 4-cylinder cars and the average horsepower of 8-cylinder cars?

```
# get mean for cyl = 4
mean.cyl.4 <- mean(mtcars[mtcars$cyl == 4,]$hp)
# mean.cyl.4 = 82.63636

# get mean for cyl = 8
mean.cyl.8 <- mean(mtcars[mtcars$cyl == 8,]$hp)
# mean.cyl.8 = 209.2143

# absolute difference
abs(mean.cyl.4, mean.cyl.8)
# = 126.5779

```

**= 126.5779**
