
---
output: html_document
editor_options:
  chunk_output_type: console
---
# Data Visualisation

## Introduction


```r
library("tidyverse")
```

No exercises.

## First Steps

### Exercise <span class="exercise-number">3.2.1</span>. {.unnumbered .exercise}

<div class="question">
Run `ggplot(data = mpg)` what do you see?
</div>

<div class="answer">

An empty plot. 
The background of the plot is created by the `ggplot()` function, 
but no geoms were specified, so there is nothing to display.


```r
ggplot(data = mpg)
```

<img src="visualize_files/figure-html/unnamed-chunk-3-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.2.2</span>. {.unnumbered .exercise}

<div class="question">

How many rows are in `mtcars`? 
How many columns?

</div>

<div class="answer">

There are 32 rows and 11 columns in the `mtcars` data frame.

```r
nrow(mtcars)
#> [1] 32
ncol(mtcars)
#> [1] 11
```

The `glimpse()` function also displays the number of rows and columns in a data frame.

```r
glimpse(mtcars)
#> Observations: 32
#> Variables: 11
#> $ mpg  <dbl> 21.0, 21.0, 22.8, 21.4, 18.7, 18.1, 14.3, 24.4, 22.8, 19....
#> $ cyl  <dbl> 6, 6, 4, 6, 8, 6, 8, 4, 4, 6, 6, 8, 8, 8, 8, 8, 8, 4, 4, ...
#> $ disp <dbl> 160.0, 160.0, 108.0, 258.0, 360.0, 225.0, 360.0, 146.7, 1...
#> $ hp   <dbl> 110, 110, 93, 110, 175, 105, 245, 62, 95, 123, 123, 180, ...
#> $ drat <dbl> 3.90, 3.90, 3.85, 3.08, 3.15, 2.76, 3.21, 3.69, 3.92, 3.9...
#> $ wt   <dbl> 2.62, 2.88, 2.32, 3.21, 3.44, 3.46, 3.57, 3.19, 3.15, 3.4...
#> $ qsec <dbl> 16.5, 17.0, 18.6, 19.4, 17.0, 20.2, 15.8, 20.0, 22.9, 18....
#> $ vs   <dbl> 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, ...
#> $ am   <dbl> 1, 1, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 1, ...
#> $ gear <dbl> 4, 4, 4, 3, 3, 3, 3, 4, 4, 4, 4, 3, 3, 3, 3, 3, 3, 4, 4, ...
#> $ carb <dbl> 4, 4, 1, 1, 2, 1, 4, 2, 2, 4, 4, 3, 3, 3, 4, 4, 4, 1, 2, ...
```

</div>

### Exercise <span class="exercise-number">3.2.3</span>. {.unnumbered .exercise}

<div class="question">

What does the `drv` variable describe? 
Read the help for `?mpg` to find out.

</div>

<div class="answer">

The `drv` variable is categorical variable which categorizes cars into front-wheels, rear-wheels, or four-wheel drive.[^layout]

