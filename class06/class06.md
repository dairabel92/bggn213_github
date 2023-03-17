Class06: R Functions
================
Daira M

## R Functions

In this class we will work through the process of developing out own
function for calculating average grades for fictional students in a
fictional setting. \#Student example first we will make a simplified
version of this problem. Validate that our function works. - add student
vectors

``` r
student1 <- c(100, 100, 100, 100, 100, 100, 100, 90)
student2 <- c(100, NA, 90, 90, 90, 90, 97, 80)
student3 <- c(90, NA, NA, NA, NA, NA, NA, NA)
#input: vector of grades (nums)
#output: numerical averages
```

calculate average with function `mean()`

``` r
mean(student1)
```

    [1] 98.75

then there is the min fucntion to return minimum score`min()`

``` r
min(student1)
```

    [1] 90

then there is `which.min()` that tells us where in the index

``` r
which.min(student1)
```

    [1] 8

``` r
student1[which.min(student1)]
```

    [1] 90

``` r
x <- (1:5)
x[3]
```

    [1] 3

``` r
#add a minus sign and it gives you everything BUT that
x[-3]
```

    [1] 1 2 4 5

back to students, lets use this function

``` r
student1[-which.min(student1)]
```

    [1] 100 100 100 100 100 100 100

now for average, we use the mean of this

``` r
mean(student1[-which.min(student1)])
```

    [1] 100

Lets focus on student number 2!

``` r
student2
```

    [1] 100  NA  90  90  90  90  97  80

A Very good student!

``` r
which.min(student2)
```

    [1] 8

``` r
student2[-which.min(student2)]
```

    [1] 100  NA  90  90  90  90  97

``` r
#now mean
mean(student2[-which.min(student2)], na.rm=TRUE)
```

    [1] 92.83333

Now we look at student three

``` r
student3
```

    [1] 90 NA NA NA NA NA NA NA

``` r
which.min(student3)
```

    [1] 1

``` r
student3[-which.min(student3)]
```

    [1] NA NA NA NA NA NA NA

``` r
mean(student3[-which.min(student3)], na.rm=TRUE)
```

    [1] NaN

# Replace NA with “0”

First we ask google what to do! find this `is.na()` function

``` r
is.na(student2)
```

    [1] FALSE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE

``` r
is.na(student3)
```

    [1] FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE

``` r
student2[is.na(student2)] <- 0
#now print out student 2
student2
```

    [1] 100   0  90  90  90  90  97  80

Now we are

``` r
x <- student2
x[is.na(x)] <- 0
mean(x[-which.min(x)])
```

    [1] 91

Lets go back to student three

``` r
y <- student3
y[is.na(y)] <- 0
mean(y[-which.min(y)])
```

    [1] 12.85714

# Function creation

Lets write out FUNCTION we now have working code snippet that can become
the body of a function. RECALL that all functions in R have 3 things:

1.  name (we pick)
2.  arguments (input)
3.  body (where work is done)

# lab Question

> Q1 Write a function grade() to determine an overall grade from a
> vector of student homework assignment scores dropping the lowest
> single score. If a student misses a homework (i.e. has an NA value)
> this can be used as a score to be potentially dropped

``` r
grade <- function(x) {
  #map NA values to zero
  x[is.na(x)] <- 0
  #get mean after dropping the lowest score with "-"
  mean(x[-which.min(x)])
}
```

Lets use this new function `grade()`

``` r
grade(student1)
```

    [1] 100

``` r
grade(student2)
```

    [1] 91

``` r
grade(student3)
```

    [1] 12.85714

Your final function should be adquately explained with code comments and
be able to work on an example class gradebook such as this one in CSV
format: “https://tinyurl.com/gradeinput”

Lets read this gradebook. To read the CSV file we are going to use the
`read.csv()` function, needs an input file. for first column to be names
we used the `row.names=` function.

``` r
gradebook <- read.csv("https://tinyurl.com/gradeinput", row.names=1)
head(gradebook, n=5)
```

              hw1 hw2 hw3 hw4 hw5
    student-1 100  73 100  88  79
    student-2  85  64  78  89  78
    student-3  83  69  77 100  77
    student-4  88  NA  73 100  76
    student-5  88 100  75  86  79

