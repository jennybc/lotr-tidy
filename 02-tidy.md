02-tidy.Rmd
================
Jenny Bryan
03 October, 2016

An important aspect of "writing data for computers" is to make your data **tidy**. Key features of **tidy** data:

-   Each column is a variable
-   Each row is an observation

But unfortunately, **untidy** data abounds. In fact, we often inflict it on ourselves, because untidy formats are more attractive for data entry or examination. So how do you make **untidy** data **tidy**?

Import untidy Lord of the Rings data
------------------------------------

We now import the untidy data that was presented in the three film-specific word count tables from [the intro](01-intro.md).

I assume that data can be found as three plain text, delimited files, one for each film. How to liberate data from spreadsheets or tables in word processing documents is beyond the scope of this tutorial. We bring the data into data frames or tibbles, one per film, and do some inspection.

``` r
library(tidyverse)
#> Loading tidyverse: ggplot2
#> Loading tidyverse: tibble
#> Loading tidyverse: tidyr
#> Loading tidyverse: readr
#> Loading tidyverse: purrr
#> Loading tidyverse: dplyr
#> Conflicts with tidy packages ----------------------------------------------
#> filter(): dplyr, stats
#> lag():    dplyr, stats
fship <- read_csv(file.path("data", "The_Fellowship_Of_The_Ring.csv"))
#> Parsed with column specification:
#> cols(
#>   Film = col_character(),
#>   Race = col_character(),
#>   Female = col_integer(),
#>   Male = col_integer()
#> )
ttow <- read_csv(file.path("data", "The_Two_Towers.csv"))
#> Parsed with column specification:
#> cols(
#>   Film = col_character(),
#>   Race = col_character(),
#>   Female = col_integer(),
#>   Male = col_integer()
#> )
rking <- read_csv(file.path("data", "The_Return_Of_The_King.csv")) 
#> Parsed with column specification:
#> cols(
#>   Film = col_character(),
#>   Race = col_character(),
#>   Female = col_integer(),
#>   Male = col_integer()
#> )
rking
#> # A tibble: 3 × 4
#>                     Film   Race Female  Male
#>                    <chr>  <chr>  <int> <int>
#> 1 The Return Of The King    Elf    183   510
#> 2 The Return Of The King Hobbit      2  2673
#> 3 The Return Of The King    Man    268  2459
```

Collect untidy Lord of the Rings data into a single data frame
--------------------------------------------------------------

We now have one data frame per film, each with a common set of 4 variables. Step one in tidying this data is to glue them together into one data frame, stacking them up row wise. This is called row binding and we use `dplyr::bind_rows()`.

``` r
lotr_untidy <- bind_rows(fship, ttow, rking)
str(lotr_untidy)
#> Classes 'tbl_df', 'tbl' and 'data.frame':    9 obs. of  4 variables:
#>  $ Film  : chr  "The Fellowship Of The Ring" "The Fellowship Of The Ring" "The Fellowship Of The Ring" "The Two Towers" ...
#>  $ Race  : chr  "Elf" "Hobbit" "Man" "Elf" ...
#>  $ Female: int  1229 14 0 331 0 401 183 2 268
#>  $ Male  : int  971 3644 1995 513 2463 3589 510 2673 2459
lotr_untidy
#> # A tibble: 9 × 4
#>                         Film   Race Female  Male
#>                        <chr>  <chr>  <int> <int>
#> 1 The Fellowship Of The Ring    Elf   1229   971
#> 2 The Fellowship Of The Ring Hobbit     14  3644
#> 3 The Fellowship Of The Ring    Man      0  1995
#> 4             The Two Towers    Elf    331   513
#> 5             The Two Towers Hobbit      0  2463
#> 6             The Two Towers    Man    401  3589
#> 7     The Return Of The King    Elf    183   510
#> 8     The Return Of The King Hobbit      2  2673
#> 9     The Return Of The King    Man    268  2459
```

Assembling one large data object from lots of little ones is common data preparation task. When the pieces are as similar as they here, it's nice to assemble them into one object right away. In other scenarios, you may need to do some remedial work on the pieces before they can be fitted together nicely.

A good guiding principle is to glue the pieces together as early as possible, because it's easier and more efficient to tidy a single object than 20 or 1000.

Tidy the untidy Lord of the Rings data
--------------------------------------

We are still violating one of the fundamental principles of **tidy data**. "Word count" is a fundamental variable in our dataset and it's currently spread out over two variables, `Female` and `Male`. Conceptually, we need to gather up the word counts into a single variable and create a new variable, `Gender`, to track whether each count refers to females or males. We use the `gather()` function from the tidyr package to do this.

