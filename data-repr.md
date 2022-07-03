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
- efficiently manipulate factors and vectors

![](images/data-science.png)

Resources for this course
========================================================

![](https://r4ds.had.co.nz/cover.png)

***
## [R for Data Science (R4DS) free online book by Hadley Wickham](https://r4ds.had.co.nz)

- Vectors/lists: ch 20
- Factors: ch 15
- Reading data: ch 11

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
> x[1]
first 
  0.3 
> x[-1]
second  third  other 
   0.1   -5.0   12.0 
> x[-length(x)]
 first second  third 
   0.3    0.1   -5.0 
> x[c(-1, -length(x))]
second  third 
   0.1   -5.0 
```
- You can't mix positive and negative vector indices in a single
index expression. R will complain.
- This is similar to the `df %>% select(-var)` syntax
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

Indexing 2D objects
========================================================
Similar syntax is used for 2D entities

```r
> my_example_matrix[1:3, c(2,2)]
Error in eval(expr, envir, enclos): object 'my_example_matrix' not found
```
- the general pattern is `matrix[row_index, column_index]`.
- leaving either blank returns all rows or columns

```r
> my_example_matrix[1:3,]
Error in eval(expr, envir, enclos): object 'my_example_matrix' not found
> my_example_matrix[,c(T,T,F,F,T)]
Error in eval(expr, envir, enclos): object 'my_example_matrix' not found
```

Comparing tidyverse vs. vector indexing
====================================

```r
> df <- tibble(x=x, y=rnorm(4), z=rnorm(4))
```
**Tidyverse**

```r
> df %>% 
+   filter(x>0) %>%
+   select(x,y)
# A tibble: 3 × 2
      x      y
  <dbl>  <dbl>
1   0.3 0.415 
2   0.1 2.23  
3  12   0.0329
```
**Vector indexing**

```r
> df[df$x>0, c(1,2)] # df$x takes the column x from the data frame df (details later)
# A tibble: 3 × 2
      x      y
  <dbl>  <dbl>
1   0.3 0.415 
2   0.1 2.23  
3  12   0.0329
```
- What are the advantages/disadvantages of each?

Tidyverse vs. vector indexing
====================================

- **Tidyverse**:
  - operations are ordered top-to-bottom/left-to-right in the way that they are being performed so the code can be read like natural language
  - each operation gets its own line, which facilitates finding bugs
  - functions are named sensibly so the code can be understood without knowledge of symbols like `$` or `[`. Even `$>$` is made to look like an arrow to suggest the left-to-right flow
  - variables are always referred to as bare names (no quotes or `$`)
  - will be easier for you to skim over and understand in 6 months
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

Working with strings
========================================================
type: section

String basics
========================================================

```r
> c("1234", "sum(c(1,2,3,4))", "Alejandro", "a long string with weird characters!@#$!%>?", "NA", NA) 
[1] "1234"                                       
[2] "sum(c(1,2,3,4))"                            
[3] "Alejandro"                                  
[4] "a long string with weird characters!@#$!%>?"
[5] "NA"                                         
[6] NA                                           
```
- character vectors (or string vectors) store strings, which are arbitrary text (including spaces) or `NA` 
- even if the text can be interepreted by you as code or numbers, quoting it in `""` tells `R` that it is to be taken literally as text (note `"NA"` is a string, while `NA` is a special value that indicates a missing string if it is in a string vector)

stringr
========================================================
- Many of the functions we will use to work with strings come from the `stringr` package, which is `tidyverse`-associated, but not loaded with `library(tidyverse)`

```r
> library(stringr)
```

String basics
========================================================
- Strings are created by quoting text with `" "` or `' '`. I always use `" "` to be consistent.

```r
> string1 <- "This is a string"
```
- special characters (see `?"'"`) can be included by "escaping" them with `\`. `\\` is a literal backslash.

```r
> string2_wrong = "This is a "string""
Error: <text>:1:29: unexpected symbol
1: string2_wrong = "This is a "string
                                ^
```

```r
> string2_right = "This is a \"string\""
```
- check the length of a string with `str_length()` (why not `length()`?)

```r
> x = "This is a string"
> str_length(x)
[1] 16
> length(x)
[1] 1
```

Combining strings
===
- Use `str_c()` to combine strings

```r
> str_c("x", "y", "z")
[1] "xyz"
```
- `sep` controls what gets stuck between them (`""` by default)

```r
> str_c("x", "y", "z", sep = ", ")
[1] "x, y, z"
```
- also works with vectors of strings

```r
> str_c("prefix-", c("a", "b", "c"), "-suffix")
[1] "prefix-a-suffix" "prefix-b-suffix" "prefix-c-suffix"
> str_c(c("1", "2", "3"), c("a", "b", "c"), sep="-")
[1] "1-a" "2-b" "3-c"
```
- and with a single vector, if you set the `collapse` argument

```r
> name = "Alejandro"
> x = c("Good afternoon,", name, "how are you?")
> str_c(x, collapse=" ")
[1] "Good afternoon, Alejandro how are you?"
```

Subsetting 
===
- str_sub() does subsetting by start and end letters
- these can be negative numbers to count from the end of the string backwards

```r
> str_sub("Hello world", start=1, end=5)
[1] "Hello"
> str_sub("Hello world", start=-5, end=-1)
[1] "world"
```
- Also works on vectors

```r
> x <- c("Apple", "Banana", "Pear")
> str_sub(x, 1, 3)
[1] "App" "Ban" "Pea"
```

Searching in strings
===
- `str_detect()` tells you if the query string is in the string (or vector of strings) you're looking at

```r
> x <- c("apple", "banana", "pear")
> str_detect(x, "e")
[1]  TRUE FALSE  TRUE
```

- `str_subset()` returns the strings in the vector that match the query

```r
> str_subset(x, "e") # compare to x[str_detect(x,"e")]
[1] "apple" "pear" 
```

Exercise: find rows according to string match
===

```r
> tibble(
>   word = words, # words is an example string vector loaded by stringr
>   i = seq_along(word) # seq_along(x) is the same as 1:length(x)
> )
```
- Make this data fram for yourself
- Write code that returns all of rows with words that contain `"tr"`

Answer: find rows according to string match
===

```r
> tibble(
+   word = words, # words is an example string vector loaded by stringr
+   i = seq_along(word) # seq_along(x) is the same as 1:length(x)
+ ) %>%
+   filter(str_detect(word, "tr"))
# A tibble: 26 × 2
   word          i
   <chr>     <int>
 1 centre      136
 2 contract    187
 3 control     188
 4 country     198
 5 district    234
 6 electric    264
 7 extra       295
 8 industry    424
 9 introduce   431
10 straight    811
# … with 16 more rows
```

Counting the number of matches
===
- `str_count()` counts how many times the query appears in the string

```r
> x <- c("apple", "banana", "pear")
> str_count(x, "a")
[1] 1 3 1
```
- it pairs naturally with `mutate()`

```r
> tibble(
+     word = words, 
+     i = seq_along(word) ) %>%
+   mutate(count_e = str_count(word, "e")) %>%
+   arrange(desc(count_e)) %>%
+   head()
# A tibble: 6 × 3
  word           i count_e
  <chr>      <int>   <int>
1 experience   292       4
2 believe       86       3
3 between       90       3
4 degree       221       3
5 difference   229       3
6 eleven       265       3
```

Replacing parts of a string
===
- sometimes you want to remove something from a string or replace it with something else.
- `str_replace_all()` lets you do this

```r
> str_replace_all(x, "a", "-")
[1] "-pple"  "b-n-n-" "pe-r"  
```
- use `""` as the replacement string to delete the part of the string that matches

```r
> str_replace_all(x, "a", "-")
[1] "-pple"  "b-n-n-" "pe-r"  
```
- you can also replace multiple strings at a time using a named vector

```r
> x <- c("1 house", "2 cars", "3 people")
> replacement_dictionary = c("1" = "one", "2" = "two", "3" = "three")
> str_replace_all(x, replacement_dictionary)
[1] "one house"    "two cars"     "three people"
```

Splitting up a string
===
- `str_split()` splits a string into multiple strings

```r
> str_split("hello, how are you today?", " ")
[[1]]
[1] "hello," "how"    "are"    "you"    "today?"
```
- it returns a list, which is like a vector that can contain other vectors or arbitrary length as elements (more on this later). This allows it to operate on multiple strings at once without mashing the results together

```r
> greeting = c("hello, how are you today?", 
+              "I'm fine, thank you")
> str_split(greeting, " ")
[[1]]
[1] "hello," "how"    "are"    "you"    "today?"

[[2]]
[1] "I'm"   "fine," "thank" "you"  
```
- we'll learn more about how to operate on lists in the functional programming course

Matching complicated patterns: regular expressions
===
- sometimes you want to match a pattern that is more complicated than a fixed string. For instance, how would you find all strings that have a vowel in them? Or match an email address?
- **regular expressions** (regexps) are a concise way of solving this problem, but they aren't pretty
- don't try and memorize all this, just get an idea of what's possible and then look at the cheat sheet later
- a regular expression is a string that is interpreted in a particular way as a query
- all of the `str_` functions we've talked about take regular expressions as queries. 

```r
> x <- c("apple", "banana", "pear")
> str_subset(x, "an")
[1] "banana"
```

- The `.` matches any character

```r
> str_subset(x, ".a.")
[1] "banana" "pear"  
```

Repetition
===

```r
> babs = c("bb", "bab", "baab", "baaab")
```
- + after a character means match that character one or more times

```r
> str_subset(babs, "ba+b")
[1] "bab"   "baab"  "baaab"
```
- * after a character means match that character zero or more times

```r
> str_subset(babs, "ba*b")
[1] "bb"    "bab"   "baab"  "baaab"
```
- ? after a character means match that character zero or one time

```r
> str_subset(babs, "ba?b")
[1] "bb"  "bab"
```
- {n,m} after a character means match between n and m repetitions

```r
> str_subset(babs, "ba{2,3}b")
[1] "baab"  "baaab"
```

Anchors
=== 
- anchors tell the regexp to look at particular places in the string
- `^` matches the beginning
- `$` matches the end

```r
> x <- c("apple", "banana", "pear")
> str_subset(x, "^a")
[1] "apple"
> str_subset(x, "a$")
[1] "banana"
```

Character classes
===
- Besides `.`, here are some other special sequences that match categories of characters
- `\\d`: matches any digit.
- `\\s`: matches any whitespace (e.g. space, tab, newline).
- `[abc]`: matches a, b, or c.
- `[a-zA-Z]`: matches any  letter
- `[^abc]`: matches anything except a, b, or c.

```r
> str_subset(c("abc", "xyz"), "[aeiou]")
[1] "abc"
```

Exercise: what does this regex match?
===
`"^[a-zA-Z]+\\d+[a-zA-Z]+$"`

Answer: what does this regex match?
===
`"^[a-zA-Z]+\\d+[a-zA-Z]+$"` matches any string starting with a letter and ending a letter, but containing at least one number

```r
> test_strs = c("c12341aaa", "123aa", "ba123", "asf  asdf", "G5s")
> str_subset(test_strs, "^[a-zA-Z]+\\d+[a-zA-Z]+$")
[1] "c12341aaa" "G5s"      
```

Escaping special characters
===
- `\\` is used to escape special characters in regexps so that they can be interpreted literally

```r
> str_extract(c("abc", "a.c", "bef"), "a\\.c")
[1] NA    "a.c" NA   
```
- It takes two `\\` because a regex is a string. Thus the first `\` puts a literal `\` in the string, which is then interpreted as an escape character by the regex engine.

Extracting matches
===
- `str_extract()` will get the portion of the string that matches your regex

```r
> emails = c("karina@stanford.edu", "nathalie@gmail.com", "carlos@kp.org")
> str_extract(emails, "@.*")
[1] "@stanford.edu" "@gmail.com"    "@kp.org"      
```

Exercise: domain names
===

```r
> emails = c("karina@stanford.edu", "nathalie@gmail.com", "carlos@kp.org")
```
Get the first part of the domain name out of these email addresses (for instance `kp` from `carlos@kp.org`)

Exercise: domain names
===

```r
> emails = c("karina@stanford.edu", "nathalie@gmail.com", "carlos@kp.org")
```
Get the first domain names out of these email addresses (for instance `kp` from `carlos@kp.org`)

```r
> emails %>%
+   str_extract("@.*\\.") %>%
+   str_sub(2,-2)
[1] "stanford" "gmail"    "kp"      
```

Look-arounds
===
- Sometimes you want to check if a string matches a pattern and then only return part of that pattern.
- For example, if you're looking for history of disease, you may want the disease, but not the term "history of"

```r
> notes = c("... patient has history of MI ...",
+           "... family history of diabetes ...",
+           "... patient has nausea ... ")
> str_extract(notes, "(?<=history of )[a-zA-Z]+(?= )")
[1] "MI"       "diabetes" NA        
```
- `(?=...)`: positive look-ahead assertion. Matches if ... matches at the current input.
- `(?!...)`: negative look-ahead assertion. Matches if ... does not match at the current input.
- `(?<=...)`: positive look-behind assertion. Matches if ... matches text preceding the current position, with the last character of the match being the character just before the current position. 
- `(?<!...)`: negative look-behind assertion. Matches if ... does not match text preceding the current position.

Regex is a broad topic
===
- There is a lot more to learn. This is a regex that matches valid email addresses:

```
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9]))\.){3}(?:(2(5[0-5]|[0-4][0-9])|1[0-9][0-9]|[1-9]?[0-9])|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\])
```
- But you can go very far with very little!
- Regex is not just in R, it is used across almost every programming language

stringr cheat sheet
===
<div align="center">
<img src="https://www.rstudio.com/wp-content/uploads/2018/08/strings.png", height=1000, width=1400>
</div>

Factors
===
type: section

Factor basics
===
- factors are R's representation of variables that can take values in a limited number of categoires
- the `tidyverse`-adjacent `forcats` (forcats = for-categoricals, also an anagram of factor!) package has useful functions for working with them

```r
> library(forcats)
```
- consider a variable that stores a month of the year:

```r
> months_vec <- c("Dec", "Apr", "Jan", "Mar")
```
- if you sort this vector, it sorts alphabetically, not by the order of the months
- you can accidentally add months that aren't legitimate: `c(months_vec, "Jam")`
- you can make a factor by hand with `factor()`

```r
> month_levels <- c(
+   "Jan", "Feb", "Mar", "Apr", "May", "Jun", 
+   "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
+ )
> factor(months_vec, levels = month_levels)
[1] Dec Apr Jan Mar
Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
```

Factor levels
===

```r
> month_levels <- c(
+   "Jan", "Feb", "Mar", "Apr", "May", "Jun", 
+   "Jul", "Aug", "Sep", "Oct", "Nov", "Dec"
+ )
> factor(months_vec, levels = month_levels)
[1] Dec Apr Jan Mar
Levels: Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec
```
- the "levels" of the factor are the values that it is allowed to take as well as the order that it should be sorted in if desired
- `levels()` returns a character vector of the factor levels
- if the levels are not supplied, it takes the values in the vector as the levels in alphabetical order

```r
> factor(months_vec)
[1] Dec Apr Jan Mar
Levels: Apr Dec Jan Mar
```
- you can set the order of the levels to be by the order in which they appear in the vector using `fct_inorder()`

```r
> factor(months_vec) %>%
+   fct_inorder()
[1] Dec Apr Jan Mar
Levels: Dec Apr Jan Mar
```

Example: gss_cat
===
- This is an example data frame loaded by `forcats` that has many factor columns, which are identifiable by the `<fct>` tag
- It’s a sample of data from the General Social Survey, which is a long-running US survey conducted by the independent research organization NORC at the University of Chicago.

```r
> gss_cat
# A tibble: 21,483 × 9
    year marital         age race  rincome        partyid  relig  denom  tvhours
   <int> <fct>         <int> <fct> <fct>          <fct>    <fct>  <fct>    <int>
 1  2000 Never married    26 White $8000 to 9999  Ind,nea… Prote… South…      12
 2  2000 Divorced         48 White $8000 to 9999  Not str… Prote… Bapti…      NA
 3  2000 Widowed          67 White Not applicable Indepen… Prote… No de…       2
 4  2000 Never married    39 White Not applicable Ind,nea… Ortho… Not a…       4
 5  2000 Divorced         25 White Not applicable Not str… None   Not a…       1
 6  2000 Married          25 White $20000 - 24999 Strong … Prote… South…      NA
 7  2000 Never married    36 White $25000 or more Not str… Chris… Not a…       3
 8  2000 Divorced         44 White $7000 to 7999  Ind,nea… Prote… Luthe…      NA
 9  2000 Married          44 White $25000 or more Not str… Prote… Other        0
