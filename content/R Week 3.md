---
title: "R Week 3"
---

## The pipe operator

You will frequently need to perform a series of steps to answer a data related question.

Such a series of steps can often be represented as a **nested function** i.e.

![[Pipe.png|300]]

-   we start by first calculating `h(data)`,
-   then proceed to calculate `g(h(data)),`
-   and finally calculate `f(g(h(data)))`.

When a nested function consists of multiple functions and arguments it can become difficult to both write and read the function.

Alternatively, a series of steps can be viewed as a flow chart of **functions**

![[Flow.png|470]]

-   the **function** `f( )` process some data and sends the output to the **function** `g( )`,
-   the **function** `g( )` then process the output received from the **function** `f( )` and send the output to the function `h( )` and
-   the **function** `h( )` then process the output received from the **function** `g( )` and produce the final answer

In this section we will discuss how we can write code in a flow chart style using the **pipe operator** `%>%`.

To illustrate how flow chart style code can be written, we will use the data set `mtcars`. To load the `mtcars` data set the **function** `data()` can be used

```{r Load_mtcars, exercise=TRUE}
data(mtcars) # loads the data frame into the global environment
```

The `mtcars` data set was extracted from the 1975 Motor Trend magazine, and comprises of the fuel consumption `mpg` and ten other accepts (features) of 32 automobiles (instances). We can view the first three **rows** of the data by using the **function** `head()`

```{r mtcars_head, exercise=TRUE}
head(mtcars, n = 3) # displays the first three rows
```

Using the data set `mtcars` we can calculate various statistics. For example, suppose that we want to determine the mean miles per gallon `mpg` of cars per their number of cylinders `cyl` for cars with more than one carburettors `carb`.

![[Example1.png]]

Our first option to calculate the statistic illustrated above is to break the calculation up into a series of steps, where we save the result of each step using an **object**.

```{r mtcars_example1, exercise=TRUE}
# 1. Remove cars with less than 2 carburettors 
a <- filter(mtcars, carb > 1) 
# 2. Group data
b <- group_by(a, cyl) 
# 3. calculate the mpg mean per group 
c <- summarise(b, mean(mpg)) 
# 4. sort the results by cyl
arrange(c, cyl) 
```

Breaking the calculation down into a series of steps makes it easy to read the code, however as a result unnecessary copies of the same **object** is created.

A second option to calculate the statistic of interest, is to use **nested functions**

```{r mtcars_example2, exercise=TRUE}
arrange(summarise(group_by(filter(mtcars, carb > 1), cyl), mean(mpg)), cyl)
```

Using **nested functions** avoid the use of explicitly creating **objects**, but can the readability of the code be improved?

We can express the functions as a flow chart using the **pipe operator** `>%>`.

```{r mtcars_example3, exercise=TRUE}
mtcars %>% filter(carb > 1) %>% group_by(cyl) %>% summarise(mean(mpg)) %>% arrange(cyl)
```

The **pipe operator** `>%>` avoids creating unnecessary **objects** and makes it easier to read code. The **pipe operator** `>%>` can be interpreted as a flow chart where we send `input` to a **function** that produces an **output**, the **output** is then be sent to the next **function**

![[mtflow.png]]

To insert the **pipe operator** `>%>` the shortcut `Ctrl + Shift + M` can be used. **Pipes** are clearer to read when each **function** is on a separate line

```{r mtcars_example4, exercise=TRUE}
mtcars %>% 
  filter(carb > 1) %>% 
  group_by(cyl) %>% 
  summarise(mean(mpg)) %>% 
  arrange(cyl)
```

The thing to the left of a **pipe operator** `%>%` is **passed** to the **first argument** of the **function** on the \_\_right \_\_of the **pipe operator** `%>%`, while additional **function arguments** can be specified as usual

