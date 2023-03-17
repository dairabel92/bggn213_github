Class 09 - Protein Structures
================
Daira

Intro to PDB bank!

``` r
pdbstats <- read.csv("Data Export Summary.csv", row.names=1)
knitr::kable(pdbstats)
```

|                         | X.ray   | EM    | NMR    | Multiple.methods | Neutron | Other | Total   |
|:------------------------|:--------|:------|:-------|-----------------:|--------:|------:|:--------|
| Protein (only)          | 152,914 | 9,495 | 12,121 |              191 |      72 |    32 | 174,825 |
| Protein/Oligosaccharide | 9,008   | 1,663 | 32     |                7 |       1 |     0 | 10,711  |
| Protein/NA              | 8,069   | 2,949 | 282    |                6 |       0 |     0 | 11,306  |
| Nucleic acid (only)     | 2,602   | 78    | 1,434  |               12 |       2 |     1 | 4,129   |
| Other                   | 163     | 9     | 31     |                0 |       0 |     0 | 203     |
| Oligosaccharide (only)  | 11      | 0     | 6      |                1 |       0 |     4 | 22      |

> Q1: What percentage of structures in the PDB are solved by X-Ray and
> Electron Microscopy.

``` r
# pdbstats <- sum(pdbstats$X.ray)/sum(pdbstats$EM)
#did not work issue it is not reading it by number
#use a function for help `as.numeric`
# as.numeric(pdbstats)
#still didnt work because the commas
#get rid of the commas
```

The commas are causing issues so we go on the internet, use `gsub` to
replace commas with nothing in as. numeric. Use gsub\~

``` r
#?gsub
gsub(",", "", pdbstats$X.ray)
```

    [1] "152914" "9008"   "8069"   "2602"   "163"    "11"    

``` r
as.numeric(gsub(",", "", pdbstats$X.ray))
```

    [1] 152914   9008   8069   2602    163     11

``` r
n.xray <- sum(as.numeric(gsub(",", "", pdbstats$X.ray)))
```

``` r
#now fix the total 
n.total <- sum(as.numeric(gsub(",", "", pdbstats$Total)))
#now for the EM
n.EM <- sum(as.numeric(gsub(",", "", pdbstats$EM)))
```

``` r
#now to do the math
xray_percentage <- round(n.xray/n.total * 100)
em_percentage <- round(n.EM/n.total * 100)
xray_percentage
```

    [1] 86

``` r
em_percentage
```

    [1] 7

A: 86% of structures are solved by x-ray, and 7% are solved by Electron
Microscopy.

Question 2 time

``` r
#lets make it easy, write a function
char2num <- function(type) {
  sum( as.numeric( gsub(",", "", type)))
}
```

``` r
n.xray <- char2num(pdbstats$X.ray)
n.em <- char2num(pdbstats$EM)
n.total <- char2num(pdbstats$Total)
n.nmr <- char2num(pdbstats$NMR)
```

``` r
#barrys function
rm_comman_sum <- function(x) {
  sum( as.numeric( gsub(",", "", x)))
}

rm_comman_sum(pdbstats$X.ray)
```

    [1] 172767

> Q2: What proportion of structures in the PDB are protein?

``` r
rm_comman_sum(pdbstats$Total[1]) / n.total
```

    [1] 0.8689288

A: .86 or 86% of the strucures in the PDB are protein

> Q3: Type HIV in the PDB website search box on the home page and
> determine how many HIV-1 protease structures are in the current PDB?

“hint go online” A: It returns around 201,000 results if you look up
HIV-1 protease,but you can also edit your query to be more specific and
the number of hits decreases.

# USE MOL\*

its a webbased molecular viewer

Here is an image from it

![A rendering of HIV-1 Protease with an important drug molecule, PBD
CODE:1HSG](1HSG.png)

> Q4: Water molecules normally have 3 atoms. Why do we see just one atom
> per water molecule in this structure?

A: the angstrom resolution is not good enough to see the hydrogens so we
only see one atom per water molecule.

> Q5: There is a critical “conserved” water molecule in the binding
> site. Can you identify this water molecule? What residue number does
> this water molecule have

A: Yes, we can identify and the name is: H308

> Q6: Generate and save a figure clearly showing the two distinct chains
> of HIV-protease along with the ligand. You might also consider showing
> the catalytic residues ASP 25 in each chain and the critical water (we
> recommend “Ball & Stick” for these side-chains). Add this figure to
> your Quarto document.

