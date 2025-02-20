Rawan’s CSSS508 notes for R programming
================
Rawan Alaseeri
2025-02-20

A lot of the things in this document are links and things I found useful
while learning R programming, mainly from
[CSSS508](https://clanfear.github.io/CSSS508/), thanks Chuck!

It is very important through learning R is to annotate your code with
commenting and with R markdown..

## R Markdown

``` r
# dont show code output -> echo=FALSE
# shortcut for creating a chunk -> ctrl+Alt+I
```

Links:

1.  [Introduction to R
    Markdown](https://rmarkdown.rstudio.com/lesson-1.html)

2.  [R Markdown: The Definitive
    Guide](https://bookdown.org/yihui/rmarkdown/)

3.  [Cheat Sheets](https://posit.co/resources/cheatsheets/)

4.  [R Colors
    Sheet](https://sites.stat.columbia.edu/tzheng/files/Rcolor.pdf)

5.  [PrettyDoc](https://prettydoc.statr.me/index.html) for R markdown
    themes

6.  [Posit R Markdown Cheat
    Sheet](https://rstudio.github.io/cheatsheets/html/rmarkdown.html)

7.  [R Studio IDE Cheat
    Sheet](https://rstudio.github.io/cheatsheets/html/rstudio-ide.html)

### Couple more housekeeping things

``` r
# save R objects using 
save(new.object, file="new_object.RData")
# then you can open it using
load(new_object.RData)
# find working directory with 
getwd() # mine is in downloads which is probably not the best but eh
# to change the working directory 
# setwd("") #my computer copies the file path as \ so I should change it to /

# install and load packages
install.packages()
library()
```

Note to myself: for the love of God, manage your files and set folders
and a consistent folder. Also, a good way to manage files in R studio is
to create a project (eg. Thesis, Learning R, etc). this is nice because
it also changes your working directory if you need to

`.Rmd` is used to produce **document**

`.R` is just for running things

`.html` are output docs

### Subsetting

``` r
# check the structure of data using
str()
# check all catagories in some data
unique()
```

### `dplyr`

you cane use the `magritter` opration to pip data between functions.
magritter allows you to read or write the code in a more intuitive way
(from left to right instead from inside out)

``` r
# and then %>%
```

filter data frames. For example:

``` r
data %>% filter(Country == "Algeria")
# this reads as: from data, filter the data where the country is Algeria
# use == instread of = 
```

More about [operators](https://www.datacamp.com/doc/r/operators)

For example:

- `!=`: not equal to

- `>`, `>=`, `<`, `<=`: less than, less than or equal to, etc.

- `%in%`: used with checking equal to one of several values

–

Or we can combine multiple logical conditions:

- `&`: both conditions need to hold (AND)

- `|`: at least one condition needs to hold (OR)

- `!`: inverts a logical condition (`TRUE` becomes `FALSE`, `FALSE`
  becomes `TRUE`)

## Visualising Data with `ggplot2`

`ggplot2` graphics objects consist of two primary components:

1.  **Layers**, the components of a graph.

    - We *add* layers to a `ggplot2` object using `+`.
    - This includes lines, shapes, and text.

2.  **Aesthetics**, which determine how the layers appear.

    - We *set* aesthetics using *arguments* (e.g. `color="red"`) inside
      layer functions.
    - This includes locations, colors, and sizes.
    - Aesthetics also determine how data *map* to appearances.

**Layers** are the components of the graph, such as:

- `ggplot()`: initializes `ggplot2` object, specifies input data
- `geom_point()`: layer of scatterplot points
- `geom_line()`: layer of lines
- `ggtitle()`, `xlab()`, `ylab()`: layers of labels
- `facet_wrap()`: layer creating separate panels stratified by some
  factor wrapping around
- `facet_grid()`: same idea, but can split by two variables along rows
  and columns (e.g. `facet_grid(gender ~ age_group)`)
- `theme_bw()`: replace default gray background with black-and-white

Layers are separated by a `+` sign. For clarity, I usually put each
layer on a new line, unless it takes few or no arguments (e.g. `xlab()`,
`ylab()`, `theme_bw()`).

**Aesthetics** control the appearance of the layers:

- `x`, `y`: $x$ and $y$ coordinate values to use

- `color`: set color of elements based on some data value

- `group`: describe which points are conceptually grouped together for
  the plot (often used with lines)

- `size`: set size of points/lines based on some data value

- `alpha`: set transparency based on some data value

- Arguments inside `aes()` (**mapping aesthetics**) will *depend on the
  data*, e.g. `geom_point(aes(color = continent))`.

Example of a plot syntax:

``` r
lifeExp_by_year <- 
  ggplot(data = gapminder, 
       aes(x = year, y = lifeExp, 
           group = country, 
           color = continent)) +
  geom_line() +
  xlab("Year") + 
  ylab("Life expectancy") +
  ggtitle("Life expectancy over time") +
  theme_bw() + 
  facet_wrap(~ continent) +
  theme(legend.position = "none")
```

this will save the plot into lifeExp_by_year in case i need to use it
again.. this saves the code of the plot which helps you add and change
things in the code for the plot

### Scales for Color, Shape, etc.

**Scales** are layers that control how the mapped aesthetics appear. You
can modify these with a `scale_[aesthetic]_[option]()` layer where
`[aesthetic]` is `color`, `shape`, `linetype`, `alpha`, `size`, `fill`,
etc. and `[option]` is something like `manual`, `continuous` or
`discrete` (depending on nature of the variable).

Examples:

- `scale_linetype_manual()`: manually specify the linetype for each
  different value

- `scale_alpha_continuous()`: varies transparency over a continuous
  range

- `scale_color_brewer(palette = "Spectral")`: uses a palette from
  <http://colorbrewer2.org> (great site for picking nice plot colors!)

### Saving `ggplot` Plots

When you knit an R Markdown file, any plots you make are automatically
saved in the “figure” folder in `.png` format. If you want to save
another copy (perhaps of a different file type for use in a manuscript),
use `ggsave()`:

``` r
ggsave("I_saved_a_file.pdf", plot = lifeExp_by_year,
       height = 3, width = 5, units = "in")
```

If you didn’t manually set font sizes, these will usually come out at a
reasonable size given the dimensions of your output file.