If you ever need to pass an **object** to an **argument** other then the **first argument**, set the **argument** equal to `.`. For example, `y %>% f(x,.)` is equivalent to `f(x,y)` and `z %>% g(x, y, arg = .)` is equivalent to `g(x,y,arg = z)`

So far we have only illustrated how to print the results from **pipe operations**. We can also **assign** the **output** of a chain of **pipe operations** using the **assignment operator** `<-`

```{r mtcars_example5, exercise=TRUE}
result <- 
  mtcars %>% 
  filter(carb > 1) %>% 
  group_by(cyl) %>% 
  summarise(mean(mpg)) %>% 
  arrange(cyl)

result
```

**Assignment** can also be performed at the end of a chain using the **assignment operator** `->`

```{r mtcars_example6, exercise=TRUE}
mtcars %>% 
  filter(carb > 1) %>% 
  group_by(cyl) %>% 
  summarise(mean(mpg)) %>% 
  arrange(cyl) -> result

result
```

Lastly, note that the the **pipe operator** can only be used if the **package** `tidyverse` is loaded.

## dplyr

Given that our data is in a **data frame** we might want to **manipulate** the **data frame** in various ways, for example:

-   **add** or **remove row(s)** or **column(s)**,
-   **rearrange** the **row(s)** or **column(s)** or
-   **change** the **names** of **column(s)**.

All the above examples can be implemented using base R, but the base **functions** is not self-describing. Using base R code to modify **data frames** often lead to **nested functions** that are difficult to read.

The **package** `dplyr` was created for the sole purpose of simplifying the process of **manipulating**, **sorting**, **summarising** and **joining data frames**. The **functions** included in the `dplyr` **package** often leads to (i) more efficient code, (ii) code that is easier to read and (iii) code that is easier to write

The `dplyr` **package** forms part of the `tidyverse` **packages**. This means that if you have **installed** the `tidyverse` **package** previously you do not have to reinstall the `dplyr` **package**. Once **installed** the `dplyr` package can be **loaded** using the **function** `library()`

```{r dplyr1, exercise=TRUE}
library(dplyr)
```

The `dplyr` **package** includes various function to help you manipulate data frames. Some examples include:

| **Function**   | **Purpose**                                                                |
|-----------------------------|-------------------------------------------|
| **filter()**   | select **row(s)** to keep using conditions                                 |
| **distinct()** | select unique **rows(s)**                                                  |
| **slice**\_    | select, remove or duplicate **row(s)**                                     |
| **arrange()**  | order the **rows** of a data frame by the values of selected **column(s)** |
| **select()**   | select **column(s)** from a data frame to keep                             |
| **pull()**     | select a single **column** of a data frame as a **vector**                 |
| **rename()**   | assign new names to one or more **columns**                                |
| **relocate()** | change the order of **columns**                                            |
| **mutate()**   | creates new **columns(s)**                                                 |

## Tibbles

In this lesson we will work with **tibbles** instead of traditional R **data frames**. **Tibbles** are **data frames**, but are tweaked to make our life a bit easier:

-   **Tibbles** are lazy i.e. the names and data types of columns are not automatically changed
-   **Tibbles** are strict i.e. partial matching is not performed
-   **Tibbles** are self-descriptive
-   **Tibbles** print better than data frames
-   **Tibbles** do not use row names
-   The **column names** of \_\_tibbles \_\_can be more descriptive e.g. white spaces can be used in **column names**

The **function** `class(`) can be used to verify that an **object** is a **tibble**

```{r dplyr2, exercise=TRUE}
library(gapminder)
class(gapminder)
```

When **subsetting** is performed on a **data frame** the **result returned** is not always a **data frame**. When we are unsure of the **data type** of an **object**, it becomes very difficult to write code in a flow chart style. How do we now that the next operation in our flow diagram will accept the **data type** pass to it?

-   we will either have to keep track of the different **data types**,
-   write explicit **tests** or **conversions** to ensure the code execute without an error or
-   we can use **tibbles** .

