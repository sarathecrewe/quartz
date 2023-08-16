---
title: "R Week 2"
---

## ggplot2

### Overview

The **package** `ggplot2`, which forms part of the `tidyverse`, offers a powerful graphics language for creating elegant and complex plots. In this lesson we will cover how various types of plots can be created using the `ggplot2` **package**

Since the `ggplot2` **package** forms part of the `tidyverse`, you do not have to **install** the **package** if you have **installed** the `tidyverse` **package** previously. However, if you would like to **install** the **package**, the **function** `install.packages()` can be used

```{r InstallGGPlot, eval=FALSE, echo=TRUE, exercise=FALSE}
install.packages("ggplot2")
```

Once the **package** is installed, the **package** can be **loaded** using the **function** `library()`

```{r Load_GGPLOT, exercise=TRUE}
library(ggplot2) 
```

---

The design of `ggplot2` is based on the idea that any complex plot can be divided into **layers**. For example, a scatterplot and smoothed regression line can be combined to summarise the relationship between two continuous features


![[assets/Combine.png]]


---

## Primary components of a plot

### Layers and aesthetics

`ggplot2` graphic __objects__ consist of two primary components __layers__ and __aesthetics__:

1. __Layers__ are the components of a graph
    - For example, the __layer__ `geom_point()` adds a __layer__ of scatterplot points
    - Additional __layers__ can be added to a ggplot2 graphic __object__ by using the `+` __operator__ is e.g. `ggplot() + geom_point()`

2. __Aesthetics__ determine how __layers__ appear e.g. we can use __aesthetics__ to specify the colour of the points in a `geom_point()` __layer __
    - Aesthetics are set using __arguments__ inside a __layer function__       e.g. `geom_point(color = “red”)`
    - __Aesthetics__ includes location, colours and sizes

---

### Aesthetics: Setting versus mapping

Each layer has several __arguments__ that can be used to control the __appearance__ of a __layer__. When deciding how a __layer__ should appear, we first have to decide if the __appearance__ should be based on a __variable__ or not. An __aesthetic__ i.e. colour can be: 

1. __set__ to a constant value

```{r fig.height=3, fig.width=3, error = TRUE}
ggplot(mtcars, 
       aes(x = disp, y = mpg)) +
  geom_point(colour = "purple") + 
  labs(x = "Displacement", 
       y = "Milles/gallon",
       title = "Setting aesthetic")
```

2. or __mapped__ to a variable i.e. transmission

```{r fig.height=3, fig.width=4.3, error = TRUE}
ggplot(mtcars, aes(x = disp, y = mpg, colour = as.factor(am))) +
  geom_point() +
  labs(x = "Displacement", 
       y = "Milles/gallon",
       title = "Mapping aesthetic",
       colour = "Transmission") +
  scale_color_brewer(palette = "Dark2")
```

1. __Setting aesthetics__: __Arguments__ like colour, size, line type, shape, fill and alpha can be passed directly to a __layer__. These __aesthetics__ are not influenced by data. For example, we can specify that all points in a scatterplot should be purple

2. __Mapping aesthetics__: __Mapping aesthetics__ depend on data. For example, if we want the points in a scatterplot to have a different colour based on the values of a variable a __mapping aesthetic__ is required. Mapping aesthetics are specified inside the `aes()` __argument__


![[Aesthetic example.png]]

## Creating a basic plot

### Loading data

To illustrate how we can create visualisations using `ggplot2` we will be using data from the Gapminder project. We will be specifically focusing on the life expectancy of different countries. 

The Gapminder data can be accessed by __installing__ the __package__ `gapminder`

```{r InstallGapminder, eval=FALSE, echo=TRUE, exercise=FALSE}
install.packages("gapminder")
```

After __installing__ the `gapminder` __package__, __load__ the __package__ using the __function__ `library()`

```{r Load_Gapminder, exercise=TRUE}
library(gapminder) 
```

Once the __package__ is __loaded__ the data set will be stored in the __object__ `gapminder`. The __function__ `str()` can be used to view the structure of the `data` __object__

