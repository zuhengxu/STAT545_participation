---
title: "cm006 Exercises: Exploring Geometric Objects"
output: 
  html_document:
       keep_md: true
editor_options: 
  chunk_output_type: inline
---

In this worksheet, we'll be exploring various plot types (i.e., geometric objects), only using the `x` and `y` aesthetics (and `group`).

1. To get started, load the `tidyverse` and `gapminder` R packages. 

```r
library(tidyverse)
```

```
## ── Attaching packages ─────────────────────────────────────────────────── tidyverse 1.2.1 ──
```

```
## ✔ ggplot2 3.0.0     ✔ purrr   0.2.5
## ✔ tibble  1.4.2     ✔ dplyr   0.7.6
## ✔ tidyr   0.8.1     ✔ stringr 1.3.1
## ✔ readr   1.1.1     ✔ forcats 0.3.0
```

```
## ── Conflicts ────────────────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
```

```r
library(gapminder)
```


## Scatterplot

Let's look at a _scatterplot_ of `gdpPercap` vs. `lifeExp`. 

1. Fill out the grammar components below. Again, bold _must_ be specified to make a `ggplot2` plot.
    - We'll ignore "coordinate system" and "facetting" after this.

| Grammar Component     | Specification |
|-----------------------|---------------|
| __data__              | `gapminder` |
| __aesthetic mapping__ |  `x` and `y`|
| __geometric object__  | points |
| scale                 | linear |
| statistical transform | none |
| coordinate system     | rectangular |
| facetting             |  none|

2. Populate the data and aesthetic mapping in `ggplot`. What is returned? What's missing?

```r
ggplot(gapminder,aes(x=lifeExp,y=gdpPercap)) + 
  geom_point() 
```

![](cm006-exercise_files/figure-html/unnamed-chunk-2-1.png)<!-- -->


3. Add the missing component as a _layer_.

```r
ggplot(gapminder) +
  geom_point(aes(x=lifeExp,y=gdpPercap))
```

![](cm006-exercise_files/figure-html/unnamed-chunk-3-1.png)<!-- -->


Notice the "metaprogramming" again!

4. You _must_ remember to put the aesthetic mappings in the `aes` function! What happens if you forget?
 dont run


5. Put the x-axis on a log scale, first by transforming the x variable. 
    - Note: `ggplot2` does some data wrangling and computations itself! We don't always have to modify the data frame.


```r
ggplot(gapminder,aes(x=lifeExp,y=gdpPercap)) +
  geom_point()+
  scale_x_log10()
```

![](cm006-exercise_files/figure-html/unnamed-chunk-4-1.png)<!-- -->

6. Try again, this time by changing the _scale_ (this way is better).

```r
ggplot(gapminder,aes(lifeExp,gdpPercap))+
  geom_point()+
  scale_y_log10()
```

![](cm006-exercise_files/figure-html/unnamed-chunk-5-1.png)<!-- -->


7. The aesthetic mappings can be specified on the geom layer if you want, instead of the main `ggplot` call. Give it a try:

```r
ggplot(gapminder)+
  geom_point(aes(lifeExp,gdpPercap))
```

![](cm006-exercise_files/figure-html/unnamed-chunk-6-1.png)<!-- -->




8. Optional: git stage and commit

__Uses of a scatterplot__: 

- Visualize 2-dimensional distributions; dependence.
- 2 numeric variables

## Histograms, and Kernel Density Plots

Let's build a histogram of life expectancy.

1. Fill out the grammar components below. Again, bold _must_ be specified to make a `ggplot2` plot.

| Grammar Component     | Specification |
|-----------------------|---------------|
| __data__              | `gapminder` |
| __aesthetic mapping__ | `x` |
| __geometric object__  | histogram |
| scale                 | linear |
| statistical transform | none |

2. Build the histogram of life expectancy.

```r
ggplot(gapminder,aes(lifeExp)) +
  geom_histogram()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](cm006-exercise_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

3. Change the number of bins to 50.

```r
ggplot(gapminder,aes(lifeExp)) +
  geom_histogram(bins= 100) #bins= number of bins/gaps
```

![](cm006-exercise_files/figure-html/unnamed-chunk-8-1.png)<!-- -->

4. Instead of a histogram, let's create a kernel density plot.

```r
ggplot(gapminder,aes(lifeExp)) +
  geom_density(bw=0.5,fill= "orange" ) #larger binwidth more smooth
```

![](cm006-exercise_files/figure-html/unnamed-chunk-9-1.png)<!-- -->


```r
ggplot(gapminder,aes(lifeExp)) +
  geom_histogram(aes(y=..density..)) +
  geom_density()
```

```
## `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
```

![](cm006-exercise_files/figure-html/unnamed-chunk-10-1.png)<!-- -->




5. Optional: git stage and commit

__Uses of a histogram__: Explore the distribution of a single numeric variable.


## Box plots, and violin plots

Let's make _box plots_ of population for each continent. Note: y-axis is much better on a log scale!

1. Fill out the grammar components below. Again, bold _must_ be specified to make a `ggplot2` plot.

| Grammar Component     | Specification |
|-----------------------|---------------|
| __data__              | `gapminder` |
| __aesthetic mapping__ | `x` and `y` |
| __geometric object__  | boxplot |
| scale                 | log-y |
| statistical transform | 5-number summary |

2. Initiate the `ggplot` call, with the log y scale, and store it in the variable `a`. Print out `a`.

```r
#store first layer 

