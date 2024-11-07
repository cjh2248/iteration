Iterations and listcols
================
2024-10-29

## Lists

You can put anything in a list.

``` r
l = list(
  vec_numeric = 5:8,
  mat         = matrix(1:8, 2, 4),
  vec_logical = c(TRUE, FALSE),
  summary     = summary(rnorm(1000)))
```

``` r
l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $vec_logical
    ## [1]  TRUE FALSE
    ## 
    ## $summary
    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -3.367253 -0.693216  0.004745 -0.015392  0.655301  2.845477

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]
```

    ## [1] 5 6 7 8

``` r
l[[1]][1:3]
```

    ## [1] 5 6 7

## ‘for’ loop

Create a new list.

``` r
list_norms = 
  list(
    a = rnorm(20, 3, 1),
    b = rnorm(20, 0, 5),
    c = rnorm(20, 10, .2),
    d = rnorm(20, -3, 1)
  )
```

``` r
list_norms
```

    ## $a
    ##  [1] 2.970186 2.527046 3.628616 3.640132 2.274844 3.063738 2.813969 3.042530
    ##  [9] 2.996548 4.227796 4.990730 4.980889 3.476874 3.689047 2.702049 1.180381
    ## [17] 2.621069 5.149299 2.116680 1.994578
    ## 
    ## $b
    ##  [1]   3.94940673   4.53675210  -0.90406034  -0.54783105   3.17048889
    ##  [6]  -1.96297662   1.03767851  -2.91075271  -0.10128576   2.54974374
    ## [11]  -3.51969772   0.05699409   5.52539597   6.27718181 -10.50360633
    ## [16]  -1.51721151  -9.19896725   0.92254132  -5.03271123   0.11458590
    ## 
    ## $c
    ##  [1] 10.021481  9.902724 10.095098  9.652222  9.886907 10.062073 10.166983
    ##  [8] 10.106040 10.270376 10.417007  9.965201  9.952826 10.128984  9.940308
    ## [15]  9.936466 10.264832 10.139983 10.003354 10.067417 10.148908
    ## 
    ## $d
    ##  [1] -1.527886 -3.485389 -3.507122 -2.849066 -3.901653 -4.238900 -2.119092
    ##  [8] -4.512364 -2.340390 -3.300506 -3.134193 -2.767494 -2.735357 -3.026874
    ## [15] -2.817756 -2.266323 -1.746366 -3.376776 -2.983594 -2.220944

Pause and get my old function.

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  tibble(
    mean = mean_x, 
    sd = sd_x
  )
}
```

I can apply that function to each list element.

``` r
mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.20  1.05

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.403  4.41

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.166

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.94 0.781

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = mean_and_sd(list_norms[[i]])
}
```

## Let’s try map!

``` r
output = map(list_norms, mean_and_sd)
```

What if you want a different function?

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = median(list_norms[[i]])
}

output = map(list_norms, median)
```

``` r
output = map_dbl(list_norms, median, .id = "input")
```

``` r
output = map_dfr(list_norms, mean_and_sd, .id = "input")
```

## List Columns!

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norms
  )
```

``` r
listcol_df |> pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df |> pull(samp)
```

    ## $a
    ##  [1] 2.970186 2.527046 3.628616 3.640132 2.274844 3.063738 2.813969 3.042530
    ##  [9] 2.996548 4.227796 4.990730 4.980889 3.476874 3.689047 2.702049 1.180381
    ## [17] 2.621069 5.149299 2.116680 1.994578
    ## 
    ## $b
    ##  [1]   3.94940673   4.53675210  -0.90406034  -0.54783105   3.17048889
    ##  [6]  -1.96297662   1.03767851  -2.91075271  -0.10128576   2.54974374
    ## [11]  -3.51969772   0.05699409   5.52539597   6.27718181 -10.50360633
    ## [16]  -1.51721151  -9.19896725   0.92254132  -5.03271123   0.11458590
    ## 
    ## $c
    ##  [1] 10.021481  9.902724 10.095098  9.652222  9.886907 10.062073 10.166983
    ##  [8] 10.106040 10.270376 10.417007  9.965201  9.952826 10.128984  9.940308
    ## [15]  9.936466 10.264832 10.139983 10.003354 10.067417 10.148908
    ## 
    ## $d
    ##  [1] -1.527886 -3.485389 -3.507122 -2.849066 -3.901653 -4.238900 -2.119092
    ##  [8] -4.512364 -2.340390 -3.300506 -3.134193 -2.767494 -2.735357 -3.026874
    ## [15] -2.817756 -2.266323 -1.746366 -3.376776 -2.983594 -2.220944

Let’s try some operations.

``` r
listcol_df$samp[[1]]
```

    ##  [1] 2.970186 2.527046 3.628616 3.640132 2.274844 3.063738 2.813969 3.042530
    ##  [9] 2.996548 4.227796 4.990730 4.980889 3.476874 3.689047 2.702049 1.180381
    ## [17] 2.621069 5.149299 2.116680 1.994578

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.20  1.05

Can I just… map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.20  1.05
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.403  4.41
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.166
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.94 0.781

So… can I add a list column?

``` r
listcol_df = 
  listcol_df |> 
  mutate(summary = map(samp, mean_and_sd))

listcol_df
```

    ## # A tibble: 4 × 3
    ##   name  samp         summary         
    ##   <chr> <named list> <named list>    
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>
    ## 2 b     <dbl [20]>   <tibble [1 × 2]>
    ## 3 c     <dbl [20]>   <tibble [1 × 2]>
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>
