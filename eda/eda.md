EDA
================
2020-11-22

Import avocado data.

``` r
avo_df = 
  read_csv("data/avocado.csv") %>% 
  janitor::clean_names() %>% 
  select(-1) %>% 
  separate(date, c("year", "month", "day")) %>% 
  mutate(
    year = as.integer(year),
    month = as.integer(month),
    day = as.integer(day)
  ) %>% 
  rename(
    small = x4046,
    large = x4225,
    extra_large = x4770,
  ) 

avo_tidy = 
  avo_df %>% 
  pivot_longer(
    small:extra_large,
    names_to = "fruit_size",
    values_to = "quantity_sold"
  ) %>% 
  pivot_longer(
    total_bags:x_large_bags,
    names_pattern = "(.*)_bags",
    names_to = "bag_type",
    values_to = "bag_sold",
  ) %>% 
  mutate(
    bag_type = recode(bag_type, x_large = "extra_large")
  )

avo_tidy
```

    ## # A tibble: 218,988 x 11
    ##     year month   day average_price total_volume type  region fruit_size
    ##    <int> <int> <int>         <dbl>        <dbl> <chr> <chr>  <chr>     
    ##  1  2015    12    27          1.33       64237. conv… Albany small     
    ##  2  2015    12    27          1.33       64237. conv… Albany small     
    ##  3  2015    12    27          1.33       64237. conv… Albany small     
    ##  4  2015    12    27          1.33       64237. conv… Albany small     
    ##  5  2015    12    27          1.33       64237. conv… Albany large     
    ##  6  2015    12    27          1.33       64237. conv… Albany large     
    ##  7  2015    12    27          1.33       64237. conv… Albany large     
    ##  8  2015    12    27          1.33       64237. conv… Albany large     
    ##  9  2015    12    27          1.33       64237. conv… Albany extra_lar…
    ## 10  2015    12    27          1.33       64237. conv… Albany extra_lar…
    ## # … with 218,978 more rows, and 3 more variables: quantity_sold <dbl>,
    ## #   bag_type <chr>, bag_sold <dbl>

Description:  
`year`: 2015-2018  
`month`: 1-12  
`day`: 1-31  
`type`: conventional, organic  
`fruit_size`: small, large, extra\_large  
`bag_type`: total, small, large, extra\_large  

``` r
gdp_df = 
  read_csv("data/gdp-by-state.csv") %>% 
  janitor::clean_names() %>% 
  select(-1, -x2013, -x2014) %>% 
  pivot_longer(
    x2015:x2017,
    names_prefix = "x",
    names_to = "year",
    values_to = "gdp"
  )

gdp_df
```

    ## # A tibble: 180 x 3
    ##    area          year    gdp
    ##    <chr>         <chr> <dbl>
    ##  1 United States 2015  50301
    ##  2 United States 2016  50660
    ##  3 United States 2017  51337
    ##  4 Alabama       2015  36818
    ##  5 Alabama       2016  37158
    ##  6 Alabama       2017  37508
    ##  7 Alaska        2015  65971
    ##  8 Alaska        2016  63304
    ##  9 Alaska        2017  63610
    ## 10 Arizona       2015  38787
    ## # … with 170 more rows

Description:  
`year`: 2015-2017  

wo jue de ke yi zhao you guan xi de che yi che ?
<https://www.medicalnewstoday.com/articles/270406#benefits>
<https://pdf.usaid.gov/pdf_docs/PA00KP28.pdf>

yao bu zhe li zai gao dian data fao.org/faostat/en/\#search/Avocados

<https://quickstats.nass.usda.gov/results/8A9760E3-BDB0-3A88-B014-DA81BA0845BD>

Volume consumption by year: conventional vs. organic

``` r
bar_plot =
  avo_df %>% 
  group_by(year, type) %>% 
  summarise(sum_volume = sum(total_volume)) %>% 
  pivot_wider(
    names_from = type,
    values_from = sum_volume
  ) %>% 
  plot_ly(x = ~year, y = ~conventional, type = 'bar', name = 'Conventional') %>% 
  add_trace(y = ~organic, name = 'Organic') %>% 
  layout(yaxis = list(title = 'Volume Consumption'), barmode = 'stack', 
         title = "Conventional vs. Organic")

pie_plot = 
  avo_df %>% 
  group_by(type) %>% 
  summarise(sum_volume = sum(total_volume)) %>% 
  plot_ly(labels = ~type, values = ~sum_volume, type = 'pie') %>% 
  layout(title = 'United States Avocado Consumption (2015-2018)',  showlegend = FALSE)
```
