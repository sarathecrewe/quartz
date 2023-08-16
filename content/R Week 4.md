---
title: "R Week 4"
---

## Importing and Exporting Files

When searching for external objects to import, such as files, R's default behaviour is to search in your __working directory__. The __working directory__ is the location on your file system where R looks for files and where it saves output by default.

To determine your __working directory__ the __function__ `getwd()` can be used:

```{r getwd, exercise=TRUE}
getwd()
```

If you need to import a file that is not located inside your current __working directory__, you have two options: either change your __working directory__ to the location of the file or specify the full file path, such as C:/documents/rwork/data.csv.  Note that file paths should be specified using forward slashes `/` as opposed to back slashes `\`.

To change your `working directory` the __function__ `setwd()` can be used.

```{r setwd, eval=FALSE, echo=TRUE}
# Replace 'your/desired/directory' with the actual directory path
setwd("your/desired/directory")
```

When working with R Markdown (Rmd) files, you have the added advantage that your working directory is often set correctly by default. This means that when you open an Rmd file (when RStudio is closed), R will automatically consider the directory containing the Rmd file as your working directory.

## Reading Data

R includes a variety of built-in __functions__ for importing data stored in text files, like
`read.table()` and `read.csv()`. 
However, we will use functions from the __readr__ package, a __tidyverse__ package. These __functions__ are designed for improved performance, readability, and compatibility with modern data formats. 

Many of the functions defined in the __readr__ package share similar names with the base R counterparts. For instance, if you've worked with R's base functionality, you're likely familiar with the `read.csv()` function for reading comma-separated value files. In the __readr__ package, a parallel function with a nearly identical name, `read_csv()`, offers enhanced capabilities while maintaining a sense of familiarity.

## Tidy Data

Data imported into R might not be structured as an __analytic base table format__. Basic quality issues can be quickly fixed using `dplyr` functions e.g. 

| Issue | `dplyr` function |
| --- | --- |
| Fix column names | rename() |
| Remove columns | select() |
| Fix column type | mutate() + as.function |
| Remove rows | filter() |
| Replace missing values | mutate() + ifelse() |

For more advance restructuring functions from the package `tidyr` can be used; a `tidyverse` package. 

Some key functions in the package `tidyr` include

  - `separate()`: separates the values in one column into multiple columns
  - `pivot_wider()`: groups single observations scattered across multiple rows into one row by creating additional columns and
  - `pivot_longer()`: converts values spread across multiple columns into a name column that stores the original column names and a value column that stores the original values
  
To illustrate how these functions work we will use an example:
  
Assume we have a data set that records the absolute number of people diagnosed with tuberculosis (TB) per country per year. This data can be recorded in several ways. 

However, in general, to calculate the rate of TB cases per country per year i.e. the number of people per 10,000 diagnosed with TB, four operations have to be performed:

1. we have to extract the number of TB cases per country per year,
2. extract the population per country per year,
3. divide cases by population and
4. multiply the answer by 0.01

### Separate

The function `separate()` can be used to split the values in a column into multiple columns. When splitting a column into multiple columns we need to decide (i) how the split should occur and (ii) what to name the new columns. The argument `sep` of the function `separate()` can be used to specify the delimiter to split values at or the position to split values at

```{r tidy2, exercise=TRUE}

df <- tibble(country = c("Afghanistan", "Afghanistan", "Brazil", "Brazil", "China", "China"),
       year = c(1999, 2000, 1999, 2000, 1999, 2000),
       rate = c("745/19.9", "2666/20.6", "37737/172.0", "80488/174.5", "212258/1272.9", "213766/1280.4"))

head(df)

df %>% separate(col = "rate", # column to split 
                into = c("cases", "population"), # new column names 
                sep = "/") # delimiter

```

The function `separate()` by default splits a character column into multiple character columns. The default argument convert of the function `separate()` can be used to automatically convert the new columns into the correct data type

```{r tidy3, exercise=TRUE}

df <- tibble(country = c("Afghanistan", "Afghanistan", "Brazil", "Brazil", "China", "China"),
       year = c(1999, 2000, 1999, 2000, 1999, 2000),
       rate = c("745/19.9", "2666/20.6", "37737/172.0", "80488/174.5", "212258/1272.9", "213766/1280.4"))

head(df)

df %>% separate(col = "rate", # column to split 
                into = c("cases", "population"), # new column names 
                sep = "/", # delimiter
                convert = TRUE) %>% # convert columns to appropriate types automatically  
  str()
```

### pivot_wider

`pivot_wider()` makes a data set longer by increasing the number of rows and decreasing the number of columns

When we have a data set, where observations span multiple rows, the function `pivot_wider()` can be used to transform the data set to a `tidy` format. The function `pivot_wider()` increases the number of columns of a data set by decreasing the number of rows

```{r tidy4, exercise=TRUE}

