Antibiotics
================
Arianne Fong
2026-10-03

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

``` r
# WRITE YOUR CODE HERE
df_antibiotics %>%
  pivot_longer(
    names_to = "Antibiotic",
    values_to = "MIC",
    cols = c(`penicillin`, `streptomycin`, `neomycin`)
  ) -> df_q1

df_q1 %>%
  ggplot(
    aes(bacteria, MIC, color = gram)) +
  geom_point() +
  facet_wrap(~Antibiotic) +
  scale_y_log10() +
  coord_flip()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

``` r
df_q1
```

    ## # A tibble: 48 × 4
    ##    bacteria              gram     Antibiotic       MIC
    ##    <chr>                 <chr>    <chr>          <dbl>
    ##  1 Aerobacter aerogenes  negative penicillin   870    
    ##  2 Aerobacter aerogenes  negative streptomycin   1    
    ##  3 Aerobacter aerogenes  negative neomycin       1.6  
    ##  4 Brucella abortus      negative penicillin     1    
    ##  5 Brucella abortus      negative streptomycin   2    
    ##  6 Brucella abortus      negative neomycin       0.02 
    ##  7 Bacillus anthracis    positive penicillin     0.001
    ##  8 Bacillus anthracis    positive streptomycin   0.01 
    ##  9 Bacillus anthracis    positive neomycin       0.007
    ## 10 Diplococcus pneumonia positive penicillin     0.005
    ## # ℹ 38 more rows

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_q1 %>%
  ggplot(
    aes(Antibiotic, MIC)
  ) +
  geom_boxplot() +
  geom_point(
    aes(color = bacteria, shape = gram)) +
  scale_y_log10()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
## penicillin and gram positive?

df_q1 %>%
  ggplot(
    aes(gram, MIC)) +
  geom_boxplot() +
  scale_y_log10() +
  geom_point(aes(colour = bacteria)) +
  facet_wrap(~Antibiotic)
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.3-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
## neomycin and streptomycin?

df_antibiotics %>%
  ggplot(aes(neomycin, streptomycin)) +
  geom_point(aes(colour = bacteria)) +
  scale_y_log10() +
  scale_x_log10()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
## neomycin and penicillin?

df_antibiotics %>%
  ggplot(aes(neomycin, penicillin)) +
  geom_point(aes(colour = bacteria, shape = gram)) +
  scale_y_log10() +
  scale_x_log10()
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations*

\- What is your response to the question above? - From visual 3,
penicillin is most effective against Gram positive bacteria and very
ineffective at Gram negative bacteria. Neomycin and streptomycin have
more varied effectiveness for Gram negative bacteria, but have the same
median effectiveness for Gram positive and negative bacteria. From
visual 1, penicillin is most effective against Streptococcus, Bacillus
and Diplococcus genus. Streptomycin is most effective against Aerobacter
genus, and neomycin is most effective against all the other genus.

\- Which of your visuals above (1 through 5) is **most effective** at
helping to answer this question? - Visual 3 is most effective at helping
answer the antibiotics’ effectiveness against bacteria of different Gram
stain, and visual 1 is most effective at helping answer the antibiotics’
effectiveness against bacteria of different genera.

\- Why? - Visual 3 is most effective for answering the antibiotics’
effectiveness against bacteria of different Gram stain because the MICs
for all antibiotics are on the same axis so they’re easily comparable.
Additionally, this is my only visual that has Gram stain represented on
an axis, rather than as a colour or shape, making this visual the
easiest to notice relationships with Gram stain. Visual 1 is the most
effective at answering the antibiotics’ effectiveness against bacteria
of different genera because it shows all bacteria names on the y-axis,
and all antibiotic MICs on the x-axis. This is the only visualization
that represents bacteria on an axis rather than as the colour of a
point, making this visualization the easiest to read the bacteria.

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* was renamed *Streptococcus
> pneumoniae*?

*Observations*

\- What is your response to the question above? - *Diplococcus
pneumoniae* was renamed *Streptococcus pneumoniae* because its MICs for
the three antibiotics are similar to that of the Streptococcus genera.
In biology, form follows function, meaning that the structure of
biological parts dictates what it does. Therefore, the reason
Diplococcus pneumoniae has similar MICs as the Streptococcus genera is
because its physical structure is similar to the Streptococcus genera.

\- Which of your visuals above (1 through 5) is **most effective** at
helping to answer this question? - Visualization 1 is most effective at
answering why *Diplococcus pneumoniae* was renamed *Streptococcus
pneumoniae.*

\- Why? - Visualization 1 is most effective at answering why
*Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae* because
it shows all bacteria on the y-axis, instead of representing them as
colours, making it easier to notice patterns related to bacteria name.
Additionally, the MICs for each bacteria for one antibiotic are on the
same axis, making it easy to compare.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)

``` r
## challenge 5 presentation

df_q1 %>%
  mutate(
    genus = word(bacteria, 1),
    species = word(bacteria, -1)
  ) -> df_present

df_present
```

    ## # A tibble: 48 × 6
    ##    bacteria              gram     Antibiotic       MIC genus       species  
    ##    <chr>                 <chr>    <chr>          <dbl> <chr>       <chr>    
    ##  1 Aerobacter aerogenes  negative penicillin   870     Aerobacter  aerogenes
    ##  2 Aerobacter aerogenes  negative streptomycin   1     Aerobacter  aerogenes
    ##  3 Aerobacter aerogenes  negative neomycin       1.6   Aerobacter  aerogenes
    ##  4 Brucella abortus      negative penicillin     1     Brucella    abortus  
    ##  5 Brucella abortus      negative streptomycin   2     Brucella    abortus  
    ##  6 Brucella abortus      negative neomycin       0.02  Brucella    abortus  
    ##  7 Bacillus anthracis    positive penicillin     0.001 Bacillus    anthracis
    ##  8 Bacillus anthracis    positive streptomycin   0.01  Bacillus    anthracis
    ##  9 Bacillus anthracis    positive neomycin       0.007 Bacillus    anthracis
    ## 10 Diplococcus pneumonia positive penicillin     0.005 Diplococcus pneumonia
    ## # ℹ 38 more rows

``` r
# df_present %>%
#   ggplot(
#     aes(x = genus, y = MIC)
#   ) +
#   geom_boxplot() +
#   facet_grid(Antibiotic~.) +
#   scale_y_log10() +
#   coord_flip() + 
#   geom_hline(
#    mapping = aes(yintercept = filter(genus == Diplococcus, MIC))
#   )

library(ggcube)
```

    ## 
    ## Attaching package: 'ggcube'

    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     aes, element_rect

``` r
df_antibiotics %>%
  ggplot(aes(
    x = penicillin,
    y = neomycin,
    z = streptomycin,
    colour = bacteria
  )) + 
  geom_point() +
  coord_3d() +
  scale_y_log10() + 
  scale_x_log10() +
  scale_z_continuous(transform = "log10")
```

![](c05-antibiotics-assignment_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->