```{r View_Gapminder, exercise=TRUE}
str(gapminder) 
```

The `gapminder` object is a __tibble__; a special kind of __data frame__ with 1704 rows and 6 columns. 

---

### Painting

The first step in creating a `ggplot2` graphics __object__ is to define a `ggplot` __object__ using the __function__ `ggplot()`. The __function__ `ggplot()` simply creates  a blank canvas. This blank canvas can be used to add graphical elements to. Run the code below to view the blank canvas

```{r Base, exercise=TRUE}
ggplot()
```

__Adding a layer__

We can add layers to our blank canvas using the `+` operator. For instance we can add a `geom_point` __layer__ to our initial blank slate created with the __function__ `ggplot()`. 

```{r Base1, exercise=TRUE}
ggplot() + 
  geom_point()
```

Since we have not specified the data that we want to plot or how we want our data to be plotted, our canvas will remain blank.

__Adding data__

To specify what data to use in our plot, the __argument__ `data` should be set. Passing a data set to the __argument__ `data` is however not enough, we need to inform `ggplot2` how we want our data to __map__ to the plot. 

Inside the __function__ `aes()`, we specify the values that should be used for the x and y axis. 

Try plotting the life expectancy i.e. `lifeExp` of South Africa over time i.e. `year` modifying the code below:

```{r Base2, exercise=TRUE, exercise.blanks = "___+"}
ggplot() + 
  geom_point(data = gapminder[gapminder$country == "South Africa",1:6], 
             aes(x = ___, y = ___))
```

```{r Base2-solution}
ggplot() + 
  geom_point(data = gapminder[gapminder$country == "South Africa",1:6], 
             aes(x = year, y = lifeExp)) 
```

The __layer__ `geom_point`, mapped each data instance to a point on the graph. 

__Adding a layer__

If we wanted to add a line graph to our existing `ggplot2` graphics __object__, we can simply add a new __layer__. In this case we use the `geom_line` __layer__, a __layer__ that connect data points with a line. Add the layer `geom_line` in the code below

```{r Base3, exercise=TRUE, exercise.blanks = "___+"}
ggplot() + 
  geom_point(data = gapminder[gapminder$country == "South Africa",1:6], 
             aes(x = year, y = lifeExp)) +
  ___(data = gapminder[gapminder$country == "South Africa",1:6], 
             aes(x = year, y = lifeExp))
```

```{r Base3-solution}
ggplot() + 
  geom_point(data = gapminder[gapminder$country == "South Africa",1:6], 
             aes(x = year, y = lifeExp)) +
  geom_line(data = gapminder[gapminder$country == "South Africa",1:6], 
             aes(x = year, y = lifeExp))
```

New __layers__ will always be drawn over previous __layers__. 

__Avoid work; inherit__

It seems rather tedious to specify the __data__ and the __aesthetics__ for each __layer__. 

To avoid repetitive code, any __mapping aesthetic arguments__ specified in the `ggplot` __layer__, will be inherited by subsequent __layers__. The code below produces the same plot as the code in the previous block; with less typing

```{r Base4, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() 
```

This does not imply that we cannot overwrite the __arguments__ in subsequent __layers__.

## Adding flavour 

### Colour

If we want to change the colour of the line graph we can simply __set__ the colour __aesthetic__ equal to the value “blue”. 

Try changing the colour of the line to blue; without changing the colour of the points 

```{r Base5, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(___) 
```

```{r Base5-solution}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(colour = "blue") 
```

What happens if we use a __mapping aesthetic__ as oppose to a __setting aesthetic__ to set the colour of the line graph?

```{r Base6, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(aes(colour = "blue"))
```

When we set the colour using a __mapping aesthetic__, our line plot uses the colour red even though we specified blue. Using `aes(colour = “blue”)` maps the vector `c(“blue”)` to the colour element of the line plot. 

