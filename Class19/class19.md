Class 19 Pertussis Mini Project
================
Dairabel

# Web scraping

Here I extract the CDC figures for Pertussis cases in USA:
https://www.cdc.gov/pertussis/surv-reporting/cases-by-year.html

``` r
library(ggplot2)
ggplot(cdc, aes(Year, Cases)) + 
  geom_line() + 
  geom_point() +
  geom_smooth() +
  labs(x="year", y="cases", title="Pertussis Cases in USA across the years")+ scale_y_continuous(labels=scales::label_comma())
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](class19_files/figure-commonmark/unnamed-chunk-2-1.png)

in 1946 we got a vaccination! we should add it as a line.

``` r
library(ggplot2)
ggplot(cdc, aes(Year, Cases)) + 
  geom_line() + 
  geom_point() +
  geom_smooth() +
  geom_vline(xintercept = 1946, color="red", linetype=2)+
  labs(x="year", y="cases", title="Pertussis Cases in USA across the years")+ scale_y_continuous(labels=scales::label_comma())
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](class19_files/figure-commonmark/unnamed-chunk-3-1.png)

the US and other countries swtiched from original wP vaccine to new Ap
Vaccine in 1996. lets add it to our plot.

``` r
library(ggplot2)
ggplot(cdc, aes(Year, Cases)) + 
  geom_line() + 
  geom_point() +
  geom_smooth() +
  geom_vline(xintercept = 1946, color="red", linetype=2)+
  geom_vline(xintercept = 1996, color="blue", linetype=2)+
  labs(x="year", y="cases", title="Pertussis Cases in USA across the years")+ scale_y_continuous(labels=scales::label_comma())
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](class19_files/figure-commonmark/unnamed-chunk-4-1.png)

``` r
library(ggplot2)
ggplot(chile, aes(Year, Cases)) + 
  geom_line() + 
  geom_point() +
  geom_smooth() +
  geom_vline(xintercept = 1996, color="blue", linetype=2)+
  labs(x="year", y="cases", title="Pertussis Cases in Chile across the years")+ scale_y_continuous(labels=scales::label_comma())
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](class19_files/figure-commonmark/unnamed-chunk-6-1.png)

``` r
alldata=rbind(
  data.frame(chile, group="Chile"),
  data.frame(cdc, group="USA")
)
```

