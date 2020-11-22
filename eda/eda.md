EDA
================
2020-11-21

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
  ) %>% 
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

avo_df
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

``` r
# number of regions slightly different  
sort(unique(gdp_df$area))
```

    ##  [1] "Alabama"              "Alaska"               "Arizona"             
    ##  [4] "Arkansas"             "California"           "Colorado"            
    ##  [7] "Connecticut"          "Delaware"             "District of Columbia"
    ## [10] "Far West"             "Florida"              "Georgia"             
    ## [13] "Great Lakes"          "Hawaii"               "Idaho"               
    ## [16] "Illinois"             "Indiana"              "Iowa"                
    ## [19] "Kansas"               "Kentucky"             "Louisiana"           
    ## [22] "Maine"                "Maryland"             "Massachusetts"       
    ## [25] "Michigan"             "Mideast"              "Minnesota"           
    ## [28] "Mississippi"          "Missouri"             "Montana"             
    ## [31] "Nebraska"             "Nevada"               "New England"         
    ## [34] "New Hampshire"        "New Jersey"           "New Mexico"          
    ## [37] "New York"             "North Carolina"       "North Dakota"        
    ## [40] "Ohio"                 "Oklahoma"             "Oregon"              
    ## [43] "Pennsylvania"         "Plains"               "Rhode Island"        
    ## [46] "Rocky Mountain"       "South Carolina"       "South Dakota"        
    ## [49] "Southeast"            "Southwest"            "Tennessee"           
    ## [52] "Texas"                "United States"        "Utah"                
    ## [55] "Vermont"              "Virginia"             "Washington"          
    ## [58] "West Virginia"        "Wisconsin"            "Wyoming"

``` r
sort(unique(gdp_df$area))
```

    ##  [1] "Alabama"              "Alaska"               "Arizona"             
    ##  [4] "Arkansas"             "California"           "Colorado"            
    ##  [7] "Connecticut"          "Delaware"             "District of Columbia"
    ## [10] "Far West"             "Florida"              "Georgia"             
    ## [13] "Great Lakes"          "Hawaii"               "Idaho"               
    ## [16] "Illinois"             "Indiana"              "Iowa"                
    ## [19] "Kansas"               "Kentucky"             "Louisiana"           
    ## [22] "Maine"                "Maryland"             "Massachusetts"       
    ## [25] "Michigan"             "Mideast"              "Minnesota"           
    ## [28] "Mississippi"          "Missouri"             "Montana"             
    ## [31] "Nebraska"             "Nevada"               "New England"         
    ## [34] "New Hampshire"        "New Jersey"           "New Mexico"          
    ## [37] "New York"             "North Carolina"       "North Dakota"        
    ## [40] "Ohio"                 "Oklahoma"             "Oregon"              
    ## [43] "Pennsylvania"         "Plains"               "Rhode Island"        
    ## [46] "Rocky Mountain"       "South Carolina"       "South Dakota"        
    ## [49] "Southeast"            "Southwest"            "Tennessee"           
    ## [52] "Texas"                "United States"        "Utah"                
    ## [55] "Vermont"              "Virginia"             "Washington"          
    ## [58] "West Virginia"        "Wisconsin"            "Wyoming"
