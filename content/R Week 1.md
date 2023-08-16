---
title: "R Week 1"
---

## R Data Structures

When we perform data analysis in general we will typically use data in the form of an **analytic base table**. In an **analytic base table**, the **rows** of the table represent different **observations** and the different **variables** reported for each observation defines the **columns**

![[ABT.png]]

Up to now, we have discussed how we can store one value in an **object**. For example:

```{r t1_1, exercise = TRUE, exercise.eval = FALSE}
length <- 5 
width <- 10 
area <- length * width 
area
```

but, storing one value in an **object** will only get us that far. We require methods to **import** data from different sources in R,  to **store** the data in R and to **manipulate** the data stored in R. 

Tutorial 1 will focus on the different **data types** available in R.

Working with data in R involves selecting a **data structure** to hold your data and (ii) entering or importing the data into the **data structure** identified. R has a wide variety of **objects** available for holding data including **atomic vectors**, **matrices**, **arrays**, **lists** and **data frames**. When an **object** is created with a single value, R will create an **atomic vector** since R does not include a **scalar** data type.

![[DataTypes.png]]

## Data frame

The **analytic base table** equivalent structure in R is known as a **data frame**

For now, we will avoid the technical details of a **data frame** and rather focus on the high-level concepts. R includes several built-in data sets, some of the data sets are stored as **data frames**. To load an example data set the **function** data() can be used.   

```{r t1_2, exercise = TRUE, exercise.eval = FALSE}
data(mtcars) # loads the data frame into the global environment
```

We can view the first 6 observations of the **data frame** *mtcars* using the **function** `head()` or the last 6 observations of the **data frame** *mtcars* using the **function** `tail()`. Try modifying the code below to only display the first four observations of the **dataframe** `mtcars`

```{r t1_3, exercise = TRUE, exercise.eval = FALSE}
head(x = mtcars, n = 6) # displays the first six observations
```

```{r t1_3-hint}
head(x = mtcars, n = 4)
```

The **function** `str()` can be used to view the **str**ucture of an **object** 

```{r t1_4, exercise = TRUE, exercise.eval = FALSE}
str(mtcars) 
```

Once we have a **data frame** it becomes easy to calculate summary statistics, create plots or even analytical models. 

**Summary statistics**: The function `summary()` can be used to display the summary statistics of a **data frame**

```{r t1_5, exercise = TRUE, exercise.eval = FALSE}
summary(mtcars) 
```

**Plots**: Given a **data frame** various plots can be created, for example a histogram of the variable miles per gallon (mpg) 

```{r t1_6, exercise = TRUE, exercise.eval = FALSE}
hist(mtcars$mpg)
```

**Analytical model**: Create a linear regression model using the transmission (variable am) as input and miles per gallon (mpg) as output

```{r t1_7, exercise = TRUE, exercise.eval = FALSE}
lm(mpg~am,data=mtcars)
```

## Atomic vectors

### Introduction to vectors

The simplest and most common data structure in R is called a **vector**. There are two types of **vectors** in R: **atomic vectors** and **lists**. The main difference between **atomic vectors** and **lists** is that **atomic vectors** can only store data of the same type, while **lists** can be used to store data of different types. For example, an atomic vector can be used to store the grades achieved by students in this course

![[AVectorExample.png|470]]

Various **functions** can be used to create **vectors** in R. The most straightforward way to create a **vector** in R is by using the **function** `c()` which stands for **combine** or **concatenate**. For example, a **vector** with four elements can be created and assigned to the object `first_vector`

```{r t2_1, exercise = TRUE, exercise.eval = FALSE}
first_vector <- c(1,  3,  7, -0.5) # create a vector with four elements 
first_vector
```

Try creating a **vector** that contains the values 1,2,3,4:

```{r t2_2, exercise = TRUE, exercise.eval = FALSE}
 
```

```{r t2_2-hint}
c(1,2,3,4)
```

The **function** `c()` can also be used to combine **vectors** to form a new vector

```{r t2_3, exercise = TRUE, exercise.eval = FALSE}
c(c(1,  2,  3),  c(4,  5,  6)) # combine the vector c(1,2,3) and c(4,5,6)
```

Given a **vector**, it is possible to create a new **vector** where the new **vector** contains repetitions of the elements of the original **vector**. To repeat specific **elements** of a **vector** the function `rep()` which stands for **repetition** can be used

```{r t2_4, exercise = TRUE, exercise.eval = FALSE}
rep(c(1,  2),  times = 3) # repeat the vector c(1,2) three times
```

The above example, repeats the **vector** `c(1,2)` three times. Elements of a vector can also be repeated using the *`each`* **argument** of the **function** `rep()` 

Modify the code below to produce the **vector** c(1,2,1,2,1,2) using the *`each`* **argument** 

```{r t2_5, exercise = TRUE, exercise.eval = FALSE}
rep(c(1,  2)) 
```

```{r t2_5-hint}
rep(c(1,  2),  each = 3) 
```

Values can also be passed to both the `each` and `times` **arguments** of the `rep()` **function**. When values are passed to both the `each` and `times` arguments, the `each` operation is performed first. 

Create the vector `c(1,1,1,2,2,2,1,1,1,2,2,2)` by filling in the blanks:

```{r t2_6, exercise = TRUE, exercise.eval = FALSE,  exercise.blanks = "___"}
rep(c(1,2), times = ____, each = ____) 
```

```{r t2_6-solution}
rep(c(1,2), times = 2, each = 3) 
```

The function `head()` and `tail()` can be used to obtain a preview of the values stored in a **vector**

