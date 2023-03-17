Class 11 boxplot
================
Daira

``` r
data <- read.table("rs8067378_ENSG00000172057.6.txt", col.names = ,1)
```

> Q13: Read this file into R and determine the sample size for each
> genotype and their corresponding median expression levels for each of
> these genotypes

``` r
nrow(data)
```

    [1] 462

``` r
table(data$geno)
```


    A/A A/G G/G 
    108 233 121 

``` r
median(data$exp[data$geno=="A/A"])
```

    [1] 31.24847

``` r
median(data$exp[data$geno=="A/G"])
```

    [1] 25.06486

``` r
median(data$exp[data$geno=="G/G"])
```

    [1] 20.07363

A: There are 108 A/A, 233 A/G, and 121 G/G and the corresponding median
expression levels are: 31.24, 25.06, and 20.07, respectively.

``` r
library(ggplot2)
ggplot(data) +
  aes(x=geno, y= exp, fill=geno) +
  geom_boxplot(notch=TRUE) +
  labs(title = "A Boxplot of Genotypes and Expression for ORMDL3", x= "genotypes", y= "expression")
```

![](boxplotdm_files/figure-commonmark/unnamed-chunk-3-1.png)

> Q14: Generate a boxplot with a box per genotype, what could you infer
> from the relative expression value between A/A and G/G displayed in
> this plot? Does the SNP effect the expression of ORMDL3?

A: Based on the plot created, there is a difference between A/A and G/G,
and having the G/G snp at this location is associated with reduced
levels of ORMDL3 expression.