__Mapping aesthetics__ is used when we want to change how a __layer__ is displayed based on the underlying data. For instance, we can map the `year` __column__ of the data set to the colour __mapping aesthetic__ of the `geom_line` __layer__. In this case, `ggplot2` will assign colours based on the values in the `year` __column__. `ggplot2` will automatically assign a unique level of the __aesthetic__ to each value, a process known as __scaling__

```{r Base7, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(aes(colour = year))
```

### Scales

In the previous graph, the colours used for the __variable__ `year` were automatically selected. If we want to change how the __variable__ `year` map to a set of colours we need to add a __scale layer__ to a ggplot2 graphics __object__

A __scale layer__ use the following syntax: `scale_[aesthetic]_[option]` where:

  - `[aesthetic]` should be replaced the name of the __mapping aesthetic__ you would like to change e.g. colour, shape, linetype, alpha, size, fill, x, y ,
  - `[option]` should be used to specify how you would like to change the __aesthetic__. For example, `manual`, `continuous` or `discrete` (depending on the nature of the variable)

Examples 

  - `scale_linetype_manual()`: Manually specify the linetype of each different value
  - `scale_alpha_continuous()`: Varies transparency over a continuous range
    
To change the default colours use to __map__ the __variable__ `year` to colour, the __scale layer__ `scale_color_continuous` can be added

```{r Base8, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(aes(colour = year)) +
  scale_color_continuous(type = "gradient",
                         low = "red", 
                         high = "blue")
```

When we try to add a __scale__ that is not compatible with the __map variable__, the code will return an error

```{r Base9, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(aes(colour = year)) +
  scale_color_discrete()
```

In the above example, `ggplot2` returns an error since we are trying to __map__ a __continuous variable__ to a __discrete__ __set__ of colour.

When we map a discrete __variable__ to the __aesthetic__ colour e.g. `country`, we can __map__ the __variable__ to a discrete set of colours by adding the layer `scale_color_discrete`

```{r Base10, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line(aes(colour = country)) +
  scale_color_discrete(type = c("blue"))
```