10  2000 Married          47 White $25000 or more Strong … Prote… South…       3
# … with 21,473 more rows
```


```r
> gss_cat %>%
+   pull(marital) %>%
+   levels()
[1] "No answer"     "Never married" "Separated"     "Divorced"     
[5] "Widowed"       "Married"      
```

Ordering factor levels
===
- It’s often useful to change the order of the factor levels in a visualisation
- For example, imagine you want to explore the average number of hours spent watching TV per day across religions

```r
> relig_tv = gss_cat %>%
+   group_by(relig) %>%
+   summarise(
+     age = mean(age, na.rm = TRUE),
+     tvhours = mean(tvhours, na.rm = TRUE),
+     n = n())
> ggplot(relig_tv, aes(tvhours, relig)) + 
+   geom_point()
```

![plot of chunk unnamed-chunk-83](data-repr-figure/unnamed-chunk-83-1.png)

Ordering factor levels
===
- It would be better if the religions in this plot were ordered according to the number of TV hours

```r
> relig_tv %>%
+   mutate(relig = fct_reorder(relig, tvhours)) %>%
+ ggplot(aes(tvhours, relig)) + 
+   geom_point()
```

![plot of chunk unnamed-chunk-84](data-repr-figure/unnamed-chunk-84-1.png)
- `fct_reorder()` takes in a factor vector and a numeric vector that is used to sort the levels

Ordering factor levels
===
- `fct_infreq()` orders by how often the levels appear in the factor vector
- `fct_rev()` reverses the order

```r
> gss_cat %>%
+   mutate(marital = marital %>% 
+         fct_infreq() %>% 
+         fct_rev()) %>%
+ ggplot(aes(marital)) +
+   geom_bar()
```

![plot of chunk unnamed-chunk-85](data-repr-figure/unnamed-chunk-85-1.png)

Recoding factor levels
===

```r
> gss_cat %>% 
+   group_by(partyid) %>%
+   summarize(count=n()) %>%
+   arrange(desc(count))
# A tibble: 10 × 2
   partyid            count
   <fct>              <int>
 1 Independent         4119
 2 Not str democrat    3690
 3 Strong democrat     3490
 4 Not str republican  3032
 5 Ind,near dem        2499
 6 Strong republican   2314
 7 Ind,near rep        1791
 8 Other party          393
 9 No answer            154