The main advantage of **tibbles**, in my opinion, is that when we apply a dplyr **function** to a **tibble** the results are always returned as a **tibble**:

![[Tibble.png|470]]

## Row operations

### Filter

**Objective**: Select rows to keep using `filter()`

**Description**: Filtering is a common task used to identify or select row(s) of a data set where a particular variable matches a specific value/condition

**Function**

`filter(.data, ...)` or `data %>% filter(...)`

| **Argument** | **Description**                                                                                                                                                              |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                         |
| ...          | One or more expressions that returns logical vectors. When multiple expressions separated by commas are provided, the expressions are combined with the **and** `&` operator |

------------------------------------------------------------------------

*Example 1*

Select rows from the gapminder data set where the `country` is **equal** to `South Africa`

```{r filter1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% filter(___)
```

```{r filter1-solution}
gapminder %>% filter(country == "South Africa")
```

*Example 2*

Select rows from the gapminder data set where the country is equal to South Africa or Lesotho

```{r filter2, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% filter(country %in% ___)
```

```{r filter2-solution}
gapminder %>% filter(country %in% c("South Africa","Lesotho"))
```

*Example 3*

Select rows where the country is equal to South Africa and the year is greater than 2000

```{r filter3, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% filter(country == "South Africa", ___)
```

```{r filter3-solution}
gapminder %>% filter(country == "South Africa", year > 2000)
```

or alternatively

```{r filter4, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% filter(country == "South Africa" ___ year > 2000)
```

```{r filter4-solution}
gapminder %>% filter(country == "South Africa" & year > 2000)
```

Multiple logic rules can be applied in the `filter()` function

Rows can be filtered using the range of comparison operators, logical operators and functions to evaluate missing i.e. `NA` values

| Operator | Description              |
|----------|--------------------------|
| \<       | Less than                |
| \>       | Greater than             |
| ==       | Equal to                 |
| \<=      | Less than or equal to    |
| \>=      | Greater than or equal to |
| !=       | Not equal to             |
| %in%     | Group membership         |
| is.na    | is NA                    |
| !is.na   | is not NA                |
| &        | And                      |
| $|$      | Or                       |
| !        | Not                      |

### Distinct

**Objective**: Select unique row(s)

**Description**: Select unique row(s) based on a list of specified columns

**Function**

`distinct(.data, ...)` or `data %>% distinct(...)`

| **Argument** | **Description**                                                                                                                                                                                                                                      |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                                                                                                 |
| ...          | Column names to use to determine uniqueness of a row. When multiple rows for a given combination of columns exists, only the first rows are preserved. When column names are not specified all columns are used to determine the uniqueness of a row |
| .keep_all    | The default behaviour of the function is to drop all unspecified columns. If you want to get distinct rows by certain columns without dropping the other columns set the optional argument `.keep_all` to `TRUE`                                     |

------------------------------------------------------------------------

*Example 1*

Find all the unique years in the data set gapminder

```{r dist1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% distinct(___)
```

```{r dist1-solution}
gapminder %>% distinct(year)
```

Note that a **tibble** is returned although the result can be easily represented by an **atomic vector**

*Example 2*

Find all the unique years , but show all column

```{r dist2, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% distinct(___)
```

```{r dist2-solution}
gapminder %>% distinct(year, .keep_all = TRUE)
```

Only 12 rows are returned since the function `distinct()` only preserves the first distinct row.

### Slice

**Objective**: Select, remove or duplicate rows

**Description**: The `slice_` **functions** include various functions to select, remove or duplicate rows. To illustrate how the `slice_` **functions** work we will look at the function **slice_sample()** which can be used to randomly select rows

**Function**

`slice_sample(.data, ...)` or `data %>% slice_sample(...)`

| **Argument** | **Description**                                                                                                               |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                          |
| n, prop      | Provide either (i) `n` the number of rows to sample at random or (ii) `prop` to sample a proportion of rows from the data set |

