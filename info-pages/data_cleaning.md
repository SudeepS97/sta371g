Data Cleaning
=============

Filtering data is essential to cleaning your data in preparation for
running statistical models. Subsetting data, adding/removing rows,
filtering/replacing data, and data transformation.

For example, let’s use the cars dataset from class (we can view the
first few rows of there data):

``` r
cars <- read.csv("https://raw.githubusercontent.com/brianlukoff/sta371g/master/data/cars.csv")
cars[0:5,] #only showing the first 5 rows
```

    ##   MPG Cylinders Displacement  HP Weight Acceleration After1975 Origin
    ## 1  18         8          307 130   3504         12.0        No     US
    ## 2  15         8          350 165   3693         11.5        No     US
    ## 3  18         8          318 150   3436         11.0        No     US
    ## 4  16         8          304 150   3433         12.0        No     US
    ## 5  17         8          302 140   3449         10.5        No     US

Subsetting Data
---------------

First, let’s try to subset our data in order to narrow it down to what
we want to run our analysis on. We can do this a few different ways:

1.  Subset the data by columns
2.  Subset the data by rows
3.  Subset the data based on conditions

We can subset data by columns by specifying which column numbers we want
to keep OR by specifying which ones we do not want. The preferable
choice is whichever one takes the least amount of time. If you have 100
columns and want 90 of them, its best to specify dropping 10 than
specify all 90 of the ones you want.

To specify by which columns we want to keep, we just select a collection
of column numbers.

Let’s try to keep only the MPG and Cylinder columns:

``` r
carsKeepCols <- cars[c(1,2)] #Specify which columns using a collection
# OR
carsKeepCols <- cars[,0:2] #specifying all columns from 0-2

carsKeepCols[0:5,]
```

    ##   MPG Cylinders
    ## 1  18         8
    ## 2  15         8
    ## 3  18         8
    ## 4  16         8
    ## 5  17         8

Notice how we only have the first two columns. Now let’s try to keep
every column EXCEPT for MPG and Cylinders:

``` r
carsDropCols <- cars[-c(1,2)]

#OR

carsDropCols <- cars[,-(0:2)] #note that you should put parenthesis around the range here so the '-' doesn't get mingled into it

carsDropCols[0:5,]
```

    ##   Displacement  HP Weight Acceleration After1975 Origin
    ## 1          307 130   3504         12.0        No     US
    ## 2          350 165   3693         11.5        No     US
    ## 3          318 150   3436         11.0        No     US
    ## 4          304 150   3433         12.0        No     US
    ## 5          302 140   3449         10.5        No     US

Now, let’s subset by rows. This will work the same way as columns.

Let’s try to get only the third row of data.

``` r
carsKeepRow <- cars[c(3),]

carsKeepRow
```

    ##   MPG Cylinders Displacement  HP Weight Acceleration After1975 Origin
    ## 3  18         8          318 150   3436           11        No     US

And now let’s try to get every row EXCEPT for row 2-4 (thus, removing
the row from the dataset).

``` r
carsDropRow <- cars[-c(2:4),]

carsDropRow[0:5,] #showing the first 5 rows left in the dataset. Notice how rows 2,3 and 4 no longer appear.
```

    ##   MPG Cylinders Displacement  HP Weight Acceleration After1975 Origin
    ## 1  18         8          307 130   3504         12.0        No     US
    ## 5  17         8          302 140   3449         10.5        No     US
    ## 6  15         8          429 198   4341         10.0        No     US
    ## 7  14         8          454 220   4354          9.0        No     US
    ## 8  14         8          440 215   4312          8.5        No     US

Lastly, let’s subset the data depending on a condition. Usually, you
won’t know exactly which rows you’ll need to delete, but know that you
want to delete rows that meet a certain criteria. For this, we will use
the `subset(data, condition)` function.

Let’s subset our data on a few different conditions:

``` r
carsMPG30 <- subset(cars, MPG >=30) #subset where MPG is greater than 30
carsWeight3500 <- subset(cars, Weight < 3500) #subset where weight is less than 3500

carsUS <- subset(cars, Origin == "US") # subset where origin is equal to US
```

We can also subset on multiple conditions at a time. Let’s combine all
these subsets into one.

``` r
carsSubset <- subset(cars, MPG>=30 & Weight < 3500 & Origin == 'US')
carsSubset[0:5,]
```

    ##      MPG Cylinders Displacement HP Weight Acceleration After1975 Origin
    ## 216 30.0         4          111 80   2155         14.8       Yes     US
    ## 236 30.5         4           98 63   2051         17.0       Yes     US
    ## 237 33.5         4           98 83   2075         15.9       Yes     US
    ## 244 36.1         4           98 66   1800         14.4       Yes     US
    ## 265 30.0         4           98 68   2155         16.5       Yes     US

