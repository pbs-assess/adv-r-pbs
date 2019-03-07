Foundations: Vectors
================

# Quiz

1.  What are the four common types of atomic vectors? What are the two
    rare types?

> logical, integer, double, and character (two rarer types are complex
> and raw)

2.  What are attributes? How do you get them and set them?

> let you add additional metadata to an object; set them with attr() or
> attributes()

3.  How is a list different from an atomic vector? How is a matrix
    different from a data frame?

> All the elements of an atomic vector must be the same type, not so for
> lists. Similarly, all elements of a matrix must be the same type but a
> data frame can contain multiple types in different columns.

4.  Can you have a list that is a matrix? Can a data frame have a column
    that is a matrix?

> Copied from the answers at the end of the chapter: “You can make a
> “list-array” by assigning dimensions to a list. You can make a
> matrix a column of a data frame with df$x \<- matrix(), or by using
> I() when creating a new data frame data.frame(x = I(matrix()))."

5.  How do tibbles behave differently from data frames?

> better printing, stringsAsFactors = FALSE, drop = FALSE as defaults;
> they don’t transform non-syntactic names; they don’t coerce input
> types; tibbles won’t recycle values for other columns unless they are
> of length 1; tibbles allow you to build on columns as they are
> constructed; tibbles do not support row names on purpose; tibbles
> don’t do partial matching with $ column selecting; easier creation
> of list columns.

> Useful snippet - when changing a data frame into a tibble and you want
> to grab the rownames:

``` r
d <- tibble::as_tibble(mtcars, rownames = "name")
```

## 3.2 Atomic vectors

1.  How do you create raw and complex scalars? (See ?raw and ?complex)

> Use as.raw/as.complex

2.  Test your knowledge of the vector coercion rules by predicting the
    output of the following uses of

<!-- end list -->

``` r
c(1, FALSE) # 1, 0
```

    ## [1] 1 0

``` r
c("a", 1) # "a", "1"
```

    ## [1] "a" "1"

``` r
c(TRUE, 1L) # 1 1 
```

    ## [1] 1 1

3.  Why is 1 == “1” true? Why is -1 \< FALSE true? Why is “one” \< 2
    false?

> coercion

4.  Why is the default missing value, NA, a logical vector? What’s
    special about logical vectors? (Hint: think about c(FALSE,
    NA\_character\_).)

> Logical vectors take precedence over all ather types. All other vector
> types can be coerced to logical/from binaries

5.  Precisely what do is.atomic(), is.numeric(), and is.vector() test
    for?

> is.atomic - tests whether an object is atomic (may not be a vector),
> is.numeric - tests for modes and implicit classes, not type, and
> is.vector - return false for any mode that has attributes other than
> names

## 3.3 Attributes

1.  How is setNames() implemented? How is unname() implemented? Read the
    source code.

> Ha: `names(object) <- nm; object`

2.  What does dim() return when applied to a 1D vector? When might you
    use NROW() or NCOL()?

> NULL; “nrow and ncol return the number of rows or columns present in
> x. NCOL and NROW do the same treating a vector as 1-column matrix”

``` r
x <- c(1, 2, 3)
dim(x)
```

    ## NULL

``` r
NROW(x)
```

    ## [1] 3

``` r
NCOL(x)
```

    ## [1] 1

3.  How would you describe the following three objects? What makes them
    different from 1:5?

<!-- end list -->

``` r
x1 <- array(1:5, c(1, 1, 5))
x1
```

    ## , , 1
    ## 
    ##      [,1]
    ## [1,]    1
    ## 
    ## , , 2
    ## 
    ##      [,1]
    ## [1,]    2
    ## 
    ## , , 3
    ## 
    ##      [,1]
    ## [1,]    3
    ## 
    ## , , 4
    ## 
    ##      [,1]
    ## [1,]    4
    ## 
    ## , , 5
    ## 
    ##      [,1]
    ## [1,]    5

``` r
x2 <- array(1:5, c(1, 5, 1))
x2
```

    ## , , 1
    ## 
    ##      [,1] [,2] [,3] [,4] [,5]
    ## [1,]    1    2    3    4    5

``` r
x3 <- array(1:5, c(5, 1, 1))
x3
```

    ## , , 1
    ## 
    ##      [,1]
    ## [1,]    1
    ## [2,]    2
    ## [3,]    3
    ## [4,]    4
    ## [5,]    5

> These are multidimensional arrays, with the vector of lengths of each
> dimension given for the second argument. 1:5 doesn’t technically have
> dimensions and is not an array, it is a vector.

