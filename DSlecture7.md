DSlecture7_tidy data
================
Hening cui
2021/9/30

## `pivot_longer`

load pulse data

``` r
pulse_data = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names()
```

wide format to long format

``` r
pulse_data_tidy = 
  pulse_data %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bid",
    
  )
```

rewrite , combine and exten (to add a mutate)

``` r
pulpulse_data = 
  haven::read_sas("./data/public_pulse_data.sas7bdat") %>% 
  janitor::clean_names() %>% 
  pivot_longer(
    bdi_score_bl:bdi_score_12m, 
    names_to = "visit",
    names_prefix = "bdi_score_",
    values_to = "bid",
    ) %>% 
  relocate(id, visit) %>% 
  mutate (visit = recode (visit, "bl" = "00m"))
```

## `pivot_wider`

make up data

``` r
analysis_result = 
  tibble(
    group = c(" treatment", "treatment", "placebo", "placebo"),
    time = c("pre", "post", "pre", "post"),
    mean = c(4, 8, 3.5, 4)
  )
analysis_result %>% 
  pivot_wider(
    names_from = "time",
    values_from = "mean"
  )
```

    ## # A tibble: 3 × 3
    ##   group          pre  post
    ##   <chr>        <dbl> <dbl>
    ## 1 " treatment"   4      NA
    ## 2 "treatment"   NA       8
    ## 3 "placebo"      3.5     4
