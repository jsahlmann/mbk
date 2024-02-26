# Working sheet
## 05 - Simple Cox regression
V05.01.00 - 2024-02-26

## Load data
Change the path and the filename in the following box.

Source of data:  Mayo Clinic trial in PBC conducted between 1974 and 1984

Data set pbc.csv


```R
library(readr)
df <- read_csv("data/pbc.csv",
                 show_col_types = FALSE,
              na = c("", "."))
```

Look at the structure and the head of the dataset.


```R
str(df)
```

    spc_tbl_ [418 × 20] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
     $ id      : num [1:418] 1 2 3 4 5 6 7 8 9 10 ...
     $ time    : num [1:418] 400 4500 1012 1925 1504 ...
     $ status  : num [1:418] 2 0 2 2 1 2 0 2 2 2 ...
     $ trt     : num [1:418] 1 1 1 1 2 2 2 2 1 2 ...
     $ age     : num [1:418] 58.8 56.4 70.1 54.7 38.1 ...
     $ sex     : chr [1:418] "f" "f" "m" "f" ...
     $ ascites : num [1:418] 1 0 0 0 0 0 0 0 0 1 ...
     $ hepato  : num [1:418] 1 1 0 1 1 1 1 0 0 0 ...
     $ spiders : num [1:418] 1 1 0 1 1 0 0 0 1 1 ...
     $ edema   : num [1:418] 1 0 0.5 0.5 0 0 0 0 0 1 ...
     $ bili    : num [1:418] 14.5 1.1 1.4 1.8 3.4 0.8 1 0.3 3.2 12.6 ...
     $ chol    : num [1:418] 261 302 176 244 279 248 322 280 562 200 ...
     $ albumin : num [1:418] 2.6 4.14 3.48 2.54 3.53 3.98 4.09 4 3.08 2.74 ...
     $ copper  : num [1:418] 156 54 210 64 143 50 52 52 79 140 ...
     $ alk.phos: num [1:418] 1718 7395 516 6122 671 ...
     $ ast     : num [1:418] 137.9 113.5 96.1 60.6 113.2 ...
     $ trig    : num [1:418] 172 88 55 92 72 63 213 189 88 143 ...
     $ platelet: num [1:418] 190 221 151 183 136 NA 204 373 251 302 ...
     $ protime : num [1:418] 12.2 10.6 12 10.3 10.9 11 9.7 11 11 11.5 ...
     $ stage   : num [1:418] 4 3 4 4 3 3 3 3 2 4 ...
     - attr(*, "spec")=
      .. cols(
      ..   id = [32mcol_double()[39m,
      ..   time = [32mcol_double()[39m,
      ..   status = [32mcol_double()[39m,
      ..   trt = [32mcol_double()[39m,
      ..   age = [32mcol_double()[39m,
      ..   sex = [31mcol_character()[39m,
      ..   ascites = [32mcol_double()[39m,
      ..   hepato = [32mcol_double()[39m,
      ..   spiders = [32mcol_double()[39m,
      ..   edema = [32mcol_double()[39m,
      ..   bili = [32mcol_double()[39m,
      ..   chol = [32mcol_double()[39m,
      ..   albumin = [32mcol_double()[39m,
      ..   copper = [32mcol_double()[39m,
      ..   alk.phos = [32mcol_double()[39m,
      ..   ast = [32mcol_double()[39m,
      ..   trig = [32mcol_double()[39m,
      ..   platelet = [32mcol_double()[39m,
      ..   protime = [32mcol_double()[39m,
      ..   stage = [32mcol_double()[39m
      .. )
     - attr(*, "problems")=<externalptr> 
    


```R
head(df)
```


<table class="dataframe">
<caption>A tibble: 6 × 20</caption>
<thead>
	<tr><th scope=col>id</th><th scope=col>time</th><th scope=col>status</th><th scope=col>trt</th><th scope=col>age</th><th scope=col>sex</th><th scope=col>ascites</th><th scope=col>hepato</th><th scope=col>spiders</th><th scope=col>edema</th><th scope=col>bili</th><th scope=col>chol</th><th scope=col>albumin</th><th scope=col>copper</th><th scope=col>alk.phos</th><th scope=col>ast</th><th scope=col>trig</th><th scope=col>platelet</th><th scope=col>protime</th><th scope=col>stage</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td> 400</td><td>2</td><td>1</td><td>58.76523</td><td>f</td><td>1</td><td>1</td><td>1</td><td>1.0</td><td>14.5</td><td>261</td><td>2.60</td><td>156</td><td>1718.0</td><td>137.95</td><td>172</td><td>190</td><td>12.2</td><td>4</td></tr>
	<tr><td>2</td><td>4500</td><td>0</td><td>1</td><td>56.44627</td><td>f</td><td>0</td><td>1</td><td>1</td><td>0.0</td><td> 1.1</td><td>302</td><td>4.14</td><td> 54</td><td>7394.8</td><td>113.52</td><td> 88</td><td>221</td><td>10.6</td><td>3</td></tr>
	<tr><td>3</td><td>1012</td><td>2</td><td>1</td><td>70.07255</td><td>m</td><td>0</td><td>0</td><td>0</td><td>0.5</td><td> 1.4</td><td>176</td><td>3.48</td><td>210</td><td> 516.0</td><td> 96.10</td><td> 55</td><td>151</td><td>12.0</td><td>4</td></tr>
	<tr><td>4</td><td>1925</td><td>2</td><td>1</td><td>54.74059</td><td>f</td><td>0</td><td>1</td><td>1</td><td>0.5</td><td> 1.8</td><td>244</td><td>2.54</td><td> 64</td><td>6121.8</td><td> 60.63</td><td> 92</td><td>183</td><td>10.3</td><td>4</td></tr>
	<tr><td>5</td><td>1504</td><td>1</td><td>2</td><td>38.10541</td><td>f</td><td>0</td><td>1</td><td>1</td><td>0.0</td><td> 3.4</td><td>279</td><td>3.53</td><td>143</td><td> 671.0</td><td>113.15</td><td> 72</td><td>136</td><td>10.9</td><td>3</td></tr>
	<tr><td>6</td><td>2503</td><td>2</td><td>2</td><td>66.25873</td><td>f</td><td>0</td><td>1</td><td>0</td><td>0.0</td><td> 0.8</td><td>248</td><td>3.98</td><td> 50</td><td> 944.0</td><td> 93.00</td><td> 63</td><td> NA</td><td>11.0</td><td>3</td></tr>
</tbody>
</table>



Look at frequencies and descriptive statistics.

The summary() function is the first approach.

describe() from the Hmisc package is an alternative.


```R
summary(df)
```


           id             time          status            trt       
     Min.   :  1.0   Min.   :  41   Min.   :0.0000   Min.   :1.000  
     1st Qu.:105.2   1st Qu.:1093   1st Qu.:0.0000   1st Qu.:1.000  
     Median :209.5   Median :1730   Median :0.0000   Median :1.000  
     Mean   :209.5   Mean   :1918   Mean   :0.8301   Mean   :1.494  
     3rd Qu.:313.8   3rd Qu.:2614   3rd Qu.:2.0000   3rd Qu.:2.000  
     Max.   :418.0   Max.   :4795   Max.   :2.0000   Max.   :2.000  
                                                     NA's   :106    
          age            sex               ascites            hepato      
     Min.   :26.28   Length:418         Min.   :0.00000   Min.   :0.0000  
     1st Qu.:42.83   Class :character   1st Qu.:0.00000   1st Qu.:0.0000  
     Median :51.00   Mode  :character   Median :0.00000   Median :1.0000  
     Mean   :50.74                      Mean   :0.07692   Mean   :0.5128  
     3rd Qu.:58.24                      3rd Qu.:0.00000   3rd Qu.:1.0000  
     Max.   :78.44                      Max.   :1.00000   Max.   :1.0000  
                                        NA's   :106       NA's   :106     
        spiders           edema             bili             chol       
     Min.   :0.0000   Min.   :0.0000   Min.   : 0.300   Min.   : 120.0  
     1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.: 0.800   1st Qu.: 249.5  
     Median :0.0000   Median :0.0000   Median : 1.400   Median : 309.5  
     Mean   :0.2885   Mean   :0.1005   Mean   : 3.221   Mean   : 369.5  
     3rd Qu.:1.0000   3rd Qu.:0.0000   3rd Qu.: 3.400   3rd Qu.: 400.0  
     Max.   :1.0000   Max.   :1.0000   Max.   :28.000   Max.   :1775.0  
     NA's   :106                                        NA's   :134     
        albumin          copper          alk.phos            ast        
     Min.   :1.960   Min.   :  4.00   Min.   :  289.0   Min.   : 26.35  
     1st Qu.:3.243   1st Qu.: 41.25   1st Qu.:  871.5   1st Qu.: 80.60  
     Median :3.530   Median : 73.00   Median : 1259.0   Median :114.70  
     Mean   :3.497   Mean   : 97.65   Mean   : 1982.7   Mean   :122.56  
     3rd Qu.:3.770   3rd Qu.:123.00   3rd Qu.: 1980.0   3rd Qu.:151.90  
     Max.   :4.640   Max.   :588.00   Max.   :13862.4   Max.   :457.25  
                     NA's   :108      NA's   :106       NA's   :106     
          trig           platelet        protime          stage      
     Min.   : 33.00   Min.   : 62.0   Min.   : 9.00   Min.   :1.000  
     1st Qu.: 84.25   1st Qu.:188.5   1st Qu.:10.00   1st Qu.:2.000  
     Median :108.00   Median :251.0   Median :10.60   Median :3.000  
     Mean   :124.70   Mean   :257.0   Mean   :10.73   Mean   :3.024  
     3rd Qu.:151.00   3rd Qu.:318.0   3rd Qu.:11.10   3rd Qu.:4.000  
     Max.   :598.00   Max.   :721.0   Max.   :18.00   Max.   :4.000  
     NA's   :136      NA's   :11      NA's   :2       NA's   :6      



```R
library(Hmisc)
describe(df)
```


    df 
    
     20  Variables      418  Observations
    --------------------------------------------------------------------------------
    id 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         418        0      418        1    209.5    139.7    21.85    42.70 
         .25      .50      .75      .90      .95 
      105.25   209.50   313.75   376.30   397.15 
    
    lowest :   1   2   3   4   5, highest: 414 415 416 417 418
    --------------------------------------------------------------------------------
    time 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         418        0      399        1     1918     1253    245.1    606.8 
         .25      .50      .75      .90      .95 
      1092.8   1730.0   2613.5   3524.2   4040.6 
    
    lowest :   41   43   51   71   77, highest: 4500 4509 4523 4556 4795
    --------------------------------------------------------------------------------
    status 
           n  missing distinct     Info     Mean      Gmd 
         418        0        3    0.772   0.8301   0.9699 
                                
    Value          0     1     2
    Frequency    232    25   161
    Proportion 0.555 0.060 0.385
    
    For the frequency table, variable is rounded to the nearest 0
    --------------------------------------------------------------------------------
    trt 
           n  missing distinct     Info     Mean      Gmd 
         312      106        2     0.75    1.494   0.5015 
                          
    Value          1     2
    Frequency    158   154
    Proportion 0.506 0.494
    --------------------------------------------------------------------------------
    age 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         418        0      344        1    50.74    11.96    33.84    36.37 
         .25      .50      .75      .90      .95 
       42.83    51.00    58.24    64.30    67.92 
    
    lowest : 26.2779 28.8843 29.5551 30.2752 30.5736
    highest: 74.5243 75.0007 75.0116 76.7091 78.4394
    --------------------------------------------------------------------------------
    sex 
           n  missing distinct 
         418        0        2 
                          
    Value          f     m
    Frequency    374    44
    Proportion 0.895 0.105
    --------------------------------------------------------------------------------
    ascites 
           n  missing distinct     Info      Sum     Mean      Gmd 
         312      106        2    0.213       24  0.07692   0.1425 
    
    --------------------------------------------------------------------------------
    hepato 
           n  missing distinct     Info      Sum     Mean      Gmd 
         312      106        2     0.75      160   0.5128   0.5013 
    
    --------------------------------------------------------------------------------
    spiders 
           n  missing distinct     Info      Sum     Mean      Gmd 
         312      106        2    0.616       90   0.2885   0.4118 
    
    --------------------------------------------------------------------------------
    edema 
           n  missing distinct     Info     Mean      Gmd 
         418        0        3    0.391   0.1005   0.1756 
                                
    Value        0.0   0.5   1.0
    Frequency    354    44    20
    Proportion 0.847 0.105 0.048
    
    For the frequency table, variable is rounded to the nearest 0
    --------------------------------------------------------------------------------
    bili 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         418        0       98    0.998    3.221    3.742     0.50     0.60 
         .25      .50      .75      .90      .95 
        0.80     1.40     3.40     8.03    14.00 
    
    lowest : 0.3  0.4  0.5  0.6  0.7 , highest: 21.6 22.5 24.5 25.5 28  
    --------------------------------------------------------------------------------
    chol 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         284      134      201        1    369.5    194.5    188.4    213.6 
         .25      .50      .75      .90      .95 
       249.5    309.5    400.0    560.8    674.0 
    
    lowest :  120  127  132  149  151, highest: 1336 1480 1600 1712 1775
    --------------------------------------------------------------------------------
    albumin 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         418        0      154        1    3.497    0.473    2.750    2.967 
         .25      .50      .75      .90      .95 
       3.243    3.530    3.770    4.010    4.141 
    
    lowest : 1.96 2.1  2.23 2.27 2.31, highest: 4.3  4.38 4.4  4.52 4.64
    --------------------------------------------------------------------------------
    copper 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         310      108      158        1    97.65    83.16    17.45    24.00 
         .25      .50      .75      .90      .95 
       41.25    73.00   123.00   208.10   249.20 
    
    lowest :   4   9  10  11  12, highest: 412 444 464 558 588
    --------------------------------------------------------------------------------
    alk.phos 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         312      106      295        1     1983     1760    599.6    663.0 
         .25      .50      .75      .90      .95 
       871.5   1259.0   1980.0   3826.4   6669.9 
    
    lowest : 289     310     369     377     414    
    highest: 11046.6 11320.2 11552   12258.8 13862.4
    --------------------------------------------------------------------------------
    ast 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         312      106      179        1    122.6    60.45    54.25    60.45 
         .25      .50      .75      .90      .95 
       80.60   114.70   151.90   196.47   219.25 
    
    lowest : 26.35  28.38  41.85  43.4   45    , highest: 288    299.15 328.6  338    457.25
    --------------------------------------------------------------------------------
    trig 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         282      136      146        1    124.7    64.07    56.00    63.10 
         .25      .50      .75      .90      .95 
       84.25   108.00   151.00   195.00   230.95 
    
    lowest :  33  44  46  49  50, highest: 319 322 382 432 598
    --------------------------------------------------------------------------------
    platelet 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         407       11      243        1      257    109.7    114.9    138.2 
         .25      .50      .75      .90      .95 
       188.5    251.0    318.0    386.2    430.0 
    
    lowest :  62  70  71  76  79, highest: 517 518 539 563 721
    --------------------------------------------------------------------------------
    protime 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         416        2       48    0.998    10.73    1.029     9.60     9.80 
         .25      .50      .75      .90      .95 
       10.00    10.60    11.10    12.00    12.45 
    
    lowest : 9    9.1  9.2  9.3  9.4 , highest: 13.8 14.1 15.2 17.1 18  
    --------------------------------------------------------------------------------
    stage 
           n  missing distinct     Info     Mean      Gmd 
         412        6        4    0.893    3.024   0.9519 
                                      
    Value          1     2     3     4
    Frequency     21    92   155   144
    Proportion 0.051 0.223 0.376 0.350
    
    For the frequency table, variable is rounded to the nearest 0
    --------------------------------------------------------------------------------


Plot the variables of interest with a scatter plot matrix from package GGally.


```R
library(GGally)
library(tidyverse)
df1 <- df %>% dplyr::select(time, status, age, sex)
ggpairs(df1)
```

    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    


    
![png](output_11_1.png)
    


## Assumptions of Cox regression
-

TODO: Check completeness of the assumptions and add example code for the checks.

## Coding of categorical variables

- sex

If using character variables, the first level of the alphabetic sort order will be regardes as reference level.

For some functions the character variable has to be converted to a factor, e.g., for the contrasts() function.


```R
table(df$sex)
```


    
      f   m 
    374  44 



```R
contrasts(as.factor(df$sex))
```


<table class="dataframe">
<caption>A matrix: 2 × 1 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>m</th></tr>
</thead>
<tbody>
	<tr><th scope=row>f</th><td>0</td></tr>
	<tr><th scope=row>m</th><td>1</td></tr>
</tbody>
</table>



## Fit the model


```R
table(df$status, useNA = "always")
```


    
       0    1    2 <NA> 
     232   25  161    0 



```R
library(survival)
df1 <- df %>% select(time, status, sex) %>% na.omit()
df1$status <- ifelse(df1$status == 0, 0, 1) # recode of status, all events equal 1, censored equal 0
my_cox <- coxph(formula = Surv(time, status) ~ sex, data = df1)
```

## Summary of the model


```R
summary(my_cox)
```


    Call:
    coxph(formula = Surv(time, status) ~ sex, data = df1)
    
      n= 418, number of events= 186 
    
           coef exp(coef) se(coef)    z Pr(>|z|)  
    sexm 0.3613    1.4353   0.2089 1.73   0.0836 .
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    
         exp(coef) exp(-coef) lower .95 upper .95
    sexm     1.435     0.6967    0.9531     2.161
    
    Concordance= 0.518  (se = 0.013 )
    Likelihood ratio test= 2.75  on 1 df,   p=0.1
    Wald test            = 2.99  on 1 df,   p=0.08
    Score (logrank) test = 3.02  on 1 df,   p=0.08
    


## Estimates


```R
library(broom)
tidy(my_cox)
```


<table class="dataframe">
<caption>A tibble: 1 × 5</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>estimate</th><th scope=col>std.error</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>sexm</td><td>0.361347</td><td>0.2088782</td><td>1.729941</td><td>0.08364085</td></tr>
</tbody>
</table>



## Model fitness


```R
glance(my_cox)
```


<table class="dataframe">
<caption>A tibble: 1 × 18</caption>
<thead>
	<tr><th scope=col>n</th><th scope=col>nevent</th><th scope=col>statistic.log</th><th scope=col>p.value.log</th><th scope=col>statistic.sc</th><th scope=col>p.value.sc</th><th scope=col>statistic.wald</th><th scope=col>p.value.wald</th><th scope=col>statistic.robust</th><th scope=col>p.value.robust</th><th scope=col>r.squared</th><th scope=col>r.squared.max</th><th scope=col>concordance</th><th scope=col>std.error.concordance</th><th scope=col>logLik</th><th scope=col>AIC</th><th scope=col>BIC</th><th scope=col>nobs</th></tr>
	<tr><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>418</td><td>186</td><td>2.748578</td><td>0.09734096</td><td>3.024981</td><td>0.08199126</td><td>2.99</td><td>0.08364085</td><td>NA</td><td>NA</td><td>0.006553974</td><td>0.9919921</td><td>0.5182891</td><td>0.01261923</td><td>-1007.537</td><td>2017.075</td><td>2020.301</td><td>418</td></tr>
</tbody>
</table>



## Residuals


```R
augment(my_cox) %>% head()
```


<table class="dataframe">
<caption>A tibble: 6 × 5</caption>
<thead>
	<tr><th scope=col>Surv(time, status)</th><th scope=col>sex</th><th scope=col>.fitted</th><th scope=col>.se.fit</th><th scope=col>.resid</th></tr>
	<tr><th scope=col>&lt;Surv&gt;</th><th scope=col>&lt;fct&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td> 400, 1</td><td>f</td><td>0.000000</td><td>0.0000000</td><td> 0.9239778</td></tr>
	<tr><td>4500, 0</td><td>f</td><td>0.000000</td><td>0.0000000</td><td>-1.0953958</td></tr>
	<tr><td>1012, 1</td><td>m</td><td>0.361347</td><td>0.2088782</td><td> 0.6914445</td></tr>
	<tr><td>1925, 1</td><td>f</td><td>0.000000</td><td>0.0000000</td><td> 0.5928711</td></tr>
	<tr><td>1504, 1</td><td>f</td><td>0.000000</td><td>0.0000000</td><td> 0.6651088</td></tr>
	<tr><td>2503, 1</td><td>f</td><td>0.000000</td><td>0.0000000</td><td> 0.4433259</td></tr>
</tbody>
</table>




```R

```