Since x is a __mapping aesthetic__, it also has a __scale__. For example, we can add the __scale layer__ ``scale_x_continuous` to set the major tick marks or breaks to align with the years that we collected data for

```{r Base11, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  scale_x_continuous(breaks = seq(1952, 2007, by = 5))
```

__Scales__ can also be used to limit the data displayed. In the example, any __rows__ with years before 1995 is removed

```{r Base12, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  scale_x_continuous(limit = c(1995, NA))
```

### Labels

A x-label will be automatically created using the __name__ of the __variable mapped__ to the x __aesthetic__. To add a manual label, the __layer__ `xlab` can be added. The `xlab` __layer__ can also be used to remove the label e.g. `xlab(NULL)`

```{r Base13, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  xlab("Year") 
```

To add a custom y-label and title to a plot add the __layers__ `ylab` and `ggtitle` respectively

```{r Base14, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  xlab("Year") + 
  ylab("Life expectancy") +
  ggtitle("Life expectancy in SA")
```

The `ggtitle`, `xlab` and `ylab` are __helper layers__; shortcuts to quickly change a ggplot2 graphics __object__. In general, any labels including labels of legends can be set using the `labs` __layer __

```{r Base15, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  labs(title = "Life expectancy in SA", 
       x = "Year", 
       y = "Life expectancy")
```

### Themes

The `theme` __layer__ allows you to exercise fine control over non-data elements of a plot. The `ggplot2` __package__ includes several build-in themes e.g. `theme_grey()`, `theme_bw()`, `theme_linedraw()`, `theme_light()`, `theme_dark()`, `theme_minimal()` or `theme_classic()`

![[Theme.png]]

Adding a built-in theme is straightforward, simply add the theme as a __layer__. If you want all your plots to use the same theme use the __function__ `theme_set()` at the start of an R Script e.g. `theme_set(theme_classic())`

```{r Base16, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  labs(title = "Life expectancy in SA", 
       x = "Year", 
       y = "Life expectancy") +
  theme_bw()
```

We can also create our own theme using the `theme` __layer__:

  - The theme __layer__ has several __arguments__ (also called elements) that specify the non-data elements that can be controlled. For example, the `plot.title` element controls the appearance of the plot titles
  - Each element is associated with an element function, which describes the visual properties of the element. For example, `element_text()` sets the font size
  - There are over 30 different elements, which can be viewed by opening the help file of the __function__ `theme` e.g. `?theme`
  - If you are interested in how the different elements work refer to the book: 'ggplot2: Elegant Graphics for Data Analysis by Wickham H'

You can also add a built-in theme __layer__ followed by the theme __layer__ to override some of the built-in theme settings. I usually add the theme __layer__ last to ensure none of the other layer overrides any of the theme settings

```{r Base17, exercise=TRUE, exercise.blanks = "___+"}
ggplot(data = gapminder[gapminder$country == "South Africa",1:6], 
       aes(x = year, y = lifeExp)) + 
  geom_point() + 
  geom_line() +
  labs(title = "Life expectancy in SA", 
       x = "Year", 
       y = "Life expectancy") +
  theme(plot.title = element_text(size = 8))
```

### Saving plots as objects

Since a `ggplot2` plot is an __object__ we can assign a __name__ to the `ggplot` __object__

```{r Base18, exercise=TRUE, exercise.blanks = "___+"}
my_plot <- ggplot(data = gapminder[gapminder$country == "South Africa",1:6],
                  aes(x = year, y = lifeExp)) + 
  geom_point() 
```

When you assign a __name__ to a `ggplot` __object__, the plot will not automatically show in the `Plots` tab of `R Studio` or as an output. To show the graph simply add a single line of code with the __name__ of the __object__, e.g. the same as printing an object. Assigning a __name__ to a `ggplot` __object__ means that we can easily use it later. If I want some of my plots to look the same I usually store a custom theme in an object and add it as a layer to a `ggplot2` graphics __object__. 

Once a __name__ is assigned to a `ggplot` __object__, we can easily use the __object__. For example, a __name__ can be assigned to a `ggplot` __object__ that stores a custom theme. The __name__ of the `object` can then simply be added to any `ggplot2` __object__

```{r Base19, exercise=TRUE, exercise.blanks = "___+"}
my_theme <- theme(axis.text = element_text(size = 10)) # Custom theme 

ggplot(data = gapminder[gapminder$country == "South Africa",1:6],
                  aes(x = year, y = lifeExp)) + 
  geom_point() +
  my_theme
```

## Advance plots

### Grammer of graphics

The layer grammar of graphics consists of eight components namely: Data, Mapping, Statistics, Scales, Geometries, Facets, Coordinates and Themes. We have discussed some of these components before. In this lesson we will introduce some new __layers__ and discuss some fundamental assumptions made by `ggplot2`

![[grammer.png|350]]

### Grouping data points

Points are connected in a specific order. When creating a line plot, `ggplot2` has to decide how each data point or __instance__ should be connected. By default, data points or __instances__ will all be connected based on a heuristic.

```{r Adv1, exercise=TRUE}
ggplot(data.frame(x = c(1,1,2,2,3,3), y = c(1,2,2,1,1,2)), 
       aes(x = x, y = y)) + 
  geom_line()
```

When we try to create a line plot of the life expectancy over time for all countries in the `gapminder` data set, `ggplot2` connects all the __instances__. From the output, the life expectancy per country is not clear. We require a way to tell `ggplot2` that only specific data points should be connected  

```{r Adv2, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp)) + 
  geom_line()
```

One way to create a separate line for each country is to map the __variable__ `country` to a __mapping aesthetic__. For example, the __variable__ `country` can be __mapped__ to the colour `aesthetic`. Here, we need to include 142 countries in the legend which in turn uses up all the plotting space

```{r Adv3, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp, 
           colour = country)) + 
geom_line()
```

The theme __layer__ can be used to remove the legend of the plot. Once the legend is removed, we end up with a line for each country in the gapminder data set

```{r Adv4, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp, 
           colour = country)) + 
geom_line() +
  theme(legend.position = "none")
```

Many __geoms layers__, like `geom_line()`, use a single geometric object to display multiple rows of data. For these `geoms`, you can set the __group aesthetic__ to draw multiple objects without adding a legend or distinguishing feature to the plot

```{r Adv5, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp, 
           group = country)) + 
  geom_line() 

```

### Facets

To highlight the regional difference between life expectancy trends, the __variable__ continent can be __mapped__ to the colour __aesthetic__ 

```{r Adv6, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp, 
           group = country,  
           colour = continent)) +
  geom_line() 
```

A __facet__ can be used to split a plot into subplots where each subplot displays a subset of the data. To __facet__ a plot add the __layer__ `facet_wrap()`

```{r Adv7, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp, 
           group = country, 
           colour = continent)) + 
  geom_line() + 
  facet_wrap(c("continent"))
