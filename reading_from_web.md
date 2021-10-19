reading data from the web
================

## NSDUH data

``` r
url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"
drug_use_html = 
  read_html(url)

drug_use_df = 
  drug_use_html %>% 
  html_table() %>% # ectract every table
  first() %>% #%>% view
  slice(-1) # slice out first row
```

## Star war

``` r
sw_url = "https://www.imdb.com/list/ls070150896/"
sw_html = 
  read_html(sw_url)

sw_titles = 
  sw_html %>%
  html_elements(".lister-item-header a") %>%
  html_text()

sw_revenue = 
  sw_html %>% 
  html_element(".text-small:nth-child(7) span:nth-child(5)") %>% # give me wrong#
  html_text()

swm_df = 
  tibble(
    title = sw_titles,
    revenue = sw_revenue)
```

## Napoleon dynamite

reviews

``` r
dynamite_yrl = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(dynamite_yrl)

dynamite_review_titles = 
  dynamite_html %>%
  html_elements(".a-text-bold span") %>%
  html_text()

dynamite_stars = 
  dynamite_html %>%
  html_elements("#cm_cr-review_list .review-rating") %>%
  html_text()

dynamite_df = 
  tibble(
    titles = dynamite_review_titles,
    stars = dynamite_stars)
```

## try some APIs

get some data from an API about water

``` r
water_df = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") %>% 
  content()
```

    ## Rows: 42 Columns: 4

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

let’s see what JSON looks like

``` r
water_df = 
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") %>% 
  content("text") %>% 
  jsonlite::fromJSON() %>% 
  as_tibble() #dataframe to tibble
```

BRFSS data via API

``` r
brfss_df = 
  GET("https://chronicdata.cdc.gov/resource/acme-vg9e.csv",
      query = list("$limit" = 5000)) %>% # code snippets - jQuery
  content()
```

    ## Rows: 5000 Columns: 23

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (16): locationabbr, locationdesc, class, topic, question, response, data...
    ## dbl  (6): year, sample_size, data_value, confidence_limit_low, confidence_li...
    ## lgl  (1): locationid

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

pokemon

``` r
poke_data = 
  GET("http://pokeapi.co/api/v2/pokemon/1") %>%
  content()

poke_data[["name"]]
```

    ## [1] "bulbasaur"

``` r
poke_data[["height"]]
```

    ## [1] 7

``` r
poke_data[["abilities"]]
```

    ## [[1]]
    ## [[1]]$ability
    ## [[1]]$ability$name
    ## [1] "overgrow"
    ## 
    ## [[1]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/65/"
    ## 
    ## 
    ## [[1]]$is_hidden
    ## [1] FALSE
    ## 
    ## [[1]]$slot
    ## [1] 1
    ## 
    ## 
    ## [[2]]
    ## [[2]]$ability
    ## [[2]]$ability$name
    ## [1] "chlorophyll"
    ## 
    ## [[2]]$ability$url
    ## [1] "https://pokeapi.co/api/v2/ability/34/"
    ## 
    ## 
    ## [[2]]$is_hidden
    ## [1] TRUE
    ## 
    ## [[2]]$slot
    ## [1] 3
