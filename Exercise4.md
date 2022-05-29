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
applications

```
# A tibble: 2,018,477 x 16
   application_number filing_date examiner_name_l~ examiner_name_f~ examiner_name_m~ examiner_id examiner_art_un~ uspc_class
   <chr>              <date>      <chr>            <chr>            <chr>                  <dbl>            <dbl> <chr>     
 1 08284457           2000-01-26  HOWARD           JACQUELINE       V                      96082             1764 508       
 2 08413193           2000-10-11  YILDIRIM         BEKIR            L                      87678             1764 208       
 3 08531853           2000-05-17  HAMILTON         CYNTHIA          NA                     63213             1752 430       
 4 08637752           2001-07-20  MOSHER           MARY             NA                     73788             1648 530       
 5 08682726           2000-04-10  BARR             MICHAEL          E                      77294             1762 427       
 6 08687412           2000-04-28  GRAY             LINDA            LAMEY                  68606             1734 156       
 7 08716371           2004-01-26  MCMILLIAN        KARA             RENITA                 89557             1627 424       
 8 08765941           2000-06-23  FORD             VANESSA          L                      97543             1645 424       
 9 08776818           2000-02-04  STRZELECKA       TERESA           E                      98714             1637 435       
10 08809677           2002-02-20  KIM              SUN              U                      65530             1723 210       
# ... with 2,018,467 more rows, and 8 more variables: uspc_subclass <chr>, patent_number <chr>, patent_issue_date <date>,
#   abandon_date <date>, disposal_type <chr>, appl_status_code <dbl>, appl_status_date <chr>, tc <dbl>


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
# A tibble: 2,595 x 1
   examiner_name_first
   <chr>              
 1 JACQUELINE         
 2 BEKIR              
 3 CYNTHIA            
 4 MARY               
 5 MICHAEL            
 6 LINDA              
 7 KARA               
 8 VANESSA            
 9 TERESA             
10 SUN                
# ... with 2,585 more rows
       
       
``` r

```

       
       
``` r

```
       
       
       
``` r

```

       
       
``` r

```

       
       
       