| Value      | Description                                                                                   |
|------------|-----------------------------------------------------------------------------------------------|
| `"f"`      | [front-wheel drive](https://en.wikipedia.org/wiki/Front-wheel_drive)                          |
| `"r"`      | [rear-wheel drive](https://en.wikipedia.org/wiki/Automobile_layout#Rear-wheel-drive_layouts)  |
| `"4"`      | [four-wheel drive](https://en.wikipedia.org/wiki/Four-wheel_drive)                            |

[^layout]: See the Wikipedia article on [Automobile layout](https://en.wikipedia.org/wiki/Automobile_layout).

</div>

### Exercise <span class="exercise-number">3.2.4</span>. {.unnumbered .exercise}

<div class="question">
Make a scatter plot of `hwy` vs `cyl`.
</div>

<div class="answer">


```r
ggplot(mpg, aes(x = hwy, y = cyl)) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-6-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.2.5</span>. {.unnumbered .exercise}

<div class="question">

What happens if you make a scatter plot of `class` vs `drv`?
Why is the plot not useful?

</div>

<div class="answer">

The resulting scatterplot only has a few points and looks like grid.

```r
ggplot(mpg, aes(x = class, y = drv)) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-7-1.png" width="70%" style="display: block; margin: auto;" />

A scatter plot is not a useful way to plot these variables, since both `drv` and `class` are categorical variables which take limited number of unique combinations of `drv` and `class`.


```r
count(mpg, drv, class)
#> # A tibble: 12 x 3
#>   drv   class          n
#>   <chr> <chr>      <int>
#> 1 4     compact       12
#> 2 4     midsize        3
#> 3 4     pickup        33
#> 4 4     subcompact     4
#> 5 4     suv           51
#> 6 f     compact       35
#> # ... with 6 more rows
```

A simple scatter plot does not show how many observations there are for each (`x`, `y`) value.
Scatterplots work best for plotting a continuous x and a continuous y variable,  and when all (`x`, `y`) values are unique.

This uses material covered in a later section, but may be of interest. 
Section [7.5.2](https://r4ds.had.co.nz/exploratory-data-analysis.html#two-categorical-variables) discusses methods for plotting two categorical variables.
The first is `geom_count()` which is similar to a scatterplot but uses the size of the points to show the number of observations at an (`x`, `y`) point.

```r
ggplot(mpg, aes(x = class, y = drv)) +
  geom_count()
```

<img src="visualize_files/figure-html/unnamed-chunk-9-1.png" width="70%" style="display: block; margin: auto;" />
The second is `geom_tile()` which uses a color scale show the number of observations with each (`x`, `y`) value.

```r
mpg %>%
  count(class, drv) %>%
  ggplot(aes(x = class, y = drv)) +
    geom_tile(mapping = aes(fill = n))
```

<img src="visualize_files/figure-html/unnamed-chunk-10-1.png" width="70%" style="display: block; margin: auto;" />

In the previous plot, the are many missing tiles, which are associated with unobserved combinations of`class` and `drv` values.
The `complete()` function in **tidyr** adds new rows to a data frame for missing combinations of columns.
The following code add rows for missing combinations of `class` and `drv` and uses the `fill` argument to set `n = 0` for those new rows.


```r
mpg %>%
  count(class, drv) %>%
  complete(class, drv, fill = list(n = 0L)) %>%
  ggplot(aes(x = class, y = drv)) +
    geom_tile(mapping = aes(fill = n))
```

<img src="visualize_files/figure-html/unnamed-chunk-11-1.png" width="70%" style="display: block; margin: auto;" />

</div>

## Aesthetic mappings

### Exercise <span class="exercise-number">3.3.1</span> {.unnumbered .exercise}

<div class="question">

What’s gone wrong with this code? 
Why are the points not blue?


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, colour = "blue"))
```

<img src="visualize_files/figure-html/unnamed-chunk-12-1.png" width="70%" style="display: block; margin: auto;" />

</div>

<div class="answer">

The argument`colour = "blue"` is included within the `mapping` argument, and as such it is treated as an aesthetic, which is a mapping between a variable and a value.
In the expression, `color="blue"`, `"blue"` is interpreted not as a variable name, but as a variable that takes a single value `"blue"`.
If this is confusing, consider how `colour = 1:234` and `colour = 1` are interpreted by `aes()`.

The following code does what was expected.


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy), colour = "blue")
```

<img src="visualize_files/figure-html/unnamed-chunk-13-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.3.2</span> {.unnumbered .exercise}

<div class="question">

Which variables in `mpg` are categorical?
Which variables are continuous?
(Hint: type `?mpg` to read the documentation for the dataset).
How can you see this information when you run `mpg`?

</div>

<div class="answer">


```r
?mpg
```

When printing the data frame, this information is given at the top of each column within angled brackets.
Categorical variables have a class of "character" (`<chr>`).

```r
mpg
#> # A tibble: 234 x 11
#>   manufacturer model displ  year   cyl trans  drv     cty   hwy fl    class
#>   <chr>        <chr> <dbl> <int> <int> <chr>  <chr> <int> <int> <chr> <chr>
#> 1 audi         a4      1.8  1999     4 auto(… f        18    29 p     comp…
#> 2 audi         a4      1.8  1999     4 manua… f        21    29 p     comp…
#> 3 audi         a4      2    2008     4 manua… f        20    31 p     comp…
#> 4 audi         a4      2    2008     4 auto(… f        21    30 p     comp…
#> 5 audi         a4      2.8  1999     6 auto(… f        16    26 p     comp…
#> 6 audi         a4      2.8  1999     6 manua… f        18    26 p     comp…
#> # … with 228 more rows
```
Alternatively, `glimpse()` displays the type of each column:

```r
glimpse(mpg)
#> Observations: 234
#> Variables: 11
#> $ manufacturer <chr> "audi", "audi", "audi", "audi", "audi", "audi", "au…
#> $ model        <chr> "a4", "a4", "a4", "a4", "a4", "a4", "a4", "a4 quatt…
#> $ displ        <dbl> 1.8, 1.8, 2.0, 2.0, 2.8, 2.8, 3.1, 1.8, 1.8, 2.0, 2…
#> $ year         <int> 1999, 1999, 2008, 2008, 1999, 1999, 2008, 1999, 199…
#> $ cyl          <int> 4, 4, 4, 4, 6, 6, 6, 4, 4, 4, 4, 6, 6, 6, 6, 6, 6, …
#> $ trans        <chr> "auto(l5)", "manual(m5)", "manual(m6)", "auto(av)",…
#> $ drv          <chr> "f", "f", "f", "f", "f", "f", "f", "4", "4", "4", "…
#> $ cty          <int> 18, 21, 20, 21, 16, 18, 18, 18, 16, 20, 19, 15, 17,…
#> $ hwy          <int> 29, 29, 31, 30, 26, 26, 27, 26, 25, 28, 27, 25, 25,…
#> $ fl           <chr> "p", "p", "p", "p", "p", "p", "p", "p", "p", "p", "…
#> $ class        <chr> "compact", "compact", "compact", "compact", "compac…
```

</div>

### Exercise <span class="exercise-number">3.3.3</span> {.unnumbered .exercise}

<div class="question">

Map a continuous variable to color, size, and shape.
How do these aesthetics behave differently for categorical vs. continuous variables?

</div>

<div class="answer">

The variable `cty`, city highway miles per gallon, is a continuous variable.


```r
ggplot(mpg, aes(x = displ, y = hwy, colour = cty)) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-17-1.png" width="70%" style="display: block; margin: auto;" />

Instead of using discrete colors, the continuous variable uses a scale that varies from a light to dark blue color.


```r
ggplot(mpg, aes(x = displ, y = hwy, size = cty)) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-18-1.png" width="70%" style="display: block; margin: auto;" />

When mapped to size, the sizes of the points vary continuously with respect to the size.


```r
ggplot(mpg, aes(x = displ, y = hwy, shape = cty)) +
  geom_point()
#> Error: A continuous variable can not be mapped to shape
```

<img src="visualize_files/figure-html/unnamed-chunk-19-1.png" width="70%" style="display: block; margin: auto;" />

When a continuous value is mapped to shape, it gives an error.
Though we could split a continuous variable into discrete categories and use a shape aesthetic, this would conceptually not make sense.
A continuous numeric variable is ordered, but shapes have no natural order.
It is clear that smaller points correspond to smaller values, or once the color scale is given, which colors correspond to larger or smaller values. But it is not clear whether a square is greater or less than a circle.

</div>

### Exercise <span class="exercise-number">3.3.4</span> {.unnumbered .exercise}

<div class="question">

What happens if you map the same variable to multiple aesthetics?

</div>

<div class="answer">


```r
ggplot(mpg, aes(x = displ, y = hwy, colour = hwy, size = displ)) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-20-1.png" width="70%" style="display: block; margin: auto;" />

In the above plot, `hwy` is mapped to both location on the y-axis and color, and `displ` is mapped to both location on the x-axis and size.
The code works and produces a plot, even if it is a bad one.
Mapping a single variable to multiple aesthetics is redundant.
Because it is redundant information, in most cases avoid mapping a single variable to multiple aesthetics.

</div>

### Exercise <span class="exercise-number">3.3.5</span> {.unnumbered .exercise}

<div class="question">

What does the stroke aesthetic do? 
What shapes does it work with? 
(Hint: use `?geom_point`)

</div>

<div class="answer">

Stroke changes the size of the border for shapes (21-25).
These are filled shapes in which the color and size of the border can differ from that of the filled interior of the shape.

For example

```r
ggplot(mtcars, aes(wt, mpg)) +
  geom_point(shape = 21, colour = "black", fill = "white", size = 5, stroke = 5)
```

<img src="visualize_files/figure-html/ex.3.3.1.5-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.3.6</span>. {.unnumbered .exercise}

<div class="question">

What happens if you map an aesthetic to something other than a variable name, like `aes(colour = displ < 5)`?

</div>

<div class="answer">


```r
ggplot(mpg, aes(x = displ, y = hwy, colour = displ < 5)) +
  geom_point()
```

<img src="visualize_files/figure-html/ex.3.3.1.6-1.png" width="70%" style="display: block; margin: auto;" />

Aesthetics can also be mapped to expressions (code like `displ < 5`).
It will create a temporary variable which takes values from  the result of the expression.
In this case, it is logical variable which is `TRUE` or `FALSE`.
This also explains exercise 1, `colour = "blue"` created a categorical variable that only had one category: "blue".

</div>

## Common problems

No exercises

## Facets

### Exercise <span class="exercise-number">3.5.1</span> {.unnumbered .exercise}

<div class="question">

What happens if you facet on a continuous variable?

</div>

<div class="answer">

Let's see.

```r
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point() +
  facet_grid(. ~ cty)
```

<img src="visualize_files/figure-html/ex.3.5.1.1-1.png" width="70%" style="display: block; margin: auto;" />

It converts the continuous variable to a factor and creates facets for **all** unique values of it.

</div>

### Exercise <span class="exercise-number">3.5.2</span> {.unnumbered .exercise}

<div class="question">

What do the empty cells in plot with `facet_grid(drv ~ cyl)` mean?
How do they relate to this plot?

</div>

<div class="answer">

They are cells in which there are no values of the combination of `drv` and `cyl`.


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = drv, y = cyl))
```

<img src="visualize_files/figure-html/unnamed-chunk-21-1.png" width="70%" style="display: block; margin: auto;" />

The locations in the above plot without points are the same cells in `facet_grid(drv ~ cyl)` that have no points.

</div>

### Exercise <span class="exercise-number">3.5.3</span> {.unnumbered .exercise}

<div class="question">

What plots does the following code make? What does `.` do?

</div>

<div class="answer">

The symbol `.` ignores that dimension when faceting.
For example, plot facets by values of `drv` on the y-axis.


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(drv ~ .)
```

<img src="visualize_files/figure-html/ex.3.5.1.4.a-1.png" width="70%" style="display: block; margin: auto;" />

While this plot facets by values of `cyl` on the x-axis.


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_grid(. ~ cyl)
```

<img src="visualize_files/figure-html/ex.3.5.1.4.b-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.5.4</span> {.unnumbered .exercise}

<div class="question">

Take the first faceted plot in this section:


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy)) +
  facet_wrap(~class, nrow = 2)
```

<img src="visualize_files/figure-html/unnamed-chunk-22-1.png" width="70%" style="display: block; margin: auto;" />

What are the advantages to using faceting instead of the colour aesthetic?
What are the disadvantages?
How might the balance change if you had a larger dataset?

</div>

<div class="answer">

This is what the plot looks like when `class` is represented by the color aesthetic.


```r
ggplot(data = mpg) +
  geom_point(mapping = aes(x = displ, y = hwy, color = class))
```

<img src="visualize_files/figure-html/unnamed-chunk-23-1.png" width="70%" style="display: block; margin: auto;" />

Advantages of encoding `class` with facets instead of color include ability to encode more distinct categories.
For me, it is difficult to distinguish between the colors of `"midsize"` and `"minivan"`.

Given human visual perception, the max number of colors to use when encoding
unordered categorical (qualitative) data is nine, and in practice, often much less than that.
Placing points in different categories in different scales makes it difficult to directly compare values of individual points in different categories.
However, it can make it easier to compare the overall patterns and trends between categories.

Disadvantages of encoding the `class` variable with facets instead of color are that different
the different class is that the points for each category are on different plots,
making it more difficult to directly compare the locations of individual points.
Using the same x- and y-scales for all facets lessens this disadvantage.
Since encoding class within color also places all points on the same plot,
it visualizes the unconditional relationship between the x and y variables;
with facets, the unconditional relationship is no longer visualized since the
points are spread across multiple plots.

The benefits encoding a variable through facetting over color become more advantageous as either the number of points or the number of categories increase.
In the former, as the number of points increase, there is likely to be more
overlap.

It is difficult to handle overlapping points with color.
Jittering will still work with color.
But jittering will only work well if there are few points and the classes do not overlap much, otherwise the colors of areas will no longer be distinct and it will be hard to visually pick out the patterns of different categories.
Transparency (`alpha`) does not work well with colors since the mixing of overlapping transparent colors will no longer represent the colors of the categories.
Binning methods use already color to encode density, so color cannot be used to encode categories.

As noted before, as the number of categories increases, the difference between
colors decreases, to the point that the color of categories will no longer be
visually distinct.

</div>

### Exercise <span class="exercise-number">3.5.5</span> {.unnumbered .exercise}

<div class="question">

Read `?facet_wrap`.
What does `nrow` do? What does `ncol` do? 
What other options control the layout of the individual panels? 
Why doesn’t `facet_grid()` have `nrow` and `ncol` variables?

</div>

<div class="answer">

The arguments `nrow` (`ncol`) determines the number of rows (columns) to use when laying out the facets.
It is necessary since `facet_wrap()` only facets on one variable.

These arguments are unnecessary for `facet_grid()` since the number of rows and columns are determined by the number of unique values of the variables specified in the function.

</div>

### Exercise <span class="exercise-number">3.5.6</span> {.unnumbered .exercise}

<div class="question">

When using `facet_grid()` you should usually put the variable with more unique levels in the columns.
Why?

</div>

<div class="answer">

IF the plot is laid out horizontally, there will be more space for columns.
You should put the variable with more unique levels in the columns if the plot is laid out landscape.
It is easier to compare relative levels of y by scanning horizontally, so it may be easier to visually compare these levels.

</div>

## Geometric Objects

### Exercise <span class="exercise-number">3.6.1</span> {.unnumbered .exercise}

<div class="question">

What geom would you use to draw a line chart? 
A boxplot? 
A histogram? 
An area chart?

</div>

<div class="answer">

-   line chart: `geom_line()`
-   boxplot: `geom_boxplot()`
-   histogram: `geom_hist()`
-   area chart: `geom_area()`

</div>

### Exercise <span class="exercise-number">3.6.2</span> {.unnumbered .exercise}

<div class="question">

Run this code in your head and predict what the output will look like. 
Then, run the code in R and check your predictions.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, colour = drv)) +
  geom_point() +
  geom_smooth(se = FALSE)
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/ex-3-6-2-1.png" width="70%" style="display: block; margin: auto;" />

</div>

<div class="answer">

This will produce a scatter plot with `displ` on the x-axis, `hwy` on the y-axis.
The points will be colored by `drv`.
There will be a smooth line, without standard errors, fit through each `drv` group.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, colour = drv)) +
  geom_point() +
  geom_smooth(se = FALSE)
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-24-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.6.3</span> {.unnumbered .exercise}

<div class="question">

What does `show.legend = FALSE` do?
What happens if you remove it?
Why do you think I used it earlier in the chapter?

</div>

<div class="answer">

The theme option `show.legend = FALSE` hides the legend box. 

Consider this example earlier in the chapter.

```r
ggplot(data = mpg) +
  geom_smooth(
    mapping = aes(x = displ, y = hwy, colour = drv),
    show.legend = FALSE
  )
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-25-1.png" width="70%" style="display: block; margin: auto;" />

In that plot, there is no legend.
Removing the `show.legend` argument or setting `show.legend = TRUE` will result in the plot having  a legend displaying the mapping between colors and `drv`.


```r
ggplot(data = mpg) +
  geom_smooth(mapping = aes(x = displ, y = hwy, colour = drv))
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-26-1.png" width="70%" style="display: block; margin: auto;" />

In the chapter, the legend is suppressed because with three plots,
adding a legend to only the last plot would make the sizes of plots different.
Different sized plots would make it more difficult to see how arguments change the appearance of the plots.
The purpose of those plots is to show the difference between no groups, using a `group` aesthetic, and using a `color` aesthetic, which creates implicit groups.
In that example, the legend isn't necessary since looking up the values associated with each color isn't necessary to make that point.

</div>

### Exercise <span class="exercise-number">3.6.4</span> {.unnumbered .exercise}

<div class="question">

What does the `se` argument to `geom_smooth()` do?

</div>

<div class="answer">

It adds standard error bands to the lines.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, colour = drv)) +
  geom_point() +
  geom_smooth(se = TRUE)
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-27-1.png" width="70%" style="display: block; margin: auto;" />

By default `se = TRUE`:


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy, colour = drv)) +
  geom_point() +
  geom_smooth()
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-28-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.6.5</span> {.unnumbered .exercise}

<div class="question">

Will these two graphs look different?
Why/why not?


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point() +
  geom_smooth()

ggplot() +
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
```

</div>

<div class="answer">

No. 
Because both `geom_point()` and `geom_smooth()` will use the same data and mappings.
They will inherit those options from the `ggplot()` object, so the mappings don't need to specified again.


```r
ggplot(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_point() +
  geom_smooth()
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-30-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot() +
  geom_point(data = mpg, mapping = aes(x = displ, y = hwy)) +
  geom_smooth(data = mpg, mapping = aes(x = displ, y = hwy))
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-31-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.6.6</span> {.unnumbered .exercise}

<div class="question">

Recreate the R code necessary to generate the following graphs.

<img src="img/visualize/unnamed-chunk-29-1.png" width="40%" />
<img src="img/visualize/unnamed-chunk-29-2.png" width="40%" />
<img src="img/visualize/unnamed-chunk-29-3.png" width="40%" />
<img src="img/visualize/unnamed-chunk-29-4.png" width="40%" />
<img src="img/visualize/unnamed-chunk-29-5.png" width="40%" />
<img src="img/visualize/unnamed-chunk-29-6.png" width="40%" />


</div>

<div class="answer">


```r
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point() +
  geom_smooth(se = FALSE)
```

<img src="visualize_files/figure-html/unnamed-chunk-38-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_smooth(mapping = aes(group = drv), se = FALSE) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-39-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot(mpg, aes(x = displ, y = hwy, colour = drv)) +
  geom_point() +
  geom_smooth(se = FALSE)
```

<img src="visualize_files/figure-html/unnamed-chunk-40-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = drv)) +
  geom_smooth(se = FALSE)
```

<img src="visualize_files/figure-html/unnamed-chunk-41-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(aes(colour = drv)) +
  geom_smooth(aes(linetype = drv), se = FALSE)
#> `geom_smooth()` using method = 'loess' and formula 'y ~ x'
```

<img src="visualize_files/figure-html/unnamed-chunk-42-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot(mpg, aes(x = displ, y = hwy)) +
  geom_point(size = 4, color = "white") +
  geom_point(aes(colour = drv))
```

<img src="visualize_files/figure-html/unnamed-chunk-43-1.png" width="70%" style="display: block; margin: auto;" />

</div>

## Statistical Transformations

### Exercise <span class="exercise-number">3.1</span> {.unnumbered .exercise}

<div class="question">
What is the default geom associated with `stat_summary()`? How could you rewrite the previous plot to use that geom function instead of the stat function?
</div>

<div class="answer">

The default geom for [`stat_summary()`](https://ggplot2.tidyverse.org/reference/stat_summary.html) is `geom_pointrange()` (see the `stat`) argument.

But, the default `stat` for [`geom_pointrange()`](https://ggplot2.tidyverse.org/reference/geom_linerange.html) is `identity()`, so use `geom_pointrange(stat = "summary")`.

```r
ggplot(data = diamonds) +
  geom_pointrange(
    mapping = aes(x = cut, y = depth),
    stat = "summary",
  )
#> No summary function supplied, defaulting to `mean_se()
```

<img src="visualize_files/figure-html/unnamed-chunk-44-1.png" width="70%" style="display: block; margin: auto;" />

The default message says that `stat_summary()` uses the `mean` and `sd` to calculate the point, and range of the line. So lets use the previous values of `fun.ymin`, `fun.ymax`, and `fun.y`:

```r
ggplot(data = diamonds) +
  geom_pointrange(
    mapping = aes(x = cut, y = depth),
    stat = "summary",
    fun.ymin = min,
    fun.ymax = max,
    fun.y = median
  )
```

<img src="visualize_files/figure-html/unnamed-chunk-45-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.2</span>. {.unnumbered .exercise}

<div class="question">
What does `geom_col()` do? How is it different to `geom_bar()`?
</div>

<div class="answer">

The `geom_col()` function has different default stat than `geom_bar()`.
The default stat of `geom_col()` is `stat_identity()`, which leaves the data as is.
This means that `geom_col()` expects that the data is already preprocessed into `x` values and `y` values which represent the bar height.

The default stat of `geom_bar()` is `stat_bin()`.
This means that `geom_bar()` only expects an `x` variable. 
The stat `stat_bin()` will first preprocess the data by count the number of observations for each value of `x`. 
These counts will be used for `y`.

</div>

### Exercise <span class="exercise-number">3.3</span>. {.unnumbered .exercise}

<div class="question">

Most geoms and stats come in pairs that are almost always used in concert.
Read through the documentation and make a list of all the pairs.
What do they have in common?

</div>

<div class="answer">

The pairs of geoms and stats that are used in concert are listed in the following table.

| geom                | stat                |
|---------------------|---------------------|
| `geom_bar()`        | `stat_count()`      |
| `geom_bin2d()`      | `stat_bin_2d()`     |
| `geom_boxplot()`    | `stat_boxplot()`    |
| `geom_contour()`    | `stat_contour()`    |
| `geom_count()`      | `stat_sum()`        |
| `geom_density()`    | `stat_density()`    |
| `geom_density_2d()` | `stat_density_2d()` |
| `geom_hex()`        | `stat_hex()`        |
| `geom_freqpoly()`   | `stat_bin()`        |
| `geom_histogram()`  | `stat_bin()`        |
| `geom_qq_line()`    | `stat_qq_line()`    |
| `geom_qq()`         | `stat_qq()`         |
| `geom_quantile()`   | `stat_quantile()`   |
| `geom_smooth()`     | `stat_smooth()`     |
| `geom_violin()`     | `stat_violin()`     |
| `geom_sf()`         | `stat_sf()`         |

Table: Complementary geoms and stats

They tend to have their names in common, `stat_smooth()` and `geom_smooth()`.
However, this is not always the case, with `geom_bar()` and `stat_count()` and `geom_histogram()` and `geom_bin()` as notable counter-examples.
Also, the pairs of geoms and stats that are used in concert almost always have each other as the default stat (for a geom) or geom (for a stat).

The following tables contain the geoms and stats in [ggplot2](https://ggplot2.tidyverse.org/reference/).

| geom                | default stat        | shared docs |
|:--------------------|:--------------------|-------------|
| `geom_abline()`     |  ||
| `geom_hline()`      |  ||
| `geom_vline()`      |  ||
| `geom_bar()`        | `stat_count()`      | x |
| `geom_col()`        |  ||
| `geom_bin2d()`      | `stat_bin_2d()`     | x |
| `geom_blank()`      |  ||
| `geom_boxplot()`    | `stat_boxplot()`    | x |
| `geom_countour()`   | `stat_countour()`   | x |
| `geom_count()`      | `stat_sum()`        | x |
| `geom_density()`    | `stat_density()`    | x |
| `geom_density_2d()` | `stat_density_2d()` | x |
| `geom_dotplot()`    | || 
| `geom_errorbarh()`  | ||
| `geom_hex()`        | `stat_hex()`        | x |
| `geom_freqpoly()`   | `stat_bin()`        | x |
| `geom_histogram()`  | `stat_bin()`        | x |
| `geom_crossbar()`   | ||
| `geom_errorbar()`   | ||
| `geom_linerange()`  | ||
| `geom_pointrange()` | ||
| `geom_map()`        | ||
| `geom_point()`      | ||
| `geom_map()`        | ||
| `geom_path()`       | ||
| `geom_line()`       | ||
| `geom_step()`       | ||
| `geom_point()`      | ||
| `geom_polygon()`    | ||
| `geom_qq_line()`    | `stat_qq_line()`    | x |
| `geom_qq()`         | `stat_qq()`         | x |
| `geom_quantile()`   | `stat_quantile()`   | x |
| `geom_ribbon()`     | ||
| `geom_area()`       | ||
| `geom_rug()`        | ||
| `geom_smooth()`     | `stat_smooth()`     | x |
| `geom_spoke()`      | ||
| `geom_label()`      | ||
| `geom_text()`       | ||
| `geom_raster()`     | ||
| `geom_rect()`       | ||
| `geom_tile()`       | ||
| `geom_violin()`     | `stat_ydensity()`   | x |
| `geom_sf()`         | `stat_sf()`         | x |

Table: ggplot2 geom layers and their default stats. 

| stat               | default geom        | shared docs |
|:--------------------|:--------------------|-------------|
| `stat_ecdf()`        | `geom_step()`       |
| `stat_ellipse()`     | `geom_path()`       | 
| `stat_function()`    | `geom_path()`       |
| `stat_identity()`    | `geom_point()`      |
| `stat_summary_2d()`  | `geom_tile()`       |
| `stat_summary_hex()` | `geom_hex()`        |
| `stat_summary_bin()` | `geom_pointrange()` |
| `stat_summary()`     | `geom_pointrange()` |
| `stat_unique()`      | `geom_point()`      | 
| `stat_count()`      | `geom_bar()`  | x |
| `stat_bin_2d()`     | `geom_tile()` | x |
| `stat_boxplot()`    | `geom_boxplot()` | x |
| `stat_countour()`   | `geom_contour()` | x |
| `stat_sum()`        | `geom_point()` | x |
| `stat_density()`    | `geom_area()` | x |
| `stat_density_2d()` | `geom_density_2d()` | x |
| `stat_bin_hex()`    | `geom_hex()` | x |
| `stat_bin()`        | `geom_bar()` |  x |
| `stat_qq_line()`    | `geom_path()` | x |
| `stat_qq()`         | `geom_point()` | x |
| `stat_quantile()`   | `geom_quantile()` | x |
| `stat_smooth()`     | `geom_smooth()` | x |
| `stat_ydensity()`   | `geom_violin()` | x |
| `stat_sf()`         | `geom_rect()` | x |

Table: ggplot2 stat layers and their default geoms.

</div>

### Exercise <span class="exercise-number">3.4</span>. {.unnumbered .exercise}

<div class="question">

What variables does `stat_smooth()` compute?
What parameters control its behavior?

</div>

<div class="answer">

The function `stat_smooth()` calculates the following variables:

-   `y`: predicted value
-   `ymin`: lower value of the confidence interval
-   `ymax`: upper value of the confidence interval
-   `se`: standard error

There's parameters such as `method` which determines which method is used to calculate the predictions and confidence interval, and some other arguments that are passed to that.

</div>

### Exercise <span class="exercise-number">3.5</span>. {.unnumbered .exercise}

<div class="question">

In our proportion bar chart, we need to set `group = 1` Why?
In other words what is the problem with these two graphs?

</div>

<div class="answer">

If `group` is not set to 1, then all the bars have `prop == 1`.
The function `geom_bar()` assumes that the groups are equal to the `x` values, since the stat computes the counts within the group.


```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, y = ..prop..))
```

<img src="visualize_files/figure-html/unnamed-chunk-46-1.png" width="70%" style="display: block; margin: auto;" />

The problem with these two plots is that the proportions are calculated within the groups.

```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, y = ..prop..))

ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, fill = color, y = ..prop..))
```

<img src="visualize_files/figure-html/unnamed-chunk-47-1.png" width="70%" style="display: block; margin: auto;" /><img src="visualize_files/figure-html/unnamed-chunk-47-2.png" width="70%" style="display: block; margin: auto;" />

This is more likely what was intended.

```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, y = ..prop.., group = 1))

ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, fill = color, y = ..prop.., group = color))
```

<img src="visualize_files/figure-html/unnamed-chunk-48-1.png" width="70%" style="display: block; margin: auto;" /><img src="visualize_files/figure-html/unnamed-chunk-48-2.png" width="70%" style="display: block; margin: auto;" />

</div>

## Position Adjustments

### Exercise <span class="exercise-number">3.8.1</span>. {.unnumbered .exercise}

<div class="question">

What is the problem with this plot?
How could you improve it?

</div>

<div class="answer">

There is overplotting because there are multiple observations for each combination of `cty` and `hwy`.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point()
```

