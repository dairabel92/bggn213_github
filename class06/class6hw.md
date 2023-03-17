Class 6: Homework Function
================
Daira

The provided code that we want to improve and adapt to be more general
for any protein-drug combination

``` r
library(bio3d)
s1 <- read.pdb("4AKE") # kinase with drug
```

      Note: Accessing on-line PDB file

``` r
s2 <- read.pdb("1AKE") # kinase no drug
```

      Note: Accessing on-line PDB file
       PDB has ALT records, taking A only, rm.alt=TRUE

``` r
s3 <- read.pdb("1E4Y") # kinase with drug
```

      Note: Accessing on-line PDB file

``` r
s1.chainA <- trim.pdb(s1, chain="A", elety="CA")
s2.chainA <- trim.pdb(s2, chain="A", elety="CA")
s3.chainA <- trim.pdb(s1, chain="A", elety="CA")

s1.b <- s1.chainA$atom$b
s2.b <- s2.chainA$atom$b
s3.b <- s3.chainA$atom$b

plotb3(s1.b, sse=s1.chainA, typ="l", ylab="Bfactor")
```

![](class6hw_files/figure-commonmark/unnamed-chunk-1-1.png)

``` r
plotb3(s2.b, sse=s2.chainA, typ="l", ylab="Bfactor")
```

![](class6hw_files/figure-commonmark/unnamed-chunk-1-2.png)

``` r
plotb3(s3.b, sse=s3.chainA, typ="l", ylab="Bfactor")
```

![](class6hw_files/figure-commonmark/unnamed-chunk-1-3.png)

> Q1. What type of object is returned from the `read.pdb()` function?

Read.pdb reads the input PDB file and returns a data.frame set for a
unique element(this case with or without drug) with 16 variables
including ATOM, CHAIN, and other information

> Q2. What does the `trim.pdb()` function do?

The trim.pdb() function pulls data from the pdb files for the Alpha
Chain of a protein, it stores this as s1.chainA”

``` r
library(bio3d)

id <-"4AKE"

s1 <- read.pdb(id)
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    C:\Users\daira\AppData\Local\Temp\Rtmp42Nwn1/4AKE.pdb exists. Skipping download

``` r
s1.chainA <- trim.pdb(s1, chain="A", elety="CA")

s1.b <- s1.chainA$atom$b

plotb3(s1.b, sse=s1.chainA, typ="l", ylab="Bfactor")
```

![](class6hw_files/figure-commonmark/unnamed-chunk-2-1.png)

``` r
pbdplot <- function(id) {
  #READ THE INPUT PBD FILE
  s1 <- read.pdb(id)
  #EXTRACT SUBSET OF CHAIN A aka ALPHA
  s1.chainA <- trim.pdb(s1, chain="A", elety="CA")
  #extracts the b factor information of Chain A"
  s1.b <- s1.chainA$atom$b
  #to plot the b-factor and alpha chain residue
  plotb3(s1.b, sse=s1.chainA, typ="l", ylab="Bfactor", top=FALSE, bot=FALSE)
}
```

``` r
pbdplot("4AKE")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    C:\Users\daira\AppData\Local\Temp\Rtmp42Nwn1/4AKE.pdb exists. Skipping download

![](class6hw_files/figure-commonmark/unnamed-chunk-4-1.png)

``` r
pbdplot("1E4Y")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    C:\Users\daira\AppData\Local\Temp\Rtmp42Nwn1/1E4Y.pdb exists. Skipping download

![](class6hw_files/figure-commonmark/unnamed-chunk-4-2.png)

``` r
pbdplot("1AKE")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    C:\Users\daira\AppData\Local\Temp\Rtmp42Nwn1/1AKE.pdb exists. Skipping download

       PDB has ALT records, taking A only, rm.alt=TRUE

![](class6hw_files/figure-commonmark/unnamed-chunk-4-3.png)

> Q3. What input parameter would turn off the marginal black and grey
> rectangles in the plots and what do they represent in this case?

The input paramater that would turn off the marginal black and grey
rectangles in the plotb3 are top=FALSE and bot=FALSE,they represent the
secondary structure elements from the read.pdb files

> Q4. What would be a better plot to compare across the different
> proteins?

A better plot to compare across proteins would be a Scatterplot

> Q5.Which proteins are more similar to each other in their B-factor
> trends. How could you quantify this?

The proteins more similar to each other are s1.b and s3.b, you find the
least distance between them on the dendogram”)

``` r
hc <- hclust(dist(rbind(s1.b,s2.b,s3.b)))
plot(hc)
```

![](class6hw_files/figure-commonmark/unnamed-chunk-5-1.png)

> Q6. How would you generalize the original code above to work with any
> set of input protein structures?

You would need to create a FUNCTION! here is the code for mine:

``` r
protein.analysis <- function (x) {
  s1 <- read.pdb(x) #read the PBD files, th input to the function
  trim.pdb(s1, chain="A", elety="CA") #extracts subset of CHAIN A information
  s1b <- s1.chainA$atom$b  #extracts the b factor information of Chain A"
  plotb3(s1b, sse=s1.chainA, type="l", ylab="Bfactor") #plots them!
}

#example of whow it works
protein.analysis("1AKE")
```

      Note: Accessing on-line PDB file

    Warning in get.pdb(file, path = tempdir(), verbose = FALSE):
    C:\Users\daira\AppData\Local\Temp\Rtmp42Nwn1/1AKE.pdb exists. Skipping download

       PDB has ALT records, taking A only, rm.alt=TRUE

![](class6hw_files/figure-commonmark/unnamed-chunk-6-1.png)