10 Don't know             1
```
- These factor levels are terrible
- we can change them with `fct_recode()`

Recoding factor levels
===

```r
> gss_cat %>%
+   mutate(partyid = fct_recode(partyid,
+     "Republican, strong"    = "Strong republican",
+     "Republican, weak"      = "Not str republican",
+     "Independent, near rep" = "Ind,near rep",
+     "Independent, near dem" = "Ind,near dem",
+     "Democrat, weak"        = "Not str democrat",
+     "Democrat, strong"      = "Strong democrat"
+   )) %>%
+   pull(partyid) %>%
+   levels()
 [1] "No answer"             "Don't know"            "Other party"          
 [4] "Republican, strong"    "Republican, weak"      "Independent, near rep"
 [7] "Independent"           "Independent, near dem" "Democrat, weak"       
[10] "Democrat, strong"     
```
- Levels that you don't mention are left alone
- You can code multiple old levels to one new level (also see `?fct_collapse`)

Exercise: plotting factors over time
===
How have the proportions of people identifying as Democrat, Republican, and Independent changed over time? Make a plot to explore. `geom_bar()` may be useful. `scale_fill_manual()` may also be useful to associate parties with their familiar colors.

Answer: plotting factors over time
===
How have the proportions of people identifying as Democrat, Republican, and Independent changed over time? Make a plot to explore. `fct_collapse()` and `geom_bar()` may be useful. `scale_fill_manual()` may also be useful to associate parties with their familiar colors.

```r
> plot_data = gss_cat %>%
+   mutate(partyid = fct_collapse(partyid,
+     Democrat = c("Not str democrat", "Strong democrat"),
+     Republican = c("Not str republican", "Strong republican"),
+     Independent = c("Independent", "Ind,near rep", "Ind,near dem"),
+     Other = c("Other party", "Don't know", "No answer"))) %>%
+   group_by(partyid, year) %>%
+   summarize(count = n()) %>%
+   mutate(proportion = count/sum(count)) 
`summarise()` has grouped output by 'partyid'. You can override using the `.groups` argument.
> 
> party_colors = c(
+   "Democrat" = "blue",
+   "Republican" = "red",
+   "Independent" = "purple",
+   "Other" = "grey"
+ )
```

Answer: plotting factors over time
===
How have the proportions of people identifying as Democrat, Republican, and Independent changed over time? Make a plot to explore. `geom_bar()` may be useful. `scale_fill_manual()` may also be useful to associate parties with their familiar colors.

```r
> plot_data %>%
+ ggplot(aes(x=year, y=proportion, fill=partyid)) + 
+   geom_bar(position = "fill", stat = "identity") + 
+   scale_fill_manual(values = party_colors)
```

![plot of chunk unnamed-chunk-89](data-repr-figure/unnamed-chunk-89-1.png)

Dates and times
=== 
type: section

Date and time objects in R
===
- Dates and times are not well captured as strings (since you can't do math with them) or as numbers (since date-time arithmetic is irregular), so they need their own data type
- we'll use the `tidyverse`-adjacent `lubridate` package to provide work with that data type

```r
> library(lubridate) # install.package("lubridate")