<img src="visualize_files/figure-html/unnamed-chunk-49-1.png" width="70%" style="display: block; margin: auto;" />

I would improve the plot by using a jitter position adjustment to decrease overplotting.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point(position = "jitter")
```

<img src="visualize_files/figure-html/unnamed-chunk-50-1.png" width="70%" style="display: block; margin: auto;" />

Jittering the points removes the overplotting and allows for us to see the relationship between the `cty` and `hwy` variables.

</div>

### Exercise <span class="exercise-number">3.8.2</span>. {.unnumbered .exercise}

<div class="question">

What parameters to `geom_jitter()` control the amount of jittering?

</div>

<div class="answer">

From the [`geom_jitter()`](https://ggplot2.tidyverse.org/reference/geom_jitter.html) documentation, there are two arguments to jitter:

-   `width` controls the amount of vertical displacement, and
-   `height` controls the amount of horizontal displacement.

The defaults values of `width` and `height` will introduce noise in both directions.
Here is what the plot looks like with the default values of `height` and `width`.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point(position = position_jitter(width = 0))
```

<img src="visualize_files/figure-html/unnamed-chunk-51-1.png" width="70%" style="display: block; margin: auto;" />

However, we can adjust them. Here are few examples to understand how adjusting
these parameters affects the look of the plot.