R by default includes the **character vectors** `letters` and `LETTERS`. The **vector** `letters` contain the 26 lower-case letters of the Roman alphabet, while the **vector** `LETTERS` contain the 26 upper-case letters of the Roman alphabet

```{r t2_7, exercise = TRUE, exercise.eval = FALSE}
letter
```

The **functions** `head()` and `tail()` can be used to preview the values stored in a **vector**

```{r t2_8, exercise = TRUE, exercise.eval = FALSE}
head(letters)
```

```{r t2_9, exercise = TRUE, exercise.eval = FALSE}
tail(letters)
```

Display the last four elements of the **vector** `letters` by filling in the blanks:

```{r t2_10, exercise = TRUE, exercise.eval = FALSE,  exercise.blanks = "___"}
tail(letters, n = ___)
```

```{r t2_10-solution}
tail(letters, n = 4)
```

## Type of atomic vectors

There are six types of **atomic vectors** in R: **logical**, **integer**, **double**, **character**, **complex** and **raw**. **Integer** and **double** **vectors** are collectively known as **numeric vectors**. We will only focus on **logical**, **integer**, **double** and **character** vectors in this course also referred to as the **primary** type of **atomic vectors**

![[TypesofVectors.png|400]]

Each **atomic vector** in R uses a special syntax to define the  **elements** of the  **vector**

**Logical vectors**: can only contain the values (i) `TRUE`  or `T` and (ii)  `FALSE` or `F`

```{r t2_11, exercise = TRUE, exercise.eval = FALSE}
logical_vector <- c(T, F, TRUE, FALSE,) # Note TRUE can be abbreviated as T
```

**Character vectors** contain **elements** of type string. **Strings** are values surrounded single quotation marks `‘’` or double quotation marks `““`

```{r t2_12, exercise = TRUE, exercise.eval = FALSE}
character_vector <- c("Andrew",  'Mike',  'John',  'Sara')
```

**Double vectors** can be specified in decimal, scientific or hexadecimal form. **Double vectors** can contain three special values: `Inf` (infinity), `-Inf` (negative infinity) and `NaN` (not a number)

```{r t2_13, exercise = TRUE, exercise.eval = FALSE}
double_vector <- c(1.2,  1.2e3,  0xcafe,  Inf,  NaN)
```

**Integer vectors** are defined similarly to **double vectors**, but the elements must be followed by `L` and cannot contain fractions 

```{r t2_14, exercise = TRUE, exercise.eval = FALSE}
int_vector <- c(1L,  1.2e3L,  0xcafeL)
```

**Double vectors** and **integer vectors** are both **numeric vectors**

### Properties of vectors

Each **vector** has three properties: (1) a **type** (2) a **length** and (3) **attributes**: 

**Type**: The **type** of a **vector**, how the **object** is internally stored, can be checked using the `typeof()` function. The `typeof()` *function* determines the R internal type or storage mode of any R object 

```{r t2_15, exercise = TRUE, exercise.eval = FALSE}
typeof(letters) # letters is a builtin character vector
```

**Length**: The number of **elements** stored in a **vector** can be determined with the **function** `length()`

```{r t2_16, exercise = TRUE, exercise.eval = FALSE}
length(letters)
```

**Attributes**: An **attribute** is a piece of information that can be attached to an **atomic vector** or any R **object**. You can think of **attributes** as `metadata` - a convenient place to store information associated with an **object**. By default an **atomic vector** does not have any **attributes** assigned to it. To display the **attributes** of an **object** the `attribute()` **function** can be used. 

```{r t2_17, exercise = TRUE, exercise.eval = FALSE}
my_vector <- 1:10
attributes(my_vector)
```

The **object** `my_vector` does not have any **attributes** assigned to it. However, this does not mean that **attributes** cannot be assigned to an **object**. The most common attributes to give an **atomic vector** are **names**, **dimensions** and **classes**. We will only discuss **names** at this point

By default an **atomic vector** will not have a **names** **attribute** assigned to it. To check whether the **names attribute** is assigned a **vector** the **function** `names()` can be used

```{r t2_18, exercise = TRUE, exercise.eval = FALSE}
weekly_rainfall = c(10,  12,  0,  4,  0) 
names(weekly_rainfall)
```

**Names** can be assigned to a **vector** either when a **vector** is created or using the **function** `names()`

```{r t2_19, exercise = TRUE, exercise.eval = FALSE}
weekly_rainfall = c("Mo" = 10, "Tu" = 12, "We" = 0, "Th" = 4, "Fr" = 0) 
names(weekly_rainfall) <- c("Mo", "Tu", "We", "Th", "Fr")
attributes(weekly_rainfall)
```

**Names** will not affect the actual values of the **vector**, nor will the **names** be affected when the **elements** of the **vector** are manipulated

When you attempt to create a **vector** with different types, R will “convert” the **elements** to a compatible type of vector 

Recall that an **atomic vectors** can only contain **elements** of the same type. If you try to create a **vector** with different elements, R will automatically **coarse** the values to a compatible type in the order:  `logical » integer » double » character`

```{r t2_20, echo=FALSE}
quiz(question("R stores the vector c(TRUE,  1L) as a _ vector",
    answer("logical"),
    answer("integer", correct = TRUE),
    answer("double"),
    answer("character"),
    allow_retry = TRUE))
```    

```{r t2_21, echo=FALSE}
quiz(question("R stores the vector c(TRUE,  1)) as a _ vector",
    answer("logical"),
    answer("integer"),
    answer("double", correct = TRUE),
    answer("character"),
    allow_retry = TRUE))
```  