------------------------------------------------------------------------

*Example 1*

Sample four random rows from the data set

```{r slice1, exercise=TRUE, exercise.blanks = "___+"}
set.seed(123) # makes random numbers repeatable 
gapminder %>% ___ # select four random rows

```

```{r slice1-solution}
set.seed(123) # makes random numbers repeatable 
gapminder %>% slice_sample(n = 4) # selects four random rows
```

### Arrange

**Objective**: Order the rows of a data frame by the values of selected column(s)

**Description**: Often used to view the rows of an observation in a specific order based on a the values in a particular column

**Function**

`arrange(.data, ...)` or `data %>% arrange(...)`

| **Argument** | **Description**                                                                                           |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                      |
| ...          | Names of columns to sort rows on separated by commas where the column names listed first takes precedence |

Values are sorted in ascending order by default. To sort in descending order using column `x` pass `desc(x)` to the `…` argument of the **function** `arrange()`

------------------------------------------------------------------------

*Example 1*

Sort the data frame gapminder by `continent` in **ascending** order and `country` in **descending** order

```{r arrange1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% arrange(___)

```

```{r arrange1-solution}
gapminder %>% arrange(continent, desc(country))
```

## Column operations

### Select

**Objective**: Select column(s) from a data frame to keep

**Description**: Columns to keep can be selected based on the (i) column names, (ii) using a column range or (ii) by using helper functions

**Function**

`select(.data, ...)` or `select %>% arrange(...)`

| **Argument** | **Description**                                                                                                                                                                                         |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                                                    |
| ...          | One or more unquoted column names, a range of column names to keep specified as x:y where x:y selects column x, column y and all the columns between the columns x and y, and/or using helper functions |

------------------------------------------------------------------------

*Example 1*

Select the columns country, year and population

```{r select1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  select(___)
```

```{r select1-solution}
gapminder %>% 
  select(country, year, pop)
```

alternatively we can specify the columns to remove using `-` signs

```{r select2, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  select(___)
```

```{r select2-solution}
gapminder %>% 
  select(-continent, -lifeExp, -gdpPercap)
```

*Example 2*

We can also use the `:` symbol to select a range of columns

```{r select3, exercise=TRUE}
gapminder %>% 
  select(country:year, pop)
```

where the value `country:year` will select the `country` column, the `year` column and all columns between the `country` and `year` columns

*Example 3*

Various **helper functions** such as `starts_with()`, `ends_with()` and `contains()` can also be used to select columns based on conditions

Try selecting all columns that starts with the character `c` or contains the phrase `gdp`

```{r select4, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  select(___)
```

```{r select4-solution}
gapminder %>% 
  select(starts_with("c"), contains("gdp"))
```

The help file of the **function** `select()` can be viewed to see thevarious helper functions available i.e. `?select()`

*Example 4*

To select `columns` based on column types, the helper function `where()` can be used. Try selecting all the numeric columns

```{r select5, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  select(where(___))
```

```{r select5-solution}
gapminder %>% 
  select(where(is.numeric))
```

The **function** passed to the `argument` of `where()` should return a single `TRUE` or `FALSE`. If you are creative you can for instance remove all columns with more than 50% missing values

```{r select6, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  select(where(function(x) sum(is.na(x)) / length(x) < 0.5)) 
```

### Pull

**Objective**: Select a single column of a data frame as a vector

**Description**: Often in specific steps of an analysis a vector is required rather than a data frame. The **function** `pull(`) allows you to select a single column or value from a data frame as a vector

**Function**

`pull(.data, ...)` or `select %>% pull(...)`

| **Argument** | **Description**                                                                                                                                                                   |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                              |
| ...          | A column specified as (i) the column name or (ii) a positive integer, giving the position counting from left or (iii) a negative integer, giving the positing counting from right |

------------------------------------------------------------------------

*Example 1*

```{r pull1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  pull(year)
```