With `width = 0` there is no horizontal jitter.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_jitter(width = 0)
```

<img src="visualize_files/figure-html/unnamed-chunk-52-1.png" width="70%" style="display: block; margin: auto;" />

With `width = 20`, there is too much horizontal jitter.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_jitter(width = 20)
```

<img src="visualize_files/figure-html/unnamed-chunk-53-1.png" width="70%" style="display: block; margin: auto;" />

With `height = 0`, there is no vertical or horizontal jitter:


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_jitter(height = 0)
```

<img src="visualize_files/figure-html/unnamed-chunk-54-1.png" width="70%" style="display: block; margin: auto;" />

With `height = 15`, there is too much vertical jitter.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_jitter(height = 15)
```

<img src="visualize_files/figure-html/unnamed-chunk-55-1.png" width="70%" style="display: block; margin: auto;" />

Note that the `height` and `width` arguments are in the units of the data.
Thus `height = 1` (`width = 1`) corresponds to different relative amounts of jittering depending on the scale of the `y` (`x`) variable.
The default values of `height` and `width` are defined to be 80% of the `resolution()` of the data, which is the smallest non-zero distance between adjacent values of a variable. 
When `x` and `y` are discrete variables, 
their resolutions are both equal to 1, and `height = 0.4` and `width = 0.4`,
since the jitter moves points in both positive and negative directions.