Below is a list of a bunch of conditionals you can use:

-   `==`: is equal to
-   `>`: is greater than
-   `<`: is less than
-   `>=`: is greater than or equal to
-   `<=`: is less than or equal to
-   `!=`: is NOT equal to
-   `&`: AND
-   `|`: OR

Filtering & Replacing Data
--------------------------

Let’s see how we can filter out bad/missing data.

There are two ways we can get rid of NAs in our dataset:

``` r
carsFilter <- cars[complete.cases(cars),]
carsFilter <- na.omit(cars)
```

We can also substitute NA data with other data.

For example, If we don’t want to delete all an entire row just because
it has an NA for one column, say Cylinders, we can replace the NAs with
a 0, mean, or median. Note that this is sometimes a bad idea as it can
lead to misrepresentation of your data.

``` r
cars$Cylinders[is.na(cars$Cylinders)] <- 0 #sets Cylinder NA to 0

cars$Cylinders[is.na(cars$Cylinders)] <- mean(cars$Cylinders) #replace with the average value

cars$Cylinders[is.na(cars$Cylinders)] <- median(cars$Cylinders) #replace with the median value
```

Its important that your categorical columns are of type Factor, ant not
strings when running your regressions. You can cast a column of strings
to factors by using the `as.factor()` function.

THIS MUST BE DONE FOR CATEGORICAL VARIABLES!

``` r
stringToFactor = c('bird','bird','dog','cat','bird','dog')
typeof(stringToFactor)
```

    ## [1] "character"

``` r
stringToFactor = as.factor(stringToFactor)
typeof(stringToFactor) #the strings are now representated as integers after making them factors.
```

    ## [1] "integer"

The `levels()` function can also be used to create levels for your
categorical variable.

``` r
stringToFactor # There are 3 levels for this data
```

    ## [1] bird bird dog  cat  bird dog 
    ## Levels: bird cat dog

``` r
#adds a new level
levels(stringToFactor) <- c(levels(stringToFactor), "hamster")

stringToFactor # there are now 4 levels to this data
```

    ## [1] bird bird dog  cat  bird dog 
    ## Levels: bird cat dog hamster

``` r
#Now, you can change data to hamster or add data as hamster
stringToFactor[1] <- "hamster"

stringToFactor
```

    ## [1] hamster bird    dog     cat     bird    dog    
    ## Levels: bird cat dog hamster

You can also replace the variable type of a column in the case where you
have numbers but they are instead read as strings (or words).

``` r
stringNums <- c('1','2','3')
typeof(stringNums)
```

    ## [1] "character"

``` r
stringNums <- as.double(stringNums)
typeof(stringNums)
```

    ## [1] "double"

Having duplicate records can sometimes be an issue in our data. We can
remove these by using the `distinct(data)` function. This will remove
instances where all the column values for a given row matches that of
any other row (a duplicate).

``` r
library(dplyr) #run install.packages('dplyr') before using this
```

    ## Warning: package 'dplyr' was built under R version 3.6.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
distinctCars <- distinct(cars)
```

Transforming data
-----------------

There are a lot of ways to transform your data in order to do analysis.
In class we saw data transformation using mathematical functions such as
`log()` and `sqrt()`, but we can transform data in many other ways as
well.

To do this, we need to create a new column for our dataset, and set it
equal to our transformation.

``` r
cars$KPL <- cars$MPG * 0.425144 #adds a Km/Liter column now

cars$OriginAge <- paste(cars$Origin, cars$After1975) # combines the columns into one

cars$OriginFirst <- substr(cars$Origin, start=0, stop=1) #gets the first letter of the Origin
cars$OriginLast <- substr(cars$Origin, start=1, stop=2) #gets the last latter of the Origin

cars$IsUS <- ifelse(cars$Origin == 'US', 1, 0) #Sets instance to 1 if Origin is US and 0 otherwise
```

Combining Tables
----------------

If you happen to have tables that are in separate pieces, you can
combine them using `bind_cols()` and if you have multiple datasets with
the same columns and want to add all the instances together (stack the
tables) you can use `bind_rows()`.

``` r
cars1 = cars[,0:3]
cars2 = cars[,4:6]

dfCombine = bind_cols(cars1, cars2) #make sure that the rows align!!!

cars3 = cars[0:5,]
cars4 = cars[6:10,]

dfStack = bind_rows(cars3, cars4) #make sure columns are the same!!!
```

You can view more code pertaining to data cleaning here:
‘<a href="https://resources.rstudio.com/rstudio-developed/data-transformation" class="uri">https://resources.rstudio.com/rstudio-developed/data-transformation</a>’
