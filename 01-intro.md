Jenny Bryan
03 October, 2016

<blockquote class="twitter-tweet" lang="en">
<p>
If I had one thing to tell biologists learning bioinformatics, it would be "write code for humans, write data for computers".
</p>
— Vince Buffalo (@vsbuffalo) <a href="https://twitter.com/vsbuffalo/statuses/358699162679787521">July 20, 2013</a>
</blockquote>
An important aspect of "writing data for computers" is to make your data **tidy**. Key features of **tidy** data:

-   Each column is a variable
-   Each row is an observation

If you are struggling to make a figure, for example, stop and think hard about whether your data is **tidy**. Untidiness is a common, often overlooked cause of agony in data analysis and visualization.

Lord of the Rings example
-------------------------

I will give you a concrete example of some untidy data I created from [this data from the Lord of the Rings Trilogy](https://github.com/jennybc/lotr).

<table border="1">
<tr>
<td>
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Mon Oct  3 22:43:07 2016 -->
<table border="1">
<caption align="top">
The Fellowship Of The Ring
</caption>
<tr>
<th>
Race
</th>
<th>
Female
</th>
<th>
Male
</th>
</tr>
<tr>
<td>
Elf
</td>
<td align="right">
1229
</td>
<td align="right">
971
</td>
</tr>
<tr>
<td>
Hobbit
</td>
<td align="right">
14
</td>
<td align="right">
3644
</td>
</tr>
<tr>
<td>
Man
</td>
<td align="right">
0
</td>
<td align="right">
1995
</td>
</tr>
</table>
</td>
<td>
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Mon Oct  3 22:43:07 2016 -->
<table border="1">
<caption align="top">
The Two Towers
</caption>
<tr>
<th>
Race
</th>
<th>
Female
</th>
<th>
Male
</th>
</tr>
<tr>
<td>
Elf
</td>
<td align="right">
331
</td>
<td align="right">
513
</td>
</tr>
<tr>
<td>
Hobbit
</td>
<td align="right">
0
</td>
<td align="right">
2463
</td>
</tr>
<tr>
<td>
Man
</td>
<td align="right">
401
</td>
<td align="right">
3589
</td>
</tr>
</table>
</td>
<td>
<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Mon Oct  3 22:43:07 2016 -->
<table border="1">
<caption align="top">
The Return Of The King
</caption>
<tr>
<th>
Race
</th>
<th>
Female
</th>
<th>
Male
</th>
</tr>
<tr>
<td>
Elf
</td>
<td align="right">
183
</td>
<td align="right">
510
</td>
</tr>
<tr>
<td>
Hobbit
</td>
<td align="right">
2
</td>
<td align="right">
2673
</td>
</tr>
<tr>
<td>
Man
</td>
<td align="right">
268
</td>
<td align="right">
2459
</td>
</tr>
</table>
</td>
</tr>
</table>
We have one table per movie. In each table, we have the total number of words spoken, by characters of different races and genders.

You could imagine finding these three tables as separate worksheets in an Excel workbook. Or hanging out in some cells on the side of a worksheet that contains the underlying data raw data. Or as tables on a webpage or in a Word document.

This data has been formatted for consumption by *human eyeballs* (paraphrasing Murrell; see Resources). The format makes it easy for a *human* to look up the number of words spoken by female elves in The Two Towers. But this format actually makes it pretty hard for a *computer* to pull out such counts and, more importantly, to compute on them or graph them.

Exercises
---------

Look at the tables above and answer these questions:

-   What's the total number of words spoken by male hobbits?
-   Does a certain `Race` dominate a movie? Does the dominant `Race` differ across the movies?

How well does your approach scale if there were many more movies or if I provided you with updated data that includes all the `Races` (e.g. dwarves, orcs, etc.)?

Tidy Lord of the Rings data
---------------------------

Here's how the same data looks in tidy form:

<!-- html table generated in R 3.3.1 by xtable 1.8-2 package -->
<!-- Mon Oct  3 22:43:07 2016 -->
<table border="1">
<tr>
<th>
Film
</th>
<th>
Gender
</th>
<th>
Race
</th>
<th>
Words
</th>
</tr>
<tr>
<td>
The Fellowship Of The Ring
</td>
<td>
Female
</td>
<td>
Elf
</td>
<td align="right">
1229
</td>
</tr>
<tr>
<td>
The Fellowship Of The Ring
</td>
<td>
Male
</td>
<td>
Elf
</td>
<td align="right">
971
</td>
</tr>
<tr>
<td>
The Fellowship Of The Ring
</td>
<td>
Female
</td>
<td>
Hobbit
</td>
<td align="right">
14
</td>
</tr>
<tr>
<td>
The Fellowship Of The Ring
</td>
<td>
Male
</td>
<td>
Hobbit
</td>
<td align="right">
3644
</td>
</tr>
<tr>
<td>
The Fellowship Of The Ring
</td>
<td>
Female
</td>
<td>
Man
</td>
<td align="right">
0
</td>
</tr>
<tr>
<td>
The Fellowship Of The Ring
</td>
<td>
Male
</td>
<td>
Man
</td>
<td align="right">
1995
</td>
</tr>
<tr>
<td>
The Two Towers
</td>
<td>
Female
</td>
<td>
Elf
</td>
<td align="right">
331
</td>
</tr>
<tr>
<td>
The Two Towers
</td>
<td>
Male
</td>
<td>
Elf
</td>
<td align="right">
513
</td>
</tr>
<tr>
<td>
The Two Towers
</td>
<td>
Female
</td>
<td>
Hobbit
</td>
<td align="right">
0
</td>
</tr>
<tr>
<td>
The Two Towers
</td>
<td>
Male
</td>
<td>
Hobbit
</td>
<td align="right">
2463
</td>
</tr>
<tr>
<td>
The Two Towers
</td>
<td>
Female
</td>
<td>
Man
</td>
<td align="right">
401
</td>
</tr>
<tr>
<td>
The Two Towers
</td>
<td>
Male
</td>
<td>
Man
</td>
<td align="right">
3589
</td>
</tr>
<tr>
<td>
The Return Of The King
</td>
<td>
Female
</td>
<td>
Elf
</td>
<td align="right">
183
</td>
</tr>
<tr>
<td>
The Return Of The King
</td>
<td>
Male
</td>
<td>
Elf
</td>
<td align="right">
510
</td>
</tr>
<tr>
<td>
The Return Of The King
</td>
<td>
Female
</td>
<td>
Hobbit
</td>
<td align="right">
2
</td>
</tr>
<tr>
<td>
The Return Of The King
</td>
<td>
Male
</td>
<td>
Hobbit
</td>
<td align="right">
2673
</td>
</tr>
<tr>
<td>
The Return Of The King
</td>
<td>
Female
</td>
<td>
Man
</td>
<td align="right">
268
</td>
</tr>
<tr>
<td>
The Return Of The King
</td>
<td>
Male
</td>
<td>
Man
</td>
<td align="right">
2459
</td>
</tr>
</table>
Notice that tidy data is generally taller and narrower. It doesn't fit nicely on the page. Certain elements get repeated alot, e.g. `Hobbit`. For these reasons, we often instinctively resist **tidy** data as inefficient or ugly. But, unless and until you're making the final product for a textual presentation of data, ignore your yearning to see the data in a compact form.

Benefits of tidy data
---------------------

With the data in tidy form, it's natural to *get a computer* to do further summarization or to make a figure. This assumes you're using language that is "data-aware", which R certainly is. Let's answer the questions posed above.

#### What's the total number of words spoken by male hobbits?

``` r
lotr_tidy %>% 
  count(Gender, Race, wt = Words)
#> Source: local data frame [6 x 3]
#> Groups: Gender [?]
#> 
#>   Gender   Race     n
#>    <chr>  <chr> <int>
#> 1 Female    Elf  1743
#> 2 Female Hobbit    16
#> 3 Female    Man   669
#> 4   Male    Elf  1994
#> 5   Male Hobbit  8780
#> 6   Male    Man  8043
## outside the tidyverse:
#aggregate(Words ~ Gender, data = lotr_tidy, FUN = sum)
```

Now it takes a small bit of code to compute the word total for both genders of all races across all films. The total number of words spoken by male hobbits is 8780. It was important here to have all word counts in a single variable, within a data frame that also included a variables for gender and race.

#### Does a certain race dominate a movie? Does the dominant race differ across the movies?

First, we sum across gender, to obtain word counts for the different races by movie.

``` r
(by_race_film <- lotr_tidy %>% 
   group_by(Film, Race) %>% 
   summarize(Words = sum(Words)))
#> Source: local data frame [9 x 3]
#> Groups: Film [?]
#> 
#>                         Film   Race Words
#>                       <fctr>  <chr> <int>
#> 1 The Fellowship Of The Ring    Elf  2200
#> 2 The Fellowship Of The Ring Hobbit  3658
#> 3 The Fellowship Of The Ring    Man  1995
#> 4             The Two Towers    Elf   844
#> 5             The Two Towers Hobbit  2463
#> 6             The Two Towers    Man  3990
#> 7     The Return Of The King    Elf   693
#> 8     The Return Of The King Hobbit  2675
#> 9     The Return Of The King    Man  2727
## outside the tidyverse:
#(by_race_film <- aggregate(Words ~ Race * Film, data = lotr_tidy, FUN = sum))
```

We can stare hard at those numbers to answer the question. But even nicer is to depict the word counts we just computed in a barchart.

``` r
p <- ggplot(by_race_film, aes(x = Film, y = Words, fill = Race))
p + geom_bar(stat = "identity", position = "dodge") +
  coord_flip() + guides(fill = guide_legend(reverse = TRUE))
```

![](01-intro_files/figure-markdown_github/barchart-lotr-words-by-film-race-1.png)

Hobbits are featured heavily in The Fellowhip of the Ring, where as Men had a lot more screen time in The Two Towers. They were equally prominent in the last movie, The Return of the King.

Again, it was important to have all the data in a single data frame, all word counts in a single variable, and associated variables for Film and Race.

Take home message
-----------------

Having the data in **tidy** form was a key enabler for our data aggregations and visualization.

Tidy data is integral to efficient data analysis and visualization.

If you're skeptical about any of the above claims, it would be interesting to get the requested word counts, the barchart, or the insight gained from the chart *without* tidying or plotting the data. And imagine redoing all of that on the full dataset, which includes 3 more Races, e.g. Dwarves.

### Where to next?

In [the next lesson](02-tidy.md), we'll show how to tidy this data.

Our summing over gender to get word counts for combinations of film and race is an example of **data aggregation**. It's a frequent companion task with tidying and reshaping. Learn more at:

-   Simple aggregation with the tidyverse: `dplyr::count()` and `dplyr::group_by()` + `dplyr::summarize()`, [STAT 545 coverage](http://stat545.com/block010_dplyr-end-single-table.html#group_by-is-a-mighty-weapon), [Data transformation](http://r4ds.had.co.nz/transform.html) chapter in R for Data Science.
-   General aggregation with the tidyverse: [STAT 545 coverage](http://stat545.com/block024_group-nest-split-map.html) of general Split-Apply-Combine via nested data frames.
-   Simple aggregation with base R: `aggregate()`.
-   General aggregation with base R: `tapply()`, `split()`, `by()`, etc.

The figure was made with ggplot2, a popular package that implements the Grammar of Graphics in R.

### Resources

-   [Tidy data](http://r4ds.had.co.nz/tidy-data.html) chapter in R for Data Science, by Garrett Grolemund and Hadley Wickham
    -   [tidyr](https://github.com/hadley/tidyr) R package
    -   The tidyverse meta-package, within which `tidyr` lives: [tidyverse](https://github.com/hadley/tidyverse).
-   [Bad Data Handbook](http://shop.oreilly.com/product/0636920024422.do) by By Q. Ethan McCallum, published by O'Reilly.
    -   Chapter 3: Data Intended for Human Consumption, Not Machine Consumption by Paul Murrell.
-   Nine simple ways to make it easier to (re)use your data by EP White, E Baldridge, ZT Brym, KJ Locey, DJ McGlinn, SR Supp. *Ideas in Ecology and Evolution* 6(2): 1–10, 2013. <doi:10.4033/iee.2013.6b.6.f> <http://library.queensu.ca/ojs/index.php/IEE/article/view/4608>
    -   See the section "Use standard table formats"
-   Tidy data by Hadley Wickham. Journal of Statistical Software. Vol. 59, Issue 10, Sep 2014. <http://www.jstatsoft.org/v59/i10>