4.  An early draft used this code to illustrate structure():

<!-- end list -->

``` r
structure(1:5, comment = "my attribute")
```

    ## [1] 1 2 3 4 5

But when you print that object you don’t see the comment attribute. Why?
Is the attribute missing, or is there something else special about it?
(Hint: try using help.)

> from ?attr: “Note that some attributes (namely class, comment, dim,
> dimnames, names, row.names and tsp) are treated specially and have
> restrictions on the values which can be set.”

## 3.4 S3 Atomic vectors

1.  What sort of object does table() return? What is its type? What
    attributes does it have? How does the dimensionality change as you
    tabulate more variables?

<!-- end list -->

``` r
x <- table(factor(c("a", "b")))
x
```

    ## 
    ## a b 
    ## 1 1

``` r
str(x)
```

    ##  'table' int [1:2(1d)] 1 1
    ##  - attr(*, "dimnames")=List of 1
    ##   ..$ : chr [1:2] "a" "b"

``` r
attributes(x)
```

    ## $dim
    ## [1] 2
    ## 
    ## $dimnames
    ## $dimnames[[1]]
    ## [1] "a" "b"
    ## 
    ## 
    ## $class
    ## [1] "table"

``` r
x <- table(factor(c("a", "b", "c")))
x
```

    ## 
    ## a b c 
    ## 1 1 1

``` r
attributes(x)
```

    ## $dim
    ## [1] 3
    ## 
    ## $dimnames
    ## $dimnames[[1]]
    ## [1] "a" "b" "c"
    ## 
    ## 
    ## $class
    ## [1] "table"

2.  What happens to a factor when you modify its levels?

<!-- end list -->

``` r
f1 <- factor(letters)
f1
```

    ##  [1] a b c d e f g h i j k l m n o p q r s t u v w x y z
    ## Levels: a b c d e f g h i j k l m n o p q r s t u v w x y z

``` r
str(f1)
```

    ##  Factor w/ 26 levels "a","b","c","d",..: 1 2 3 4 5 6 7 8 9 10 ...

``` r
attributes(f1)
```

    ## $levels
    ##  [1] "a" "b" "c" "d" "e" "f" "g" "h" "i" "j" "k" "l" "m" "n" "o" "p" "q"
    ## [18] "r" "s" "t" "u" "v" "w" "x" "y" "z"
    ## 
    ## $class
    ## [1] "factor"

``` r
as.numeric(f1)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    ## [24] 24 25 26

``` r
levels(f1) <- rev(levels(f1))
f1
```

    ##  [1] z y x w v u t s r q p o n m l k j i h g f e d c b a
    ## Levels: z y x w v u t s r q p o n m l k j i h g f e d c b a

``` r
str(f1)
```

    ##  Factor w/ 26 levels "z","y","x","w",..: 1 2 3 4 5 6 7 8 9 10 ...

``` r
attributes(f1)
```

    ## $levels
    ##  [1] "z" "y" "x" "w" "v" "u" "t" "s" "r" "q" "p" "o" "n" "m" "l" "k" "j"
    ## [18] "i" "h" "g" "f" "e" "d" "c" "b" "a"
    ## 
    ## $class
    ## [1] "factor"

``` r
as.numeric(f1)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    ## [24] 24 25 26

> The ordering of the `levels` attribute changes but the underlying
> values do not.

3.  What does this code do? How do f2 and f3 differ from f1?

<!-- end list -->

``` r
f2 <- rev(factor(letters))
f2
```

    ##  [1] z y x w v u t s r q p o n m l k j i h g f e d c b a
    ## Levels: a b c d e f g h i j k l m n o p q r s t u v w x y z

``` r
as.numeric(f2)
```

    ##  [1] 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4
    ## [24]  3  2  1

> f2 has reversed underlying values an levels in a different order

``` r
f3 <- factor(letters, levels = rev(letters))
f3
```

    ##  [1] a b c d e f g h i j k l m n o p q r s t u v w x y z
    ## Levels: z y x w v u t s r q p o n m l k j i h g f e d c b a