```{r t2_22, echo=FALSE}
quiz(question("R stores the vector typeof(c('a',  1))) as a _ vector",
    answer("logical"),
    answer("integer"),
    answer("double"),
    answer("character", correct = TRUE),
    allow_retry = TRUE))
```

### Logical vectors

**Logical vectors** can contain the values `TRUE`, `FALSE` and `NA` (for “not” available). Logical vectors are typically a product of performing a logical test, for example: 

```{r t2_l1, exercise = TRUE, exercise.eval = FALSE}
c(1, 2, 3) == 1
```

The example above evaluates whether each element in the vector `c(1, 2, 3)` is equal to 1 using the comparison operator `==`. Recall that the `=` operator is reserved for assignment, instead `==` is used to determine equality 

R includes all the standard comparison operators : `>` , `>=` , `<` , `<=` , `!=` (not equal) and `==` (equal) 

```{r t2_l2, exercise = TRUE, exercise.eval = FALSE}
c(1, 2, 3) == 1
```

To test if two objects are exactly equal the function `identical()` can be used

```{r t2_l3, exercise = TRUE, exercise.eval = FALSE}
v1 <- c(4, 4, 9, 12) 
v2 <- c(4, 4, 9, 13) 
identical(v1, v2)
```

```{r t2_l4, exercise = TRUE, exercise.eval = FALSE}
v1 <- c(4, 4, 9, 12) 
v2 <- c(4, 4, 9, 12) 
identical(v1, v2)
```

Sometimes you wish to test for “nearly equal”. The function `all.equal()` test for equality with a tolerance difference of 1.5e-8

```{r t2_l5, exercise = TRUE, exercise.eval = FALSE}
v1 <- c(4.00000005, 4.00000008) 
v2 <- c(4.00000002, 4.00000006) 
all.equal(v1, v2)
```

If the difference is greater than the tolerance level, the mean relative difference is returned

```{r t2_l6, exercise = TRUE, exercise.eval = FALSE}
v1 <- c(4.0005, 4.0008) 
v2 <- c(4.0002, 4.0006) 
all.equal(v1, v2)
```

To evaluate more than one logical expressions the AND `&` operator or the OR `|` operator can be used. For the AND `&` operator both conditions must be `TRUE` to be `TRUE`

```{r t2_l7, exercise = TRUE, exercise.eval = FALSE}
(3 > 5) & (4 == 4)
```

For the OR `|` operator at least one condition must be `TRUE` to be `TRUE`

```{r t2_l8, exercise = TRUE, exercise.eval = FALSE}
(3 > 5) | (4 == 4)
```

Lastly a condition can be switched using the NOT `!` operator

```{r t2_l9, exercise = TRUE, exercise.eval = FALSE}
!(3 > 5)
```

Consider the following code
```{r t2_l20, exercise = TRUE, exercise.eval = FALSE}
result <- ((111 >= 111) | !(TRUE)) & ((4 + 1) == 5) # result = TRUE
```

To understand the output of the code, run the different parts in the block below. Why is the final output return TRUE?

```{r t2_l21, exercise = TRUE, exercise.eval = FALSE}

```

The function `%in%` avoids the use of using the OR `|` operator excessively  

```{r t2_l22, exercise = TRUE, exercise.eval = FALSE}
c(1, 2, 3, 4) %in% c(1, 2)
```

The function `which()` can be used to return the indices of elements that evaluate to `TRUE`

```{r t2_l23, exercise = TRUE, exercise.eval = FALSE}
which(c(1, 2, 3, 4) %in% c(1, 2))
```

Math operations can also be performed with logical vectors, since `TRUE = 1` and `FALSE = 0`

```{r t2_l24, exercise = TRUE, exercise.eval = FALSE}
(c(1,0,1) == 1) + 1
```

Typically use cases includes determining the proportion of elements that are **TRUE** of a **logical vector** 

```{r t2_l25, exercise = TRUE, exercise.eval = FALSE}
mean(c(1, 2, 3) == 1)
```

### Character vectors

**Character vectors** stores data as strings (“text”) and is typically used to store information such as names, addresses and IDs as: 

```{r t2_c1, exercise = TRUE, exercise.eval = FALSE}
first_names <- c("Andrew", "Beth", "Carly", "Dan")
```

String operators can be performed on strings to determine useful properties, such as the length of each string:

```{r t2_c2, exercise = TRUE, exercise.eval = FALSE}
nchar(first_names)
```

Note that R uses a global string pool. This means that a unique string is only stored in memory once, reducing the amount of memory required to store duplicate strings

### Numeric vectors

**Numeric Vectors**, rather then using the function `c()`, can also be created with the function `seq()` which stands for sequence. The function `seq()` creates a vector which starts at the value passed to the from argument, in increments of 1, up to the value passed to the to argument. For example, a numeric vector can be created starting from 3 and ending at 10: 

```{r t2_n1, exercise = TRUE, exercise.eval = FALSE}
seq(from = 3,  to = 10)
```

The `seq()` function can also be written shorthand using the `:` operator

```{r t2_n2, exercise = TRUE, exercise.eval = FALSE}
3:10
```

The `seq()` function will always try to create an integer vector first, if not possible a double vector will be created

To create a vector using `seq()` with increments other then 1, a value can be passed to the optional by argument of the function. For example, a vector can be created which starts at 10 up to 0 in increments of -2

```{r t2_n3, exercise = TRUE, exercise.eval = FALSE}
seq(from = 10, to = 0.2,  by = -2)
```

