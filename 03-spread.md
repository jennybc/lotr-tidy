Jenny Bryan
03 October, 2016

Enough about tidy data. How do I make it messy?

No prose yet.

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
lotr_tidy <- read_csv(file.path("data", "lotr_tidy.csv"))
#> Parsed with column specification:
#> cols(
#>   Film = col_character(),
#>   Race = col_character(),
#>   Gender = col_character(),
#>   Words = col_integer()
#> )

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

## practicing with spread: let's get one variable per Race
lotr_tidy %>% 
  spread(key = Race, value = Words)
#> # A tibble: 6 × 5
#>                         Film Gender   Elf Hobbit   Man
#> *                      <chr>  <chr> <int>  <int> <int>
#> 1 The Fellowship Of The Ring Female  1229     14     0
#> 2 The Fellowship Of The Ring   Male   971   3644  1995
#> 3     The Return Of The King Female   183      2   268
#> 4     The Return Of The King   Male   510   2673  2459
#> 5             The Two Towers Female   331      0   401
#> 6             The Two Towers   Male   513   2463  3589

## practicing with spread: let's get one variable per Gender
lotr_tidy %>% 
  spread(key = Gender, value = Words)
#> # A tibble: 9 × 4
#>                         Film   Race Female  Male
#> *                      <chr>  <chr>  <int> <int>
#> 1 The Fellowship Of The Ring    Elf   1229   971
#> 2 The Fellowship Of The Ring Hobbit     14  3644
#> 3 The Fellowship Of The Ring    Man      0  1995
#> 4     The Return Of The King    Elf    183   510
#> 5     The Return Of The King Hobbit      2  2673
#> 6     The Return Of The King    Man    268  2459
#> 7             The Two Towers    Elf    331   513
#> 8             The Two Towers Hobbit      0  2463
#> 9             The Two Towers    Man    401  3589

## practicing with spread ... and unite: let's get one variable per combo of Race and Gender
lotr_tidy %>% 
  unite(Race_Gender, Race, Gender) %>% 
  spread(key = Race_Gender, value = Words)
#> # A tibble: 3 × 7
#>                         Film Elf_Female Elf_Male Hobbit_Female Hobbit_Male
#> *                      <chr>      <int>    <int>         <int>       <int>
#> 1 The Fellowship Of The Ring       1229      971            14        3644
#> 2     The Return Of The King        183      510             2        2673
#> 3             The Two Towers        331      513             0        2463
#> # ... with 2 more variables: Man_Female <int>, Man_Male <int>

## to do: show splitting into film-specific data frames?
```