A: done see above (above question 4)

> Q7: \[Optional\] As you have hopefully observed HIV protease is a
> homodimer (i.e. it is composed of two identical chains). With the aid
> of the graphic display can you identify secondary structure elements
> that are likely to only form in the dimer rather than the monomer?

A: Y yes we found the secondary structures, the 21 D ()

\#lets do some bioinformatics with these structures

We are going to use the `bio3d` package

``` r
library(bio3d)

#lets read a pdb file
p <- read.pdb("1hsg")
```

      Note: Accessing on-line PDB file

``` r
p
```


     Call:  read.pdb(file = "1hsg")

       Total Models#: 1
         Total Atoms#: 1686,  XYZs#: 5058  Chains#: 2  (values: A B)

         Protein Atoms#: 1514  (residues/Calpha atoms#: 198)
         Nucleic acid Atoms#: 0  (residues/phosphate atoms#: 0)

         Non-protein/nucleic Atoms#: 172  (residues: 128)
         Non-protein/nucleic resid values: [ HOH (127), MK1 (1) ]

       Protein sequence:
          PQITLWQRPLVTIKIGGQLKEALLDTGADDTVLEEMSLPGRWKPKMIGGIGGFIKVRQYD
          QILIEICGHKAIGTVLVGPTPVNIIGRNLLTQIGCTLNFPQITLWQRPLVTIKIGGQLKE
          ALLDTGADDTVLEEMSLPGRWKPKMIGGIGGFIKVRQYDQILIEICGHKAIGTVLVGPTP
          VNIIGRNLLTQIGCTLNF

    + attr: atom, xyz, seqres, helix, sheet,
            calpha, remark, call

lets look at atom

``` r
head(p$atom)
```

      type eleno elety  alt resid chain resno insert      x      y     z o     b
    1 ATOM     1     N <NA>   PRO     A     1   <NA> 29.361 39.686 5.862 1 38.10
    2 ATOM     2    CA <NA>   PRO     A     1   <NA> 30.307 38.663 5.319 1 40.62
    3 ATOM     3     C <NA>   PRO     A     1   <NA> 29.760 38.071 4.022 1 42.64
    4 ATOM     4     O <NA>   PRO     A     1   <NA> 28.600 38.302 3.676 1 43.40
    5 ATOM     5    CB <NA>   PRO     A     1   <NA> 30.508 37.541 6.342 1 37.87
    6 ATOM     6    CG <NA>   PRO     A     1   <NA> 29.296 37.591 7.162 1 38.40
      segid elesy charge
    1  <NA>     N   <NA>
    2  <NA>     C   <NA>
    3  <NA>     C   <NA>
    4  <NA>     O   <NA>
    5  <NA>     C   <NA>
    6  <NA>     C   <NA>

``` r
#you can see the heart and soul of the structure all the atoms
```

> Q What is the residue for the atom

``` r
#get ResID for first atom
p$atom[1,"resid"]
```

    [1] "PRO"

``` r
p$atom$resid[1]
```

    [1] "PRO"

Normal Mode NMA

``` r
adk <- read.pdb("6s36")
```

      Note: Accessing on-line PDB file
       PDB has ALT records, taking A only, rm.alt=TRUE

``` r
adk
```


     Call:  read.pdb(file = "6s36")

       Total Models#: 1
         Total Atoms#: 1898,  XYZs#: 5694  Chains#: 1  (values: A)

         Protein Atoms#: 1654  (residues/Calpha atoms#: 214)
         Nucleic acid Atoms#: 0  (residues/phosphate atoms#: 0)

         Non-protein/nucleic Atoms#: 244  (residues: 244)
         Non-protein/nucleic resid values: [ CL (3), HOH (238), MG (2), NA (1) ]

       Protein sequence:
          MRIILLGAPGAGKGTQAQFIMEKYGIPQISTGDMLRAAVKSGSELGKQAKDIMDAGKLVT
          DELVIALVKERIAQEDCRNGFLLDGFPRTIPQADAMKEAGINVDYVLEFDVPDELIVDKI
          VGRRVHAPSGRVYHVKFNPPKVEGKDDVTGEELTTRKDDQEETVRKRLVEYHQMTAPLIG
          YYSKEAEAGNTKYAKVDGTKPVAEVRADLEKILG

    + attr: atom, xyz, seqres, helix, sheet,
            calpha, remark, call