a=ggplot(gapminder,aes(continent, pop)) +
    scale_y_log10()
a 
```

![](cm006-exercise_files/figure-html/unnamed-chunk-11-1.png)<!-- -->



3. Add the boxplot geom to `a`.

```r
a + geom_boxplot()
```

![](cm006-exercise_files/figure-html/unnamed-chunk-12-1.png)<!-- -->



4. A violin plot is a kernel density on its side, made symmetric. Add that geom to `a`.
    - What's better here, boxplots or violin plots? Why?

```r
  a+geom_violin()
```

![](cm006-exercise_files/figure-html/unnamed-chunk-13-1.png)<!-- -->

5. Optional: git stage and commit

__Use of boxplot__: Visualize 1-dimensional distributions (of a single numeric variable).

## Jitter plots

Let's work up to the concept of a _jitter plot_. As above, let's explore the population for each continent, but using points (again, with the y-axis on a log scale).

Let's hold off on identifying the grammar. 

1. Initiate the `ggplot` call to make a scatterplot of `continent` vs `pop`; initiate the log y scale. Store the call in the variable `b`.

```r
a + geom_point(alpha=0.1)
```

![](cm006-exercise_files/figure-html/unnamed-chunk-14-1.png)<!-- -->



2. Add the point geom to `b`. Why is this an ineffective plot?


```r
b= geom_point(alpha=0.1)
a + b
```

![](cm006-exercise_files/figure-html/unnamed-chunk-15-1.png)<!-- -->


3. A solution is to jitter the points. Add the jitter geom. Re-run the command a few times -- does the plot change? Why?

```r
a + geom_jitter(alpha=0.25)
```

![](cm006-exercise_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

```r
c = geom_jitter(alpha=0.25)
```


4. How does the grammar differ from a box plot or violin plot?
    - ANSWER: 

5. We can add multiple geom _layers_ to our plot. Put a jitterplot overtop of the violin plot, starting with our base `b`. Try vice-versa. 


```r
a+geom_violin()+
  c
```

![](cm006-exercise_files/figure-html/unnamed-chunk-17-1.png)<!-- -->


6. Optional: git stage and commit

__Uses of jitterplot__: Visualize 1-dimensional distributions, AND get a sense of the sample size.

## Time/Line Plots

Let's make some time/line plot, starting with Canada's life expectancy over time.

1. Fill out the grammar components below. Again, bold _must_ be specified to make a `ggplot2` plot.

| Grammar Component     | Specification |
|-----------------------|---------------|
| __data__              | `gapminder` |
| __aesthetic mapping__ |  |
| __geometric object__  |  |
| scale                 |  |
| statistical transform |  |

2. In one readable call, write code that:
    1. Filters the data to Canada only
    2. Pipes the filtered data into `ggplot`
    3. Makes the time plot of `lifeExp` over time
    4. Also displays the points

```r
gapminder %>% 
  filter(country =="Canada") %>% 
  ggplot(aes(year,lifeExp)) + #you cnnot use %>% 
  geom_line() + #connect dots always left to right
  geom_point()
```

![](cm006-exercise_files/figure-html/unnamed-chunk-18-1.png)<!-- -->



3. Attempt to overlay line plots for all countries. That is, repeat the above code, but don't filter. What's wrong here?


```r
h = ggplot(gapminder,aes(year, lifeExp)) 
h + geom_line()
```

![](cm006-exercise_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

```r
h + geom_line(aes(group=country),alpha=0.2)
```

![](cm006-exercise_files/figure-html/unnamed-chunk-19-2.png)<!-- -->



4. Use the `group` aesthetic to fix the problem.



5. Optional: git stage and commit

__Uses of time/line plots__: Visualize trends of a numeric variable over time.

## Path plots

Let's see how Rwanda's life expectancy and GDP per capita have evolved over time, using a path plot.

1. Make a scatterplot. Store it in the variable `c`. 



2. We want to connect the dots from earliest point to latest. What happens if we add the "line" geom to `c`?



3. Add the appropriate geom to `c`. In that geom, specify a property of the geom: `arrow=arrow()`. 


4. Optional: git stage and commit

__Uses of path plots__: The four "corners" of the plot usually indicate different qualities. This plot allows you to see how Rwanda (or some entity) evolves over these qualities.

## Bar plots

How many countries are in each continent? Use the year 2007.

1. Fill out the grammar components below. Again, bold _must_ be specified to make a `ggplot2` plot.

| Grammar Component     | Specification |
|-----------------------|---------------|
| __data__              | `gapminder` |
| __aesthetic mapping__ |  |
| __geometric object__  |  |
| scale                 |  |
| statistical transform |  |

2. After filtering the gapminder data to 2007, make a bar chart of the number of countries in each continent. Store everything except the geom in the variable `d`.



3. Notice the y-axis. Oddly, `ggplot2` doesn't make it obvious how to change to proportion. Try adding a `y` aesthetic: `y=..count../sum(..count..)`.



4. Optional: git stage, commit, and push!

__Uses of bar plots__: Get a sense of relative quantities of categories, or see the probability mass function of a categorical random variable.