Attaching package: 'lubridate'
The following objects are masked from 'package:base':

    date, intersect, setdiff, union
```
- The data types that we will work with are `date` and `dttm` (date-time, also unhelpfully called POSIXct elsewhere in R).

```r
> tibble(date_time = now(), # a date-time (dttm), prints as a string
+        date = today()) # a date, also prints as a string
# A tibble: 1 × 2
  date_time           date      
  <dttm>              <date>    
1 2022-07-02 11:54:24 2022-07-02
```
- Always use the simplest possible data type that works for your needs. Date-times are more complicated because of the need to handle time zones.

Creating dates from a string (or number)
===
- Dates:

```r
> c(ymd("2017-01-31"), mdy("January 31st, 2017"),
+   dmy("31-Jan-2017"), ymd(20170131))
[1] "2017-01-31" "2017-01-31" "2017-01-31" "2017-01-31"
```
- Date-times

```r
> ymd_hms("2017-01-31 20:11:59")
[1] "2017-01-31 20:11:59 UTC"
> mdy_hm("01/31/2017 08:01")
[1] "2017-01-31 08:01:00 UTC"
```
- All of these get printed out as strings, but they are actually `date` or dttm` objects
- these also all work on vectors, even if they fromatted heterogenously 

```r
> x <- c(20090101, "2009-01-02", "2009 01 03", "2009-1-4",
+        "2009-1, 5", "Created on 2009 1 6", "200901 !!! 07")
> ymd(x)
[1] "2009-01-01" "2009-01-02" "2009-01-03" "2009-01-04" "2009-01-05"
[6] "2009-01-06" "2009-01-07"
```
- these functions are all pretty smart and detect most common delimitors (-, /, :, etc.), but you should check their input and output with sample data to make sure they are working correctly

