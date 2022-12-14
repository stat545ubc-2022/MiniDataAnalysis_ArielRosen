Mini Data Analysis Milestone 2
================
Ariel Rosen
October 28, 2022

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.3      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
#Taking a look at my data: 
head(apt_buildings)
```

    ## # A tibble: 6 × 37
    ##      id air_co…¹ ameni…² balco…³ barri…⁴ bike_…⁵ exter…⁶ fire_…⁷ garba…⁸ heati…⁹
    ##   <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ## 1 10359 NONE     Outdoo… YES     YES     0 indo… NO      YES     YES     HOT WA…
    ## 2 10360 NONE     Outdoo… YES     NO      0 indo… NO      YES     YES     HOT WA…
    ## 3 10361 NONE     <NA>    YES     NO      Not Av… NO      YES     NO      HOT WA…
    ## 4 10362 NONE     <NA>    YES     YES     Not Av… YES     YES     NO      HOT WA…
    ## 5 10363 NONE     <NA>    NO      NO      12 ind… NO      YES     NO      HOT WA…
    ## 6 10364 NONE     <NA>    NO      NO      Not Av… <NA>    YES     NO      HOT WA…
    ## # … with 27 more variables: intercom <chr>, laundry_room <chr>,
    ## #   locker_or_storage_room <chr>, no_of_elevators <dbl>, parking_type <chr>,
    ## #   pets_allowed <chr>, prop_management_company_name <chr>,
    ## #   property_type <chr>, rsn <dbl>, separate_gas_meters <chr>,
    ## #   separate_hydro_meters <chr>, separate_water_meters <chr>,
    ## #   site_address <chr>, sprinkler_system <chr>, visitor_parking <chr>,
    ## #   ward <chr>, window_type <chr>, year_built <dbl>, year_registered <dbl>, …

Taking a look at the dataset, I see that it is not completely tidy. I
chose to look at the first 8 variables, from “id” to “fire_alarm”. In
these 8 variables, all the columns are variables, and each row is an
observation. However, some of the cells include more than one variable.
For example, in the “bike_parking” variable, the cells have a
description of the both the indoor and the outdoor parking spots in the
same cell, or describes bike parking as being unavailable.

<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

``` r
#I want to tidy the first 8 variables of the data set by splitting the "bike_parking" variable into 2, one for outdoor bike parking and one for indoor bike parking. 

tidy_bike_parking <- apt_buildings %>%
  separate(col = "bike_parking", 
           into = c("indoor_bike_parking", "outdoor_bike_parking"), 
           sep = "and")
```

    ## Warning: Expected 2 pieces. Missing pieces filled with `NA` in 2260 rows [3, 4,
    ## 6, 7, 8, 10, 11, 12, 13, 15, 16, 17, 18, 19, 21, 22, 23, 25, 26, 27, ...].

``` r
head(tidy_bike_parking)
```

    ## # A tibble: 6 × 38
    ##      id air_co…¹ ameni…² balco…³ barri…⁴ indoo…⁵ outdo…⁶ exter…⁷ fire_…⁸ garba…⁹
    ##   <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ## 1 10359 NONE     Outdoo… YES     YES     "0 ind… " 10 o… NO      YES     YES    
    ## 2 10360 NONE     Outdoo… YES     NO      "0 ind… " 34 o… NO      YES     YES    
    ## 3 10361 NONE     <NA>    YES     NO      "Not A…  <NA>   NO      YES     NO     
    ## 4 10362 NONE     <NA>    YES     YES     "Not A…  <NA>   YES     YES     NO     
    ## 5 10363 NONE     <NA>    NO      NO      "12 in… " 0 ou… NO      YES     NO     
    ## 6 10364 NONE     <NA>    NO      NO      "Not A…  <NA>   <NA>    YES     NO     
    ## # … with 28 more variables: heating_type <chr>, intercom <chr>,
    ## #   laundry_room <chr>, locker_or_storage_room <chr>, no_of_elevators <dbl>,
    ## #   parking_type <chr>, pets_allowed <chr>, prop_management_company_name <chr>,
    ## #   property_type <chr>, rsn <dbl>, separate_gas_meters <chr>,
    ## #   separate_hydro_meters <chr>, separate_water_meters <chr>,
    ## #   site_address <chr>, sprinkler_system <chr>, visitor_parking <chr>,
    ## #   ward <chr>, window_type <chr>, year_built <dbl>, year_registered <dbl>, …

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *How has the way Toronto apartment buildings are heated changed from
    1805 to today?*
2.  *How has accessibility changed in apartment buildings in Toronto
    from 1805 to today?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

These two research questions present data frames that I am interested in
further exploring. They both explore different variables over time
(years), which I think will be useful for this assignment. Furthermore,
I am interested in tidying some data to answer these particular research
questions because I think they produced interesting plots in the last
Milestone that I would like to pursue further.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

**1.How has the way Toronto apartment buildings are heated changed from
1805 to today?**

``` r
tidy_heating_data <- apt_buildings %>%
  arrange(year_built) %>% #arrange in ascending order of year built
  select(id, heating_type, year_built) %>% #dropping irrelevant columns 
  pivot_wider(names_from = "heating_type", 
              values_from = "year_built") #tidy'ing to observe the years each heating system was put in 