</div>

### Exercise <span class="exercise-number">3.8.3</span>. {.unnumbered .exercise}

<div class="question">

Compare and contrast `geom_jitter()` with `geom_count()`.

</div>

<div class="answer">

The geom `geom_jitter()` adds random variation to the locations points of the graph.
In other words, it "jitters" the locations of points slightly.
This method reduces overplotting since two points with the same location are unlikely to have the same random variation.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_jitter()
```

<img src="visualize_files/figure-html/unnamed-chunk-56-1.png" width="70%" style="display: block; margin: auto;" />

However, the reduction in overlapping comes at the cost of slightly changing the `x` and `y` values of the points.

The geom `geom_count()` sizes the points relative to the number of observations.
Combinations of (`x`, `y`) values with more observations will be larger than those with fewer observations.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_count()
```

<img src="visualize_files/figure-html/unnamed-chunk-57-1.png" width="70%" style="display: block; margin: auto;" />

This method does not change the `x` and `y` coordinates of the points.
However, if the points are close together and counts are large, the size of some
points can itself create overplotting.
For example, in the following example a third variable mapped to color is added to the plot. In this case, `geom_count()` is less readable than `geom_jitter()` when adding a third variable as color aesthetic.


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy, color = class)) +
  geom_jitter()
```

<img src="visualize_files/figure-html/unnamed-chunk-58-1.png" width="70%" style="display: block; margin: auto;" />


```r
ggplot(data = mpg, mapping = aes(x = cty, y = hwy, color = class)) +
  geom_count()