``` r
library(ggplot2)
ggplot(alldata, aes(Year, Cases)) + 
  geom_line() + 
  geom_point() +
  geom_smooth() +
  facet_wrap(~group)+
  geom_vline(xintercept = 1996, color="blue", linetype=2)+
  labs(x="year", y="cases", title="Pertussis Cases across the years")+   scale_y_continuous(labels=scales::label_comma())
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](class19_files/figure-commonmark/unnamed-chunk-8-1.png)

\#Exploring CMI-PB data

Getting data as much as we can. getting RNAseq data. We wil use
**jsonlite** package

``` r
library(jsonlite)
```

``` r
subject <-read_json("http://cmi-pb.org/api/subject", simplifyVector = TRUE)
head(subject)
```

      subject_id infancy_vac biological_sex              ethnicity  race
    1          1          wP         Female Not Hispanic or Latino White
    2          2          wP         Female Not Hispanic or Latino White
    3          3          wP         Female                Unknown White
    4          4          wP           Male Not Hispanic or Latino Asian
    5          5          wP           Male Not Hispanic or Latino Asian
    6          6          wP         Female Not Hispanic or Latino White
      year_of_birth date_of_boost      dataset
    1    1986-01-01    2016-09-12 2020_dataset
    2    1968-01-01    2019-01-28 2020_dataset
    3    1983-01-01    2016-10-10 2020_dataset
    4    1988-01-01    2016-08-29 2020_dataset
    5    1991-01-01    2016-08-29 2020_dataset
    6    1988-01-01    2016-10-10 2020_dataset

> Q4. How many wP and aP subjects are there?

``` r
table(subject$infancy_vac)
```


    aP wP 
    47 49 

A: 49 with wP, 47 with aP

> Q5. How many Male and Female subjects/patients are in the dataset?

> Q6??. How many females non white individuals are there in the dataset?

``` r
table(subject$race, subject$biological_sex)
```

                                               
                                                Female Male
      American Indian/Alaska Native                  0    1
      Asian                                         18    9
      Black or African American                      2    0
      More Than One Race                             8    2
      Native Hawaiian or Other Pacific Islander      1    1
      Unknown or Not Reported                       10    4
      White                                         27   13

A: 29 non white individuals. *counting the unknown*

\##specimen table

``` r
library(lubridate)
```


    Attaching package: 'lubridate'

    The following objects are masked from 'package:base':

        date, intersect, setdiff, union

> Q7. Using this approach determine (i) the average age of wP
> individuals, (ii) the average age of aP individuals; and (iii) are
> they significantly different?

\#specimen \##Joining multiple tables

``` r
specimen <- read_json("http://cmi-pb.org/api/specimen", simplifyVector = TRUE)
head(specimen)
```

      specimen_id subject_id actual_day_relative_to_boost
    1           1          1                           -3
    2           2          1                          736
    3           3          1                            1
    4           4          1                            3
    5           5          1                            7
    6           6          1                           11
      planned_day_relative_to_boost specimen_type visit
    1                             0         Blood     1
    2                           736         Blood    10
    3                             1         Blood     2
    4                             3         Blood     3
    5                             7         Blood     4
    6                            14         Blood     5

``` r
titer <- read_json("http://cmi-pb.org/api/ab_titer", simplifyVector = TRUE)
head(titer)
```

      specimen_id isotype is_antigen_specific antigen        MFI MFI_normalised
    1           1     IgE               FALSE   Total 1110.21154       2.493425
    2           1     IgE               FALSE   Total 2708.91616       2.493425
    3           1     IgG                TRUE      PT   68.56614       3.736992
    4           1     IgG                TRUE     PRN  332.12718       2.602350
    5           1     IgG                TRUE     FHA 1887.12263      34.050956
    6           1     IgE                TRUE     ACT    0.10000       1.000000
       unit lower_limit_of_detection
    1 UG/ML                 2.096133
    2 IU/ML                29.170000
    3 IU/ML                 0.530000
    4 IU/ML                 6.205949
    5 IU/ML                 4.679535
    6 IU/ML                 2.816431

> Q9. Complete the code to join specimen and subject tables to make a
> new merged data frame containing all specimen records along with their
> associated subject details:

``` r
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
meta <- inner_join(specimen, subject)
```

    Joining with `by = join_by(subject_id)`

``` r
dim(meta)
```

    [1] 729  13

``` r
head(meta)
```

      specimen_id subject_id actual_day_relative_to_boost
    1           1          1                           -3
    2           2          1                          736
    3           3          1                            1
    4           4          1                            3
    5           5          1                            7
    6           6          1                           11
      planned_day_relative_to_boost specimen_type visit infancy_vac biological_sex
    1                             0         Blood     1          wP         Female
    2                           736         Blood    10          wP         Female
    3                             1         Blood     2          wP         Female
    4                             3         Blood     3          wP         Female
    5                             7         Blood     4          wP         Female
    6                            14         Blood     5          wP         Female
                   ethnicity  race year_of_birth date_of_boost      dataset
    1 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    2 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    3 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    4 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    5 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    6 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset

> Q10. Now using the same procedure join meta with titer data so we can
> further analyze this data in terms of time of visit aP/wP, male/female
> etc.

``` r
library(dplyr)
abdata <- inner_join(titer, meta)
```

    Joining with `by = join_by(specimen_id)`

``` r
dim(abdata)
```

    [1] 32675    20

``` r
head(abdata)
```

      specimen_id isotype is_antigen_specific antigen        MFI MFI_normalised
    1           1     IgE               FALSE   Total 1110.21154       2.493425
    2           1     IgE               FALSE   Total 2708.91616       2.493425
    3           1     IgG                TRUE      PT   68.56614       3.736992
    4           1     IgG                TRUE     PRN  332.12718       2.602350
    5           1     IgG                TRUE     FHA 1887.12263      34.050956
    6           1     IgE                TRUE     ACT    0.10000       1.000000
       unit lower_limit_of_detection subject_id actual_day_relative_to_boost
    1 UG/ML                 2.096133          1                           -3
    2 IU/ML                29.170000          1                           -3
    3 IU/ML                 0.530000          1                           -3
    4 IU/ML                 6.205949          1                           -3
    5 IU/ML                 4.679535          1                           -3
    6 IU/ML                 2.816431          1                           -3
      planned_day_relative_to_boost specimen_type visit infancy_vac biological_sex
    1                             0         Blood     1          wP         Female
    2                             0         Blood     1          wP         Female
    3                             0         Blood     1          wP         Female
    4                             0         Blood     1          wP         Female
    5                             0         Blood     1          wP         Female
    6                             0         Blood     1          wP         Female
                   ethnicity  race year_of_birth date_of_boost      dataset
    1 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    2 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    3 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    4 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    5 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    6 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset

> Q11. How many specimens (i.e. entries in abdata) do we have for each
> isotype?

``` r
table(abdata$isotype)
```


     IgE  IgG IgG1 IgG2 IgG3 IgG4 
    6698 1413 6141 6141 6141 6141 

A: There are 6698 entries for IgE, 1413 for IgG, 6141 for IgG1, 6141 for
IgG2, 6141 for IgG3, and 6141 for IgG4. We have six isotypes.

> Q12. What do you notice about the number of visit 8 specimens compared
> to other visits?

``` r
print(table(abdata$visit))
```


       1    2    3    4    5    6    7    8 
    5795 4640 4640 4640 4640 4320 3920   80 

A: We notice that it is the smallest at 80 compared to all others with
four digits. Meaning that it is still going.

\#4. Examine IgG1 Ab titer levels

``` r
ig1 <- abdata %>%
  filter(isotype == "IgG1", visit!=8)