df <- tibble(country = c("Afghanistan", "Afghanistan", "Afghanistan", "Afghanistan", "Brazil", "Brazil","Brazil", "Brazil", "China", "China","China", "China"),
       year = c(1999, 1999, 2000, 2000, 1999, 1999, 2000, 2000, 1999, 1999, 2000, 2000),
       type = rep(c("cases", "population"),6),
       value = c(745, 19.9, 2666, 20.6, 37737, 172, 80488, 174.5, 212258, 1272.9, 213766, 1280.4))

head(df)

df %>% pivot_wider(names_from = "type", 
                   values_from = "value")

```

The function `pivot_wider()` will assign data types to the new columns based on the values in the columns

### pivot_longer

`pivot_longer()` makes a data set longer by increasing the number of rows and decreasing the number of columns 
When we have a data set, where a variable is stored across a set of columns, the function `pivot_longer()` can be used to transform the data set to the `tidy` format. The function `pivot_longer()` increases the number of rows of a data set by decreasing the number of columns

```{r tidy5, exercise=TRUE}

df <- tibble(country = c("Afghanistan", "Brazil","China"),
             "1999" = c(745, 37737, 212258),
             "2000" = c(2666, 80488, 213766))
head(df)

df %>% pivot_longer(c(2,3))

```

The output produced by `pivot_longer()` has two main problems (i) the new column names are not descriptive and (ii) the values stored in the column name will be automatically stored as characters

When using the function `pivot_longer()` the arguments:

  - `names_to` can be used to assign a name to the column that contains the old column names,
  - `values_to` can be used to assign a name to the column that contains the values and
  - `names_transform` can be used to change the data type used for the name column 

```{r tidy6, exercise=TRUE}

df <- tibble(country = c("Afghanistan", "Brazil","China"),
             "1999" = c(745, 37737, 212258),
             "2000" = c(2666, 80488, 213766))
head(df)

df %>% pivot_longer(cols = c(2,3), 
                    names_to = "date", 
                    names_transform = list("date" = as.numeric), 
                    values_to = "cases")


```


## Joining Data From Multiple Sources

We have discussed various functions to manipulate and reshape a single data frame, but what happens if our data is in separate data frames? 

-   Bind functions: Bind functions can be used to add rows or columns to a data frame 
-   Join functions: Join functions can be used to merge two data frames based on specific conditions

![[Combine.png]]


## Adding Rows

Rows can be added to a data frame using the function `rbind()`. However, `rbind()` will coarse all values to a compatible data type

```{r rbind, exercise=TRUE}
tibble_A <- tibble("ID" = c(1:3), "Dummy" = letters[1:3])
tibble_B <- tibble("ID" = letters[1:3], "Dummy" = letters[4:6]) 
rbind(tibble_A, tibble_B)
```

Note from the example above that both columns are changed to type `character`. 

The package `dplyr` provides an alternative function to add rows to a data frame, namely `bind_rows()`. Unlike `rbind()` the function `bind_rows()` will show an error when the data type of columns are not the same

```{r rbind2, exercise=TRUE}
tibble_A <- tibble("ID" = c(1:3), "Dummy" = letters[1:3]) 
tibble_B <- tibble("ID" = letters[1:3], "Dummy" = letters[4:6]) 
bind_rows(tibble_A, tibble_B)
```

When rows are added to a data frame using `rbind()` both data frames must have the same columns. If the columns differ, R will show an error e.g.

```{r rbind3, exercise=TRUE}
rbind(tibble("ID" = 1, "Value" = 1), tibble("ID" = 1))
```

However, when the function `bind_rows()` is used to add rows to a data frame, the values of missing columns are filled with missing values i.e. `NA` 

```{r rbind4, exercise=TRUE}
bind_rows(tibble("ID" = 1, "Value" = 1), tibble("ID" = 1))
```

## Adding Columns

Columns can be added to a data frame using the function `cbind()`. However, the function `cbind()` does not check that duplicate column names are introduced

```{r cbind, exercise=TRUE}
cbind(tibble("ID" = 1, "Value" = 1), tibble("ID" = 1))
```

In the example above we end up with two columns with the same name. Columns with non-unique names are difficult to select. The function `cbind()` also does not allow a column or multiple columns to be added with a different number of elements e.g.

```{r cbind2, exercise=TRUE}
cbind(tibble("ID" = 1:2, "Value" = 1:2), tibble("ID" = 1:3))
```

However, a column that contains one value will be repeated e.g. 

```{r cbind3, exercise=TRUE}
cbind(tibble("ID" = 1:2, "Value" = 1:2), tibble("ID" = 1:3))
```

The package `dplyr` provides an alternative function to add columns to a data frame, namely `bind_cols()`. The function `bind_cols()` automatically corrects duplicate column names 

```{r cbind4, exercise=TRUE}
bind_cols(tibble("ID" = 1, "Value" = 1), tibble("ID" = 1))
```

but will also show an error when trying to add columns with a different number of elements. 

In general using the functions `bind_rows()` and `bind_cols()` are recommended. 

## Join

When data is stored in separate data sets we need a method to join or combine the data sets into one data set. Joining operations allows us to merge two data sets using conditions.

When deciding how data sets should be joined, we generally consider:

  - which rows to keep or remove,
  - the columns to keep or remove and
  - which matching conditions to use

![[Join.png]]


The `dplyr` packages contain various join functions. Perhaps the most commonly used join function is `left_join()`.

__Example 1__

Consider the data set A that consists of two features: an ID and feature X1. Data set B also consists of two features: an ID and feature X2. When we perform a `left_join()`, all the instances of data set A is kept, but we add information to data set A from data set B by matching on ID. More specifically the feature X2 is added. 

To illustrate how a left join can be performed we will use data sets from the `nycflights13` package. The nycflights13 package contains five data frames:

  - flights: contains information on all flights that departed from New York City in 2013
  - airlines: contains airline abbreviations
  - airports: contains airport metadata
  - planes: contains aeroplane metadata
  - weather: contains hourly weather data from JFK, LGA and EWR
  
We first, load the `nycflights13` data set and the data frame `flights`

```{r flight, exercise=TRUE}
library(nycflights13)
data(flights) # running this command will store the data frame in the objects flights
```

We can view the instances of the data frame `flights` by using the function `head()`

```{r flight1, exercise=TRUE}
head(flights)
```

Given the data, suppose we are interested in calculating which airline had the most flights to Seattle from New York. We start by filtering our data set to get all the flights to Seattle. Once we have collected all the data instances we perform a group by operation and count the number of instances per carrier. Finally, sorting the results helps us to quickly see which carrier had the most flights

```{r flight2, exercise=TRUE}
flights %>% 
  filter(dest == "SEA") %>% # get all flights to Seattle 
  select(carrier) %>% # select carrier code 
  group_by(carrier) %>% # group data by name 
  summarise(count = n()) %>% # provide the current group size 
  arrange(desc(count)) # sort on count