In some cases you want to generate a numeric vector with a specific number of elements between two numbers. To generate a vector with a specific number of elements a value can be passed to the length.out argument of the `seq()` function. For example, a vector of length 10 can be generated as follows:

```{r t2_n4, exercise = TRUE, exercise.eval = FALSE}
seq(from = 3,  to = 10,  length.out = 10)
```

When performing arithmetic operations on numeric vectors, R perform element-wise operations by default 

```{r t2_n5, exercise = TRUE, exercise.eval = FALSE}
c(1, 2, 3) + c(4, 5, 6) # c(1 + 4, 2 + 5, 3 + 6)
c(1, 2, 3, 4)^2 # to the power of 2
```

When vectors of different lengths are used, R will recycle the shorter vector by repeating the vector to match the longer vector

```{r t2_n6, exercise = TRUE, exercise.eval = FALSE}
c(1,  1,  1,  1) * c(1,  2) # c(1*1, 1*2, 1*1, 1*2)
```

When the longer vector is not a multiple of the shorter vectors, R will still perform **recycling**, however a warning will be shown 

```{r t2_n7, exercise = TRUE, exercise.eval = FALSE}
c(1,  1,  1,  1) * c(1,  2,  3) # c(1*1, 1*2, 1*3, 1*1)
```

Some functions perform operations on an entire vector as oppose to working **element-wise**

```{r t2_n8, exercise = TRUE, exercise.eval = FALSE}
sum(c(1,2,3,4))
```

```{r t2_n9, exercise = TRUE, exercise.eval = FALSE}
max(c(1,2,3,4))
```

Some other useful functions includes: `min()` , `median()`, `sd()` and `var()`

Sometimes operations will produce `Inf` (positive infinity), `-Inf` (negative infinity) or `NaN` (Not a Number) as a result from a calculation

```{r t2_n10, exercise = TRUE, exercise.eval = FALSE}
c(-2,  -1,  0,  1,  2)/0
```

To determine whether a function is `Inf` or `–Inf` the function `is.infinite()` can be used

```{r t2_n11, exercise = TRUE, exercise.eval = FALSE}
is.infinite(c(-2, -1, 0,  1,  2)/0)
```

To determine whether a function is `NaN` the function `is.nan()` can be used

```{r t2_n12, exercise = TRUE, exercise.eval = FALSE}
is.na(c(-2, -1,  0,  1,  2)/0)
```

We can combine functions to perform common operations on numeric vectors. For instance some models expect all values to be within the range 0 to 1. To convert values to the range 0 to 1, normalisation can be used $X_{normalised} = \frac{X- X_{min}}{X_{max}-X_{min}}$

```{r t2_n13, exercise = TRUE, exercise.eval = FALSE}
x <- c(0, 2, 55, 23, 20, 48, 76) 
(x - min(x)) / (max(x) - min(x))
```

### Subsetting vectors

Elements of a vector can be selected, subset, in a several ways. To illustrate subsetting consider the vector

```{r t2_s1, exercise = TRUE, exercise.eval = FALSE}
first_names <- c("Andrew", "Beth", "Carly", "Dan")
```

**Option 1** Passing a single index or vector of entries to keep using [ ]

```{r t2_s2, exercise = TRUE, exercise.eval = FALSE}
first_names <- c("Andrew", "Beth", "Carly", "Dan")
first_names[c(1,4)]
```

**Option 2** Passing a single index or vector of entries to drop using [-]

```{r t2_s3, exercise = TRUE, exercise.eval = FALSE}
first_names <- c("Andrew", "Beth", "Carly", "Dan")
first_names[-c(1,4)] # or first_names[c(-1, -4)]
```

**Option 3** Passing a logical vector of entries to keep (TRUE) and entries to drop (FALSE) using []

```{r t2_s4, exercise = TRUE, exercise.eval = FALSE}
first_names <- c("Andrew", "Beth", "Carly", "Dan")
first_names[nchar(first_names) > 4]
```

Note that if the logical vector passed is of a different length than the vector to be subset, recycling will be applied

```{r t2_s5, exercise = TRUE, exercise.eval = FALSE}
first_names <- c("Andrew", "Beth", "Carly", "Dan")
first_names[c(TRUE, FALSE)]
```

**Option 4** Names can be assigned to a vector when a vector is created or using the function `names()`. Once names are defined, names can be used to subset a vector

```{r t2_s6, exercise = TRUE, exercise.eval = FALSE}
weekly_rainfall = c("Mo" = 10, "Tu" = 12, "We" = 0, "Th" = 4, "Fr" = 0) # option 1 
names(weekly_rainfall) <- c("Mo", "Tu", "We", "Th", "Fr") # option 2 
weekly_rainfall
weekly_rainfall[c("Mo", "Tu")]
```

### Missing values

Most data sets will contain missing values. R use the encoding `NA` (not available), without quotes, to represent missing values 

```{r t2_m1, exercise = TRUE, exercise.eval = FALSE}
vector_with_missing <- c(1, 2, 3, NA, 4, 5, 6, NA)
```

When you try to apply operations to vectors with `NA` values, most functions will return an error or simply `NA`

```{r t2_m2, exercise = TRUE, exercise.eval = FALSE}
mean(vector_with_missing)
```

In some functions, the optional argument `na.rm = TRUE` can be used to ignore the `NA` values in calculations

```{r t2_m3, exercise = TRUE, exercise.eval = FALSE}
mean(vector_with_missing, na.rm = TRUE)
```

