# Quiz #2

## 1.

```
Register an application with the Github API here https://github.com/settings/applications. Access the API to get information on your instructors repositories (hint: this is the url you want "https://api.github.com/users/jtleek/repos"). Use this data to find the time that the datasharing repo was created. What time was it created? This tutorial may be useful (https://github.com/hadley/httr/blob/master/demo/oauth2-github.r). You may also need to run the code in the base R package and not R studio.

library(httr)
library(httpuv)
library(jsonlite)

oauth_endpoints("github")
myapp <- oauth_app("github", key = "4ecc22f67aea306cf018", secret = "f2e9c9b462f9271b8ccf819cd6d3ba7e591c2add")
github_token <- oauth2.0_token(oauth_endpoints("github"), myapp)
gtoken <- config(token = github_token)
gURL <- "https://api.github.com/users/jtleek/repos"
req <- GET(gURL, gtoken)
stop_for_status(req)
```

## 2.

```
The sqldf package allows for execution of SQL commands on R data frames. We will use the sqldf package to practice the queries we might send with the dbSendQuery command in RMySQL. Download the American Community Survey data and load it into an R object called
 acs


https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv

Which of the following commands will select only the data for the probability weights pwgtp1 with ages less than 50?

library(sqldf)
download.file("https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06pid.csv")
acs <- read.csv("acs.csv")
sqldf("select pwgtp1 from acs where AGEP < 50")
```

## 3.

```
Using the same data frame you created in the previous problem, what is the equivalent function to unique(acs$AGEP)

sqldf("select distinct AGEP from acs")
```

## 4.

```
How many characters are in the 10th, 20th, 30th and 100th lines of HTML from this page:

http://biostat.jhsph.edu/~jleek/contact.html

(Hint: the nchar() function in R may be helpful)

url <- "http://biostat.jhsph.edu/~jleek/contact.html"
> con <- url(url)
> htmlCode <- readLines(con)
> close(con)
> sapply(htmlCode[c(10, 20, 30, 100)], nchar)
<meta name="Distribution" content="Global" />               <script type="text/javascript">
                                           45                                            31
                                        })();                 \t\t\t\t<ul class="sidemenu">
                                            7                                            25
```

## 5.

```
Read this data set into R and report the sum of the numbers in the fourth of the nine columns.

https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for

Original source of the data: http://www.cpc.ncep.noaa.gov/data/indices/wksst8110.for

(Hint this is a fixed width file format)

url <- "https://d396qusza40orc.cloudfront.net/getdata%2Fwksst8110.for"
download.file(url, "wksst8110.csv", method = "curl")
dataFrame <- read.fwf("wksst8110.csv", widths = c(10,-5,4,4,-5,4,4,-5,4,4,-5,4,4) ,skip = 4)
sum(dataFrame[,4])
32426.7
```