Creating dates from components
===
- Sometimes the dates and times you get are split up (usually in columns)

```r
> flights %>% 
+   select(year, month, day, hour, minute)
# A tibble: 336,776 × 5
    year month   day  hour minute
   <int> <int> <int> <dbl>  <dbl>
 1  2013     1     1     5     15
 2  2013     1     1     5     29
 3  2013     1     1     5     40
 4  2013     1     1     5     45
 5  2013     1     1     6      0
 6  2013     1     1     5     58
 7  2013     1     1     6      0
 8  2013     1     1     6      0
 9  2013     1     1     6      0
10  2013     1     1     6      0
# … with 336,766 more rows
```

Creating dates from components
===
- You can join them into dates or datetimes with `make_date()` or `make_datetime()`

```r
> flights %>% 
+   select(year, month, day, hour, minute) %>% 
+   mutate(departure = make_datetime(year, month, day, hour, minute))
# A tibble: 336,776 × 6
    year month   day  hour minute departure          
   <int> <int> <int> <dbl>  <dbl> <dttm>             
 1  2013     1     1     5     15 2013-01-01 05:15:00
 2  2013     1     1     5     29 2013-01-01 05:29:00
 3  2013     1     1     5     40 2013-01-01 05:40:00
 4  2013     1     1     5     45 2013-01-01 05:45:00
 5  2013     1     1     6      0 2013-01-01 06:00:00
 6  2013     1     1     5     58 2013-01-01 05:58:00
 7  2013     1     1     6      0 2013-01-01 06:00:00
 8  2013     1     1     6      0 2013-01-01 06:00:00
 9  2013     1     1     6      0 2013-01-01 06:00:00
10  2013     1     1     6      0 2013-01-01 06:00:00
# … with 336,766 more rows
```

Creating dates from components
===
- lets do this for all of the times in the `flights` data frame

```r
> flights %>% 
+   head() %>% 
+   pull(arr_time)
[1]  830  850  923 1004  812  740
```
- note that `arr_time`, `sched_arr_time`, `dep_time`, and `sched_dep_time` are numeric and in an HHMM format! We have to fix that first (why is this bad?) 

```r
> flights_fixed = flights %>%
+   mutate(arr_time_hour = str_sub(arr_time, 1, -3) %>% as.numeric(),
+          arr_time_min = str_sub(arr_time, -2, -1) %>% as.numeric(),
+          sched_arr_time_hour = str_sub(sched_arr_time, 1, -3) %>% as.numeric(),
+          sched_arr_time_min = str_sub(sched_arr_time, -2, -1) %>% as.numeric(),
+          dep_time_hour = str_sub(dep_time, 1, -3) %>% as.numeric(),
+          dep_time_min = str_sub(dep_time, -2, -1) %>% as.numeric(),
+          sched_dep_time_hour = str_sub(sched_dep_time, 1, -3) %>% as.numeric(),
+          sched_dep_time_min = str_sub(sched_dep_time, -2, -1) %>% as.numeric())
```
- Why does `str_sub()` work even though the thing being passed to it is numeric?
- This is a lot of typing and copy-paste. Later we will see how to write our own functions that make this less work and reduce the potential for typos

Creating dates from components
===
- Now we can aggregate all of these columns into nice `dttm`s

```r
> flights_dt = flights_fixed %>% 
+   # filter(!is.na(dep_time), !is.na(arr_time)) %>% 
+   mutate(
+     dep_time = make_datetime(year, month, day, 
+                              dep_time_hour, dep_time_min),
+     arr_time = make_datetime(year, month, day, 
+                              arr_time_hour, arr_time_min),
+     sched_dep_time = make_datetime(year, month, day, 
+                                    sched_dep_time_hour, sched_dep_time_min),
+     sched_arr_time = make_datetime(year, month, day, 
+                                    sched_arr_time_hour, sched_dep_time_min)
+   ) %>% 
+   select(ends_with("time"), origin, dest, ends_with("delay"))
> head(flights_dt)
# A tibble: 6 × 9
  dep_time            sched_dep_time      arr_time           
  <dttm>              <dttm>              <dttm>             
1 2013-01-01 05:17:00 2013-01-01 05:15:00 2013-01-01 08:30:00
2 2013-01-01 05:33:00 2013-01-01 05:29:00 2013-01-01 08:50:00
3 2013-01-01 05:42:00 2013-01-01 05:40:00 2013-01-01 09:23:00
4 2013-01-01 05:44:00 2013-01-01 05:45:00 2013-01-01 10:04:00
5 2013-01-01 05:54:00 2013-01-01 06:00:00 2013-01-01 08:12:00
6 2013-01-01 05:54:00 2013-01-01 05:58:00 2013-01-01 07:40:00
# … with 6 more variables: sched_arr_time <dttm>, air_time <dbl>, origin <chr>,
#   dest <chr>, dep_delay <dbl>, arr_delay <dbl>
```