Most operations that involve missing values will simply return a missing value. After all, R has “no idea” what the missing value represents 

```{r t2_m4, exercise = TRUE, exercise.eval = FALSE}
NA > 3
```

Similarly testing whether two missing values are equal with return `NA`

```{r t2_m5, exercise = TRUE, exercise.eval = FALSE}
NA == NA # R has no idea if the two missing values are the same value
```

When the actual value represented by `NA` is not important, R can return a non-NA output

```{r t2_m6, exercise = TRUE, exercise.eval = FALSE}
NA^0 # returns 1 
NA | TRUE # return TRUE 
NA & FALSE # return FALSE
```

To test whether a specific element of a vector is missing the `is.na()` function can be used

```{r t2_m7, exercise = TRUE, exercise.eval = FALSE}
vector_with_missing <- c(1, 2, 3, NA, 4, 5, 6, NA) 
is.na(vector_with_missing)
```

The `is.na()` function can also be used to create a subset of a vector that excludes missing values

```{r t2_m8, exercise = TRUE, exercise.eval = FALSE}
vector_with_missing[!is.na(vector_with_missing)]
```

## Matrices and Arrays

A **matrix** extends the idea of vectors into two dimensions: rows and columns. A simple way of thinking of a matrix is the simple reordering of the values of a vector into two dimensions where all rows of the matrix are the same length

![[Matrix.png]]

Elements of a vector can also be arranged in more than two dimensions, known as an **array**. For example, a colour image is typically represented as a three-dimensional array. As with atomic vectors, all the elements of an array and matrix must be of the same type. 

![[Array.png]]

### Creating matrices

A matrix can be directly constructed using the `matrix()` function. The `byrow` argument of the `matrix()` function determines whether the data fill is by row or by column


```{r t4_1, exercise = TRUE, exercise.eval = FALSE}
matrix(1:9,  nrow = 3) # create a matrix with 3 rows
```

```{r t4_2, exercise = TRUE, exercise.eval = FALSE}
matrix(1:9, nrow = 3, ncol = 3, byrow = TRUE) # create a matrix with 3 rows
```

R will try to construct a matrix even if it means that elements of the vector used to construct the matrix is repeated or dropped. R will not necessarily show a warning message if elements are repeated or dropped 

```{r t4_3, exercise = TRUE, exercise.eval = FALSE}
matrix(1:5, nrow = 2)
```

```{r t4_4, exercise = TRUE, exercise.eval = FALSE}
matrix(1:6, nrow = 2, ncol = 2)
```

A matrix can also be created by binding vectors together with the function `rbind()` which stands for row bind or `cbind()` which stands for column bind 

```{r t4_5, exercise = TRUE, exercise.eval = FALSE}
rbind(c(1, 2, 3), c(4, 5, 6)) # Bind rows to form a matrix
```

```{r t4_6, exercise = TRUE, exercise.eval = FALSE}
cbind(c(1, 2), c(3, 4), c(5, 6)) # Bind columns to form a matrix
```

Vectors used to construct a matrix must be of the same length. If vectors of different lengths are provided, R will recycle the elements of the shorter vector(s) 

```{r t4_7, exercise = TRUE, exercise.eval = FALSE}
cbind(c(1, 2), c(3))
```

As with atomic vectors, all the elements of a matrix must be of the same type. If a matrix is constructed with elements of different types, R will coarse the elements to the same type 

```{r t4_8, exercise = TRUE, exercise.eval = FALSE}
cbind(c("1", "2"), c(3, 4))
```

The functions `cbind()` and `rbind()` can also be used to extend existing matrices. Again, keep in mind the length and type of the “added” vectors

```{r t4_9, exercise = TRUE, exercise.eval = FALSE}
square_matrix <- matrix(c(1, 1, 1, 1), nrow = 2) 
cbind(square_matrix, c(2, 2))
```

```{r t4_10, exercise = TRUE, exercise.eval = FALSE}
square_matrix <- matrix(c(1, 1, 1, 1), nrow = 2) 
cbind(square_matrix, c("2")) # repeat + coarse
```

The diagonal of a matrix can be obtained by using the `diag()` function

```{r t4_11, exercise = TRUE, exercise.eval = FALSE}
diag(matrix(c(1, 0, 0, 1),  nrow = 2))
```

The diag() function can also be used to construct a diagonal matrix, such as the identify matrix

```{r t4_12, exercise = TRUE, exercise.eval = FALSE}
diag(x = 1, nrow = 3) # x is used to specify the value that is used to fill the diagonal
```

### Properties of matrices

Recall that an atomic vector has three properties: (i) a **type**, (ii) a **length** and (iii) **attributes**. A matrix has the same three properties as an atomic vector but includes some unique attributes

**Type**: To verify how a matrix is internally stored, the `typeof()` function can be used

```{r t4_13, exercise = TRUE, exercise.eval = FALSE}
typeof(matrix(c(1, 1, 1, 1), nrow = 2))
```

Internally R simply stores the matrix defined in the example above as “double” 

**Length** The number of elements stored in a matrix can be determined with the function `length()`

```{r t4_14, exercise = TRUE, exercise.eval = FALSE}
length(matrix(c(1, 1, 1, 1), nrow = 2))
```

**Attributes**: By default, an atomic vector has no attributes assigned to it 
```{r t4_15, exercise = TRUE, exercise.eval = FALSE}
my_vector <- 1:20 
attributes(my_vector)
```

To transform an atomic vector into a matrix or array, the dimension dim() attribute of the vector can be set 