```

<img src="visualize_files/figure-html/unnamed-chunk-59-1.png" width="70%" style="display: block; margin: auto;" />

As that example shows, unfortunately there is no universal solution to overplotting. 
The costs and benefits of different approaches will depend on the structure of the data and the goal of the data scientist.

</div>

### Exercise <span class="exercise-number">3.8.4</span>. {.unnumbered .exercise}

<div class="question">

What’s the default position adjustment for `geom_boxplot()`? 
Create a visualization of the `mpg` dataset that demonstrates it.

</div>

<div class="answer">

The default position for `geom_boxplot()` is `"dodge2"`, which is a shortcut for `position_dodge2`.
This does not change the vertical position of a geom, but moves a geom horizontally in order to avoid overlapping another geom.
See the documentation for [`position_dodge2()`](https://ggplot2.tidyverse.org/reference/position_dodge.html) for additional discussion on how it works.

When we add `colour = class` to the box plot, the different classes within `drv` are placed side by side, i.e. dodged. 

```r
ggplot(data = mpg, aes(x = drv, y = hwy, colour = class)) +
  geom_boxplot()
```

<img src="visualize_files/figure-html/unnamed-chunk-60-1.png" width="70%" style="display: block; margin: auto;" />
If `position_identity()` is used the boxplots overlap.

```r
ggplot(data = mpg, aes(x = drv, y = hwy, colour = class)) +
  geom_boxplot(position = "identity")