``` r
as.numeric(f1)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23
    ## [24] 24 25 26

``` r
as.numeric(f3)
```

    ##  [1] 26 25 24 23 22 21 20 19 18 17 16 15 14 13 12 11 10  9  8  7  6  5  4
    ## [24]  3  2  1

``` r
levels(f1)
```

    ##  [1] "z" "y" "x" "w" "v" "u" "t" "s" "r" "q" "p" "o" "n" "m" "l" "k" "j"
    ## [18] "i" "h" "g" "f" "e" "d" "c" "b" "a"

``` r
levels(f3)
```

    ##  [1] "z" "y" "x" "w" "v" "u" "t" "s" "r" "q" "p" "o" "n" "m" "l" "k" "j"
    ## [18] "i" "h" "g" "f" "e" "d" "c" "b" "a"

> f3 has reversed underlying values but levels in the same order

## 3.5 Lists

1.  List all the ways that a list differs from an atomic vector.

> Lists can contain references to objects of multiple types but atomic
> vectors must all be the same type.

``` r
v <- 1:2
j <- list(1, 2)
v
```

    ## [1] 1 2

``` r
j
```

    ## [[1]]
    ## [1] 1
    ## 
    ## [[2]]
    ## [1] 2

``` r
v[[1]] <- "a"
j[[1]] <- "a"
v
```

    ## [1] "a" "2"

``` r
j
```

    ## [[1]]
    ## [1] "a"
    ## 
    ## [[2]]
    ## [1] 2

``` r
sapply(j, "[[", 1)
```

    ## [1] "a" "2"

> Note that turning a list into a vector changes they types of elements
> so they are all the same.

2.  Why do you need to use unlist() to convert a list to an atomic
    vector? Why doesn’t as.vector() work?

> FIXME: the rules for coercion are complex and require their own
> function

3.  Compare and contrast c() and unlist() when combining a date and
    date-time into a single vector.

<!-- end list -->

``` r
x <- as.POSIXct("2018-08-01 22:00")
y <- as.Date("1jan2018", "%d%b%Y")
z <- list(x, y)
c(z)
```

    ## [[1]]
    ## [1] "2018-08-01 22:00:00 PDT"
    ## 
    ## [[2]]
    ## [1] "2018-01-01"

``` r
unlist(z)
```

    ## [1] 1533186000      17532

> c() keeps a list where each element refers to an object of a different
> class unlist() coerces the elements to numeric

## 3.6 Data Frames and tibbles

1.  Can you have a data frame with zero rows? What about zero columns?

> rows: yes, columns: no

2.  What happens if you attempt to set rownames that are not unique?

<!-- end list -->

``` r
x <- data.frame(1:10)
# rownames(x) <- c(1, 1:9)
# Error in `.rowNamesDF<-`(x, value = value) :
#   duplicate 'row.names' are not allowed
```

3.  If df is a data frame, what can you say about t(df), and t(t(df))?
    Perform some experiments, making sure to try different column types.

<!-- end list -->

``` r
df <- data.frame(x = 1:10, y = 1:10)
t(df)
```

    ##   [,1] [,2] [,3] [,4] [,5] [,6] [,7] [,8] [,9] [,10]
    ## x    1    2    3    4    5    6    7    8    9    10
    ## y    1    2    3    4    5    6    7    8    9    10

``` r
t(t(df))
```

    ##        x  y
    ##  [1,]  1  1
    ##  [2,]  2  2
    ##  [3,]  3  3
    ##  [4,]  4  4
    ##  [5,]  5  5
    ##  [6,]  6  6
    ##  [7,]  7  7
    ##  [8,]  8  8
    ##  [9,]  9  9
    ## [10,] 10 10

> The type is coerced

4.  What does as.matrix() do when applied to a data frame with columns
    of different types? How does it differ from data.matrix()?

<!-- end list -->

``` r
df <- data.frame(x = 1:10, y = letters[1:10])
as.matrix(df)
```

    ##       x    y  
    ##  [1,] " 1" "a"
    ##  [2,] " 2" "b"
    ##  [3,] " 3" "c"
    ##  [4,] " 4" "d"
    ##  [5,] " 5" "e"
    ##  [6,] " 6" "f"
    ##  [7,] " 7" "g"
    ##  [8,] " 8" "h"
    ##  [9,] " 9" "i"
    ## [10,] "10" "j"

``` r
data.matrix(df)
```

    ##        x  y
    ##  [1,]  1  1
    ##  [2,]  2  2
    ##  [3,]  3  3
    ##  [4,]  4  4
    ##  [5,]  5  5
    ##  [6,]  6  6
    ##  [7,]  7  7
    ##  [8,]  8  8
    ##  [9,]  9  9
    ## [10,] 10 10

> Different coercion rules. From the help: data.matrix: “Return the
> matrix obtained by converting all the variables in a data frame to
> numeric mode and then binding them together as the columns of a
> matrix. Factors and ordered factors are replaced by their internal
> codes.”
