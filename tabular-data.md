Tabular Data Foundations
========================================================
author: Alejandro Schuler
date: 2022
transition: none
width: 1680
height: 1050

Adapted from [Steve Bagley](https://profiles.stanford.edu/steven-bagley) and based on [R for Data Science by Hadley Wickham](https://r4ds.had.co.nz/)

Edited by David Connell


<style>
.small-code pre code {
  font-size: 0.5em;
}
</style>

Introduction
========================================================
type: section


Goals of this section
========================================================
By the end of these slides you should be able to...

- write simple R scripts
- find, read, and understand package and function documentation 
- read and write tabular data into R from CSV files
- manipulate and subset tabular data

![](images/data-science.png)

RStudio
========================================================
type: section


The basics of interaction using the console window
========================================================

Please open RStudio on DataHub by [going to this link](https://publichealth.datahub.berkeley.edu/hub/user-redirect/git-pull?repo=https%3A%2F%2Fgithub.com%2Fpre-142-training%2Ftutorial-playground&urlpath=rstudio%2F&branch=main). You'll have to right click and then choose "Open Link in New Tab".

You will get more out of this tutorial if you try out these things in R yourself!!




A simple example in the console
========================================================

The R console window is the left (or lower-left) window in RStudio.

- The box contains an expression that will be evaluated by R, followed by the result of evaluating that expression.

```r
> 1 + 2
[1] 3
```
- `3` is the answer
- `[1]` means: the answer is a vector (a list of elements of the same type) and this line starts with the first element of that vector.
- It does not mean the answer has one element (although that is true in this case).

***

![](images/console.png)

Spaces (mostly) don't matter
========================================================

```r
> 1 +2
> 1+ 2
> 1+2
> 1 + 2
```

These all do the same thing. The result of each line is `3`:


```
[1] 3
```

Basic calculations and vectors
========================================================
type: section

R is a scientific calculator
========================================================

```r
> 1 + 2 * 3 # R respects order of operations
[1] 7
> 3/4
[1] 0.75
> 6^3
[1] 216
> log(10) # natural log
[1] 2.302585
> log10(10) # log base 10
[1] 1
> sqrt(16)
[1] 4
```

Vectors
========================================================

```r
> c(2.1, -4, 22)
[1]  2.1 -4.0 22.0
```
- A vector is a one-dimensional sequence of zero or more numbers (or other values).
- Vectors are created by wrapping the values separated by commas with the `c(` `)` function, which is short for "combine"

```r
> 1:50
 [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
[26] 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50
```
- The colon `:` is a handy shortcut to create a vector that is
a sequence of integers from the first number to the second number
(inclusive).
- Many R functions and operators automatically work when you input with
multi-element vectors.
- Long vectors wrap around. (Your screen may have a different width than what is shown here.)
- Look at the `[ ]` notation. The second output line starts
with 26, which is the 26^th element of the vector.
- This notation will help you figure out where you are in a long vector.

Elementwise operations on a vector
========================================================
  - An operation is elementwise (or element-wise) if the action you perform on a vector produces a vector with the same dimensions as the original.
 
  - The code below multiplies each element of `1:10` by the corresponding
element of `1:10`, that is, it squares each element.

```r
> (1:10)*(1:10)
 [1]   1   4   9  16  25  36  49  64  81 100
```
- Equivalently, we
could use exponentiation:

```r
> (1:10)^2
 [1]   1   4   9  16  25  36  49  64  81 100
```

Operator precedence: the rules
========================================================
- R does not evaluate strictly left to right.
- R respects the traditional mathematical order of operations ([PEMDAS](https://www.purplemath.com/modules/orderops.htm)), but R has special operators that have their own precedence, a kind of priority for
evaluation.
- For example, the sequence operator `:` has a higher precedence than addition `+`.

```r
> 1 + 0:10
 [1]  1  2  3  4  5  6  7  8  9 10 11
> 0:10 + 1 # which operator gets executed first?
 [1]  1  2  3  4  5  6  7  8  9 10 11
> (0:10) + 1
 [1]  1  2  3  4  5  6  7  8  9 10 11
> 0:(10 + 1)
 [1]  0  1  2  3  4  5  6  7  8  9 10 11
```
- ADVANCED: check out [a complete list of R operators and their precedence](https://stat.ethz.ch/R-manual/R-devel/library/base/html/Syntax.html).

Variables
========================================================
type: section

Saving values by setting a variable
========================================================

```r
> x <- 10
```
- To do more complex computations, we need to be able to give
names to things.
- Read this as "x gets 10" or "assign 10 to x"
- R prints no result from this assignment, but what you entered
causes a side effect: R has stored the association between
x and 10. (Look at the Environment pane.)

***

![](images/environment.png)


Using the value of a variable
========================================================

```r
> x
[1] 10
> x / 5
[1] 2
```
- When R sees the name of a variable, it uses the stored value of
that variable in the calculation.
- Here R uses the value of x, which is 10.
- `/` is the division operator.
- We can break complex calculations into named parts. This is a
simple, but very useful kind of abstraction.


Assignment has no undo
========================================================

```r
> x <- 10
> x
[1] 10
> x <- x + 1
> x
[1] 11
```
- If you assign to a name with an existing value, that value is overwritten.
- There is no way to undo an assignment, so be careful in reusing variable names.

Naming variables
========================================================
- It is important to pick meaningful variable names.
- Names can be too short, so don't use `x` and `y` everywhere.
- Names can be too long (`Main.database.first.object.header.length`).
- Avoid silly names.
- Pick names that will make sense to someone else (including the
person you will be in six months).
- ADVANCED: See `?make.names` for the complete rules on
what can be a name.


Case matters for names in R
========================================================

```r
> a <- 1
> A # this causes an error because A does not have a value
```
```
Error: object 'A' not found
```
- R cares about upper and lower case in names.
- We also see that some error messages in R are a bit obscure.

Exercise: birth year
===
- Make a variable that represents the age you will be at the end of this year
- Make a variable that represents the current year
- Use them to compute the year of your birth and save that as a variable
- Print the value of that variable

Answer: birth year
===

```r
> my_age_end_of_year = 31
> this_year = 2022
> my_birth_year = this_year - my_age_end_of_year
> my_birth_year
[1] 1991
```

Functions
========================================================
type: section

What is a function?
========================================================
+ Functions are the backbone of the R programming language; the majority of things you will do in R rely on them!
+ A function is a set of statements that performs a specific task
+ User is able to call or pass information to the function
+ Function will perform task and return object or value or action

### Where do functions come from?
+ Many built into base R
+ Many many more available through different package libraries
+ ADVANCED: You can create your own!



*Source: OOMPH course PHW251 - R for Public Health*

Calling built-in functions
========================================================
- To call a function, type the function name, then the argument or
arguments in parentheses. Arguments are the data and/or options we want the function to work on.
- Use a comma to separate the arguments, if more than one.

```r
> sqrt(2)
[1] 1.414214
```
- Many basic R functions operate on multi-element vectors as
easily as on vectors containing a single number.

```r
> sqrt(0:10)
 [1] 0.000000 1.000000 1.414214 1.732051 2.000000 2.236068 2.449490 2.645751
 [9] 2.828427 3.000000 3.162278
```

Functions and variable assignment
========================================================

```r
> x <- 4
> sqrt(x)
[1] 2
> x
[1] 4
> y <- sqrt(x)
> y
[1] 2
> x <- 10
> y
[1] 2
```
- What do you observe about the value of `y` after changing the value of `x`?

Functions and variable assignment
========================================================
- functions generally do not affect the variables you pass to them (`x` remains the same after `sqrt(x)`)
- The results of a function call will simply be printed out if you do not save the result to a variable
- Saving the result to a variable lets you use it later, like any other variable you define manually
- Once a variable has been assigned (`y`), it keeps its value until updated, even if you change other variables (`x`) that went into the original assignment of that variable

Getting help in the RStudio console
========================================================

```r
> sum
```
- start typing `sum`, then hit the `TAB` key (or just wait a second)
- RStudio will display help on all functions that start `sum`.
- Use (up arrow, down arrow) to move through
the list.
- Use `RETURN` or `ENTER` to select the current
item.

Using the built-in help
========================================================
Type `?name` for help on name. Example:

```r
> ?log
```
- This will show information about the `log` function (and related functions) in the Help pane, including the name and meaning of the arguments and returned values.
- The help display is hyperlinked, so clicking on a blue link will take you to related material.
- R documentation often describes a set of functions, such as all the ones related to logarithms, on a single help page.
- Parts of the R documentation are rather obscure.


Exercise: convert weights
========================================================

```r
> weights <- c(1.1, 2.2, 3.3)
```
- These weights are in pounds.
- Convert them to kilograms.
- (Hint: 2.2 lb = 1.0 kg)


Answer: convert weights
========================================================

```r
> weights <- c(1.1, 2.2, 3.3)
> # this divides the weights, element-wise, by the conversion factor:
> weights / 2.2
[1] 0.5 1.0 1.5
```


Some functions operate on vectors and give back a single number
========================================================

```r
> shoesize <- c(9, 12, 6, 10, 10, 16, 8, 4)
> shoesize
[1]  9 12  6 10 10 16  8  4
> sum(shoesize)
[1] 75
> sum(shoesize)/length(shoesize)
[1] 9.375
> mean(shoesize)
[1] 9.375
```


Exercise: subtract the mean
========================================================

```r
> x <- c(7, 3, 1, 9)
```
- Subtract the mean of `x` from `x`, and then `sum`
the result.


Answer: subtract the mean
========================================================

```r
> x <- c(7, 3, 1, 9)
> mean(x)
[1] 5
> x - mean(x)
[1]  2 -2 -4  4
> sum(x - mean(x)) # answer in one expression
[1] 0
```

RStudio scripts
============================================================
type: section


Using the script pane
============================================================
- Writing a series of expressions in the console rapidly gets
messy and confusing.
- The console window gets reset when you restart RStudio.
- It is better (and easier) to write expressions and functions
in the script pane (upper left), building up your analysis.
- There, you can enter expressions, evaluate them, and save the
contents to a .R file for later use.


Script pane example
============================================================
- Create a script pane: File > New File > R Script
- Put your cursor in the script pane.
- Type: `factorial(1:10)`
- Then hit `Command-RETURN` (Mac), or `Ctrl-ENTER` (Windows).
- That line is copied to the console pane and evaluated.
- You can save the script to a file.
- Explore the RStudio ``Code`` menu for other commands.

***

![](images/script.png)

Adding comments
========================================================

```r
> # This is a comment
> 1 + 2 # add some numbers
[1] 3
```
- Use a `#` to start a comment.
- A comment extends to the end of the
line and is ignored by R.

Introduction to Data Manipulation
========================================================
type: section

Tidyverse
========================================================

- The tidyverse collection of R packages is used in this tutorial, and in PHW142, to provide lots of additional functionality that's not present in the basic R programming language.

![](images/tidyverse.png)

Need to install the tidyverse set of packages
========================================================

- If you're working in R locally (installed on your computer), you will need to install the tidyverse package
- If you're on RStudio on DataHub it has already been installed, and you will only need to load the tidyverse with `library()`
- Do this:


```r
> install.packages("tidyverse")
```


```r
> library("tidyverse")
```

- "tidyverse" is a coherent set of packages for operating a kind of data called the "data frame".
- It is not built-in, so you need to install it (once), then load it each time you restart R.
- Put `library("tidyverse")` at the top of every script file so you can always access tidyverse functions.


Data frame: a two-dimensional data structure
========================================================
A data frame is one of the most powerful features in R.
- This is a compound data structure that can contain different
types of data, similar to an Excel spreadsheet.
- Typically, each row in a data frame contains information about one
instance of some (real-world) object.
- Each column can be thought of as a variable, containing the values for the corresponding instances.
- All the values in one column should be of the same type (a number, a category, text, etc.), but
different columns can be of different types.
- Each column is actually a vector, so a data frame is like a list of vectors

Data frame example
========================================================


```r
> mtc
# A tibble: 32 × 11
     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
 1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
 2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
 3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
 4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
 5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
 6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
 7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
 8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
 9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
# … with 22 more rows
```
- This data set contains data on 32 different cars.
- A `tibble` is a kind of data frame. This one has 32 rows and 11 columns.  We only see the first 10 rows because of limited slide/screen space.
- Across the top is the name of each column. The next row shows the type of data in the column. `<dbl>`, means double-precision floating point number, which is a computer science term for any number with a decimal point in it (e.g. `1.3333`, `3.14159`, `1.0`)

Data frame example
========================================================

```r
> mtc
# A tibble: 32 × 11
     mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
 1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
 2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
 3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
 4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
 5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
 6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
 7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
 8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
 9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
# … with 22 more rows
```
- Typically, each row in a data frame describes an instance of some (real-world) object. (Yes: one row for each model of car.)
- Each column contains the values of a variable for the corresponding instance. (Yes: one column for each variable.)

Reading in flat files
========================================================

```r
> mtc = read_csv("https://tinyurl.com/mtcars-csv")
```
- `read_csv` (from the `readr` package, part of `tidyverse`) reads in data frames that are stored in `.csv` files (.csv = comma-separated values)
- these files may be online (accessed via URL), or local, in which case the syntax is `read_csv("path/to/file/mtcars.csv")`
- try `?read_csv` to learn a bit more
- `.csv`s can also be exported from spreadsheets and databases and then saved locally to be read into R.

Making data frames
========================================================
- you can use `tibble()` to make your own data frames from scratch in R
- you don't need to do this yourself--we're just going to use this data in some examples

```r
> my_data = tibble( # newlines don't do anything, just increase code readability
+   mrn = c(1, 2, 3, 4),
+   age = c(33, 48, 8, 29)
+ )
> my_data
# A tibble: 4 × 2
    mrn   age
  <dbl> <dbl>
1     1    33
2     2    48
3     3     8
4     4    29
```

Data frame properties
========================================================
- `dim()` gives the dimensions of the data frame. `ncol()` and `nrow()` give you the number of columns and the number of rows, respectively.

```r
> dim(my_data)
[1] 4 2
> ncol(my_data)
[1] 2
> nrow(my_data)
[1] 4
```

- `names()` gives you the names of the columns (a vector)

```r
> names(my_data)
[1] "mrn" "age"
```

Data frame properties
========================================================
- `glimpse()` shows you a lot of information

```r
> glimpse(my_data)
Rows: 4
Columns: 2
$ mrn <dbl> 1, 2, 3, 4
$ age <dbl> 33, 48, 8, 29
```
- `head()` returns the first `n` rows

```r
> head(my_data, n=2)
# A tibble: 2 × 2
    mrn   age
  <dbl> <dbl>
1     1    33
2     2    48
```

dplyr verbs
========================================================
The rest of this section shows the basic data frame functions ("verbs") in the `dplyr` package (part of `tidyverse`). Each operation takes a data frame and produces a new data frame.

- `filter()` picks out rows according to specified conditions
- `select()` picks out columns according to their names
- `arrange()` sorts the row by values in some column(s)
- `mutate()` creates new columns, often based on operations on other columns

All verbs work similarly:

1. The first argument (arguments are the things inside the parentheses next to the function) is a data frame.
2. The subsequent arguments describe what to do with the data frame, using the variable names (without quotes).
3. The result is a new data frame.

Together these properties make it easy to chain together multiple simple steps to achieve a complex result. Let’s dive in and see the basics of how these verbs work.

filter() subsets the rows of a data frame
========================================================

```r
> filter(mtc, mpg >= 25)
# A tibble: 6 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  32.4     4  78.7    66  4.08  2.2   19.5     1     1     4     1
2  30.4     4  75.7    52  4.93  1.62  18.5     1     1     4     2
3  33.9     4  71.1    65  4.22  1.84  19.9     1     1     4     1
4  27.3     4  79      66  4.08  1.94  18.9     1     1     4     1
5  26       4 120.     91  4.43  2.14  16.7     0     1     5     2
6  30.4     4  95.1   113  3.77  1.51  16.9     1     1     5     2
```
- This produces (and prints out) a new tibble (data frame), which contains all the rows where the mpg value in that row is greater than or equal to 25.
- There are only 6 rows in this data frame.
- There are still 11 columns.

Combining constraints in filter
========================================================

```r
> filter(mtc, mpg >= 25, qsec < 19)
# A tibble: 4 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  30.4     4  75.7    52  4.93  1.62  18.5     1     1     4     2
2  27.3     4  79      66  4.08  1.94  18.9     1     1     4     1
3  26       4 120.     91  4.43  2.14  16.7     0     1     5     2
4  30.4     4  95.1   113  3.77  1.51  16.9     1     1     5     2
```
- This filters by the **conjunction** of the two constraints---both must be satisfied.
- Constraints appear as second (and third...) arguments, separated by commas.


Filtering out all rows
=========================================================

```r
> filter(mtc, mpg > 60)
# A tibble: 0 × 11
# … with 11 variables: mpg <dbl>, cyl <dbl>, disp <dbl>, hp <dbl>, drat <dbl>,
#   wt <dbl>, qsec <dbl>, vs <dbl>, am <dbl>, gear <dbl>, carb <dbl>
```
- If the constraint is too severe, then you will select **no** rows, and produce a zero row sized tibble.

Comparison operators
=========================================================
- `==` tests for equality (do not use `=` which is for assignment)
- `>` and `<` test for greater-than and less-than
- `>=` and `<=` are greater-than-or-equal and less-than-or-equal
- these can also be used directly on vectors outside of data frames

```r
> c(1,5,-22,4) > 0
[1]  TRUE  TRUE FALSE  TRUE
```

Exercise: cars with powerful engines
==========================================================
- How many cars have engines with horsepower (`hp`) greater than 200?


Answer: cars with powerful engines
===========================================================

```r
> filter(mtc, hp > 200)
# A tibble: 7 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  14.3     8   360   245  3.21  3.57  15.8     0     0     3     4
2  10.4     8   472   205  2.93  5.25  18.0     0     0     3     4
3  10.4     8   460   215  3     5.42  17.8     0     0     3     4
4  14.7     8   440   230  3.23  5.34  17.4     0     0     3     4
5  13.3     8   350   245  3.73  3.84  15.4     0     0     3     4
6  15.8     8   351   264  4.22  3.17  14.5     0     1     5     4
7  15       8   301   335  3.54  3.57  14.6     0     1     5     8
```
- Answer: 7

select() subsets columns by name
=========================================================

```r
> select(mtc, mpg, qsec, wt)
# A tibble: 32 × 3
     mpg  qsec    wt
   <dbl> <dbl> <dbl>
 1  21    16.5  2.62
 2  21    17.0  2.88
 3  22.8  18.6  2.32
 4  21.4  19.4  3.22
 5  18.7  17.0  3.44
 6  18.1  20.2  3.46
 7  14.3  15.8  3.57
 8  24.4  20    3.19
 9  22.8  22.9  3.15
10  19.2  18.3  3.44
# … with 22 more rows
```
- The select function will return a subset of the data frame, using only the requested columns in the order specified.

select() subsets columns by name
=========================================================
- `select()` can also be used to select everything **except for** certain columns by using the minus character `-`

```r
> select(mtc, -hp)
# A tibble: 32 × 10
     mpg   cyl  disp  drat    wt  qsec    vs    am  gear  carb
   <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
 1  21       6  160   3.9   2.62  16.5     0     1     4     4
 2  21       6  160   3.9   2.88  17.0     0     1     4     4
 3  22.8     4  108   3.85  2.32  18.6     1     1     4     1
 4  21.4     6  258   3.08  3.22  19.4     1     0     3     1
 5  18.7     8  360   3.15  3.44  17.0     0     0     3     2
 6  18.1     6  225   2.76  3.46  20.2     1     0     3     1
 7  14.3     8  360   3.21  3.57  15.8     0     0     3     4
 8  24.4     4  147.  3.69  3.19  20       1     0     4     2
 9  22.8     4  141.  3.92  3.15  22.9     1     0     4     2
10  19.2     6  168.  3.92  3.44  18.3     1     0     4     4
# … with 22 more rows
```

pull() is a friend of select()
=========================================================
- `select()` has a friend called `pull()` which returns a vector instead of a (one-column) data frame

```r
> select(mtc, hp)
# A tibble: 32 × 1
      hp
   <dbl>
 1   110
 2   110
 3    93
 4   110
 5   175
 6   105
 7   245
 8    62
 9    95
10   123
# … with 22 more rows
> pull(mtc, hp)
 [1] 110 110  93 110 175 105 245  62  95 123 123 180 180 180 205 215 230  66  52
[20]  65  97 150 150 245 175  66  91 113 264 175 335 109
```

Saving the result
=========================================================

```r
> filter(mtc, mpg < 11)
# A tibble: 2 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  10.4     8   472   205  2.93  5.25  18.0     0     0     3     4
2  10.4     8   460   215  3     5.42  17.8     0     0     3     4
> head(mtc)
# A tibble: 6 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  21       6   160   110  3.9   2.62  16.5     0     1     4     4
2  21       6   160   110  3.9   2.88  17.0     0     1     4     4
3  22.8     4   108    93  3.85  2.32  18.6     1     1     4     1
4  21.4     6   258   110  3.08  3.22  19.4     1     0     3     1
5  18.7     8   360   175  3.15  3.44  17.0     0     0     3     2
6  18.1     6   225   105  2.76  3.46  20.2     1     0     3     1
```
- `select()` and `filter()` are functions, so they do not modify their input. You can see `mtc` is unchanged after calling `filter()` on it. This holds for functions in general.

Saving the result
=========================================================
- To save a new version of mtc, assign it to a variable using `<-`

```r
> mtc_first_row <- filter(mtc, mpg < 11)
> mtc_first_row
# A tibble: 2 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  10.4     8   472   205  2.93  5.25  18.0     0     0     3     4
2  10.4     8   460   215  3     5.42  17.8     0     0     3     4
```

arrange() sorts rows
===========================================================
- `arrange` takes a data frame and a column, and sorts the rows by the values in that column (ascending order).

```r
> powerful <- filter(mtc, hp > 200)
> arrange(powerful, mpg)
# A tibble: 7 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  10.4     8   472   205  2.93  5.25  18.0     0     0     3     4
2  10.4     8   460   215  3     5.42  17.8     0     0     3     4
3  13.3     8   350   245  3.73  3.84  15.4     0     0     3     4
4  14.3     8   360   245  3.21  3.57  15.8     0     0     3     4
5  14.7     8   440   230  3.23  5.34  17.4     0     0     3     4
6  15       8   301   335  3.54  3.57  14.6     0     1     5     8
7  15.8     8   351   264  4.22  3.17  14.5     0     1     5     4
```

Arrange can sort by more than one column
===========================================================
- This is useful if there is a tie in sorting by the first column.

```r
> arrange(powerful, gear, disp)
# A tibble: 7 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  13.3     8   350   245  3.73  3.84  15.4     0     0     3     4
2  14.3     8   360   245  3.21  3.57  15.8     0     0     3     4
3  14.7     8   440   230  3.23  5.34  17.4     0     0     3     4
4  10.4     8   460   215  3     5.42  17.8     0     0     3     4
5  10.4     8   472   205  2.93  5.25  18.0     0     0     3     4
6  15       8   301   335  3.54  3.57  14.6     0     1     5     8
7  15.8     8   351   264  4.22  3.17  14.5     0     1     5     4
```


Use the desc function to sort by descending values
===========================================================

```r
> arrange(powerful, desc(mpg))
# A tibble: 7 × 11
    mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
  <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
1  15.8     8   351   264  4.22  3.17  14.5     0     1     5     4
2  15       8   301   335  3.54  3.57  14.6     0     1     5     8
3  14.7     8   440   230  3.23  5.34  17.4     0     0     3     4
4  14.3     8   360   245  3.21  3.57  15.8     0     0     3     4
5  13.3     8   350   245  3.73  3.84  15.4     0     0     3     4
6  10.4     8   472   205  2.93  5.25  18.0     0     0     3     4
7  10.4     8   460   215  3     5.42  17.8     0     0     3     4
```

mutate() creates new columns
================================================================

```r
> mtc_vars_subset = select(mtc, mpg, hp)
> mutate(mtc_vars_subset, gpm = 1/mpg)
# A tibble: 32 × 3
     mpg    hp    gpm
   <dbl> <dbl>  <dbl>
 1  21     110 0.0476
 2  21     110 0.0476
 3  22.8    93 0.0439
 4  21.4   110 0.0467
 5  18.7   175 0.0535
 6  18.1   105 0.0552
 7  14.3   245 0.0699
 8  24.4    62 0.0410
 9  22.8    95 0.0439
10  19.2   123 0.0521
# … with 22 more rows
```
- This uses `mutate` to add a new column to which is the reciprocal of `mpg`.
- The thing on the left of the `=` is a new name that you make up which you would like the new column to be called
- The expression on the right of the `=` defines what will go into the new column
-mutate() can create multiple columns at the same time and use multiple columns to define a single new one

mutate() can create multiple new columns at once
================================================================

```r
> mutate(mtc_vars_subset, # the newlines make it more readable
+       gpm = 1/mpg,
+       mpg_hp_ratio = mpg/hp) 
# A tibble: 32 × 4
     mpg    hp    gpm mpg_hp_ratio
   <dbl> <dbl>  <dbl>        <dbl>
 1  21     110 0.0476       0.191 
 2  21     110 0.0476       0.191 
 3  22.8    93 0.0439       0.245 
 4  21.4   110 0.0467       0.195 
 5  18.7   175 0.0535       0.107 
 6  18.1   105 0.0552       0.172 
 7  14.3   245 0.0699       0.0584
 8  24.4    62 0.0410       0.394 
 9  22.8    95 0.0439       0.24  
10  19.2   123 0.0521       0.156 
# … with 22 more rows
```
- As before, the result of this function is only saved if you assign it to a variable. In this example, `mtc_vars_subset` is unchanged after the mutate.

dplyr verbs summary
========================================================

- `filter()` picks out rows according to specified conditions
- `select()` picks out columns according to their names
- `arrange()` sorts the row by values in some column(s)
- `mutate()` creates new columns, often based on operations on other columns

All verbs work similarly:

1. The first argument (arguments are the things inside the parentheses next to the function) is a data frame.
2. The subsequent arguments describe what to do with the data frame, using the variable names (without quotes).
3. The result is a new data frame.

Together these properties make it easy to chain together multiple simple steps to achieve a complex result.

dplyr cheatsheet
============================================================
[Download the full cheatsheet here](https://github.com/rstudio/cheatsheets/blob/main/data-transformation.pdf)
![](images/dplyr.png)


Resources for this tutorial
========================================================

![](https://r4ds.had.co.nz/cover.png)

***
## [R for Data Science (R4DS) free online book by Hadley Wickham](https://r4ds.had.co.nz)

- Fundamentals: ch 1, 4, 6
- Input/output: ch 11
- Data manipulation: ch 5, 13
- Visualization: ch 3, 28

## [RStudio cheatsheets](https://www.rstudio.com/resources/cheatsheets/)

- Extremely useful reference guides for functions used in this tutorial
