Foundations: Names and Values
================

``` r
library(lobstr)
```

# Quiz

1.  Given the following data frame, how do I create a new column called
    “3” that contains the sum of 1 and 2? You may only use $, not
    \[\[. What makes 1, 2, and 3 challenging as variable names?

<!-- end list -->

``` r
df <- data.frame(runif(3), runif(3))
names(df) <- c(1, 2)
```

``` r
df$`3` <- df$`1` + df$`2`
```

> Challenging because you have to backtick everything.

2.  In the following code, how much memory does y occupy?

<!-- end list -->

``` r
x <- runif(1e6)
y <- list(x, x, x)
```

> About 8 mb.

``` r
lobstr::obj_size(x)
```

    ## 8,000,048 B

``` r
lobstr::obj_size(y)
```

    ## 8,000,128 B

3.  On which line does a get copied in the following example?

<!-- end list -->

``` r
a <- c(1, 5, 3, 2)
b <- a
b[[1]] <- 10
```

> a does not get copied until b is modified on the third line.

# 2.2.2

1.  Explain the relationship between a, b, c and d in the following
    code:

<!-- end list -->

``` r
a <- 1:10
b <- a
c <- b
d <- 1:10
```

``` r
lobstr::obj_addr(a)
```

    ## [1] "0x7c41b50"

``` r
lobstr::obj_addr(b)
```

    ## [1] "0x7c41b50"

``` r
lobstr::obj_addr(c)
```

    ## [1] "0x7c41b50"

``` r
lobstr::obj_addr(d)
```

    ## [1] "0x767d158"

> a, b, c are names pointing to the same object. d is a name pointing to
> a different object that happens to have the same values.

2.  The following code accesses the mean function in multiple ways. Do
    they all point to the same underlying function object? Verify this
    with lobstr::obj\_addr().

<!-- end list -->

``` r
mean
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x00000000070fd398>
    ## <environment: namespace:base>

``` r
base::mean
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x00000000070fd398>
    ## <environment: namespace:base>

``` r
get("mean")
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x00000000070fd398>
    ## <environment: namespace:base>

``` r
evalq(mean)
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x00000000070fd398>
    ## <environment: namespace:base>

``` r
match.fun("mean")
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x00000000070fd398>
    ## <environment: namespace:base>

``` r
lobstr::obj_addr(mean)
```

    ## [1] "0x70fd440"

``` r
lobstr::obj_addr(base::mean)
```

    ## [1] "0x70fd440"

``` r
lobstr::obj_addr(get("mean"))
```

    ## [1] "0x70fd440"

``` r
lobstr::obj_addr(evalq(mean))
```

    ## [1] "0x70fd440"

``` r
lobstr::obj_addr(match.fun("mean"))
```

    ## [1] "0x70fd440"

> Yes, they all point to the same underlying function object.

3.  By default, base R data import functions, like read.csv(), will
    automatically convert non-syntactic names to syntactic ones. Why
    might this be problematic? What option allows you to suppress this
    behaviour

> Can be problematic because of the complex set of rules for the
> conversion. You may not know what you are getting. Can suppress with
> the `check.names` argument.

4.  What rules does make.names() use to convert non-syntactic names into
    syntactic ones?

> Read the help. E.g. “The character”X" is prepended if necessary. All
> invalid characters are translated to “.”. A missing value is
> translated to “NA”. Names which match R keywords have a dot appended
> to them. Duplicated values are altered by make.unique."

5.  I slightly simplified the rules that govern syntactic names. Why is
    .123e1 not a syntactic name? Read ?make.names for the full details.

> “A syntactically valid name consists of letters, numbers and the dot
> or underline characters and starts with a letter or the dot not
> followed by a number.”

# 2.3.6

1.  Why is tracemem(1:10) not useful?

> Because `tracemem()` traces what happens with memory for a given
> object. 1:10 hasn’t been saved to an object with a name.

2.  Explain why tracemem() shows two copies when you run this code.
    Hint: carefully look at the difference between this code and the
    code shown earlier in the section.

<!-- end list -->

``` r
x <- c(1L, 2L, 3L)
tracemem(x)
```

    ## [1] "<000000000C259BF8>"

``` r
x[[3]] <- 4
```

    ## tracemem[0x000000000c259bf8 -> 0x000000000c2e2008]: eval eval withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous> 
    ## tracemem[0x000000000c2e2008 -> 0x000000000c2dbfe8]: eval eval withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous>

> FIXME This is starting with a vector of integers, but 4 is not an
> integer to R, so I assume it coerces the class as well.

3.  Sketch out the relationship between the following objects:

<!-- end list -->

``` r
a <- 1:10
b <- list(a, a)
c <- list(b, a, 1:10)
```

``` r
lobstr::ref(a, b, c)
```

    ## [1:0xc528df0] <int> 
    ##  
    ## o [2:0xc51fc28] <list> 
    ## +-[1:0xc528df0] 
    ## \-[1:0xc528df0] 
    ##  
    ## o [3:0xc5d5878] <list> 
    ## +-[2:0xc51fc28] 
    ## +-[1:0xc528df0] 
    ## \-[4:0xc5fc390] <int>

> `b` is a list and its contents make two references to `a`. `c` is a
> list in which the first element references `b` the second element
> references `a` and the third element references a new set of values.

4.  What happens when you run this code?

<!-- end list -->

``` r
x <- list(1:10)
x[[2]] <- x
```

> In the second step, R makes the second element of the list `x`
> reference the first element.

5.  Draw a picture.

# 2.4.1

1.  In the following example, why are object.size(y) and obj\_size(y) so
    radically different? Consult the documentation of object.size().

<!-- end list -->

``` r
y <- rep(list(runif(1e4)), 100)

object.size(y)
```

    ## 8005648 bytes

``` r
obj_size(y)
```

    ## 80,896 B

> From the documentation for object.size() “does not detect if elements
> of a list are shared, for example.”

2.  Take the following list. Why is its size somewhat misleading?

<!-- end list -->

``` r
funs <- list(mean, sd, var)
#obj_size(funs) - NOTE this crashes R completely for Chris on Windows
#> 17,608 B
```

> The `sd()` function points to the `var()` function and then takes the
> square root. FIXME: but what exactly are the implications of this for
> this question?

3.  Predict the output of the following code:

<!-- end list -->

``` r
a <- runif(1e6)
obj_size(a)
```

    ## 8,000,048 B

``` r
b <- list(a, a)
obj_size(b)
```

    ## 8,000,112 B

``` r
obj_size(a, b)
```

    ## 8,000,112 B

> The same as above.

``` r
b[[1]][[1]] <- 10
obj_size(b)
```

    ## 16,000,160 B

``` r
obj_size(a, b)
```

    ## 16,000,160 B

> The object size doubles because originally b was referencing two
> copies of a, but now the first element of b is different. `obj_size(a,
> b)` is the same because one element of `b` still references `a`.

``` r
b[[2]][[1]] <- 10
obj_size(b)
```

    ## 16,000,160 B

``` r
obj_size(a, b)
```

    ## 24,000,208 B

> The last line above is now twice the size because both elements of b
> reference different objects than a.

# 2.5.3

1.  Explain why the following code doesn’t create a circular list.

<!-- end list -->

``` r
x <- list()
x[[1]] <- x
```

> Only environments can contain themselves. Here, R creates a new object
> and assigns it the value of the original `x`. I.e. modify in place
> does not
happen.

<!-- 2. Wrap the two methods for subtracting medians into two functions, then use the ‘bench’ package (Hester 2018) to carefully compare their speeds. How does performance change as the number of columns increase?* -->

``` r
f1 <- function(x) {
  for (i in seq_along(medians)) {
    x[[i]] <- x[[i]] - medians[[i]]
  }
}
f2 <- function(x) {
  for (i in 1:5) {
    x[[i]] <- x[[i]] - medians[[i]]
  }
}

set.seed(1)
x <- data.frame(matrix(runif(5 * 1e4), ncol = 5))
medians <- vapply(x, median, numeric(1))
x.list <- as.list(x)
bench::mark(f1(x))
```

    ## # A tibble: 1 x 10
    ##   expression    min   mean median   max `itr/sec` mem_alloc  n_gc n_itr
    ##   <chr>      <bch:> <bch:> <bch:> <bch>     <dbl> <bch:byt> <dbl> <int>
    ## 1 f1(x)      79.7us 94.1us 85.7us 313us    10628.     531KB    78  4170
    ## # ... with 1 more variable: total_time <bch:tm>

``` r
bench::mark(f2(x.list))
```

    ## # A tibble: 1 x 10
    ##   expression   min  mean median   max `itr/sec` mem_alloc  n_gc n_itr
    ##   <chr>      <bch> <bch> <bch:> <bch>     <dbl> <bch:byt> <dbl> <int>
    ## 1 f2(x.list)  25us  36us 32.8us 252us    27800.     411KB   100  9660
    ## # ... with 1 more variable: total_time <bch:tm>

> The version with lists ends up being about twice as fast.

3.  What happens if you attempt to use tracemem() on an environment?

<!-- end list -->

``` r
e <- rlang::env()
# tracemem(e)
# > Error in tracemem(e) : 'tracemem' is not useful for promise and environment objects
```
