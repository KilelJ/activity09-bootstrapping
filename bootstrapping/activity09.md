Activity 9 - Bootstrapping
================

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
library(tidymodels)
```

    ## ── Attaching packages ────────────────────────────────────── tidymodels 1.2.0 ──
    ## ✔ broom        1.0.5      ✔ rsample      1.2.1 
    ## ✔ dials        1.2.1      ✔ tune         1.2.1 
    ## ✔ infer        1.0.7      ✔ workflows    1.1.4 
    ## ✔ modeldata    1.3.0      ✔ workflowsets 1.1.0 
    ## ✔ parsnip      1.2.1      ✔ yardstick    1.3.1 
    ## ✔ recipes      1.0.10     
    ## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
    ## ✖ scales::discard() masks purrr::discard()
    ## ✖ dplyr::filter()   masks stats::filter()
    ## ✖ recipes::fixed()  masks stringr::fixed()
    ## ✖ dplyr::lag()      masks stats::lag()
    ## ✖ yardstick::spec() masks readr::spec()
    ## ✖ recipes::step()   masks stats::step()
    ## • Learn how to get started at https://www.tidymodels.org/start/

## Creating Data

### 1. Go back through the previous code and explain what each line is

    doing by providing a comment. You are provided with an example in
    the first line and places for the rest of that code “sentence”. Do
    this for all the previous code chunk.

``` r
# Set a random seed value so we can obtain the same "random" results
  set.seed(2023)

  # Create a data frame/tibble named sim_dat
  sim_dat <- tibble(
  # Explain what next line is doing
    x1 = runif(20, -5, 5),
    
  #  The line `x1 = runif(20, -5, 5)` generates 20 random numbers uniformly distributed between -5 and 5 and assigns them to the `x1` column in the data frame `sim_dat`.
    
  # Explain what next line is doing
    x2 = runif(20, 0, 100),
  
# The line `x2 = runif(20, 0, 100)` generates 20 random numbers uniformly distributed between 0 and 100 and assigns them to the `x2` column in the data frame `sim_dat`.

  # Explain what next line is doing
    x3 = rbinom(20, 1, 0.5)

# The line `x3 = rbinom(20, 1, 0.5)` generates 20 random binary numbers (0 or 1) from a binomial distribution with a probability of 0.5  and assigns them to the column `x3` in the data frame `sim_dat`.
    )

  b0 <- 2
  b1 <- 0.25
  b2 <- -0.5
  b3 <- 1
  sigma <- 1.5

  errors <- rnorm(20, 0, sigma)

  sim_dat <- sim_dat %>% 
    mutate(
      y = b0 + b1*x1 + b2*x2 + b3*x3 + errors,
      x3 = case_when(
        x3 == 0 ~ "No",
        TRUE ~ "Yes"
        )
      )
```

### 2. What is the true (population-level) model? Note that we are adding

    noise/variability, but based on the above code you can see what the
    “baseline” model is.

    Ignoring the errors term (which represents the random noise/variability), the true (population-level) model is: y=2+0.25⋅x1−0.5⋅x2+1⋅x3

Where: y is the dependent variable. x1 is a continuous independent
variable uniformly distributed between -5 and 5. x2 is a continuous
independent variable uniformly distributed between 0 and 100. x3 is a
binary independent variable which is either 0 or 1 (later converted to
“No” or “Yes”).
