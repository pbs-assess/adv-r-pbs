Foundations: Vectors
================

## 3.2 Atomic vectors

1.  How do you create raw and complex scalars? (See ?raw and ?complex)

> Use as.raw/as.complex

2.  Test your knowledge of the vector coercion rules by predicting the
    output of the following uses of

<!-- end list -->

``` r
c(1, FALSE) #1, 0
c("a", 1) #``a'', ``1''
c(TRUE, 1L) # 1 1 
```

3.  Why is 1 == “1” true? Why is -1 \< FALSE true? Why is “one” \< 2
    false?

> coercion

4.  Why is the default missing value, NA, a logical vector? What’s
    special about logical vectors? (Hint: think about c(FALSE,
    NA\_character\_).)

> Logical vectors take precedence over all ather types. All other vector
> types can be coerced to logical/from binarys

5.  Precisely what do is.atomic(), is.numeric(), and is.vector() test
    for?

> is.atomic - tests whether an object is atomic (may not be a vector),
> is.numeric - tests for modes and implicit classes, not type, and
> is.vector - return false for any mode that has attributes other than
> names

## 3.3 Attributes

1.  How is setNames() implemented? How is unname() implemented? Read the
    source code.

2.  What does dim() return when applied to a 1D vector? When might you
    use NROW() or NCOL()?

3.  How would you describe the following three objects? What makes them
    different from 1:5?

<!-- end list -->

``` r
x1 <- array(1:5, c(1, 1, 5))
x2 <- array(1:5, c(1, 5, 1))
x3 <- array(1:5, c(5, 1, 1))
```

4.  An early draft used this code to illustrate structure():

<!-- end list -->

``` r
structure(1:5, comment = "my attribute")
```

But when you print that object you don’t see the comment attribute. Why?
Is the attribute missing, or is there something else special about it?
(Hint: try using help.)

## 3.4 S3 Atomic vectors

1.  What sort of object does table() return? What is its type? What
    attributes does it have? How does the dimensionality change as you
    tabulate more variables?

> 

2.  What happens to a factor when you modify its levels?

3.  
<!-- end list -->

``` r
f1 <- factor(letters)
levels(f1) <- rev(levels(f1))
```

3.  What does this code do? How do f2 and f3 differ from f1?

<!-- end list -->

``` r
f2 <- rev(factor(letters))

f3 <- factor(letters, levels = rev(letters))
```

## 3.5 Lists

1.  List all the ways that a list differs from an atomic vector.

> 

2.  Why do you need to use unlist() to convert a list to an atomic
    vector? Why doesn’t as.vector() work?

> 

3.  Compare and contrast c() and unlist() when combining a date and
    date-time into a single vector.

> 

## 3.6 Data Frames and tibbles

1.  Can you have a data frame with zero rows? What about zero columns?

> 

2.  What happens if you attempt to set rownames that are not unique?

> 

3.  If df is a data frame, what can you say about t(df), and t(t(df))?
    Perform some experiments, making sure to try different column types.

> 

4.  What does as.matrix() do when applied to a data frame with columns
    of different types? How does it differ from data.matrix()?

>
