[Go to table of contents](../../readme.md)

# Subsettings

## Basics

```r
>  x <- c("a", "b", "c", "c", "d", "a")
> x
[1] "a" "b" "c" "c" "d" "a"
> x[5]
[1] "d"
> x[7]
[1] NA
> x[1:4]
[1] "a" "b" "c" "c"
> x[x > "a"]
[1] "b" "c" "c" "d"
> u <- x > "a"
> u
[1] FALSE  TRUE  TRUE  TRUE  TRUE FALSE
> x[u]
[1] "b" "c" "c" "d"
```

## Lists

```r
> x <- list(foo = 1:4, bar = 0.6)
> x[1]
$foo
[1] 1 2 3 4

> x[[1]]
[1] 1 2 3 4
> x$bar
[1] 0.6
> x[["bar"]]
[1] 0.6
> x["bar"]
$bar
[1] 0.6
> x[[2]]
[1] 0.6
> x[[0]]
Error in x[[0]] : 
  attempt to select less than one element in get1index <real>
```

```r
> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
> x[c(1, 3)]
$foo
[1] 1 2 3 4

$baz
[1] "hello"
```

The `[[` (double brackets) operator can be used with *computed* indices; `$` can only be used with literal names.

```r
> x <- list(foo = 1:4, bar = 0.6, baz = "hello")
> name <- "foo"
> x[[name]] # computed index for 'foo'
[1] 1 2 3 4
> x$name    # element 'name' doesn't exist
NULL
> x$foo     # element 'foo' exist
[1] 1 2 3 4
```

The `[[` can take an integer sequence:

```r
> x <- list(a = list(10, 12, 14), b = c(3.14, 2.81))
> x[[c(1, 3)]]
[1] 14
> x[[1]][[3]]
[1] 14
> x[[c(2, 1)]]
[1] 3.14
```

## Matrices

Matrices can be subsetted in the usual way with *`(i, j)`* type indices:

```r
> x <- matrix(1:6, 2, 3)
> x[1, 2]
[1] 3
> x[2, 1]
[1] 2
> x
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6
```

Indices can also be missing:

```r
> x[1, ]
[1] 1 3 5
> x[, 2]
[1] 3 4
```

By default, when a single element of a matrix is retrieved, it is returned as a vector of length 1 rather than a 1 × 1 matrix. This behavior can be turned off by setting `drop = FALSE`:

```r
> x <- matrix(1:6, 2, 3)
> x[1, 2]
[1] 3
> x[1, 2, drop = F]
     [,1]
[1,]    3
> 
```

Subsetting a single column or row will give you a vector, not a matrix (by default):

```r
> x[1, ]
[1] 1 3 5
> x[1, , drop = F]
     [,1] [,2] [,3]
[1,]    1    3    5
> 
```

## Partial Matching

Partial matching of names is allowed with `[[` and `$`:

```r
> x <- list(aardvark = 1:5)
> x$a
[1] 1 2 3 4 5
> x[['a']]
NULL
> x[['a', exact = F]]
[1] 1 2 3 4 5
```

## Removing Missing Values

A common task is to remove missing values (`NAS`).

```r
> x <- c(1, 2, NA, 4, NA, 5)
> bad <- is.na(x)
> x[!bad]
[1] 1 2 4 5
> bad
[1] FALSE FALSE  TRUE FALSE  TRUE FALSE
> 
```

```r
> x <- c(1, 2, NA, 4, NA, 5)
> y <- c('a', 'b', NA, 'd', NA, 'f')
> good <- complete.cases(x, y)
> good
[1]  TRUE  TRUE FALSE  TRUE FALSE  TRUE
> x[good]
[1] 1 2 4 5
> y[good]
[1] "a" "b" "d" "f"
> x[!good]
[1] NA NA
> y[!good]
[1] NA NA
```

```r
> airquality
    Ozone Solar.R Wind Temp Month Day
1      41     190  7.4   67     5   1
2      36     118  8.0   72     5   2
3      12     149 12.6   74     5   3
4      18     313 11.5   62     5   4
5      NA      NA 14.3   56     5   5
6      28      NA 14.9   66     5   6
7      23     299  8.6   65     5   7
8      19      99 13.8   59     5   8
9       8      19 20.1   61     5   9
10     NA     194  8.6   69     5  10
...
```

```r
> airquality[1:6, ]     # showing the first 6 rows
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
5    NA      NA 14.3   56     5   5
6    28      NA 14.9   66     5   6
> good <- complete.cases(airquality) # which rows are complete
> good
  [1]  TRUE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE  TRUE
 [13]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
 [25] FALSE FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE
 [37] FALSE  TRUE FALSE  TRUE  TRUE FALSE FALSE  TRUE FALSE FALSE  TRUE  TRUE
 [49]  TRUE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE FALSE
 [61] FALSE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
 [73]  TRUE  TRUE FALSE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE FALSE
 [85]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE
 [97] FALSE FALSE  TRUE  TRUE  TRUE FALSE FALSE  TRUE  TRUE  TRUE FALSE  TRUE
[109]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE FALSE  TRUE
[121]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
[133]  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE  TRUE
[145]  TRUE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE  TRUE  TRUE
> airquality[good, ][1:6, ]
  Ozone Solar.R Wind Temp Month Day
1    41     190  7.4   67     5   1
2    36     118  8.0   72     5   2
3    12     149 12.6   74     5   3
4    18     313 11.5   62     5   4
7    23     299  8.6   65     5   7
8    19      99 13.8   59     5   8
```