(tidy_heating_data)
```

    ## # A tibble: 3,455 × 5
    ##       id `HOT WATER` ELECTRIC `FORCED AIR GAS`  `NA`
    ##    <dbl>       <dbl>    <dbl>            <dbl> <dbl>
    ##  1 12064        1805       NA               NA    NA
    ##  2 10424        1809       NA               NA    NA
    ##  3 10906          NA     1838               NA    NA
    ##  4 12869        1880       NA               NA    NA
    ##  5 10899        1885       NA               NA    NA
    ##  6 10905          NA     1885               NA    NA
    ##  7 10437        1888       NA               NA    NA
    ##  8 12662        1890       NA               NA    NA
    ##  9 13409        1891       NA               NA    NA
    ## 10 12473        1895       NA               NA    NA
    ## # … with 3,445 more rows

**How has accessibility changed in apartment buildings in Toronto from
1805 to today?**

``` r
tidy_accessibility_data <- apt_buildings %>%
  select(id, year_built, barrier_free_accessibilty_entr, no_of_accessible_parking_spaces, no_of_units,  no_barrier_free_accessible_units)%>% #dropping irrelevant data 
  mutate(percent_accessible_units = (no_barrier_free_accessible_units/no_of_units) * 100)%>%
                     #adding a column for the percent of accessible units out of the total number of units
  arrange(year_built)#arrange the data in ascending order of year built

(tidy_accessibility_data)
```

    ## # A tibble: 3,455 × 7
    ##       id year_built barrier_free_accessibilty_…¹ no_of…² no_of…³ no_ba…⁴ perce…⁵
    ##    <dbl>      <dbl> <chr>                          <dbl>   <dbl>   <dbl>   <dbl>
    ##  1 12064       1805 NO                                 0      69       0    0   
    ##  2 10424       1809 NO                                 4      10       0    0   
    ##  3 10906       1838 YES                                0      10       2   20   
    ##  4 12869       1880 NO                                 6      13       0    0   
    ##  5 10899       1885 YES                                0      28       2    7.14
    ##  6 10905       1885 NO                                 0      19       0    0   
    ##  7 10437       1888 NO                                 0      16       0    0   
    ##  8 12662       1890 NO                                 0      16       0    0   
    ##  9 13409       1891 NO                                 0      16       0    0   
    ## 10 12473       1895 NO                                 0      11       0    0   
    ## # … with 3,445 more rows, and abbreviated variable names
    ## #   ¹​barrier_free_accessibilty_entr, ²​no_of_accessible_parking_spaces,
    ## #   ³​no_of_units, ⁴​no_barrier_free_accessible_units, ⁵​percent_accessible_units

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

``` r
#Basing my plot on accessibility_over_time from Milestone 1: 
accessibility_over_time <- apt_buildings %>%
                    ggplot(aes(x = year_built, y = no_barrier_free_accessible_units))+ 
                    geom_point(alpha = 0.5, aes(color = barrier_free_accessibilty_entr))+ 
                    ggtitle("Accessibility features over time")+
                    ylab("Number of barrier free accessible units")+
                    xlab("Year built")
print(accessibility_over_time)
```

    ## Warning: Removed 155 rows containing missing values (geom_point).

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

``` r
#Creating factors to work with in the data set and plotting it the same way as above 
year_levels <- tidy_accessibility_data %>%
mutate(time_period = fct(case_when(year_built < 1850 ~ "1800-1850", 
                                   year_built < 1900 ~ "1850-1900", 
                                   year_built < 1950 ~ "1900-1950", 
                                   year_built < 2000 ~ "1950-2000",
                                   year_built < 2050 ~ "2000 to today"), 
  levels = c('1800-1850', '1850-1900','1900-1950', '1950-2000', '2000 to today')))
print(year_levels)
```

    ## # A tibble: 3,455 × 8
    ##       id year_built barrier_free_acces…¹ no_of…² no_of…³ no_ba…⁴ perce…⁵ time_…⁶
    ##    <dbl>      <dbl> <chr>                  <dbl>   <dbl>   <dbl>   <dbl> <fct>  
    ##  1 12064       1805 NO                         0      69       0    0    1800-1…
    ##  2 10424       1809 NO                         4      10       0    0    1800-1…
    ##  3 10906       1838 YES                        0      10       2   20    1800-1…
    ##  4 12869       1880 NO                         6      13       0    0    1850-1…
    ##  5 10899       1885 YES                        0      28       2    7.14 1850-1…
    ##  6 10905       1885 NO                         0      19       0    0    1850-1…
    ##  7 10437       1888 NO                         0      16       0    0    1850-1…
    ##  8 12662       1890 NO                         0      16       0    0    1850-1…
    ##  9 13409       1891 NO                         0      16       0    0    1850-1…
    ## 10 12473       1895 NO                         0      11       0    0    1850-1…
    ## # … with 3,445 more rows, and abbreviated variable names
    ## #   ¹​barrier_free_accessibilty_entr, ²​no_of_accessible_parking_spaces,
    ## #   ³​no_of_units, ⁴​no_barrier_free_accessible_units, ⁵​percent_accessible_units,
    ## #   ⁶​time_period

``` r
#Making a new plot using the new year_levels data and using percent of accessible units 
accessibility_over_time_year_levels <- year_levels %>%
                    ggplot(aes(x = time_period, y = percent_accessible_units))+ 
                    geom_point(alpha = 0.5, aes(color = barrier_free_accessibilty_entr))+ 
                    ggtitle("Accessibility features over time")+
                    ylab("Percent of barrier free accessible units")+
                    xlab("Time period built")