How do we use our function for this? we want to use the `apply()`
function to help grade all the students. The `apply()` function will
apply any function over the rows (MARGIN=1) or columns (MARGIN=2) of any
data.frame/matrix/etc.

``` r
apply(gradebook, MARGIN=1 , grade)
```

     student-1  student-2  student-3  student-4  student-5  student-6  student-7 
         91.75      82.50      84.25      84.25      88.25      89.00      94.00 
     student-8  student-9 student-10 student-11 student-12 student-13 student-14 
         93.75      87.75      79.00      86.00      91.75      92.25      87.75 
    student-15 student-16 student-17 student-18 student-19 student-20 
         78.75      89.50      88.00      94.50      82.75      82.75 

lets save this as a results

``` r
results <- apply(gradebook, MARGIN=1 , grade)
results
```

     student-1  student-2  student-3  student-4  student-5  student-6  student-7 
         91.75      82.50      84.25      84.25      88.25      89.00      94.00 
     student-8  student-9 student-10 student-11 student-12 student-13 student-14 
         93.75      87.75      79.00      86.00      91.75      92.25      87.75 
    student-15 student-16 student-17 student-18 student-19 student-20 
         78.75      89.50      88.00      94.50      82.75      82.75 

> Q2 Using your grade() function and the supplied gradebook, Who is the
> top scoring student overall in the gradebook? \[3pts\]

``` r
which.max(results)
```

    student-18 
            18 

``` r
print("Student 18 is the top scoring overall in the gradebook")
```

    [1] "Student 18 is the top scoring overall in the gradebook"

> Q3 From your analysis of the gradebook, which homework was toughest on
> students (i.e. obtained the lowest scores overall? \[2pts\]

well we could calculate mean of hw

``` r
meanhw <- apply(gradebook, MARGIN=2 , mean, na.rm=TRUE)
meanhw
```

         hw1      hw2      hw3      hw4      hw5 
    89.00000 80.88889 80.80000 89.63158 83.42105 

``` r
which.min(meanhw)
```

    hw3 
      3 

but that didn’t work, so then we could also take sum

``` r
sumhw <- apply(gradebook, MARGIN=2, sum, na.rm=TRUE)
sumhw
```

     hw1  hw2  hw3  hw4  hw5 
    1780 1456 1616 1703 1585 

``` r
which.min(sumhw)
```

    hw2 
      2 

Lets replace our NAs with zeros! because we need to mask those NA values
with mask

``` r
mask <- gradebook
mask[ is.na(mask)] <-0
head(mask, n=6)
```

              hw1 hw2 hw3 hw4 hw5
    student-1 100  73 100  88  79
    student-2  85  64  78  89  78
    student-3  83  69  77 100  77
    student-4  88   0  73 100  76
    student-5  88 100  75  86  79
    student-6  89  78 100  89  77

``` r
which.min(apply(mask, 2, mean))
```

    hw2 
      2 

``` r
print("Answer to question 3: Homework 2 is the toughest")
```

    [1] "Answer to question 3: Homework 2 is the toughest"

> Q4 Optional Extension: From your analysis of the gradebook, which
> homework was most predictive of overall score (i.e. highest
> correlation with average grade score)? \[1pt\]

why do we care? focus on assessment what is more useful for a course.
the `cor` looks at correlation. you need x and y.

``` r
#results is the average grade for students, we can go through hws.
cor(mask$hw5, results)
```

    [1] 0.6325982

``` r
cor(mask$hw2, results)
```

    [1] 0.176778

``` r
cor(mask$hw1, results)
```

    [1] 0.4250204

``` r
cor(mask$hw3, results)
```

    [1] 0.3042561

``` r
cor(mask$hw4, results)
```

    [1] 0.3810884

Can we use `apply()` function for this?

``` r
apply(mask, 2, cor, y=results)
```

          hw1       hw2       hw3       hw4       hw5 
    0.4250204 0.1767780 0.3042561 0.3810884 0.6325982 

``` r
print("Homework 5 was most predictive of overall score")
```

    [1] "Homework 5 was most predictive of overall score"
