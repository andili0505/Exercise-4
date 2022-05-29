Exercise 4
================

``` r
library(readr)

```

``` r
data_path <- "C:/Users/user/Desktop/Summer 2022/ORGB-672 Org Network/Exercise 4"
applications <- read_parquet(data_path,"app_data_sample.parquet")
edges <- read_csv("edges_sample.csv")

```
    ## Rows: 32906 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr  (1): application_number
    ## dbl  (2): ego_examiner_id, alter_examiner_id
    ## date (1): advice_date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.


``` r
edges

```
# A tibble: 32,906 x 4
   application_number advice_date ego_examiner_id alter_examiner_id
   <chr>              <date>                <dbl>             <dbl>
 1 09402488           2008-11-17            84356             66266
 2 09402488           2008-11-17            84356             63519
 3 09402488           2008-11-17            84356             98531
 4 09445135           2008-08-21            92953             71313
 5 09445135           2008-08-21            92953             93865
 6 09445135           2008-08-21            92953             91818
 7 09479304           2008-12-15            61767             69277
 8 09479304           2008-12-15            61767             92446
 9 09479304           2008-12-15            61767             66805
10 09479304           2008-12-15            61767             70919
# ... with 32,896 more rows

##  Gender, Race, Tenure

``` r
examiner_names <- applications %>% 
  distinct(examiner_name_first)

examiner_names

```

``` r

```

``` r

```
``` r

```

``` r

```