print(accessibility_over_time_year_levels)
```

    ## Warning: Removed 154 rows containing missing values (geom_point).

![](mini-project-2_files/figure-gfm/unnamed-chunk-6-2.png)<!-- -->

<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year):

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 1

``` r
#reordering the time periods
reorder_accessibility_over_time_year_levels <- year_levels %>%
  mutate(time_period = fct_rev(time_period))%>%
                    ggplot(aes(x = time_period, y = percent_accessible_units))+ 
                    geom_point(alpha = 0.5, aes(color = barrier_free_accessibilty_entr))+ 
                    ggtitle("Accessibility features over time")+
                    ylab("Percent of barrier free accessible units")+
                    xlab("Time period built")
print(reorder_accessibility_over_time_year_levels)
```

    ## Warning: Removed 154 rows containing missing values (geom_point).

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

I decided to reverse the order of the time period built, to have the
present day closer to the left, using the `fct_rev` function. This could
be useful if the focus is on time periods closer to today.
<!----------------------------------------------------------------------------->

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2

``` r
#grouping buildings built prior to 1950 into an "other" category (before 1950)
(collapse_accessibility_over_time_year_levels <- year_levels %>%
  mutate(fct_collapse(time_period,"Before 1950" = c("1900-1950", "1850-1900", "1800-1850")))%>% 
  rename(other_time_period_groups = `fct_collapse(...)`)%>%
  ggplot(aes(x = other_time_period_groups, y = percent_accessible_units))+ 
                    geom_point(alpha = 0.5, aes(color = barrier_free_accessibilty_entr))+ 
                    ggtitle("Accessibility features over time")+
                    ylab("Percent of barrier free accessible units")+
                    xlab("Time period built"))
```

    ## Warning: Removed 154 rows containing missing values (geom_point).

![](mini-project-2_files/figure-gfm/unnamed-chunk-8-1.png)<!-- --> I
decided to group all buildings built prior to 1950 into an “other”
category using `fct_collapse` because there is less data for buildings
built before 1950, so this helps build a neater plot. Furthermore, this
grouping could be useful for someone who is only interested in buildings
built after 1950, but still wants to keep buildings before 1950 in the
dataset.

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: *How has accessibility changed in apartment
buildings in Toronto from 1805 to today?*

**Variable of interest**: *percent accessible units*

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
#fitting a linear regression model to percent accessible units (Y) from year built (X) 
lin_reg_percent_accessible <- lm(percent_accessible_units ~ year_built, tidy_accessibility_data)
print(lin_reg_percent_accessible)
```

    ## 
    ## Call:
    ## lm(formula = percent_accessible_units ~ year_built, data = tidy_accessibility_data)
    ## 
    ## Coefficients:
    ## (Intercept)   year_built  
    ##  -176.83119      0.09412

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

``` r
#looking for the p-value from the above linear regression model 
broom::tidy(lin_reg_percent_accessible)
```

    ## # A tibble: 2 × 5
    ##   term         estimate std.error statistic   p.value
    ##   <chr>           <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept) -177.       45.6        -3.88 0.000106 
    ## 2 year_built     0.0941    0.0232      4.05 0.0000515

-   Based on the outputted tibble, the p-value for the year-built is
    5.15e-05
    <!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
#I want to convert the barrier_entrance dataset from Milestone 1 to a csv in my output folder 
barrier_entrance <- filter (apt_buildings, `barrier_free_accessibilty_entr` == "NO") %>%
            select(`id`,`barrier_free_accessibilty_entr`)
dir.create(here::here("output"))
```

    ## Warning in dir.create(here::here("output")): '/Users/ariel/Desktop/STAT 545/
    ## MiniDataAnalysis_ArielRosen_Milestone 2/output' already exists

``` r
write_csv(barrier_entrance, file = here::here("output", "barrier_entrance.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

``` r
#Writing the linear regression model from Task 3 to an RDS, then saving it to the output folder
saveRDS(lin_reg_percent_accessible, file = here::here("output", "lin_reg_percent_accessible.rds"))

#read the file
readRDS(file = here::here("output","lin_reg_percent_accessible.rds"))
```

    ## 
    ## Call:
    ## lm(formula = percent_accessible_units ~ year_built, data = tidy_accessibility_data)
    ## 
    ## Coefficients:
    ## (Intercept)   year_built  
    ##  -176.83119      0.09412

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least four folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

``` r
#Making a Milestone 1 folder: 
dir.create(here::here("Milestone1"))

#Making a Milestone 2 folder: 
dir.create(here::here("Milestone2"))
```

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