dim(ig1)
```

    [1] 6126   20

``` r
head(ig1)
```

      specimen_id isotype is_antigen_specific antigen        MFI MFI_normalised
    1           1    IgG1                TRUE     ACT 274.355068      0.6928058
    2           1    IgG1                TRUE     LOS  10.974026      2.1645083
    3           1    IgG1                TRUE   FELD1   1.448796      0.8080941
    4           1    IgG1                TRUE   BETV1   0.100000      1.0000000
    5           1    IgG1                TRUE   LOLP1   0.100000      1.0000000
    6           1    IgG1                TRUE Measles  36.277417      1.6638332
       unit lower_limit_of_detection subject_id actual_day_relative_to_boost
    1 IU/ML                 3.848750          1                           -3
    2 IU/ML                 4.357917          1                           -3
    3 IU/ML                 2.699944          1                           -3
    4 IU/ML                 1.734784          1                           -3
    5 IU/ML                 2.550606          1                           -3
    6 IU/ML                 4.438966          1                           -3
      planned_day_relative_to_boost specimen_type visit infancy_vac biological_sex
    1                             0         Blood     1          wP         Female
    2                             0         Blood     1          wP         Female
    3                             0         Blood     1          wP         Female
    4                             0         Blood     1          wP         Female
    5                             0         Blood     1          wP         Female
    6                             0         Blood     1          wP         Female
                   ethnicity  race year_of_birth date_of_boost      dataset
    1 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    2 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    3 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    4 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    5 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset
    6 Not Hispanic or Latino White    1986-01-01    2016-09-12 2020_dataset

> Q how many antigenrs are there?

``` r
table(abdata$antigen)
```


        ACT   BETV1      DT   FELD1     FHA  FIM2/3   LOLP1     LOS Measles     OVA 
       1970    1970    2135    1970    2529    2135    1970    1970    1970    2135 
        PD1     PRN      PT     PTM   Total      TT 
       1970    2529    2529    1970     788    2135 

> Q13. Complete the following code to make a summary boxplot of Ab titer
> levels (MFI) for all antigens:

``` r
ggplot(ig1) +
  aes(MFI, antigen) +
  geom_boxplot() 
```

![](class19_files/figure-commonmark/unnamed-chunk-21-1.png)

> Q14. What antigens show differences in the level of IgG1 antibody
> titers recognizing them over time? Why these and not others?

``` r
ggplot(ig1) +
  aes(MFI, antigen) +
  geom_boxplot() + 
  facet_wrap(vars(visit), nrow=2)
```

![](class19_files/figure-commonmark/unnamed-chunk-22-1.png)

A: the fim2/3 antigens shown differences in levels of IgG1, when we look
at terminology it is the mixture of FIM2 and FIM3, the fim are fimbria.
ALso we see the FHA which is Filamentous hemagglutinin. These are found
on the exterior of the bacteria and involved with moving
around/attaching and it makes sense that the vaccine would affect them
and make them have higher levels of Ig1 antibody.

peak around visit 5 or 6. th median is the bar in middle of the box.

``` r
ggplot(ig1) +
  aes(MFI, antigen, col=infancy_vac ) +
  geom_boxplot(show.legend = TRUE) + 
  facet_wrap(vars(visit), nrow=2) +
  theme_bw()
```

![](class19_files/figure-commonmark/unnamed-chunk-23-1.png)

``` r
ggplot(ig1) +
  aes(MFI, antigen, col=infancy_vac ) +
  geom_boxplot(show.legend = FALSE) + 
  facet_wrap(vars(infancy_vac, visit), nrow=2)