```{r t4_16, exercise = TRUE, exercise.eval = FALSE}
dim(my_vector) <- c(4,5) # rearrange the vector into 4 rows and 5 columns 
my_vector
```

To verify that the “transformed” vector is indeed a matrix the attribute class can be checked
```{r t4_17, exercise = TRUE, exercise.eval = FALSE}
my_vector <- 1:20 
dim(my_vector) <- c(4, 5) # assign values to the dim() attribute of my_vector
class(my_vector)
```

The `class` attribute helps us understand the type of R object, while typeof specifies how the object is stored internally

```{r t4_18, exercise = TRUE, exercise.eval = FALSE}
typeof(my_vector)
```

Like vectors, names can be assigned to each element of a matrix. However, it is much more common to assign names to the rows and columns of a matrix. The function rownames() and colnames() can be used to set the names of the rows and columns of a matrix

```{r t4_19, exercise = TRUE, exercise.eval = FALSE}
new_hope <- c(461, 314) 
empire_strikes <- c(291, 248) 
return_jedi <- c(301, 166) 
box_office <- rbind(new_hope, empire_strikes, return_jedi) 
titles <- c("A New Hope", "The Empire Strikes Back", "Return of the Jedi") 
rownames(box_office) <- titles 
region <- c("US", "non-US") 
colnames(box_office) <- region 
box_office
```

Assume we have three numeric vectors of length two, where each numeric vector represents a Star Wars movie and the elements the US box office revenue and the Non-US box office revenue. We can combine the three numeric vectors using `rbind()` into a matrix box_office 

```{r t4_20, exercise = TRUE, exercise.eval = FALSE}
new_hope <- c(461, 314) 
empire_strikes <- c(291, 248) 
return_jedi <- c(301, 166) 
box_office <- rbind(new_hope, empire_strikes, return_jedi)
```

Using `rbind()` will automatically assign the names of the vectors to the rownames attribute of the matrix box_office

```{r t4_21, exercise = TRUE, exercise.eval = FALSE}
new_hope <- c(461, 314) 
empire_strikes <- c(291, 248) 
return_jedi <- c(301, 166) 
box_office <- rbind(new_hope, empire_strikes, return_jedi)
rownames(box_office)
```

To view the attributes assigned to an object the function `attributes()` can be used

```{r t4_22, exercise = TRUE, exercise.eval = FALSE}
my_matrix <- matrix(c(1, 1)) 
attributes(my_matrix)
```

```{r t4_23, exercise = TRUE, exercise.eval = FALSE}
my_matrix <- matrix(c(1, 1)) 
rownames(my_matrix) <- c("row 1", "row 2" # adds the attribute dimnames
attributes(my_matrix)
```

### Matrix calculations

Math operations are performed on matrices entry-wise given that the matrices are of the same dimensions 

```{r t4_24, exercise = TRUE, exercise.eval = FALSE}
(matrix(c(1, 1, 1, 1),  nrow = 2) + matrix(c(1,  1,  1, 1),  nrow = 2)) * 2
```

However, using matrices of different dimensions will result in an error

```{r t4_25, exercise = TRUE, exercise.eval = FALSE}
matrix(c(1, 1, 1, 1),  nrow = 2) + matrix(c(1,  1, 1, 1, 1, 1),  nrow = 2) 
```

Actual matrix multiplication (not-entry wise) is performed using the %*% operator

```{r t4_26, exercise = TRUE, exercise.eval = FALSE}
matrix(c(1, 1, 1, 1),  nrow = 2) %*% matrix(c(2, 2),  nrow = 2) 
```

When matrix multiplication is performed with matrices of incompatible dimensions an error will be generate 

```{r t4_27, exercise = TRUE, exercise.eval = FALSE}
matrix(c(1, 1, 1, 1),  nrow = 2) %*% matrix(c(2, 2),  ncol = 2)
```

The transpose of a matrix can be computed using the function `t()` for transpose. Recall that the transpose of a matrix is 

```{r t4_28, exercise = TRUE, exercise.eval = FALSE}
test_matrix = matrix(1:6,  nrow = 2)
t(test_matrix)
```

To invert a matrix, use the function `solve()`

```{r t4_29, exercise = TRUE, exercise.eval = FALSE}
my_matrix <- matrix(c(22, 49, 28, 64), nrow = 2) 
my_matrix_inv <- solve(my_matrix) 
my_matrix_inv
```

Note however if we try to check whether the invert hold, the off-diagonals of the result are not exactly zero 

```{r t4_30, exercise = TRUE, exercise.eval = FALSE}
my_matrix %*% my_matrix_inv
```

### Subsetting matrices

A matrix can be subset in a similar way as a vector. Instead of vectors, the index \[rows, columns] is used:

```{r t4_31, exercise = TRUE, exercise.eval = FALSE}
char_matrix <- matrix(letters,  nrow = 2,  ncol = 2) 
char_matrix
```

```{r t4_32, exercise = TRUE, exercise.eval = FALSE}
char_matrix <- matrix(letters,  nrow = 2,  ncol = 2) 
char_matrix[2, 2] # subset row 2 column 2
```

```{r t4_33, exercise = TRUE, exercise.eval = FALSE}
char_matrix <- matrix(letters,  nrow = 2,  ncol = 2) 
char_matrix[, 2] # keep all rows, subset column 2
```

If the columns or rows of a matrix has names assigned to it, the rownames or colnames can be used to subset a matrix