Plotting with dates
===
- ggplot2 understands `ddtm`s

```r
> flights_dt %>% 
+   filter(ymd(20130101) < dep_time & dep_time < ymd(20130102)) %>% # get data from jan 1 2013
+ ggplot(aes(dep_time)) + # plot how many flights were leaving at each time of day
+   geom_freqpoly(binwidth = 600) # 600 s = 10 minutes
```

![plot of chunk unnamed-chunk-100](data-repr-figure/unnamed-chunk-100-1.png)

Accessing dttm elements
===
- You can pull out individual parts of the date with the accessor functions `year()`, `month()`, `mday()` (day of the month), `yday()` (day of the year), `wday()` (day of the week), `hour()`, `minute()`, and `second()`


```r
> flights_dt %>% 
+   mutate(wday = wday(dep_time, label = TRUE)) %>% 
+   ggplot(aes(x = wday)) +
+     geom_bar()
```

![plot of chunk unnamed-chunk-101](data-repr-figure/unnamed-chunk-101-1.png)

Accessing dttm elements
===
- You can pull out individual parts of the date with the accessor functions `year()`, `month()`, `mday()` (day of the month), `yday()` (day of the year), `wday()` (day of the week), `hour()`, `minute()`, and `second()`


```r
> flights_dt %>% 
+   filter(!is.na(dep_time)) %>%
+   mutate(minute = minute(dep_time)) %>% 
+   group_by(minute) %>% 
+   summarise(
+     avg_delay = mean(arr_delay, na.rm = TRUE),
+     n = n()) %>% 
+ ggplot(aes(minute, avg_delay)) +
+   geom_line()
```

![plot of chunk unnamed-chunk-102](data-repr-figure/unnamed-chunk-102-1.png)

Date-time arithmetic
===
We will disucss `dttm` subtraction, addition, and division. These require the following data types:
- durations, which represent an exact number of seconds.
- periods, which represent human units like weeks and months.
- intervals, which represent a starting and ending point.

Durations
===
- Durations represent an exact span of time (i.e. in seconds)

```r
> usa_age <- today() - ymd(17760704)
> usa_age
Time difference of 89847 days
```
- Subtracting `dttm`s in R gives something called a `difftime`, which ambiguously represents differences in weeks, days, hours, or seconds. A `duration` always uses seconds so it is preferable.
- You can conver to a duration with `as.duration()`

```r
> as.duration(usa_age)
[1] "7762780800s (~245.99 years)"
```
- `dseconds()`, `dminutes()`, `dhours()`, `ddays()`, `dweeks()`, and `dyears()` make durations of the given length of time and are vectorized

```r
> ddays(194)
[1] "16761600s (~27.71 weeks)"
> dweeks(1:4)
[1] "604800s (~1 weeks)"  "1209600s (~2 weeks)" "1814400s (~3 weeks)"
[4] "2419200s (~4 weeks)"
```

Duration arithmetic
===
- Durations can be added together and multiplied by numbers

```r
> 2 * (as.duration(usa_age) + dyears(1) + dweeks(12) + dhours(15))
[1] "15603300000s (~494.44 years)"
```
- Or added and subtracted from `ddtm`s

```r
> today() - as.duration(usa_age) # should give July 4 1776
[1] "1776-07-04"
```

- Weird things can happen with time zones

```r
> one_pm <- ymd_hms("2016-03-12 13:00:00", tz = "America/New_York")
> one_pm
[1] "2016-03-12 13:00:00 EST"
> one_pm + ddays(1) # not 1 PM anymore?!
[1] "2016-03-13 14:00:00 EDT"
```

Periods
===
- `Period`s are like `Duration`s but in "natural" human units
- `seconds()`, `minutes()`, `hours()`, `days()`, `weeks()`, `months()`, and `years()` make durations of the given length of time

```r
> days(194)
[1] "194d 0H 0M 0S"
> weeks(5:9)
[1] "35d 0H 0M 0S" "42d 0H 0M 0S" "49d 0H 0M 0S" "56d 0H 0M 0S" "63d 0H 0M 0S"
```
- Question: Why is there `months()` but no `dmonths()`?

Period arithmetic
===
- Periods can be added together and multiplied by numbers, just like Durations

```r
> 2 * (dyears(1) + dweeks(12) + dhours(15))
[1] "77738400s (~2.46 years)"
```
- Or added and subtracted from `ddtm`s

```r
> today() - as.period(usa_age) # should give July 4 1776
[1] "1776-07-04"
```

- And they do more of what you would expect given daylight savings

```r
> one_pm <- ymd_hms("2016-03-12 13:00:00", tz = "America/New_York")
> one_pm
[1] "2016-03-12 13:00:00 EST"
> one_pm + days(1) # it knows that one "day" on this day is actually 23 hours, not 24
[1] "2016-03-13 13:00:00 EDT"
```

Example using periods
===
- some flights appear to arrive before they depart!