```

![](class19_files/figure-commonmark/unnamed-chunk-24-1.png)

> Q15. Filter to pull out only two specific antigens for analysis and
> create a boxplot for each. You can chose any you like. Below I picked
> a “control” antigen (“Measles”, that is not in our vaccines) and a
> clear antigen of interest (“FIM2/3”, extra-cellular fimbriae proteins
> from B. pertussis that participate in substrate attachment).

``` r
filter(ig1, antigen=="FHA") %>%
  ggplot() +
  aes(MFI, col=infancy_vac) +
  geom_boxplot(show.legend = ) +
  facet_wrap(vars(visit)) +
  theme_bw() +
  theme(axis.text.x = element_text(angle=45, hjust=1)) +
  labs(title="FHA antigen levels per visit")
```

![](class19_files/figure-commonmark/unnamed-chunk-25-1.png)

second one is going to be FIM2/3

``` r
filter(ig1, antigen=="FIM2/3") %>%
  ggplot() +
  aes(MFI, col=infancy_vac) +
  geom_boxplot(show.legend = ) +
  facet_wrap(vars(visit)) +
  theme_bw() +
  theme(axis.text.x = element_text(angle=45, hjust=1)) +
  labs(title="FIM2/3 antigen levels per visit")
```

![](class19_files/figure-commonmark/unnamed-chunk-26-1.png)

> Q16. What do you notice about these two antigens time courses and the
> FIM2/3 data in particular?

A: We notice that wP has higher average values from week 1-3 but then
after that, the aP has higher average values than wP.

> Q17. Do you see any clear difference in aP vs. wP responses?

A: NO we dont see a big big difference, they are almost overlapping for
each visit.

\#5. Obtaining CMI-PB RNASeq data

``` r
url <- "https://www.cmi-pb.org/api/v2/rnaseq?versioned_ensembl_gene_id=eq.ENSG00000211896.7"

rna <- read_json(url, simplifyVector = TRUE) 
```

we will se join again!

``` r
ssrna <- inner_join(rna, meta)
```

    Joining with `by = join_by(specimen_id)`

``` r
dim(ssrna)
```

    [1] 360  16

``` r
head(ssrna)
```

      versioned_ensembl_gene_id specimen_id raw_count      tpm subject_id
    1         ENSG00000211896.7         344     18613  929.640         44
    2         ENSG00000211896.7         243      2011  112.584         31
    3         ENSG00000211896.7         261      2161  124.759         33
    4         ENSG00000211896.7         282      2428  138.292         36
    5         ENSG00000211896.7         345     51963 2946.136         44
    6         ENSG00000211896.7         244     49652 2356.749         31
      actual_day_relative_to_boost planned_day_relative_to_boost specimen_type
    1                            3                             3         Blood
    2                            3                             3         Blood
    3                           15                            14         Blood
    4                            1                             1         Blood
    5                            7                             7         Blood
    6                            7                             7         Blood
      visit infancy_vac biological_sex              ethnicity               race
    1     3          aP         Female     Hispanic or Latino More Than One Race
    2     3          wP         Female Not Hispanic or Latino              Asian
    3     5          wP           Male     Hispanic or Latino More Than One Race
    4     2          aP         Female     Hispanic or Latino              White
    5     4          aP         Female     Hispanic or Latino More Than One Race
    6     4          wP         Female Not Hispanic or Latino              Asian
      year_of_birth date_of_boost      dataset
    1    1998-01-01    2016-11-07 2020_dataset
    2    1989-01-01    2016-09-26 2020_dataset
    3    1990-01-01    2016-10-10 2020_dataset
    4    1997-01-01    2016-10-24 2020_dataset
    5    1998-01-01    2016-11-07 2020_dataset
    6    1989-01-01    2016-09-26 2020_dataset

> Q18. Make a plot of the time course of gene expression for IGHG1 gene
> (i.e. a plot of visit vs. tpm).

``` r
ggplot(ssrna) +
  aes(visit, tpm, group=subject_id) +
  geom_point() +
  geom_line(alpha=0.2)
```

![](class19_files/figure-commonmark/unnamed-chunk-29-1.png)

> Q19.: What do you notice about the expression of this gene (i.e. when
> is it at it’s maximum level)?

A: We notice that its peak is around visit 4.

> Q20. Does this pattern in time match the trend of antibody titer data?
> If not, why not?

No, its does not match the antibody titer, that one keeps increasing
even after the expression is gone.

``` r
ggplot(ssrna) +
  aes(tpm, col=infancy_vac) +
  geom_histogram(alpha=0.5) +
  facet_wrap(vars(visit))
```

    `stat_bin()` using `bins = 30`. Pick better value with `binwidth`.

![](class19_files/figure-commonmark/unnamed-chunk-30-1.png)

``` r
ssrna %>%  
  filter(visit==4) %>% 
  ggplot() +
    aes(tpm, col=infancy_vac) + 
  geom_density() + 
    geom_rug() 
```

![](class19_files/figure-commonmark/unnamed-chunk-31-1.png)