```{r t4_34, exercise = TRUE, exercise.eval = FALSE}
test_matrix <- matrix(1:6, nrow = 3) 
row.names(test_matrix) <- c("a", "b", "c") 
test_matrix[c("a","c"),]
```

In the previous example, we saw that R returns a vector if a matrix ends up having just one row or column after subsetting 

```{r t4_35, exercise = TRUE, exercise.eval = FALSE}
char_matrix <- matrix(letters,  nrow = 2,  ncol = 2) 
char_matrix[, 2]
```

To prevent the behaviour the optional argument drop should be set to FALSE

```{r t4_36, exercise = TRUE, exercise.eval = FALSE}
char_matrix <- matrix(letters,  nrow = 2,  ncol = 2) 
char_matrix[, 2,  drop = FALSE]
```

## Lists

A **list** in R can be used to store objects of multiple types. Storing objects of multiple types makes lists extremely versatile. A good analogy is to think of a list as your to-do list: items in your to-do list will likely differ in length, characteristics, and the type of activity that has to be done. For example, we can use a list to store a single value, a numeric vector and a matrix

![[List.png|500]]

The results of models are often returned as a list; therefore it is critical to understand how to work with lists

### Creating lists

To create a list the function `list()` can be used

```{r t5_1, exercise = TRUE, exercise.eval = FALSE}
list(5,  c(1:4),  matrix(c(1:4),  nrow = 2))
```

- the value `5` is stored in the first index of the list
- the numeric vector `c(1:4)` is stored in the second index of the list
- the matrix `matrix(c(1:4), nrow = 2)` is stored in the third index of the list

list can contain different data objects of different data types

```{r t5_2, exercise = TRUE, exercise.eval = FALSE}
my_list <- list(TRUE, c(1:4), matrix(c("a", "b", "c", "d"), nrow = 2)) 
str(my_list)
```

In the above example, we create a list containing (i) an atomic vector of type logical with a single element, (ii) an atomic vector of type integer with four elements and (iii) a matrix of type character with four elements 

Lists are very versatile data structures capable of storing any data object. For example, a list can be used to store a list  

```{r t5_3, exercise = TRUE, exercise.eval = FALSE}
child_list <- list(TRUE, c(1:4), matrix(c("a", "b", "c", "d"), nrow = 2)) 
parent_list <- list(child_list, c(1:4)) 
str(parent_list)
```

In the above example, we create a list named `parent_list` which stores a list in the first element and an atomic vector in the second element

### Extending lists

If we try to add an list to an existing list using the function list(), R will add the existing list as an element to the current list

```{r t5_4, exercise = TRUE, exercise.eval = FALSE}
l1 <- list(1:3, "a", c(TRUE, FALSE, TRUE)) 
l2 <- list(l1, c(2.5, 4.2)) 
str(l2)
```

To extend a list with a different list, simply use the function `append()`

```{r t5_5, exercise = TRUE, exercise.eval = FALSE}
l1 <- list(1:3, "a", c(TRUE, FALSE, TRUE)) 
l2 <- append(l1, c(2.5, 4.2)) 
str(l2)
```

### Properties of lists

Recall that an atomic vector has three properties: (i) a **type**, (ii) a **length** and (iii) **attributes**. A list has the same three properties as an atomic vector but includes some unique attributes

**Type**: The data type of a list is a list. Recall that a list is a type of vector that R internally store as the data type list 

```{r t5_6, exercise = TRUE, exercise.eval = FALSE}
my_list = list(first_thing = 55, second_thing = c(60, 42)) 
typeof(my_list)
```

**Length**: The length of a list is simply the number of elements in the list

```{r t5_7, exercise = TRUE, exercise.eval = FALSE}
my_list = list(first_thing = 55, second_thing = c(60, 42)) 
length(my_list)
```

**Attributes**: Like atomic vectors, names can also be assigned to the elements of a list. Names can be assigned to the elements of a list when the list is created or by using the function names() 

```{r t5_8, exercise = TRUE, exercise.eval = FALSE}
my_list = list(first_thing = 55,  second_thing = c(60,42),  third_thing = c("a",  "b")) 
names(my_list)
```

```{r t5_9, exercise = TRUE, exercise.eval = FALSE}
my_list = list(first_thing = 55,  second_thing = c(60,42),  third_thing = c("a",  "b")) 
names(my_list)
```

### Subsetting lists

There are three subsetting operators `[[`, `[` and `$` that can be used to subset a list. When thinking of how to subset a list it is often useful to think of a list as a train where each carriage of the train is an element of the list. Since the elements of a list can be named, the carriages of the train can be assigned names

![[Train.png]]

“If list x is a train (list) carrying objects, then `x[[2]]` is the object in car 2; `x[c(1:2)]` is a train (list) of cars 1-2” - @RLangTip

In other words, single brackets `[]` is used to select one or more elements from a list as a list, while double brackets `[[]]` are used to select the elements of a list 

To obtain the actual elements stored in a list use double brackets `[[ ]]`

```{r t5_10, exercise = TRUE, exercise.eval = FALSE}
my_list <- list(5,  c(1:4),  matrix(c(1:4),  nrow = 2)) 
my_list[[2]]
```

When single brackets `[ ]` is used to access list elements a list is returned instead 

```{r t5_11, exercise = TRUE, exercise.eval = FALSE}
my_list <- list(5,  c(1:4),  matrix(c(1:4),  nrow = 2)) 
my_list[1]
```

Single brackets are useful to obtain multiple elements stored in the list, since double brackets cannot be used to select multiple elements from the list

![[List1.png]]

![[List2.png]]