```r
> flights_dt %>% 
+   filter(arr_time < dep_time)  %>% 
+   head()
# A tibble: 6 × 9
  dep_time            sched_dep_time      arr_time           
  <dttm>              <dttm>              <dttm>             
1 2013-01-01 21:02:00 2013-01-01 21:08:00 2013-01-01 01:46:00
2 2013-01-01 21:40:00 2013-01-01 21:35:00 2013-01-01 02:10:00
3 2013-01-01 22:17:00 2013-01-01 22:29:00 2013-01-01 02:49:00
4 2013-01-01 22:17:00 2013-01-01 21:30:00 2013-01-01 01:40:00
5 2013-01-01 22:29:00 2013-01-01 21:59:00 2013-01-01 01:49:00
6 2013-01-01 23:26:00 2013-01-01 21:30:00 2013-01-01 01:31:00
# … with 6 more variables: sched_arr_time <dttm>, air_time <dbl>, origin <chr>,
#   dest <chr>, dep_delay <dbl>, arr_delay <dbl>
```
- These are overnight
- We used the same date information for both the departure and the arrival times, but these flights arrived on the following day. 

Example using periods
===
- We can fix this by adding days(1) to the arrival time of each overnight flight.

```r
> flights_dt <- flights_dt %>% 
+   mutate(
+     overnight = arr_time < dep_time,
+     arr_time = arr_time + days(overnight), # will add 1 day if flight is overnight
+     sched_arr_time = sched_arr_time + days(overnight)
+   )
```

Intervals 
=== 
- An `interval` is a specific span of time with a start and end date(-time)
- They can be defined with the `%--%` operator, which you can read as "from... until" as in "from July 4 1776 until today"

```r
> mdy("July 4 1776") %--% today()
[1] 1776-07-04 UTC--2022-07-02 UTC
```
- You can use %within% to see if a date or `dttm` falls in the interval

```r
> flights_dt %>% 
+   filter(dep_time %within% (mdy("feb 15 2013") %--% mdy("feb 25 2013"))) %>% 
+   head()
# A tibble: 6 × 10
  dep_time            sched_dep_time      arr_time           
  <dttm>              <dttm>              <dttm>             
1 2013-02-15 04:54:00 2013-02-15 05:00:00 2013-02-15 06:47:00
2 2013-02-15 05:16:00 2013-02-15 05:15:00 2013-02-15 08:08:00
3 2013-02-15 05:30:00 2013-02-15 05:30:00 2013-02-15 08:22:00
4 2013-02-15 05:36:00 2013-02-15 05:45:00 2013-02-15 10:33:00
5 2013-02-15 05:40:00 2013-02-15 05:40:00 2013-02-15 08:55:00
6 2013-02-15 05:49:00 2013-02-15 06:00:00 2013-02-15 06:43:00
# … with 7 more variables: sched_arr_time <dttm>, air_time <dbl>, origin <chr>,
#   dest <chr>, dep_delay <dbl>, arr_delay <dbl>, overnight <lgl>
```

Exercise: first days of the month
===
Create a vector of dates giving the first day of every month in 2015. Create a vector of dates giving the first day of every month in the current year.

Answer: first days of the month
===
Create a vector of dates giving the first day of every month in 2015. Create a vector of dates giving the first day of every month in the current year.


```r
> ymd(20150101) + month(1:12)
 [1] "2015-01-02" "2015-01-03" "2015-01-04" "2015-01-05" "2015-01-06"
 [6] "2015-01-07" "2015-01-08" "2015-01-09" "2015-01-10" "2015-01-11"
[11] "2015-01-12" "2015-01-13"
> make_date(year(today()), 1, 1) + month(1:12)
 [1] "2022-01-02" "2022-01-03" "2022-01-04" "2022-01-05" "2022-01-06"
 [6] "2022-01-07" "2022-01-08" "2022-01-09" "2022-01-10" "2022-01-11"
[11] "2022-01-12" "2022-01-13"
```

Fancy:

```r
> today () %>%
+   year() %>%
+   make_date(1,1) + 
+   month(1:12)
 [1] "2022-01-02" "2022-01-03" "2022-01-04" "2022-01-05" "2022-01-06"
 [6] "2022-01-07" "2022-01-08" "2022-01-09" "2022-01-10" "2022-01-11"
[11] "2022-01-12" "2022-01-13"
```

lubridate cheat sheet
===
<div align="center">
<img src="https://www.rstudio.com/wp-content/uploads/2018/08/lubridate.png", height=1000, width=1400>
</div>

Data import
===
type:section

Rationale
===
- Sometimes R fails to read in the data from a file
- Or you will read data into R and find strange errors when you try to manipulate it
- This is often caused by type mismatches- e.g. you expected a column to have been read in as a factor, but it was actually read in as a logical.

```r
> file = readr_example("challenge.csv")
> challenge = read_csv(file)
Rows: 2000 Columns: 2
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
dbl  (1): x
date (1): y

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
```

Diagnosing intake errors
===
- Use `readr::problems()` on the returned object to learn more about the errors

```r
> problems(challenge)
# A tibble: 0 × 5
# … with 5 variables: row <int>, col <int>, expected <chr>, actual <chr>,
#   file <chr>
```
- This tells us that `read_csv()` was expecting the `y` column to be logical, but when we look at what was actually in the file at rows 1001+, there are what appear to be dates!
- This happens because `read_csv()` does not know what type of data are in the file- you haven't told it, so it has to guess. 
- The way it guesses is by checking the first 1000 rows of each column and picking the most likely data type.
- You can tell `read_csv()` to check more rows before guessing by using the `guess_max` argument

Specifying data types
===
- In general, you may already know what types the columns should be, so you can provide those to `read_csv()`. 