```

Our previous plot had various text that overlapped. To remove any text that overlap, the legend can be removed and the text size reduced 

```{r Adv8, exercise=TRUE}
ggplot(data = gapminder, aes(x = year, y = lifeExp,  group = country, 
                             colour = continent)) + 
  geom_line() + 
  facet_wrap(c("continent")) + 
  theme(legend.position = "none", 
        text = element_text(size = 8))

```

### Modifying position

Suppose we are interested in understanding the distribution of life expectancy over years. We could create a scatterplot, but due to multiple points overlapping, the scatterplot is difficult to interpret. All __layers__ include a position adjustment __argument__ that can be used to resolve overlapping data. The default position can be changed using the position __argument__

```{r Ad1, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp)) +
  geom_point()
```

To avoid overlapping data points a small amount of noise can be added to the y-coordinate of each data point. To add a small amount of y noise to each data point, the __argument__ `position` can be set to `jitter`

```{r Ad2, exercise=TRUE}
ggplot(data = gapminder, 
       aes(x = year, y = lifeExp)) + 
  geom_point(position = "jitter", 
             aes(colour = as.character(year))) + 
  scale_x_continuous(breaks = 
                       seq(1952, 2007, by = 5)) + 
  labs(colour = "year")

```

## Statistical transformations

Imagine we wanted to develop a __function__ that produces a bar chart from data. When we design the __function__ we will have to decide:

1. if the user simply passes the raw data and we calculate the count per category or
2. If the user passes the count per category

![[Decision.png]]

When a `geom_bar()` __layer__ is added to a ggplot graphic __object__, ggplot2 computes the count per category from the raw data

```{r Adv9, exercise=TRUE}
student_data <- data.frame(student_number = 1:6, 
                           degree = c(rep("Engineering", 2), 
                                      rep("Computer Science", 3),
                                      "Accounting")) 

ggplot(data = student_data, 
       aes(x = degree)) + 
  geom_bar()
```

The algorithm that `ggplot` uses to calculate statics from raw data is called a `stat`, short for statistical transformation. The figure below illustrates how the process works for the `geom_bar` __layer__:

![[geom_bar.png]]

  - To determine which __stat__ is applied by a geom __layer__ the help file of the __geom__ can be viewed i.e. `?geom_bar`
  - The default __stat__ used for the `geom_bar` __layer__ is `count`, which means that `geom_bar()` uses `stat_count()`
  - You can generally use __geom__ and __stats__ interchangeably i.e. a bar plot can be created with `geom_bar()` or `stat_count()`
  
The default __argument__ passed to the stat __argument__ of a __geom layer__ can be changed. For example, the __stat__ can be changed from `count` to `identity`. When using the stat `identity` the height of the bars is plotted to the raw values of the __variable mapped__ to the y __aesthetic__
  
```{r Adv10, exercise=TRUE}
data_count <- data.frame(degree = c("Engineering", "Accounting", "Computer Science"), 
                         count = c(2, 3, 1)) 

ggplot(data = data_count , 
       aes(x = degree, y = count)) + 
  geom_bar(stat = "identity")
```



