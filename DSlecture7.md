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
    group = c("treatment", "treatment", "placebo", "placebo"),
    time = c("pre", "post", "pre", "post"),
    mean = c(4, 8, 3.5, 4)
  )
analysis_result %>% 
  pivot_wider(
    names_from = "time",
    values_from = "mean"
  ) %>% 
  knitr::kable()
```

| group     | pre | post |
|:----------|----:|-----:|
| treatment | 4.0 |    8 |
| placebo   | 3.5 |    4 |

## binding rows

using lotR data

``` r
fellowship_ring = 
  readxl::read_excel("./data/lotR_Words.xlsx", range = "B3:D6") %>% 
  mutate(movie = "fellowship_ring")
two_towers = 
  readxl::read_excel("./data/lotR_Words.xlsx", range = "F3:H6") %>% 
  mutate(movie = "two_towers")
return_king = 
  readxl::read_excel("./data/lotR_Words.xlsx", range = "J3:L6") %>% 
  mutate(movie = "return_king")
```

BInd all rows together

``` r
lotr_tidy = 
  bind_rows(fellowship_ring, two_towers, return_king) %>% 
  janitor::clean_names() %>% 
  relocate(movie) %>% 
  pivot_longer(
    female:male,
    names_to = "gender",
    values_to = "words"
  )
lotr_new = rbind (fellowship_ring, two_towers, return_king)
```