```r
> challenge = read_csv(file,
+    col_types = cols(
+      y = col_date()
+    ))
> head(challenge)
# A tibble: 6 × 2
      x y     
  <dbl> <date>
1   404 NA    
2  4172 NA    
3  3004 NA    
4   787 NA    
5    37 NA    
6  2332 NA    
```
- This is a more robust solution than using more rows to guess
- Now we see that the problem was caused because the first 1000 rows of `y` are NAs
- Column types are provided to read_csv as named arguments to `cols()`, which itself is a named argument to `col_types`. 
- You do not need to specify all columns (here we let it guess what `x` is) but it is often good practice to do so if possible

Specifying data types
===
- Note that `col_date()` guessed the formatting of the dates (correctly in this case). 
- You can specify it using the `format` argument.

```r
> challenge = read_csv(file,
+    col_types = cols(
+      y = col_date(format="%Y-%m-%d")
+    ))
> tail(challenge)
# A tibble: 6 × 2
      x y         
  <dbl> <date>    
1 0.805 2019-11-21
2 0.164 2018-03-29
3 0.472 2014-08-04
4 0.718 2015-08-16
5 0.270 2020-02-04
6 0.608 2019-01-06
```
- You can also use a character string as a shortcut

```r
> challenge = read_csv(file, col_types = "dD")
```
- Each character stands for the datatype of the corresponding column.
  - In this case, `d` in position 1 means the first column is a double, `D` in position two says the second column is a date.

Specifying data types
===
- Factors can also be read in with a high level of control

```r
> df = readr_example("mtcars.csv")  %>%
+ read_csv(col_types = cols(
+   cyl = col_factor(levels=c("4", "6", "8"))
+ ))
```
- This will let you catch unexpected factor levels and set the proper order up-front! 
- To allow all levels, don't use the `levels` argument

```r
> df = readr_example("mtcars.csv")  %>%
+ read_csv(col_types = cols(
+   cyl = col_factor()
+ ))
```

Non-csv flat files
===
- Besides .csv, you may find data in .tsv (tab-separated values) and other more exotic formats. 
- Many of these are still delimited text files ("flat files"), which means that the data are stored as text with special characters between new lines and columns. This is an exmaple .csv:

```r
> toy_csv = "1,2,3\n4,5,6"
```
- This is an example .tsv

```r
> toy_tsv = "1\t2\t3\n4\t5\t6"
```
- The only difference is the **delimiter** which is the character that breaks up columns. 

Non-csv flat files
===
- Both can be read in using `read_delim()`

```r
> read_delim("1,2,3\n4,5,6", delim=",", col_names = c("x","y","z"))
Rows: 2 Columns: 3
── Column specification ────────────────────────────────────────────────────────
Delimiter: ","
dbl (3): x, y, z

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
# A tibble: 2 × 3
      x     y     z
  <dbl> <dbl> <dbl>
1     1     2     3
2     4     5     6
> read_delim("1\t2\t3\n4\t5\t6", delim="\t", col_names = c("x","y","z"))
Rows: 2 Columns: 3
── Column specification ────────────────────────────────────────────────────────
Delimiter: "\t"
dbl (3): x, y, z

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.
# A tibble: 2 × 3
      x     y     z
  <dbl> <dbl> <dbl>
1     1     2     3
2     4     5     6
```
- `read_csv()` is just a shortcut to `read_delim()` that has `delim=","` hardcoded in.

Non-flat files
===
- There are also other packages that let you read in from other formats
  - `haven` reads in SPSS, Stata, and SAS files
  - `readxl` reads in `.xls` and `.xlsx`
  - `DBI` with a database backend (e.g. `odbc`) reads in from databases
  - `jsonlite` and `xml2` for heirarchical data
  - `rio` for more esoteric formats
  
Writing files
===

```r
> write_csv(challenge, "/Users/c242587/Desktop/challenge.csv")
```
- metadata about column types is lost when writing to .csv
- use `write_rds()` (and `read_rds()`) to save to a binary R format that preserves column types

```r
> write_rds(challenge, "/Users/c242587/Desktop/challenge.rds")
```

Exercise: file I/O
===
What will happen if I run this code?

```r
> file = readr_example("challenge.csv")
> challenge = read_csv(file,
>    col_types = cols(
>      y = col_date()
>    ))
> write_csv(challenge, "/Users/c242587/Desktop/challenge.csv")
> challenge2 = read_csv("/Users/c242587/Desktop/challenge.csv")
```

Answer: file I/O
===
What will happen if I run this code?

```r
> file = readr_example("challenge.csv")
> challenge = read_csv(file,
+    col_types = cols(
+      y = col_date()
+    ))
> write_csv(challenge, "/Users/c242587/Desktop/challenge.csv")
Error: Cannot open file for writing:
* '/Users/c242587/Desktop/challenge.csv'
> challenge2 = read_csv("/Users/c242587/Desktop/challenge.csv")
Error: '/Users/c242587/Desktop/challenge.csv' does not exist.
```

readr cheat sheet
===
<div align="center">
<img src="https://www.rstudio.com/wp-content/uploads/2018/08/data-import.png", height=1000, width=1400>
</div>

Wrap-up
========================================================
type: section

Goals of this course
========================================================
By the end of the course you should be able to...

- create and index vectors of different types
- efficiently manipulate strings, factors, and date-time vectors
- tightly control the intake of tabular data

<div align="center">
<img src="https://d33wubrfki0l68.cloudfront.net/571b056757d68e6df81a3e3853f54d3c76ad6efc/32d37/diagrams/data-science.png" width=800 height=300>
</div>

Resources for this course
========================================================

R for Data Science (R4DS): https://r4ds.had.co.nz
<div align="center">
<img src="https://r4ds.had.co.nz/cover.png">
</div>

***

- Vectors/lists: ch 20
- Strings, factors, date-times: ch 14, 15, 16
- Reading data: ch 11
