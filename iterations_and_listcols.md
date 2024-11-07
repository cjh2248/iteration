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
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.91770 -0.66125  0.04751  0.03312  0.74007  2.87970

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
    ##  [1] 1.742573 3.474062 2.623906 2.763059 1.823318 1.471932 1.697103 4.647014
    ##  [9] 7.527599 3.136986 3.662194 4.001266 3.020621 2.020522 2.888720 4.040627
    ## [17] 2.269031 3.705640 4.310867 3.056599
    ## 
    ## $b
    ##  [1] -3.2101635 -4.4885269 -1.3878971  7.1013348  8.1509184 -4.3625599
    ##  [7] -0.7868321  4.5107872 -4.1842042 -3.4691112  5.3996262 12.4837242
    ## [13] -2.3866714 -0.6104349  6.0907349  4.2364471 -3.3521830 -1.6861895
    ## [19]  2.3128048 13.4918315
    ## 
    ## $c
    ##  [1]  9.992701 10.095149 10.200331 10.168674 10.148032 10.067636 10.383092
    ##  [8] 10.087969  9.742655 10.134018  9.936445  9.860674  9.899192 10.181466
    ## [15] 10.452048 10.275251  9.660535  9.765980 10.184116 10.128142
    ## 
    ## $d
    ##  [1] -4.3491812 -3.7934638  0.4108108 -2.0428507 -1.6114284 -3.3111506
    ##  [7] -2.9427164 -4.7814159 -4.5437730 -4.4918838 -2.4605512 -3.8117863
    ## [13] -4.3782703 -2.4680433 -3.6559889 -3.8371106 -4.2404773 -5.5117999
    ## [19] -4.1862446 -2.7308788

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
    ## 1  3.19  1.38

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.69  5.66

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.1 0.207

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.44  1.35

Let’s use a for loop:

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = mean_and_sd(list_norms[[i]])
}
```