Given a list with names, the names can be used to select elements of a list either by (i) using the name and double brackets `[[ ]]` or using a `$` followed by the name 

```{r t5_12, exercise = TRUE, exercise.eval = FALSE}
my_list = list(first_thing = 55,  second_thing = c(60,42),  third_thing = c("a",  "b")) 
my_list[["first_thing"]] # or try my_list$first_thing
```

Subsetting can be used to extend a list. For example, we can assign an object to an element of a list that does not exist  

```{r t5_13, exercise = TRUE, exercise.eval = FALSE}
l1 <- list(c(1,1)) 
l1[3] <- TRUE 
str(l1)
```

or by adding a new named element

```{r t5_14, exercise = TRUE, exercise.eval = FALSE}
l1 <- list(c(1,1)) 
l1$"New element" <- TRUE 
str(l1)
```

## Dataframes

A **data frame** stores data in a **list** of **equal length vectors**. Each **element** of the **list** can be thought of as a column and the **length** of each element of the **list** is the number of rows. Since a data frame consist of a **list**, a data frame can store different types of data i.e. **numeric**, **logical**, **character** … in each column

![[Dataframe.png|540]]

### Creating a data frame

In most cases, we will create a data frame by importing a data set from an external source. However, data frames can also be created explicitly using the function `data.frame()`. Run the code below to create a data frame with three rows and four columns


```{r t6_1, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = 1:3, 
                 col2 = c("this", "is", "text"), 
                 col3 = c(TRUE, FALSE, TRUE), 
                 col4 = c(2.5, 4.2, pi)) 
str(df)
```

- If you do not provide names for the columns of a data frame, R will assign custom column names but it is not recommended. 
- In addition, avoid using duplicate column names

The elements of a data frame should be of equal length. Try running the code below; you should get an error that the number of rows differs

```{r t6_2, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 2, 3), col2 = c(1, 2))
```

R will only perform recycling when an atomic vector of length 1 is provided but is best avoided. Note that R will automatically **recycle** the value provided for column 2

```{r t6_3, exercise = TRUE, exercise.eval = FALSE}
data.frame(col1 = c(1,2,3), col2 = c(1))
```

Apart from atomic vectors, matrices and lists can be used to construct a data frame, but when lists are used the elements must be of the equal length. 

```{r t6_4, exercise = TRUE, exercise.eval = FALSE, exercise.cap = "Create a data frame from a matrix"}
data.frame(matrix(c(1, 2, 3, 4), nrow = 2, dimnames = list(NULL, c("a", "b"))))
```

```{r t6_5, exercise = TRUE, exercise.eval = FALSE, exercise.cap = "Create a data frame from a list"}
data.frame(list("col1" = c(1, 2, 3), "col2" = c(1, 2, 3)))
```

### Extending data frames

**Columns**: Columns can be added to a data frame using the function `cbind()`. Note that when using `cbind()` one of the objects being combined must be a data frame otherwise a matrix is created

```{r t6_6, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 2), col2 = c(3, 4)) 
cbind(df, col3 = c(5, 6))
```

**Rows**: Rows can be added to a data frame using the function `rbind()`. However, when adding rows to a data frame the data type of columns can change. R will **coarse** all values to a compatible data type

```{r t6_7, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 2), col2 = c(3, 4)) 
df <- rbind(df, c("1", "2")) 
str(df)
```

### Properties of data frames

An atomic vector has three properties: (i) a **type**, (ii) a **length** and (iii) **attributes**. A data frame has the same three properties as an atomic vector but includes some unique **attributes**

**Length:** The length of a data frame is the number of columns of the data frame

```{r t6_8, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 2), col2 = c(3, 4)) 
length(df)
```

**Data Type:** The data type of a data frame is a list. R stores a data frame as a list with some special conditions

```{r t6_9, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 2), col2 = c(3, 4)) 
typeof(df)
```

**Attributes**: Data frames can have additional attributes such as row names and column names 

```{r t6_10, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 2), col2 = c(3, 4)) 
attributes(df)
```

Row names can be added or changed using the function `rownames()`. Column names can be changed using the function `colnames()` or the function `names()`


### Subsetting data frames

If you subset a data frame using a single index \[columns], a data frame behave like a list and return the selected columns with all rows as a data frame

```{r t6_11, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df[1]
```

```{r t6_12, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df[c("col1", "col3")]
```

Since the subsetting of a data frame behave like lists, double brackets [[ ]] or a $ followed by the name of a column can be used to select the elements of the data frame. As with list, the result is returned as the most simplified data type i.e. an atomic vector and not a data frame  

```{r t6_13, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df[["col1"]]
```

```{r t6_14, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df$col1
```

If you subset a data frame using two vectors i.e. \[rows, columns], a data frame behaves like a matrix and return the selected rows and columns as the most simplified data structure by default 


```{r t6_15, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df[1, c(1, 2)] # return row 1 and column 1 and 2
```

```{r t6_16, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df[, 1] # returns a vector
```

```{r t6_17, exercise = TRUE, exercise.eval = FALSE}
df <- data.frame(col1 = c(1, 1), col2 = c(2, 2), col3 = c(3, 3)) 
df[, 1,  drop = FALSE] # returns a data frame
```

The rows of a data frame can also be selected using logical vectors. For example, using the built-in data frame cars  

```{r t6_18, exercise = TRUE, exercise.eval = FALSE}
cars[cars$speed == 24,]
```

```{r t6_19, exercise = TRUE, exercise.eval = FALSE}
cars[cars$speed == 24, "dist",  drop = FALSE]
```
