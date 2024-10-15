strings and factors
================
Wenjie Wu

``` r
string_vec = c("my", "name", "is", "xx")

str_detect(string_vec, "name")
```

    ## [1] FALSE  TRUE FALSE FALSE

``` r
str_replace(string_vec, "xx", "XX")
```

    ## [1] "my"   "name" "is"   "XX"

``` r
str_replace(string_vec, "x", "X" )
```

    ## [1] "my"   "name" "is"   "Xx"

``` r
string_vec = c(
  "i think we all rule for participating",
  "i think i have been caught",
  "i think this will be quite fun actually",
  "it will be fun, i think"
  )

str_detect(string_vec, "i think")
```

    ## [1] TRUE TRUE TRUE TRUE

``` r
str_detect(string_vec, "^i think") # Beginning of the line
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
string_vec = c(
  "Time for a Pumpkin Spice Latte!",
  "went to the #pumpkinpatch last weekend",
  "Pumpkin Pie is obviously the best pie",
  "SMASHING PUMPKINS -- LIVE IN CONCERT!!"
  )

str_detect(string_vec, "pumpkin")
```

    ## [1] FALSE  TRUE FALSE FALSE

``` r
str_detect(string_vec, "Pumpkin")
```

    ## [1]  TRUE FALSE  TRUE FALSE

``` r
str_detect(string_vec, "[Pp]umpkin") # Two options
```

    ## [1]  TRUE  TRUE  TRUE FALSE

``` r
string_vec = c(
  '7th inning stretch',
  '1st half soon to begin. Texas won the toss.',
  'she is 5 feet 4 inches tall',
  '3AM - cant sleep :('
  )

str_detect(string_vec, "[0-9][a-zA-Z]") # Find all numbers followed by letters
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
string_vec = c(
  'Its 7:11 in the evening',
  'want to go to 7-11?',
  'my flight is AA711',
  'NetBios: scanning ip 203.167.114.66'
  )

str_detect(string_vec, "7.11") # . matches everything
```

    ## [1]  TRUE  TRUE FALSE  TRUE

``` r
string_vec = c(
  'The CI is [2, 5]',
  ':-]',
  ':-[',
  'I found the answer on pages [6-7]'
  )

str_detect(string_vec, "\\[") # \\ + what are really looking for --> [
```

    ## [1]  TRUE FALSE  TRUE  TRUE

``` r
sex_vec = factor(c("male", "male", "female", "female"))

as.numeric(sex_vec)
```

    ## [1] 2 2 1 1

``` r
sex_vec = fct_relevel(sex_vec, "male")

as.numeric(sex_vec)
```

    ## [1] 1 1 2 2

NSDUH

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
```

``` r
marj_use_df = 
  drug_use_html |>
  html_table() |>
  first() |>
  slice(-1) |>
  select(-contains("P Value")) |>
  pivot_longer(
    cols = -State,
    names_to = "age_year",
    values_to = "percent"
  ) |>
  separate(age_year, int = c("age", "year"), sep = "\\(") |>
  mutate(
    year = str_replace(year, "\\)", ""),
    percent = str_remove(percent, "[a-c]$"),
    percent = as.numeric(percent)
  )
```

``` r
marj_use_df |>
  filter(age == "12-17") |>
  mutate(
    State = fct_reorder(State, percent)
  ) |>
  ggplot(aes(x = State, y = percent, color = year)) +
  geom_point() +
  theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = 1))
```

![](strings_and_factors_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->
