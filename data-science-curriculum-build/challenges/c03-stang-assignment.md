---
editor_options: 
  markdown: 
    wrap: 72
---

# Aluminum Data

Oliver Buchwald 2025-02-20

-   [Grading Rubric](#grading-rubric)
    -   [Individual](#individual)
    -   [Submission](#submission)
-   [Loading and Wrangle](#loading-and-wrangle)
    -   [**q1** Tidy `df_stang` to produce `df_stang_long`. You should
        have column names `thick, alloy, angle, E, nu`. Make sure the
        `angle` variable is of correct type. Filter out any invalid
        values.](#q1-tidy-df_stang-to-produce-df_stang_long-you-should-have-column-names-thick-alloy-angle-e-nu-make-sure-the-angle-variable-is-of-correct-type-filter-out-any-invalid-values)
-   [EDA](#eda)
    -   [Initial checks](#initial-checks)
        -   [**q2** Perform a basic EDA on the aluminum data *without
            visualization*. Use your analysis to answer the questions
            under *observations* below. In addition, add your own
            *specific* question that you’d like to answer about the
            data—you’ll answer it below in
            q3.](#q2-perform-a-basic-eda-on-the-aluminum-data-without-visualization-use-your-analysis-to-answer-the-questions-under-observations-below-in-addition-add-your-own-specific-question-that-youd-like-to-answer-about-the-datayoull-answer-it-below-in-q3)
    -   [Visualize](#visualize)
        -   [**q3** Create a visualization to investigate your question
            from q2 above. Can you find an answer to your question using
            the dataset? Would you need additional information to answer
            your
            question?](#q3-create-a-visualization-to-investigate-your-question-from-q2-above-can-you-find-an-answer-to-your-question-using-the-dataset-would-you-need-additional-information-to-answer-your-question)
        -   [**q4** Consider the following
            statement:](#q4-consider-the-following-statement)
-   [References](#references)

*Purpose*: When designing structures such as bridges, boats, and planes,
the design team needs data about *material properties*. Often when we
engineers first learn about material properties through coursework, we
talk about abstract ideas and look up values in tables without ever
looking at the data that gave rise to published properties. In this
challenge you’ll study an aluminum alloy dataset: Studying these data
will give you a better sense of the challenges underlying published
material values.

In this challenge, you will load a real dataset, wrangle it into tidy
form, and perform EDA to learn more about the data.

<!-- include-rubric -->

# Grading Rubric {#grading-rubric}

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual {#individual}

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|------------------------|------------------------|------------------------|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission {#submission}

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

```         
## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
## ✔ dplyr     1.1.4     ✔ readr     2.1.5
## ✔ forcats   1.0.0     ✔ stringr   1.5.1
## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
## ✔ purrr     1.0.2     
## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
## ✖ dplyr::filter() masks stats::filter()
## ✖ dplyr::lag()    masks stats::lag()
## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors
```

``` r
library(ggforce)
```

*Background*: In 1946, scientists at the Bureau of Standards tested a
number of Aluminum plates to determine their
[elasticity](https://en.wikipedia.org/wiki/Elastic_modulus) and
[Poisson’s ratio](https://en.wikipedia.org/wiki/Poisson%27s_ratio).
These are key quantities used in the design of structural members, such
as aircraft skin under [buckling
loads](https://en.wikipedia.org/wiki/Buckling). These scientists tested
plats of various thicknesses, and at different angles with respect to
the [rolling](https://en.wikipedia.org/wiki/Rolling_(metalworking))
direction.

# Loading and Wrangle {#loading-and-wrangle}

<!-- -------------------------------------------------- -->

The `readr` package in the Tidyverse contains functions to load data
form many sources. The `read_csv()` function will help us load the data
for this challenge.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/stang.csv"

## Load the data
df_stang <- read_csv(filename)
```

```         
## Rows: 9 Columns: 8
## ── Column specification ────────────────────────────────────────────────────────
## Delimiter: ","
## chr (1): alloy
## dbl (7): thick, E_00, nu_00, E_45, nu_45, E_90, nu_90
## 
## ℹ Use `spec()` to retrieve the full column specification for this data.
## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

``` r
df_stang
```

```         
## # A tibble: 9 × 8
##   thick  E_00 nu_00  E_45  nu_45  E_90 nu_90 alloy  
##   <dbl> <dbl> <dbl> <dbl>  <dbl> <dbl> <dbl> <chr>  
## 1 0.022 10600 0.321 10700  0.329 10500 0.31  al_24st
## 2 0.022 10600 0.323 10500  0.331 10700 0.323 al_24st
## 3 0.032 10400 0.329 10400  0.318 10300 0.322 al_24st
## 4 0.032 10300 0.319 10500  0.326 10400 0.33  al_24st
## 5 0.064 10500 0.323 10400  0.331 10400 0.327 al_24st
## 6 0.064 10700 0.328 10500  0.328 10500 0.32  al_24st
## 7 0.081 10000 0.315 10000  0.32   9900 0.314 al_24st
## 8 0.081 10100 0.312  9900  0.312 10000 0.316 al_24st
## 9 0.081 10000 0.311    -1 -1      9900 0.314 al_24st
```

Note that these data are not tidy! The data in this form are convenient
for reporting in a table, but are not ideal for analysis.

### **q1** Tidy `df_stang` to produce `df_stang_long`. You should have column names `thick, alloy, angle, E, nu`. Make sure the `angle` variable is of correct type. Filter out any invalid values.

*Hint*: You can reshape in one `pivot` using the `".value"` special
value for `names_to`.

``` r
## TASK: Tidy `df_stang`

df_stang_long <- 
  df_stang %>%
  pivot_longer(
    names_to = c(".value", "angle"),
    names_sep = "_",
    cols = c(-thick, -alloy)
  ) %>% 
  filter(E > 0) %>% 
  transform(angle = as.integer(angle))


df_stang_long
```

```         
##    thick   alloy angle     E    nu
## 1  0.022 al_24st     0 10600 0.321
## 2  0.022 al_24st    45 10700 0.329
## 3  0.022 al_24st    90 10500 0.310
## 4  0.022 al_24st     0 10600 0.323
## 5  0.022 al_24st    45 10500 0.331
## 6  0.022 al_24st    90 10700 0.323
## 7  0.032 al_24st     0 10400 0.329
## 8  0.032 al_24st    45 10400 0.318
## 9  0.032 al_24st    90 10300 0.322
## 10 0.032 al_24st     0 10300 0.319
## 11 0.032 al_24st    45 10500 0.326
## 12 0.032 al_24st    90 10400 0.330
## 13 0.064 al_24st     0 10500 0.323
## 14 0.064 al_24st    45 10400 0.331
## 15 0.064 al_24st    90 10400 0.327
## 16 0.064 al_24st     0 10700 0.328
## 17 0.064 al_24st    45 10500 0.328
## 18 0.064 al_24st    90 10500 0.320
## 19 0.081 al_24st     0 10000 0.315
## 20 0.081 al_24st    45 10000 0.320
## 21 0.081 al_24st    90  9900 0.314
## 22 0.081 al_24st     0 10100 0.312
## 23 0.081 al_24st    45  9900 0.312
## 24 0.081 al_24st    90 10000 0.316
## 25 0.081 al_24st     0 10000 0.311
## 26 0.081 al_24st    90  9900 0.314
```

Use the following tests to check your work.

``` r
## NOTE: No need to change this
## Names
assertthat::assert_that(
              setequal(
                df_stang_long %>% names,
                c("thick", "alloy", "angle", "E", "nu")
              )
            )
```

```         
## [1] TRUE
```

``` r
## Dimensions
assertthat::assert_that(all(dim(df_stang_long) == c(26, 5)))
```

```         
## [1] TRUE
```

``` r
## Type
assertthat::assert_that(
              (df_stang_long %>% pull(angle) %>% typeof()) == "integer"
            )
```

```         
## [1] TRUE
```

``` r
print("Very good!")
```

```         
## [1] "Very good!"
```

# EDA {#eda}

<!-- -------------------------------------------------- -->

## Initial checks {#initial-checks}

<!-- ------------------------- -->

### **q2** Perform a basic EDA on the aluminum data *without visualization*. Use your analysis to answer the questions under *observations* below. In addition, add your own *specific* question that you’d like to answer about the data—you’ll answer it below in q3.

``` r
colnames(df_stang_long)
```

```         
## [1] "thick" "alloy" "angle" "E"     "nu"
```

``` r
## count number of unique alloys
df_stang_long %>%
  count(alloy)
```

```         
##     alloy  n
## 1 al_24st 26
```

``` r
## count number of samples of each angle
df_stang_long %>%
  count(angle)
```

```         
##   angle n
## 1     0 9
## 2    45 8
## 3    90 9
```

``` r
## list and count number of samples of each thickness
df_stang_long %>%
  count(thick)
```

```         
##   thick n
## 1 0.022 6
## 2 0.032 6
## 3 0.064 6
## 4 0.081 8
```

**Observations**:

-   Is there “one true value” for the material properties of Aluminum?
    -   Broadly speaking, no - E and mu both have a bit of variation
        from sample to sample. Properties can very based on material
        imperfections, varying test setups, and more, so it follows that
        the data would have some variation from sample to sample.
-   How many aluminum alloys are in this dataset? How do you know?
    -   Only one - All the samples in the data set are listed as al_24st
        alloy
-   What angles were tested?
    -   0 degrees, 45 degrees, and 90 degrees
-   What thicknesses were tested?
    -   The thicknesses tested were 0.022, 0.032, 0.064 and 0.081.
        Presumably, these measurements are in inches and correspond to
        23, 20,14, and 12 gauge sheet metal, respectively. 0.022 is an
        odd thickness to have (who uses 23 gauge sheet metal?), so I’m
        quite curious as to where and why these are the samples at play
-   My question: Does rolling angle have an impact on material
    properties?

## Visualize {#visualize}

<!-- ------------------------- -->

### **q3** Create a visualization to investigate your question from q2 above. Can you find an answer to your question using the dataset? Would you need additional information to answer your question?

``` r
## TASK: Investigate your question from q1 here
##convert angle back to categorical data
df_stang_plot <-
  df_stang_long %>%
  transform(angle = as.character(angle))

df_stang_plot %>%
  ggplot(aes(x = angle, y = E, fill = angle)) +
  geom_boxplot() + 
  labs(title = 'Young’s Modulus Relative to Rolling Angle', 
       x = 'Rolling Angle (degrees)', 
       y = 'Young’s Modulus (E)')
```

![](c03-stang-assignment_files/figure-gfm/q3-task-1.png)<!-- -->

``` r
df_stang_plot %>%
  ggplot(aes(x = angle, y = E, color = angle)) +
  geom_jitter(size = 3) +  
  labs(title = 'Young’s Modulus Relative to Rolling Angle', 
       x = 'Rolling Angle (degrees)', 
       y = 'Young’s Modulus (E)') 
```

![](c03-stang-assignment_files/figure-gfm/q3-task-2.png)<!-- -->

**Observations**:

-   There does not seem to be a strong trend here. It appears from the
    boxplot that the 45 degree samples are more consistent around the
    median value, which is slightly higher than the samples rolled at
    other angles. However, the maximum and minimum values are pretty
    similar regardless of direction, which makes me suspicious of
    drawing any conclusion. The scatterplot further confirms my
    suspicion that there doesn’t really seem to be any meaningful trend.

``` r
df_stang_plot %>%
  ggplot(aes(x = angle, y = nu, fill = angle)) +
  geom_boxplot() + 
  labs(title = "Poisson's Ratio Relative to Rolling Angle", 
       x = 'Rolling Angle (degrees)', 
       y = "Poisson's Ratio (nu)")
```

![](c03-stang-assignment_files/figure-gfm/q3-task-pt2-1.png)<!-- -->

``` r
df_stang_plot %>%
  ggplot(aes(x = angle, y = nu, color = angle)) +
  geom_jitter(size = 3) + 
  labs(title = "Poisson's Ratio Relative to Rolling Angle", 
       x = 'Rolling Angle (degrees)', 
       y = "Poisson's Ratio (nu)") 
```

![](c03-stang-assignment_files/figure-gfm/q3-task-pt2-2.png)<!-- -->

``` r
df_stang_plot %>%
  ggplot(aes(x = nu, color = angle)) +
  geom_density() + 
  labs(title = "Density of Poisson's Ratio", 
       x = "Poisson's Ratio (nu)") 
```

![](c03-stang-assignment_files/figure-gfm/q3-task-pt2-3.png)<!-- -->

**Observations (Part 2!)**:

-   The boxplot for Poisson’s ratio shows a median which is
    comparatively higher at 45 degrees than other angles. From a
    mechanical perspective, this makes some degree of sense, since
    Poisson’s ratio relates strain in perpendicular directions, and
    rotating some microstructure by 45 degrees could allow a better path
    for that to happen.
-   Comparing to the scatterplot, this interpretation seems a bit more
    substantiated than for Young’s modulus. The data does seem to show a
    concentration in 45-degree measurements around 0.330 that is much
    more defined than anything for the 0 or 90 degree samples. However,
    it still has a fairly high number of outliers (3/8 samples), and I’m
    hesitant to make any conclusion off of so few samples.
-   The density plot shows a more defined mode for the 45 degree parts,
    which is another visualization for what I’m getting at, but it feels
    frankly kind of silly to make a density plot of so few points.
-   All of this to say: I don’t think its reasonable to conclude a
    relationship from this data. Its possible that more investigation,
    and particularly testing more samples *might* beget some trend, but
    it just as well might not. If there is orientation-related variation
    in strain, it seems to be very small, on the order of 1.8% (if we
    compare the medians). Hopefully, anyone who’s designing parts with
    these materials has a factor of safety of more than 1.01, so its
    likely that this investigation isn’t needed.

### **q4** Consider the following statement: {#q4-consider-the-following-statement}

> “A material’s property (or material property) is an intensive property
> of some material, i.e. a physical property that does not depend on the
> amount of the material.”$$2$$

Note that the “amount of material” would vary with the thickness of a
tested plate. Does the following graph support or contradict the claim
that “elasticity `E` is an intensive material property.” Why or why not?
Is this evidence *conclusive* one way or another? Why or why not?

``` r
## NOTE: No need to change; run this chunk
df_stang_long %>%
  ggplot(aes(nu, E, color = as_factor(thick))) +
  geom_point(size = 3) +
  theme_minimal()
```

![](c03-stang-assignment_files/figure-gfm/q4-vis-1.png)<!-- -->

**Observations**:

-   Does this graph support or contradict the claim above?
    -   It seems to contradict the claim. Greater thickness appears to
        correspond to a lower elasticity modulus, at least for the 0.081
        thickness
-   Is this evidence *conclusive* one way or another?
    -   No
    -   Outside of the 0.081 thickness, the other three thicknesses are
        pretty much comparable. If we exclude that data, the lowest
        value for E is found at the 0.032 thickness, the highest is
        shared by 0.022 and 0.064 thickness, but the medians and ranges
        really show no discernible trend.
    -   This graph is also not a clear answer to that question, in my
        opinion. Why are we considering nu? Poisson’s ratio should be a
        dimensionless constant for a material, so I suppose it could be
        used as a control against material chages, but this graph is
        super odd to me. I’d consider making a box or scatter plot of
        this data instead, bucketed by material thickness. Or maybe a
        geom_smooth plot overlaid with a scatter plot, to try and get an
        idea of the possible trend based on size. I feel like I’ve made
        enough graphs for today, though.

# References {#references}

<!-- -------------------------------------------------- -->

$$1$$ Stang, Greenspan, and Newman, “Poisson’s ratio of some structural
alloys for large strains” (1946) Journal of Research of the National
Bureau of Standards, (pdf
link)$$<https://nvlpubs.nist.gov/nistpubs/jres/37/jresv37n4p211_A1b.pdf>$$

$$2$$ Wikipedia, *List of material properties*, accessed 2020-06-26,
(link)$$<https://en.wikipedia.org/wiki/List_of_materials_properties>$$