``` r
lotr_tidy <-
  gather(lotr_untidy, key = 'Gender', value = 'Words', Female, Male)
lotr_tidy
#> # A tibble: 18 × 4
#>                          Film   Race Gender Words
#>                         <chr>  <chr>  <chr> <int>
#> 1  The Fellowship Of The Ring    Elf Female  1229
#> 2  The Fellowship Of The Ring Hobbit Female    14
#> 3  The Fellowship Of The Ring    Man Female     0
#> 4              The Two Towers    Elf Female   331
#> 5              The Two Towers Hobbit Female     0
#> 6              The Two Towers    Man Female   401
#> 7      The Return Of The King    Elf Female   183
#> 8      The Return Of The King Hobbit Female     2
#> 9      The Return Of The King    Man Female   268
#> 10 The Fellowship Of The Ring    Elf   Male   971
#> 11 The Fellowship Of The Ring Hobbit   Male  3644
#> 12 The Fellowship Of The Ring    Man   Male  1995
#> 13             The Two Towers    Elf   Male   513
#> 14             The Two Towers Hobbit   Male  2463
#> 15             The Two Towers    Man   Male  3589
#> 16     The Return Of The King    Elf   Male   510
#> 17     The Return Of The King Hobbit   Male  2673
#> 18     The Return Of The King    Man   Male  2459
```

Tidy data ... mission accomplished!

To explain our call to `gather()` above, let's read it from right to left: we took the variables `Female` and `Male` and gathered their *values* into a single new variable `Words`. This forced the creation of a companion variable `Gender`, a *key*, which tells whether a specific value of `Words` came from `Female` or `Male`. All other variables, such as `Film`, remain unchanged and are simply replicated as needed. The documentation for `gather()` gives more examples and documents additional arguments.

Write the tidy data to a delimited file
---------------------------------------

Now we write this multi-film, tidy dataset to file for use in various downstream scripts for further analysis and visualization. This would make an excellent file to share on the web with others, providing a tool-agnostic, ready-to-analyze entry point for anyone wishing to play with this data.

``` r
write_csv(lotr_tidy, path = file.path("data", "lotr_tidy.csv"))
```

You can inspect this delimited file here: [lotr\_tidy.csv](data/lotr_tidy.csv).

Exercises
---------

The word count data is given in these two **untidy** and gender-specific files:

-   [Female.csv](data/Female.csv)
-   [Male.csv](data/Male.csv)

Write an R script that reads them in and writes a single tidy data frame to file. Literally, reproduce the `lotr_tidy` data frame and the `lotr_tidy.csv` data file from above.

Write R code to compute the total number of words spoken by each race across the entire trilogy. Do it two ways:

-   Using film-specific or gender-specific, untidy data frames as the input data.
-   Using the `lotr_tidy` data frame as input.

Reflect on the process of writing this code and on the code itself. Which is easier to write? Easier to read?

Write R code to compute the total number of words spoken in each film. Do this by copying and modifying your own code for totalling words by race. Which approach is easier to modify and repurpose -- the one based on multiple, untidy data frames or the tidy data?

Take home message
-----------------

It is untidy to have have data parcelled out across different files or data frames. We used `dplyr::bind_rows()` above to combine film-specific data frames into one large data frame.

It is untidy to have a conceptual variable, e.g. "word count", spread across multiple variables, such as word counts for males and word counts for females. We used the `gather()` function from the tidyr package to stack up all the word counts into a single variable, create a new variable to convey male vs. female, and do the replication needed for the other variables.

Many data analytic projects will benefit from a script that marshals data from different files, tidies the data, and writes a clean result to file for further analysis.

Watch out for how **untidy** data seduces you into working with it more than you should:

-   Data optimized for consumption by human eyeballs *is* attractive, so it's hard to remember it's suboptimal for computation. How can something that looks so pretty be so wrong?
-   Tidy data often has lots of repetition, which triggers hand-wringing about efficiency and aesthetics. Until you can document a performance problem, keep calm and tidy on.
-   Tidying operations are unfamiliar to many of us and we avoid them, subconsciously preferring to faff around with other workarounds that are more familiar.

### Where to next?

In the [optional bonus content](03-tidy-bonus-content.md), I show how to tidy this data using only base R functions. At the other extreme, I also show how to tidy with add-on packages that are capable of more advanced data manipulations.

### Resources

-   [Tidy data](http://r4ds.had.co.nz/tidy-data.html) chapter in R for Data Science, by Garrett Grolemund and Hadley Wickham
    -   [tidyr](https://github.com/hadley/tidyr) R package
    -   The tidyverse meta-package, within which `tidyr` lives: [tidyverse](https://github.com/hadley/tidyversee).
-   [Bad Data Handbook](http://shop.oreilly.com/product/0636920024422.do) by By Q. Ethan McCallum, published by O'Reilly.
    -   Chapter 3: Data Intended for Human Consumption, Not Machine Consumption by Paul Murrell.
-   Nine simple ways to make it easier to (re)use your data by EP White, E Baldridge, ZT Brym, KJ Locey, DJ McGlinn, SR Supp. *Ideas in Ecology and Evolution* 6(2): 1–10, 2013. <doi:10.4033/iee.2013.6b.6.f> <http://library.queensu.ca/ojs/index.php/IEE/article/view/4608>
    -   See the section "Use standard table formats"
-   Tidy data by Hadley Wickham. Journal of Statistical Software. Vol. 59, Issue 10, Sep 2014. <http://www.jstatsoft.org/v59/i10>
