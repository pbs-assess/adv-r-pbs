Foundations sections 1 to 4
================

``` r
library(tidyverse)
```

    ## -- Attaching packages ----------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 3.1.0       v purrr   0.3.0  
    ## v tibble  2.0.1       v dplyr   0.8.0.1
    ## v tidyr   0.8.2       v stringr 1.4.0  
    ## v readr   1.2.1       v forcats 0.3.0

    ## Warning: package 'tibble' was built under R version 3.5.2

    ## Warning: package 'tidyr' was built under R version 3.5.2

    ## Warning: package 'purrr' was built under R version 3.5.2

    ## Warning: package 'dplyr' was built under R version 3.5.2

    ## Warning: package 'stringr' was built under R version 3.5.2

    ## -- Conflicts -------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

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

    ## [1] "0x611c4b0"

``` r
lobstr::obj_addr(b)
```

    ## [1] "0x611c4b0"

``` r
lobstr::obj_addr(c)
```

    ## [1] "0x611c4b0"

``` r
lobstr::obj_addr(d)
```

    ## [1] "0x63db3a0"

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
    ## <bytecode: 0x0000000007045e80>
    ## <environment: namespace:base>

``` r
base::mean
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x0000000007045e80>
    ## <environment: namespace:base>

``` r
get("mean")
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x0000000007045e80>
    ## <environment: namespace:base>

``` r
evalq(mean)
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x0000000007045e80>
    ## <environment: namespace:base>

``` r
match.fun("mean")
```

    ## function (x, ...) 
    ## UseMethod("mean")
    ## <bytecode: 0x0000000007045e80>
    ## <environment: namespace:base>

``` r
lobstr::obj_addr(mean)
```

    ## [1] "0x7045f28"

``` r
lobstr::obj_addr(base::mean)
```

    ## [1] "0x7045f28"

``` r
lobstr::obj_addr(get("mean"))
```

    ## [1] "0x7045f28"

``` r
lobstr::obj_addr(evalq(mean))
```

    ## [1] "0x7045f28"

``` r
lobstr::obj_addr(match.fun("mean"))
```

    ## [1] "0x7045f28"

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

1:10 does not refer to a variable name and therefore cannot be accessed
later by any code.

*2. Explain why tracemem() shows two copies when you run this code.
Hint: carefully look at the difference between this code and the code
shown earlier in the section.*

``` r
x <- c(1L, 2L, 3L)
tracemem(x)
```

    ## [1] "<000000000DDB9800>"

``` r
x[[3]] <- 4
```

    ## tracemem[0x000000000ddb9800 -> 0x000000000e36d258]: eval eval withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous> 
    ## tracemem[0x000000000e36d258 -> 0x000000000e192b58]: eval eval withVisible withCallingHandlers handle timing_fn evaluate_call <Anonymous> evaluate in_dir block_exec call_block process_group.block process_group withCallingHandlers process_file <Anonymous> <Anonymous>

This is because the original vector is of type `integer` and we are
inserting a new value of type `numeric`, so the whole list is copied and
a new reference is generated. This could be a huge performance hit if
done accidentally on long lists.

*3. Sketch out the relationship between the following objects:*

``` r
a <- 1:10
b <- list(a, a)
c <- list(b, a, 1:10)
```

Answer:

``` r
a <- 1:10
b <- list(a, a)
lobstr::ref(a)
```

    ## [1:0x91fdc28] <int>

``` r
lobstr::ref(b)
```

    ## o [1:0xde9a768] <list> 
    ## +-[2:0x91fdc28] <int> 
    ## \-[2:0x91fdc28]

The elements of `b` both have the same address because they are
identical and R is smart and won’t make a new object if it doesn’t have
to.

``` r
c <- list(b, a, 1:10)
lobstr::ref(c)
```

    ## o [1:0xe179db8] <list> 
    ## +-o [2:0xde9a768] <list> 
    ## | +-[3:0x91fdc28] <int> 
    ## | \-[3:0x91fdc28] 
    ## +-[3:0x91fdc28] 
    ## \-[4:0x6442000] <int>

List `c` contains list `b` which is identical to the object `b` itself,
then vector `a`, then a newly created vector. Only one new memory
allocation happens in this call.

*4. What happens when you run this code?*

``` r
x <- list(1:10)
x[[2]] <- x
```

Answer:

``` r
x <- list(1:10)
lobstr::ref(x)
```

    ## o [1:0xd19d3f8] <list> 
    ## \-[2:0xbe59c18] <int>

After the list is created, we set an element of it to be the list
itself. One would expect some sort of recursive behaviour, but..

``` r
x[[2]] <- x
lobstr::ref(x)
```

    ## o [1:0xdede5c8] <list> 
    ## +-[2:0xbe59c18] <int> 
    ## \-o [3:0xd19d3f8] <list> 
    ##   \-[2:0xbe59c18]

The second list element is a simple memory address copy of the first.
This outlines that because variables are stored in R as references, this
kind of assignment is easy to implement.

# 2.5.3

*1. Explain why the following code doesn’t create a circular list.*

``` r
x <- list()
x[[1]] <- x
```

Answer:

``` r
x <- list()
lobstr::ref(x)
```

    ## o [1:0xca6cbc0] <list>

``` r
x[[1]] <- x
lobstr::ref(x)
```

    ## o [1:0xe3f3dc8] <list> 
    ## \-o [2:0xca6cbc0] <list>

After the second call, the original list reference is now the sublist
reference and the outer list reference has been given a new address.

*2. Wrap the two methods for subtracting medians into two functions,
then use the ‘bench’ package (Hester 2018) to carefully compare their
speeds. How does performance change as the number of columns increase?*

``` r
set.seed(42)
sub.med <- function(x, m){
    for (i in seq_along(m)) {
      x[[i]] <- x[[i]] - m[[i]]
    }
  }

results <- bench::press(
  rows = 50000,
  cols = c(5, 50, 500, 2000),
  rep = 1,
  {
    dat <- data.frame(matrix(runif(rows), ncol = cols))
    med <- vapply(dat, median, numeric(1))
    bench::mark(
      df  = sub.med(dat, med),
      lst = sub.med(as.list(dat), med)
   )
  }
)
```

    ## Running with:
    ##    rows  cols   rep

    ## 1 50000     5     1

    ## 2 50000    50     1

    ## 3 50000   500     1

    ## 4 50000  2000     1

    ## Warning: Some expressions had a GC in every iteration; so filtering is
    ## disabled.

``` r
results %>%
  unnest() %>%
  select(expression, cols, Time = total_time) %>%
  distinct() %>%
  mutate(expression = replace(expression, expression == "df", "Data Frame"),
         expression = replace(expression, expression == "lst", "List"),
         Time = as.numeric(Time)) %>%
  ggplot(aes(x = Time, y = cols, color = expression)) +
  geom_point() +
  geom_path(size = 1) +
  xlim(c(0, NA)) +
  ylim(c(0, NA)) +
  labs(x = "Time to finish",
       y = "Number of columns in data frame",
       fill = "") +
  scale_color_discrete(name = "Object type") +
  ggtitle("Times for for loop to run") +
  theme(plot.title = element_text(hjust = 0.5, face = "bold"))
```

![](exercises_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->

Times are much slower when using for loops with data frames than with
lists. Memory allocation is also much higher.

*3. What happens if you attempt to use tracemem() on an environment?*

Error in tracemem(e) : ‘tracemem’ is not useful for promise and
environment objects
