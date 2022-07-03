Data Representation
========================================================
author: Alejandro Schuler and David Connell
date: 2022
transition: none
width: 1680
height: 1050

Based on [R for Data Science by Hadley Wickham](https://r4ds.had.co.nz/)



Introduction to the course
========================================================
type: section


Goals of this course
========================================================
By the end of the course you should be able to...

- create and index vectors of different types
- efficiently manipulate vectors

![](images/data-science.png)

Resources for this course
========================================================

![](https://r4ds.had.co.nz/cover.png)

***
## [R for Data Science (R4DS) free online book by Hadley Wickham](https://r4ds.had.co.nz)

- Vectors/lists: ch 20

## [RStudio cheatsheets](https://www.rstudio.com/resources/cheatsheets/)

- Extremely useful reference guides for functions used in this course

Vectors and data types
========================================================
type: section

Tidyverse
========================================================
As usual, let's load the tidyverse before proceeding

```r
> library(tidyverse)
── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──
✓ tibble  3.1.5     ✓ dplyr   1.0.7
✓ tidyr   1.1.4     ✓ stringr 1.4.0
✓ readr   2.0.2     ✓ forcats 0.5.1
✓ purrr   0.3.4     
── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
x dplyr::filter() masks stats::filter()
x dplyr::lag()    masks stats::lag()
```

Vector basics
========================================================
"As you dig deeper into R, you need to learn about vectors, the objects that underlie tibbles. If you’ve learned R in a more traditional way, you’re probably already familiar with vectors, as most R resources start with vectors and work their way up to tibbles. I think it’s better to start with tibbles because they’re immediately useful, and then work your way down to the underlying components" - Hadley Wickam

Vector basics
========================================================
- A vector in R is an ordered sequence of elements
- Each element has a particular **type**, e.g. string, numeric
- If all the elements in the vector are of the same type, then we call it an **atomic vector** or simply a **vector**
- If the elements are of different types, it is called a **list**, which will be discussed later

***

![](images/data-structures-overview.png)

Why care about vectors? 
========================================================
- We can already manipulate data frames with tidyverse functions

```r
> orange <- as_tibble(Orange)
> orange %>%
+   mutate(age_yrs = age/365) %>%
+   mutate(approx_age_yr = round(age_yrs)) %>%
+   group_by(approx_age_yr) %>%
+   summarize(mean_circ = mean(circumference))
# A tibble: 5 × 2
  approx_age_yr mean_circ
          <dbl>     <dbl>
1             0      31  
2             1      57.8
3             2      93.2
4             3     140. 
5             4     175. 
```

- However, each column in a dataframe is actually a **vector** and the functions used inside `summarize()` and `mutate()` (`round()` and `mean()`) operate on these vectors. 
- Since we'll be manipulating data frames all the time in biostats, we should understand a little bit about these basic building blocks.

Vector basics
========================================================
- As we have already seen, vectors can be created with the `c()` function:

```r
> shoesize <- c(9, 12, 6, 10, 10, 16, 8, 4) # integer vector
> people <- c("Vinnie", "Patricia", "Gabriel") # character (string) vector
```
- Elements of a vector can be named

```r
> c(x = 1, y = 2, z = 4)
x y z 
1 2 4 
```
- All vectors have two key properties: **length** and **type**, which you can check as follows:

```r
> typeof(people)
[1] "character"
> typeof(shoesize)
[1] "double"
> length(people)
[1] 3
```

Vector basics
========================================================
- vectors of the same type can be combined with `c()`

```r
> c(shoesize, c(4, 8, 10, 10))
 [1]  9 12  6 10 10 16  8  4  4  8 10 10
```
- Regular numbers, strings, etc. are actually all treated as vectors of length 1

```r
> 12
[1] 12
```

Logical vectors
========================================================
`%%` is the modulus operator, also known as the remainder operator. It gives you the remainder left over when you divide the thing to the left of the `%%` by the thing to the right of the `%%`.

```r
> 1:10 %% 3 == 0 # which numbers in the sequence 1:10 are divisible by 3?
 [1] FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE
> c(TRUE, TRUE, FALSE, NA) 
[1]  TRUE  TRUE FALSE    NA
```
- logical vectors can only be one of three values: `TRUE`, `FALSE`, or `NA`.

Basic logical operations
========================================================

```r
> TRUE & FALSE
[1] FALSE
> TRUE | FALSE
[1] TRUE
> !TRUE
[1] FALSE
```

- `&` is logical AND. It is true only if both arguments are true.
- `|` is logical OR. It is true if either argument is true.
- `!` is logical NOT.
- ADVANCED: see `?Logic` for more.

Numeric vectors
========================================================

```r
> sqrt(1:10)
 [1] 1.000000 1.414214 1.732051 2.000000 2.236068 2.449490 2.645751 2.828427
 [9] 3.000000 3.162278
> c(0, 1)/0
[1] NaN Inf
```
- Numeric vectors can be any number or one of three special values: 
  - `Inf` (1/0 = Infinite)
  - `NaN` (0/0 = "Not a Number")
  - `NA` (missing or "Not Available")
- R uses scientific notation so `6.023e23` evaluates as `6.023 * 10^23` 

Floating point numbers
========================================================

```r
> 9.87
[1] 9.87
> pi
[1] 3.141593
```
- Numeric vectors are typically doubles (sometimes integer)
- "double" means double-precision floating point.
- These are the computer representation of real numbers, that is,
numbers that necessarily have digits to the right of the decimal
point.
- ADVANCED: Integers take less space to store so they are preferable sometimes.
- ADVANCED: R assumes the numbers you type are floating point, even if you do not use a decimal point. R tries to avoid printing decimal points
when it can.


NA represents missing values
========================================================

```r
> 1 + NA
[1] NA
> sqrt(NA)
[1] NA
> x <- c(1, 2, NA, 4)
> 1 + x
[1]  2  3 NA  5
> sqrt(x)
[1] 1.000000 1.414214       NA 2.000000
```
- It is quite common to have data sets where some of the values
are missing.
- Missing values are marked in R with `NA` (Not Available).
- `NA` can appear in any vector, list, or data frame. In
general, when `NA` is used in an element-wise vector
operation, the corresponding element of the result will be
`NA`.


Working with missing values
========================================================

```r
> mean(c(1, 2, NA, 4))
[1] NA
> mean(c(1, 2, NA, 4), na.rm = TRUE)
[1] 2.333333
> mean(c(1, 2, 4)) # identical to the previous result
[1] 2.333333
```
For R functions that aggregate results over an entire
vector, R can either:
  
  1. return `NA` if any of the relevant arguments are `NA`
  2. or leave out the `NA` values, and process the rest

Many functions let you control `NA` processing.
- In the second example, R leaves `NA` out of the numerator and denominator in the calculation of the mean value.
- `rm` is short for "remove," so `na.rm` means "first remove the NA's
from the input, and then proceed."
- The syntax for doing this annoyingly often varies from function to function.


How to test for NA
========================================================

```r
> x <- c(1:5, NA)
> x
[1]  1  2  3  4  5 NA
> x == NA                                 # this doesn't work!
[1] NA NA NA NA NA NA
> is.na(x)
[1] FALSE FALSE FALSE FALSE FALSE  TRUE
```
- You shouldn't use `==` to check if a value is `NA`. (Why not?)
- Use `is.na` instead.
- In R, `NULL` is a special value representing a empty or
absent object. It has its own type. It is often used as a placeholder for named arguments in function definitions.
- So: `NULL` is not `NA`. `NULL` is an empty/absent value. `NA` is a missing value.

Exercise: find the missing values
========================================================

```r
> # install.packages(nycflights13)
> library(nycflights13)
```
Count the number of missing values in the `arr_delay` column of the `flights` data frame.

```r
> head(flights)
# A tibble: 6 × 19
   year month   day dep_time sched_dep_time dep_delay arr_time sched_arr_time
  <int> <int> <int>    <int>          <int>     <dbl>    <int>          <int>
1  2013     1     1      517            515         2      830            819
2  2013     1     1      533            529         4      850            830
3  2013     1     1      542            540         2      923            850
4  2013     1     1      544            545        -1     1004           1022
5  2013     1     1      554            600        -6      812            837
6  2013     1     1      554            558        -4      740            728
# … with 11 more variables: arr_delay <dbl>, carrier <chr>, flight <int>,
#   tailnum <chr>, origin <chr>, dest <chr>, air_time <dbl>, distance <dbl>,
#   hour <dbl>, minute <dbl>, time_hour <dttm>
```

Answer: Count the number of missing values
========================================================

```r
> arr_delay <- pull(flights, arr_delay) # get the vector of arr_delay values
> head(arr_delay) # look at the first few items in the vector
[1]  11  20  33 -18 -25  12
> head(is.na(arr_delay)) # is.na() produces a logical vector
[1] FALSE FALSE FALSE FALSE FALSE FALSE
> sum(is.na(arr_delay)) # sum the logical vector
[1] 9430
```
When you add up all the elements in a logical vector, TRUE = 1 and FALSE = 0, so `sum()` effectively counts all TRUE values.

Type coercion
========================================================
- Some vector types can be easily converted to other types:

```r
> as.character(shoesize)
[1] "9"  "12" "6"  "10" "10" "16" "8"  "4" 
```
- See `as.character()`, `as.logical()`, `as.numeric()`, etc.
- Coercion most often happens in the background when you use a vector in a context that is expecting a specific type:

```r
> shoe_gt_8 = shoesize > 8
> typeof(shoe_gt_8)
[1] "logical"
> sum(shoe_gt_8) # under the hood, sum(as.numeric(shoe_gt_8))
[1] 5
> mean(shoe_gt_8) # under the hood, mean(as.numeric(shoe_gt_8))
[1] 0.625
```

Testing type
========================================================
Use these to see if something is a vector or of the desired type. They all return either TRUE or FALSE
- is_logical() 
- is_integer()
- is_double()
- is_character()
- is_atomic() 
- is_list()
- is_vector()

```r
> is_double(0.14)
[1] TRUE
> is_logical(TRUE)
[1] TRUE
> is_logical(0.14)
[1] FALSE
```
- These functions are imported by `tidyverse`. Base R has its own equivalents but they are not well designed and sometimes produce surprising results. 

Subsetting a vector
========================================================
- We can get parts of a vector out by subsetting it. This is like `filter()`ing a data frame, but it looks a little different with vectors. We use `[ ]` with an index vector inside the brackets

```r
> x <- c("first"=0.3, "second"=0.1, "third"=-5, "other"=12)
> x
 first second  third  other 
   0.3    0.1   -5.0   12.0 
```
There are a few ways to subset a vector:
- with a numeric index vector of integers

```r
> x[c(1,3)]
first third 
  0.3  -5.0 
```
- with a logical index vector (of the same length)

```r
> x[c(T,F,T,T)] # T is short for TRUE, F is short for FALSE
first third other 
  0.3  -5.0  12.0 
```
- with a character index vector (if the vector is named)

```r
> x[c("first", "other")]
first other 
  0.3  12.0 
```

Indexing with integers
========================================================

```r
> x
 first second  third  other 
   0.3    0.1   -5.0   12.0 
> x[1] # same as x[c(1)] since 1 is already a vector (of length 1)
first 
  0.3 
> x[2:4]
second  third  other 
   0.1   -5.0   12.0 
> x[c(3, 1)]
third first 
 -5.0   0.3 
> x[c(1,1,1,1,1,1,4)]
first first first first first first other 
  0.3   0.3   0.3   0.3   0.3   0.3  12.0 
```
- Indexing returns a subsequence of the vector. It does not change
the original vector. Assign the result to a new variable to save it if you need it later.
- R starts counting vector indices from 1.
- You can index using a multi-element index vector.
- You can repeat index positions.

Indexing with integers (negatives)
========================================================

```r
> x
 first second  third  other 
   0.3    0.1   -5.0   12.0 
> x[1] # choose the first element only
first 
  0.3 
> x[-1] # remove the first element
second  third  other 
   0.1   -5.0   12.0 
> x[-length(x)] # remove the last element
 first second  third 
   0.3    0.1   -5.0 
> x[c(-1, -length(x))] # remove the first and last elements
second  third 
   0.1   -5.0 
```
- You can't mix positive and negative vector indices in a single
index expression. R will complain.
- What about using `0` as an index? It is ignored.


Indexing with logicals
========================================================

```r
> x
 first second  third  other 
   0.3    0.1   -5.0   12.0 
> x >= 0
 first second  third  other 
  TRUE   TRUE  FALSE   TRUE 
> x[x >= 0]
 first second  other 
   0.3    0.1   12.0 
```
- Logical values are either `TRUE` or `FALSE`.
- They are typically produced by using a comparison operator or
similar test.
- The logical index vector should be the same length as the vector being subsetted

Comparing tidyverse vs. vector indexing
====================================

```r
> df <- tibble(x=x, y=rnorm(4), z=rnorm(4))
```
**Tidyverse**
- The `%>%` pipe operator passes the result of the preceding function to the next functions

```r
> filter(df, x>0) %>% select(x,y)
# A tibble: 3 × 2
      x      y
  <dbl>  <dbl>
1   0.3 -0.225
2   0.1 -0.567
3  12    0.241
```
**Vector indexing**

```r
> df[df$x>0, c(1,2)] # df$x takes the column x from the data frame df (details later)
# A tibble: 3 × 2
      x      y
  <dbl>  <dbl>
1   0.3 -0.225
2   0.1 -0.567
3  12    0.241
```
- What are the advantages/disadvantages of each?

Tidyverse vs. vector indexing
====================================

- **Tidyverse**:
  - Operations are ordered top-to-bottom/left-to-right in the way that they are being performed so the code can be read like natural language
  - Each operation gets its own line, which facilitates finding bugs
  - Functions are named sensibly so the code can be understood without knowledge of symbols like `$` or `[`. Even `%>%` is made to look like an arrow to suggest the left-to-right flow
  - Variables are always referred to as bare names (no quotes or `$`)
  - Will be easier for you to skim over and understand in 6 months
- **Vector indexing**
  - Fewer keystrokes
  - More familiar to programmers from C++ or python

***

**Tidyverse**

```r
> df %>% 
>   filter(x>0) %>%
>   select(x,y)
```
**Vector indexing**

```r
> df[df$x>0, c(1,2)] 
```


Wrap-up
========================================================
type: section

Goals of this course
========================================================
By the end of the course you should be able to...

- create and index vectors of different types
- efficiently manipulate vectors

Resources for this course
========================================================

![](https://r4ds.had.co.nz/cover.png)

***
## [R for Data Science (R4DS) free online book by Hadley Wickham](https://r4ds.had.co.nz)

- Vectors/lists: ch 20

## [RStudio cheatsheets](https://www.rstudio.com/resources/cheatsheets/)

- Extremely useful reference guides for functions used in this course