```

<img src="visualize_files/figure-html/unnamed-chunk-61-1.png" width="70%" style="display: block; margin: auto;" />

</div>

## Coordinate Systems

### Exercise <span class="exercise-number">3.9.1</span> {.unnumbered .exercise}

<div class="question">

Turn a stacked bar chart into a pie chart using `coord_polar()`.

</div>

<div class="answer">

This is a stacked bar chart with a single category.


```r
ggplot(mpg, aes(x = factor(1), fill = drv)) +
  geom_bar()
```

<img src="visualize_files/figure-html/unnamed-chunk-62-1.png" width="70%" style="display: block; margin: auto;" />

See the documentation for [coord_polar](https://ggplot2.tidyverse.org/reference/coord_polar.html) for an example of making a pie chart. 
In particular, `theta = "y"`, meaning that the angle of the chart is the `y` variable which has to be specified.


```r
ggplot(mpg, aes(x = factor(1), fill = drv)) +
  geom_bar(width = 1) +
  coord_polar(theta = "y")
```

<img src="visualize_files/figure-html/unnamed-chunk-63-1.png" width="70%" style="display: block; margin: auto;" />

If `theta = "y"` is not specified, then you get a bull’s-eye chart.


```r
ggplot(mpg, aes(x = factor(1), fill = drv)) +
  geom_bar(width = 1) +
  coord_polar()