```

The above analysis work, but which aeroplane carrier is DL? The results only show the number of flights per carrier code; rather than the name of the carrier. Luckily we have collected an additional data set that contains the name of the carrier. 

```{r flight3, exercise=TRUE}
airlines %>%
  head()
```

The `airlines` data frame contains the full name of the carrier along with the carrier code. To add the name of the airline we perform a simple left join:

```{r flight4, exercise=TRUE}
flights %>% 
  filter(dest == "SEA") %>% # get all flights to Seattle 
  select(carrier) %>% # select carrier code 
  left_join(airlines) %>% # add airline names 
  group_by(name) %>% # group data by name 
  summarise(count = n()) %>% # provide the current group size 
  arrange(desc(count)) # sort on count
```

__Example 2__

Suppose we are inserted in determining whether there is a relationship between the age of a plane and delays. The departure and arrival delay times are available in the flight data set. However, the age of an aeroplane is only available in the planes data set

![[Relationship2.png|400]]

To add the age of aeroplanes a left join is performed using the column tailnum  

```{r flight5, exercise=TRUE}
flights %>% 
  select(dep_delay, tailnum) %>% 
  left_join(planes, by = c("tailnum")) %>% 
  mutate(age = 2013 - year) %>% 
  select(tailnum, dep_delay, age) %>% 
  group_by(age) %>% 
  summarise(delay = mean(dep_delay, na.rm = TRUE))
```
  
  
### Type of joins

The package `dplyr` contains several different join functions
  

| function | rows | columns |
| --- | --- | --- |
| `A %>% left_join(B)` | Keep all rows from A | Keep all columns |
| `A %>% right_join(B)` | Keep all rows from B | Keep all columns |
| `A %>% inner_join(B)` | Keep only rows from A that match with B | Keep all columns |
| `A %>% full_join(B)` | Keep all rows from A and B | Keep all columns |
| `A %>% semi_join(B)` | Keep rows from A that match B | Keep columns from A |
| `A %>% anti_join(B)` | Keep rows from A that do not match B | Keep columns from A |

![[TypeofJoins.png]]

We say rows match because they have some columns containing the same value. The matching conditions are specified using the `by` argument 

Matching behaviour

- When no argument is passed to the by argument, matching is performed on all columns of A and B with identical column names
- `by = c(col1, col2, col3)`, matching is performed on all identical values in col1, col2 and col3 of data set A and data set B
- `by = c(Acol1= Bcol1, Acol2 = Bcol2)`, matching is performed on values of Acol1 of data set A to the values of Bcol1 of data set B, and the values of Acol2 of data set A to the values of Bcol2 of data set B

If multiple matches are possible, a row for each possible combination will be returned, except with `semi_join()` and `anti_join()`

