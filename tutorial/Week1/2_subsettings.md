[Go to table of contents](../../readme.md)

# Subsettings

## Subsetting - Basics

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
