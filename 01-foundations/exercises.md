Foundations sections 1 to 4
================

``` r
library(lobstr)
```

# Quiz

*Given the following data frame, how do I create a new column called “3”
that contains the sum of 1 and 2? You may only use $, not \[\[. What
makes 1, 2, and 3 challenging as variable names?*

``` r
df <- data.frame(runif(3), runif(3))
names(df) <- c(1, 2)
```

``` r
df$`3` <- df$`1` + df$`2`
```

Challenging because you have to backtick everything.

*In the following code, how much memory does y occupy?*

``` r
x <- runif(1e6)
y <- list(x, x, x)
```

About 8 mb.

``` r
lobstr::obj_size(x)
```

    ## 8,000,048 B

``` r
lobstr::obj_size(y)
```

    ## 8,000,128 B

*On which line does a get copied in the following example?*

``` r
a <- c(1, 5, 3, 2)
b <- a
b[[1]] <- 10
```

a does not get copied until b is modified on the third line.

# 2.2.2

*1. Explain the relationship between a, b, c and d in the following
code:*

``` r
a <- 1:10
b <- a
c <- b
d <- 1:10

lobstr::obj_addr(a)
```

    ## [1] "0x7ff250bcf8f0"

``` r
lobstr::obj_addr(b)
```

    ## [1] "0x7ff250bcf8f0"

``` r
lobstr::obj_addr(c)
```

    ## [1] "0x7ff250bcf8f0"

``` r
lobstr::obj_addr(d)
```

    ## [1] "0x7ff251a285f8"

a, b, c are names pointing to the same object. d is a name pointing to a
different object that happens to have the same values.

*2. The following code accesses the mean function in multiple ways. Do
they all point to the same underlying function object? Verify this with
lobstr::obj\_addr().*

``` r
mean
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x7ff251f7de08>
    ## <environment: namespace:base>

``` r
base::mean
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x7ff251f7de08>
    ## <environment: namespace:base>

``` r
get("mean")
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x7ff251f7de08>
    ## <environment: namespace:base>

``` r
evalq(mean)
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x7ff251f7de08>
    ## <environment: namespace:base>

``` r
match.fun("mean")
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x7ff251f7de08>
    ## <environment: namespace:base>

``` r
lobstr::obj_addr(mean)
```

    ## [1] "0x7ff251f7deb0"

``` r
lobstr::obj_addr(base::mean)
```

    ## [1] "0x7ff251f7deb0"

``` r
lobstr::obj_addr(get("mean"))
```

    ## [1] "0x7ff251f7deb0"

``` r
lobstr::obj_addr(evalq(mean))
```

    ## [1] "0x7ff251f7deb0"

``` r
lobstr::obj_addr(match.fun("mean"))
```

    ## [1] "0x7ff251f7deb0"

Yes, they all point to the same underlying function object.

*3. By default, base R data import functions, like read.csv(), will
automatically convert non-syntactic names to syntactic ones. Why might
this be problematic? What option allows you to suppress this behaviour*

Can be problematic because of the complex set of rules for the
conversion. You may not know what you are getting. Can suppress with the
`check.names` argument.

*4. What rules does make.names() use to convert non-syntactic names into
syntactic ones?*

Read the help. E.g. “The character”X" is prepended if necessary. All
invalid characters are translated to “.”. A missing value is translated
to “NA”. Names which match R keywords have a dot appended to them.
Duplicated values are altered by make.unique."

*5. I slightly simplified the rules that govern syntactic names. Why is
.123e1 not a syntactic name? Read ?make.names for the full details.*

“A syntactically valid name consists of letters, numbers and the dot or
underline characters and starts with a letter or the dot not followed by
a number.”

# 2.3.6

*1. Why is tracemem(1:10) not useful?*

*2. Explain why tracemem() shows two copies when you run this code.
Hint: carefully look at the difference between this code and the code
shown earlier in the section.*

``` r
x <- c(1L, 2L, 3L)
tracemem(x)
```

    ## [1] "<0x7ff25536b388>"

``` r
x[[3]] <- 4
```

    ## tracemem[0x7ff25536b388 -> 0x7ff25541d288]: eval eval withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous> 
    ## tracemem[0x7ff25541d288 -> 0x7ff255440c58]: eval eval withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous>

*3. Sketch out the relationship between the following objects:*

``` r
a <- 1:10
c <- list(b, a, 1:10)
b <- list(a, a)
```

*4. What happens when you run this code?*

``` r
x <- list(1:10)
x[[2]] <- x
```

Draw a picture.

# 2.5.3

*1. Explain why the following code doesn’t create a circular list.*

``` r
x <- list()
x[[1]] <- x
```

*2. Wrap the two methods for subtracting medians into two functions,
then use the ‘bench’ package (Hester 2018) to carefully compare their
speeds. How does performance change as the number of columns increase?*

*3. What happens if you attempt to use tracemem() on an environment?*