``` r
m <- nma(adk)
```

     Building Hessian...        Done in 0.01 seconds.
     Diagonalizing Hessian...   Done in 0.19 seconds.

``` r
plot(m)
```

![](PDB_files/figure-commonmark/unnamed-chunk-14-1.png)

Make a viz for MOlstar

``` r
#make a trajectory file
mktrj(m, file="adk_m7.pdb")
```

> Q7: How many amino acid residues are there in this pdb object?

A: There are 198 residues of amino acids in this object

> Q8: Name one of the two non-protein residues?

A: One of the two non-protein is HOH 127

> Q9: How many protein chains are in this structure?

A: There are two chains (A and B)

\#now we are doing alignments

``` r
#use
#get.seq() #add input databse
#blast.pdb() 
#get.pdb()
#alignment 
#pdbaln()
#PCA
#pca()
```

Lets do it!

``` r
#BiocManager::install("msa")
#devtools::install_bitbucket("Grantlab/bio3d-view")
```

> Q10. Which of the packages above is found only on BioConductor and not
> CRAN?

A: msa

> Q11. Which of the above packages is not found on BioConductor or CRAN?

A: bio3d-view

> Q12. True or False? Functions from the devtools package can be used to
> install packages from GitHub and BitBucket?

A: TRUE

``` r
library(bio3d)
aa <- get.seq("1ake_A")
```

    Warning in get.seq("1ake_A"): Removing existing file: seqs.fasta

    Fetching... Please wait. Done.

``` r
aa
```

                 1        .         .         .         .         .         60 
    pdb|1AKE|A   MRIILLGAPGAGKGTQAQFIMEKYGIPQISTGDMLRAAVKSGSELGKQAKDIMDAGKLVT
                 1        .         .         .         .         .         60 

                61        .         .         .         .         .         120 
    pdb|1AKE|A   DELVIALVKERIAQEDCRNGFLLDGFPRTIPQADAMKEAGINVDYVLEFDVPDELIVDRI
                61        .         .         .         .         .         120 

               121        .         .         .         .         .         180 
    pdb|1AKE|A   VGRRVHAPSGRVYHVKFNPPKVEGKDDVTGEELTTRKDDQEETVRKRLVEYHQMTAPLIG
               121        .         .         .         .         .         180 

               181        .         .         .   214 
    pdb|1AKE|A   YYSKEAEAGNTKYAKVDGTKPVAEVRADLEKILG
               181        .         .         .   214 

    Call:
      read.fasta(file = outfile)

    Class:
      fasta

    Alignment dimensions:
      1 sequence rows; 214 position columns (214 non-gap, 0 gap) 

    + attr: id, ali, call

> Q13. How many amino acids are in this sequence, i.e. how long is this
> sequence?

A: 214 amino acids

``` r
#now search argainst pdb databse
#b <- blast.pdb(aa)
#plot(b)
```

``` r
#hits <- plot(b)
```

``` r
#hits$pdb.id
```

Now we will download all the strucrures

``` r
#download related pbd files
#files <- get.pdb(hits$pdb.id, path="pdbs", split=TRUE, gzip=TRUE)
```

\#Align and superpose

``` r
#pdbs <- pdbaln(files, fit = TRUE, exefile="msa")
#see the output
#pdbs
```

``` r
# Vector containing PDB codes for figure axis
#ids <- basename.pdb(pdbs$id)

# Draw schematic alignment
#plot(pdbs, labels=ids)
```

``` r
#pc.xray <- pca(pdbs)
#plot(pc.xray)
```

\#visualize the trajectory

``` r
#mktrj(pc.xray, pc=1, file="pc_1.pdb")
```

``` r
# Calculate RMSD
#rd <- rmsd(pdbs)

# Structure-based clustering
#hc.rd <- hclust(dist(rd))
#grps.rd <- cutree(hc.rd, k=3)

#plot(pc.xray, 1:2, col="grey50", bg=grps.rd, pch=21, cex=1)
```

# Normal mode analysis

> Q14. What do you note about this plot? Are the black and colored lines
> similar or different? Where do you think they differ most and why?

A: The plot is able to show us the different pbd. residues in reference
to 1AKE_A . The lies are different because they have different
fluctuations. They differ the most between 100 and 150.
