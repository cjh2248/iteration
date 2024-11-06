Writing Functions
================
2024-10-28

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -1.111648414  0.445108571  0.694168253 -0.479775657  0.848118212
    ##  [6]  0.035704497 -1.853010002  0.838950080  0.601929262  0.361650003
    ## [11]  1.165380502  0.318113165 -0.628159463  0.781631248 -0.463550923
    ## [16] -1.491752479  0.213634812  1.122355088  0.378255304  1.693009248
    ## [21]  0.422191163  0.552702550 -2.070878310  0.004385795  0.416343668
    ## [26] -2.214159894 -0.110087206 -1.114314825 -0.376016807  1.019722558

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

    ## [1] 2.039575

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

## Multiple outputs

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    stop("Argument x should be numeric")
  } else if (length(x) == 1) {
    stop("Cannot be computed for length 1 vectors")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)

  list(mean = mean_x, 
       sd = sd_x)
}
```

Check the function works.

``` r
x_vec = rnorm(100, mean = 3, sd = 4)
mean_and_sd(x_vec)
```

    ## $mean
    ## [1] 3.438893
    ## 
    ## $sd
    ## [1] 3.513146

## Multiple inputs

I’d like to do this with a function.

``` r
sim_data = tibble(
  x = rnorm(30, mean = 2, sd = 3)
)

sim_data |> 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.81  2.78

``` r
sim_mean_sd = function(n, mu = 2, sigma = 3) {
  
  sim_data = tibble(
    x = rnorm(n, mean = mu, sd = sigma),
  )
  
  sim_data |> 
    summarize(
      mu_hat = mean(x),
      sigma_hat = sd(x)
    )
}

sim_mean_sd(100, 6, 3) ## samp_size, mu, sigma 
```

    ## # A tibble: 1 × 2
    ##   mu_hat sigma_hat
    ##    <dbl>     <dbl>
    ## 1   6.01      3.13

## Let’s review Napoleon Dynamite
