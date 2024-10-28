Writing Functions
================
2024-10-28

## Do something simple

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.3     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
x_vec = rnorm(25, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.425127546 -1.132750858 -1.793457271 -0.008842229 -0.555502042
    ##  [6] -1.170593859 -0.586978706 -0.431259450 -1.877542773  1.012153868
    ## [11]  0.065013613 -0.060456120 -1.293620735  1.151813625  0.379230453
    ## [16] -0.209687103  0.693108978  0.958145345 -0.073102998  1.805223148
    ## [21]  0.358961585  0.701874470  1.462071838 -0.890946293  1.072015966

I want a function to compute z-scores.

``` r
z_scores = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Z scores cannot be computed for length 1 vectors")
  }
  
  z = mean(x) / sd(x)
  
  z
}

z_scores(x_vec)
```

    ## [1] 1.515469

Try my function on some other things. These should give errors.

``` r
z_scores(3)
```

    ## Error in z_scores(3): Z scores cannot be computed for length 1 vectors

``` r
z_scores("my name is jeff")
```

    ## Error in z_scores("my name is jeff"): Argument x should be numeric

``` r
z_scores(mtcars)
```

    ## Error in z_scores(mtcars): Argument x should be numeric

``` r
z_scores(sample(c(TRUE, FALSE), 25, replace = TRUE))
```

    ## Error in z_scores(sample(c(TRUE, FALSE), 25, replace = TRUE)): Argument x should be numeric