```

<img src="visualize_files/figure-html/unnamed-chunk-64-1.png" width="70%" style="display: block; margin: auto;" />

If you had a multiple stacked bar chart, like this,


```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill")
```

<img src="visualize_files/figure-html/unnamed-chunk-65-1.png" width="70%" style="display: block; margin: auto;" />

and apply polar coordinates to it, you end up with a multi-doughnut chart,


```r
ggplot(data = diamonds) +
  geom_bar(mapping = aes(x = cut, fill = clarity), position = "fill") +
  coord_polar(theta = "y")
```

<img src="visualize_files/figure-html/unnamed-chunk-66-1.png" width="70%" style="display: block; margin: auto;" />

</div>

### Exercise <span class="exercise-number">3.9.2</span> {.unnumbered .exercise}

<div class="question">

What does `labs()` do?
Read the documentation.

</div>

<div class="answer">

The `labs` function adds axis titles, plot titles, and a caption to the plot.


```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) +
  geom_boxplot() +
  coord_flip() +
  labs(
    y = "Highway MPG",
    x = "Year",
    title = "Highway MPG by car class",
    subtitle = "1999-2008",
    caption = "Source: http://fueleconomy.gov"
  )
```

<img src="visualize_files/figure-html/unnamed-chunk-67-1.png" width="70%" style="display: block; margin: auto;" />

The arguments to `labs()` are optional, so you can add as many or as few of these as are needed.

```r
ggplot(data = mpg, mapping = aes(x = class, y = hwy)) +
  geom_boxplot() +
  coord_flip() +
  labs(
    y = "Highway MPG",
    x = "Year",
    title = "Highway MPG by car class"
  )
```

<img src="visualize_files/figure-html/unnamed-chunk-68-1.png" width="70%" style="display: block; margin: auto;" />

This is not the only function you can use for adding titles.
You can also add axis labels with `xlab()`, `ylab()`, and the scale functions, and titles with `ggtitle()`.

</div>

### Exercise <span class="exercise-number">3.9.3</span> {.unnumbered .exercise}

<div class="question">

What’s the difference between `coord_quickmap()` and `coord_map()`?

</div>

<div class="answer">

`coord_map()` uses map projection to project 3-dimensional Earth onto a 2-dimensional plane.
By default, `coord_map()` uses the [Mercator projection](https://en.wikipedia.org/wiki/Mercator_projection).
However, this projection must be applied to all geoms in the plot.
`coord_quickmap()` uses a faster, but approximate map projection.
This approximation ignores the curvature of Earth and adjusts the map for the  latitude/longitude ratio.
This transformation is quicker than `coord_map()` because the coordinates of the individual geoms do not need to be transformed.

The **ggplot2** [documentation](https://ggplot2.tidyverse.org/reference/coord_map.html)
contains more information on and examples for these two functions.

</div>

### Exercise <span class="exercise-number">3.9.4</span> {.unnumbered .exercise}

<div class="question">

What does the plot below tell you about the relationship between city and highway mpg? 
Why is `coord_fixed()` important?
What does `geom_abline()` do?

</div>

<div class="answer">

The function `coord_fixed()` ensures that the line produced by `geom_abline()` is at a 45 degree angle.
The 45 degree line makes it easy to compare the highway and city mileage to the case in which city and highway MPG were equal.


```r
p <- ggplot(data = mpg, mapping = aes(x = cty, y = hwy)) +
  geom_point() +
  geom_abline()
p + coord_fixed()
```

<img src="visualize_files/figure-html/unnamed-chunk-69-1.png" width="70%" style="display: block; margin: auto;" />

If we didn't include `geom_coord()`, then the line would no longer have an angle of 45 degrees.

```r
p
```

<img src="visualize_files/figure-html/unnamed-chunk-70-1.png" width="70%" style="display: block; margin: auto;" />

On average, humans are best able to perceive differences in angles relative to 45 degrees.
See @Cleveland1993, @Cleveland1994,@Cleveland1993a, @ClevelandMcGillMcGill1988,  @HeerAgrawala2006 for discussion on how the aspect ratio of a plot affects perception of the values it encodes, evidence that 45 degrees is generally optimal, and methods to calculate the an aspect ratio to achieve it.
The function `ggthemes::bank_slopes()` will calculate the optimal aspect ratio to bank slopes to 45 degrees.

</div>

## The Layered Grammar of Graphics

No exercises
