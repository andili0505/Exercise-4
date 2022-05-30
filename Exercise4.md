Exercise 4
================

``` r
library(readr)
library(arrow)
library(dplyr)

```

``` r
data_path <- "C:/Users/user/Desktop/Summer 2022/ORGB-672 Org Network/Exercise 4"
applications <- read_parquet("app_data_sample.parquet", as_tibble = TRUE)
edges <- read_csv("edges_sample.csv")

```
     Rows: 32906 Columns: 4
     ── Column specification ────────────────────────────────────────────────────────
     Delimiter: ","
     chr  (1): application_number
     dbl  (2): ego_examiner_id, alter_examiner_id
     date (1): advice_date
     
     ℹ Use `spec()` to retrieve the full column specification for this data.
     ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
applications

```
  A tibble: 2,018,477 x 16
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
 ... with 2,018,467 more rows, and 8 more variables: uspc_subclass <chr>, patent_number <chr>, patent_issue_date <date>,
   abandon_date <date>, disposal_type <chr>, appl_status_code <dbl>, appl_status_date <chr>, tc <dbl>


``` r
edges

```
   A tibble: 32,906 x 4
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

## Get gender for examiners
       
``` r
examiner_names <- applications %>% 
  distinct(examiner_name_first)

examiner_names

```
   A tibble: 2,595 x 1
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
#Get a table of names and gender
       
 examiner_names_gender <- examiner_names %>% 
  do(results = gender(.$examiner_name_first, method = "ssa")) %>% 
  unnest(cols = c(results), keep_empty = TRUE) %>% 
  select(
    examiner_name_first = name,
    gender,
    proportion_female
  )
examiner_names_gender   
       
```
    A tibble: 1,822 × 3
    examiner_name_first gender proportion_female
    <chr>               <chr>              <dbl>
  1 AARON               male              0.0082
  2 ABDEL               male              0     
  3 ABDOU               male              0     
  4 ABDUL               male              0     
  5 ABDULHAKIM          male              0     
  6 ABDULLAH            male              0     
  7 ABDULLAHI           male              0     
  8 ABIGAIL             female            0.998 
  9 ABIMBOLA            female            0.944 
 10 ABRAHAM             male              0.0031
  … with 1,812 more rows
       
       
``` r
# remove extra columns from the gender table
examiner_names_gender <- examiner_names_gender %>% 
  select(examiner_name_first, gender)

# joining gender back to the dataset
applications <- applications %>% 
  left_join(examiner_names_gender, by = "examiner_name_first")

# cleaning up
rm(examiner_names)
rm(examiner_names_gender)
gc()
       
```
## Now we guess the examiner’s race using package (wru)
       
```r
library(wru)
       
```
           
``` r
examiner_surnames <- applications %>% 
  select(surname = examiner_name_last) %>% 
  distinct()
examiner_surnames
       
```
    A tibble: 3,806 × 1
    surname   
    <chr>     
  1 HOWARD    
  2 YILDIRIM  
  3 HAMILTON  
  4 MOSHER    
  5 BARR      
  6 GRAY      
  7 MCMILLIAN 
  8 FORD      
  9 STRZELECKA
 10 KIM       
  … with 3,796 more rows
       
       
``` r
examiner_race <- predict_race(voter.file = examiner_surnames, surname.only = T) %>% 
  as_tibble()
       
```

       
``` r
examiner_race
       
```

        A tibble: 3,806 × 6
        surname    pred.whi pred.bla pred.his pred.asi pred.oth
        <chr>         <dbl>    <dbl>    <dbl>    <dbl>    <dbl>
      1 HOWARD       0.643   0.295    0.0237   0.005     0.0333
      2 YILDIRIM     0.861   0.0271   0.0609   0.0135    0.0372
      3 HAMILTON     0.702   0.237    0.0245   0.0054    0.0309
      4 MOSHER       0.947   0.00410  0.0241   0.00640   0.0185
      5 BARR         0.827   0.117    0.0226   0.00590   0.0271
      6 GRAY         0.687   0.251    0.0241   0.0054    0.0324
      7 MCMILLIAN    0.359   0.574    0.0189   0.00260   0.0463
      8 FORD         0.620   0.32     0.0237   0.0045    0.0313
      9 STRZELECKA   0.666   0.0853   0.137    0.0797    0.0318
     10 KIM          0.0252  0.00390  0.00650  0.945     0.0198
      … with 3,796 more rows

       
## Now we can pick the race category that has the highest probability for each last name 
       
``` r
examiner_race <- examiner_race %>% 
  mutate(max_race_p = pmax(pred.asi, pred.bla, pred.his, pred.oth, pred.whi)) %>% 
  mutate(race = case_when(
    max_race_p == pred.asi ~ "Asian",
    max_race_p == pred.bla ~ "black",
    max_race_p == pred.his ~ "Hispanic",
    max_race_p == pred.oth ~ "other",
    max_race_p == pred.whi ~ "white",
    TRUE ~ NA_character_
  ))
examiner_race
       
```
    A tibble: 3,806 × 8
        surname    pred.whi pred.bla pred.his pred.asi pred.oth max_race_p race 
        <chr>         <dbl>    <dbl>    <dbl>    <dbl>    <dbl>      <dbl> <chr>
      1 HOWARD       0.643   0.295    0.0237   0.005     0.0333      0.643 white
      2 YILDIRIM     0.861   0.0271   0.0609   0.0135    0.0372      0.861 white
      3 HAMILTON     0.702   0.237    0.0245   0.0054    0.0309      0.702 white
      4 MOSHER       0.947   0.00410  0.0241   0.00640   0.0185      0.947 white
      5 BARR         0.827   0.117    0.0226   0.00590   0.0271      0.827 white
      6 GRAY         0.687   0.251    0.0241   0.0054    0.0324      0.687 white
      7 MCMILLIAN    0.359   0.574    0.0189   0.00260   0.0463      0.574 black
      8 FORD         0.620   0.32     0.0237   0.0045    0.0313      0.620 white
      9 STRZELECKA   0.666   0.0853   0.137    0.0797    0.0318      0.666 white
     10 KIM          0.0252  0.00390  0.00650  0.945     0.0198      0.945 Asian
      … with 3,796 more rows
 
       
``` r
## Now we can join it back to the applications data:
       
applications <- applications %>% 
  left_join(examiner_dates, by = "examiner_id")
rm(examiner_dates)
gc()
       
```

                used  (Mb) gc trigger   (Mb)  max used (Mb)
     Ncells  5164923 275.9   15221244  813.0  15221244  813
     Vcells 66116044 504.5  134585703 1026.9 134348027 1025

       
```r
Apps <- Apps %>% 
  left_join(ex_race, by = c("examiner_name_last" = "surname"))

```
       
## Tenure
```r
library(lubridate) 
```

```r
ex_dates <- Apps %>% 
  select(examiner_id, filing_date, appl_status_date) 
```

```r
ex_dates <- ex_dates %>% 
  mutate(start_date = ymd(filing_date), end_date = as_date(dmy_hms(appl_status_date)))
```

```r
ex_dates <- ex_dates %>% 
  group_by(examiner_id) %>% 
  summarise(
    earliest_date = min(start_date, na.rm = TRUE), 
    latest_date = max(end_date, na.rm = TRUE),
    tenure_days = interval(earliest_date, latest_date) %/% days(1)
    ) %>% 
  filter(year(latest_date)<2018)
```

```r
Apps <- Apps %>% 
  left_join(ex_dates, by = "examiner_id")
```

  
## Application Processing Time   
       
       

       
       
