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