You can verify that the above returns a vector by running

```{r pull2, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  pull(year) %>% 
  class()
```

### Rename

**Objective**: Assign new names to one or more columns

**Description**: Replace names of columns with descriptive names. Descriptive names improve readability and reduces the amount of formatting required e.g. when creating a plot

**Function**

`rename(.data, ...)` or `rename` %\>% pull(...)\`

| **Argument** | **Description**                                                                                                                                                          |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                     |
| ...          | Use `new_name = old_name` to rename a column. To rename multiple columns separate the assignments using commas i.e. `new_name_1 = old_name_1`, `new_name_2 = old_name_2` |

------------------------------------------------------------------------

*Example 1*

Rename the columns `lifeExp` to `Life Expectancy` and `pop` to `Population`

```{r rename1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  rename(___ = ___, ___ = ___) 
```

```{r rename1-solution}
gapminder %>% 
  rename("Life Expectancy" = lifeExp, 
         Population = pop) 
```

In a **tibble**, unlike in a **data frame**, we can use spaces in column names given that the name is surrounded by double quotations `“` or backticks \`

### Relocate

**Objective**: Change the order of columns

**Description**: Change the position of the columns of a data frame

**Function**

`relocate(.data, ...)` or `relocate` %\>% pull(...)\`

| **Argument** | **Description**                                                                                                |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                           |
| ...          | columns names separated by commas that will be moved to the front of the object passed to the `.data` argument |

The default arguments `.before` or `.after` of the **function** `relocate()` can be used to move a column(s) before or after a specific column. Only one of the default arguments can be used at a time

------------------------------------------------------------------------

*Example 1*

Move the column `pop` before the column `lifeExp`

```{r relocate1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  relocate(___, ___) 
```

```{r relocate1-solution}
gapminder %>% 
  relocate(pop, .before = lifeExp)
```

### Mutate

**Objective**: Create new column(s)

**Description**: Used to add new a column to a data set while preserving the old columns of the data set. The new variable can be derived from the values of another column(s) in the data set

**Function**

`mutate(.data, ...)` or `mutate` %\>% pull(...)\`

| **Argument** | **Description**                                                                                                                                                                                                                                                                                 |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                                                                                                                                            |
| ...          | New column(s) specified as `name = value` and separated with a comma where the `value` can be: (i) a vector of length 1 which will be recycled, (ii) a vector of the same length as the number of rows, (iii) NULL to remove a column or (iv) a data frame or tibble to create multiple columns |

------------------------------------------------------------------------

*Example 1*

Create a new column `pop_in_million` which divided the values in the `pop` column by 1 000 000

```{r mutate1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  mutate(pop_in_million = ___)

```

```{r mutate1-solution}
gapminder %>% 
  mutate(pop_in_million = pop/1000000)
```

Additional **columns** can be added in a single `mutate()` call, by separating expressions with commas

*Example 2*

The `ifelse()` function can be used to assign values to a column based on a condition. Use the function **mutate** to change the country `Swaziland` to `Estwatini`

```{r mutate2, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  mutate(country == ifelse(___))

```

```{r mutate2-solution}
gapminder %>% 
  mutate(country = ifelse(country == "Swaziland",
                                      "Eswatini",
                                      as.character(country))) 
```

The **function** `mutate()` will replace the values in an existing column, when an existing column name is provided

*Example 3*

The **function** `case_when()` can be used to perform multiple `ifelse()` operations]

```{r mutate3, exercise=TRUE}
gapminder %>% 
  mutate(gdp = case_when(gdpPercap < 700 ~ "Low", 
                         gdpPercap < 800 ~ "Moderate", 
                         TRUE ~ "High"))
```

## Summarise data

### Summarise

**Objective**: Calculate the summary statistic of specific columns

**Description**: The `summarise()` **function** takes column(s) and computes something (any calculation that can aggregate multiple values into a single value) using the values of every row

**Function**

`summarise(.data, ...)` or `summarise` %\>% pull(...)\`

| **Argument** | **Description**                                                                                                                                                                                                                                                                                    |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                                                                                                                                                                                                                               |
| ...          | Summary statistic to calculate specified as `name = function()` where the `name` is optional and the `function()` can be any summary function like `min()`, `mean()` and `max()`. Multiple statistics can be calculated by separating summary statistics using commas or by using helper functions |

------------------------------------------------------------------------

*Example 1*

For South Africa, calculate the number of observations, max population, mean life expectancy and the range of life expectancy over the years

```{r sum1, exercise=TRUE, exercise.blanks = "___+", exercise.lines = 8}
gapminder %>% 
filter(country == "South Africa") %>% 
  summarise(count = n(),  # dplyr count function 
            max_pop = ___, 
            ___,
            range_life_exp = ___ - ___)

```

```{r sum1-solution}
gapminder %>% 
  filter(country == "South Africa") %>% 
  summarise(count = n(),  # dplyr count function 
            max_pop = max(pop), 
            mean(lifeExp),  # no name specified
            range_life_exp = max(lifeExp) - min(lifeExp))
```

The new columns are calculated using all the rows of the gapminder data set filtered for the country South Africa

### Across

**Objective**: Avoid repetitive code due to applying the same function to multiple columns

**Description**: The `across()` function is a **helper function** that can be used to apply one or more functions to one or more columns

**Function**

`across(.cols, .fns)`

| **Argument** | **Description**                                                                                                           |
|-----------------------------|-------------------------------------------|
| .cols        | A column name or a vector of column names e.g. c(col1, col2)                                                              |
| .fns         | A **reference** to a function e.g. mean or a list of references to functions e.g. list(mean = mean, max = max, min = min) |

Note that a **reference** to a **function** must be provided i.e. a **function** without **parenthesis**

------------------------------------------------------------------------

*Example 1*

Calculate the mean, maximum and minimum value of the column `lifeExp` and the column `pop`

```{r sum2, exercise=TRUE, exercise.blanks = "___+", exercise.lines = 8}
gapminder %>% 
  filter(country == "South Africa") %>% 
  summarise(across(___, 
                   list(___)))
```

```{r sum2-solution}
gapminder %>% 
  filter(country == "South Africa") %>% 
  summarise(across(c(lifeExp, pop), 
                   list(mean = mean, 
                        max = max, 
                        min = min))) 
```

Names are automatically created based on the column and list names

------------------------------------------------------------------------

The **function** `across(`)\` can be used in additional ways to avoid repetition

-   `across(everything())` will summarise / mutate all columns. For example, to calculate the mean and standard deviation of all the columns for the data set data:

```{r sum3, exercise=TRUE}
gapminder %>% 
summarise(across(everything(), list(mean = mean, sd = sd)))
```

-   `across(where())` will summarise / mutate all columns that satisfy some logical condition. For example, to calculate the mean and standard deviation for all numeric columns of the data set data

```{r sum4, exercise=TRUE}
gapminder %>% 
summarise(across(where(is.numeric), list(mean = mean, sd = sd))) 
```

Other helper functions previously discussed can also be used inside the **function** across() e.g. `starts_with()`, `ends_with()` and `contains()`

## Group statistics

-   To gain better insights, we can calculate statistics by a group
-   For example, suppose we have a data set composed of a key and data column. Then we can compute the mean of the values in the data column for each group or unique value present in the key column
-   The operations of calculating group statistics can be thought of as a split, apply and combine operation

![[split.png]]

In the `dplyr` **package**, we will use the `group_by()` **function** to implement the split operation, while the `summarise()` **function** can be used to perform the apply and combine operations

### Group by

**Objective**: Group rows

**Description**: Takes an existing data object and transform it into a grouped data object. Once a data set is grouped, operations can be performed "by group"

**Function**

`group_by(.data, ...)` or `data %>% group_by(...)`

| **Argument** | **Description**                                                                       |
|-----------------------------|-------------------------------------------|
| .data        | data frame or tibble                                                                  |
| ...          | A column name to group the data frame by or multiple column names separated by commas |

-   Also read up on the `ungroup()` **function**

------------------------------------------------------------------------

*Example 1*

Calculate the mean `lifeExp` per `country`

```{r group1, exercise=TRUE, exercise.blanks = "___+"}
gapminder %>% 
  ___ %>% 
  summarise(lifeExp_mean = mean(lifeExp)) 
```

```{r group1-solution}
gapminder %>% 
group_by(country) %>% 
summarise(lifeExp_mean = mean(lifeExp))  
```

If we did not use the `group_by()` **function** in the example above, the operation would have computed the mean life expectancy using all the rows of the gapminder data set

### Window

We have previously seen how we can compute some statistics of South Africa using the `summarise()` **function** i.e. calculating the minimum, mean and maximum life expectancy

```{r window1, exercise=TRUE}
gapminder %>% 
  filter(country == "South Africa") %>% 
  select(lifeExp) %>% 
  summarise(min_lifeExp = min(lifeExp), 
            mean_lifeExp = mean(lifeExp), 
            max_lifeExp = max(lifeExp))
```

However, we do not have any way to directly calculate (i) how South Africa compares against other countries and (ii) whether the life expectancy of South Africa has improved or not

**Window functions** allow us to compare rows to each other. We will start by looking at the two **offset** **functions** known as `lag()` and `lead()`. The **function** `lag()` retrieves the previous element of a **vector**, while the function `lead()` retrieves the next element of a **vector**

```{r lag1, exercise=TRUE}
x <- 1:5 
lag(x)
```

In the example above, we use the **function** `lag()` to retrieve the previous element of the **vector** `x`. Since the first element of the **vector** `x` does not have a previous element the **function** `lag()` returns `NA`

Accessing the previous and next \_\_element \_\_of a **vector** can help us to calculate **trends** or create new **variables**

------------------------------------------------------------------------

*Example 1*

Calculate the change in life expectancy per `country` per year

```{r lag2, exercise=TRUE}
gapminder %>% 
  filter(year > 2000) %>% 
  arrange(country, year) %>% 
  mutate(change_lifeExp = lifeExp - lag(lifeExp)) %>%
  select(country, year, lifeExp, change_lifeExp)
```

At first our code seems to work, but what happens when the `country` changes? To prevent the problem we can use `group_by()`

*Example 2*

Calculate the change in life expectancy per `country`

```{r lag3, exercise=TRUE}
gapminder %>% 
  filter(year > 2000) %>% 
  arrange(country, year) %>% 
  group_by(country) %>% 
  mutate(change_lifeExp = lifeExp - lag(lifeExp)) %>% 
  select(country, year, lifeExp, change_lifeExp)
```

### Ranking

**Window functions** allow us to compare **rows** to each other. **Ranking functions** takes a **vector** to order, and returns various types of **ranks**. The **function** `row_number()` assigns **ranks** to values in a **vector** based on the minimum value, where ties are resolved by **assigning** the lowest **rank** to the value that appears first

```{r rank1, exercise=TRUE}
x <- c(10, 2, 10, 6, 4) 
row_number(x)
```

The **function** `min_rank()` works exactly the same as the **function** `row_number()` except that ties are resolved by assigning the same rank to equal values

```{r rank2, exercise=TRUE}
x <- c(10, 2, 10, 6, 4) 
min_rank(x)
```

------------------------------------------------------------------------

*Example 1*

Rank the `countries` by `lifeExp` in the `year` 2007

```{r rank3, exercise=TRUE}
gapminder %>% 
  select(country, year, lifeExp) %>% 
  filter(year == 2007) %>% 
  mutate(rank = row_number(lifeExp)) %>% 
  arrange(rank) 
```
