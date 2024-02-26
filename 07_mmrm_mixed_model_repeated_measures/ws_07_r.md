# Working sheet
## 07 - MMRM - Mixed model repeated measures
V07.01.00 - 2024-02-26

## Load data
Change the path and the filename in the following box.


```R
library(readr)
df <- read_csv("data/df07.csv",
               show_col_types = FALSE,
               na = "."
              )

```

Look at the structure and the head of the dataset.


```R
str(df)
```

    spc_tbl_ [400 × 7] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
     $ USUBJID: chr [1:400] "Subject_0001" "Subject_0002" "Subject_0003" "Subject_0004" ...
     $ Group  : chr [1:400] "Group_1" "Group_2" "Group_1" "Group_2" ...
     $ Time   : chr [1:400] "Time_1" "Time_1" "Time_1" "Time_1" ...
     $ Y_comp : num [1:400] -0.0767 -0.412 -1.2297 0.0348 0.5125 ...
     $ XB     : num [1:400] 0 0 0 0 0 0 0 0 0 0 ...
     $ error  : num [1:400] -0.0767 -0.412 -1.2297 0.0348 0.5125 ...
     $ Y_mar  : num [1:400] -0.0767 -0.412 -1.2297 0.0348 0.5125 ...
     - attr(*, "spec")=
      .. cols(
      ..   USUBJID = [31mcol_character()[39m,
      ..   Group = [31mcol_character()[39m,
      ..   Time = [31mcol_character()[39m,
      ..   Y_comp = [32mcol_double()[39m,
      ..   XB = [32mcol_double()[39m,
      ..   error = [32mcol_double()[39m,
      ..   Y_mar = [32mcol_double()[39m
      .. )
     - attr(*, "problems")=<externalptr> 
    


```R
head(df)
```


<table class="dataframe">
<caption>A tibble: 6 × 7</caption>
<thead>
	<tr><th scope=col>USUBJID</th><th scope=col>Group</th><th scope=col>Time</th><th scope=col>Y_comp</th><th scope=col>XB</th><th scope=col>error</th><th scope=col>Y_mar</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Subject_0001</td><td>Group_1</td><td>Time_1</td><td>-0.07673563</td><td>0</td><td>-0.07673563</td><td>-0.07673563</td></tr>
	<tr><td>Subject_0002</td><td>Group_2</td><td>Time_1</td><td>-0.41197323</td><td>0</td><td>-0.41197323</td><td>-0.41197323</td></tr>
	<tr><td>Subject_0003</td><td>Group_1</td><td>Time_1</td><td>-1.22969052</td><td>0</td><td>-1.22969052</td><td>-1.22969052</td></tr>
	<tr><td>Subject_0004</td><td>Group_2</td><td>Time_1</td><td> 0.03483386</td><td>0</td><td> 0.03483386</td><td> 0.03483386</td></tr>
	<tr><td>Subject_0005</td><td>Group_1</td><td>Time_1</td><td> 0.51250275</td><td>0</td><td> 0.51250275</td><td> 0.51250275</td></tr>
	<tr><td>Subject_0006</td><td>Group_2</td><td>Time_1</td><td> 0.23896566</td><td>0</td><td> 0.23896566</td><td> 0.23896566</td></tr>
</tbody>
</table>


Look at frequencies and descriptive statistics.

The summary() function is the first approach.

describe() from the Hmisc package is an alternative.

```R
summary(df)
```


       USUBJID             Group               Time               Y_comp        
     Length:400         Length:400         Length:400         Min.   :-3.48061  
     Class :character   Class :character   Class :character   1st Qu.:-0.79310  
     Mode  :character   Mode  :character   Mode  :character   Median : 0.03148  
                                                              Mean   : 0.04075  
                                                              3rd Qu.: 0.76989  
                                                              Max.   : 3.64434  
                                                                                
           XB        error              Y_mar        
     Min.   :0   Min.   :-3.48061   Min.   :-3.4806  
     1st Qu.:0   1st Qu.:-0.79310   1st Qu.:-0.8918  
     Median :0   Median : 0.03148   Median :-0.1460  
     Mean   :0   Mean   : 0.04075   Mean   :-0.1855  
     3rd Qu.:0   3rd Qu.: 0.76989   3rd Qu.: 0.4886  
     Max.   :0   Max.   : 3.64434   Max.   : 2.9347  
                                    NA's   :60       



```R
library(Hmisc)
describe(df)
```


    df 
    
     7  Variables      400  Observations
    --------------------------------------------------------------------------------
    USUBJID 
           n  missing distinct 
         400        0      100 
    
    lowest : Subject_0001 Subject_0002 Subject_0003 Subject_0004 Subject_0005
    highest: Subject_0096 Subject_0097 Subject_0098 Subject_0099 Subject_0100
    --------------------------------------------------------------------------------
    Group 
           n  missing distinct 
         400        0        2 
                              
    Value      Group_1 Group_2
    Frequency      200     200
    Proportion     0.5     0.5
    --------------------------------------------------------------------------------
    Time 
           n  missing distinct 
         400        0        4 
                                          
    Value      Time_1 Time_2 Time_3 Time_4
    Frequency     100    100    100    100
    Proportion   0.25   0.25   0.25   0.25
    --------------------------------------------------------------------------------
    Y_comp 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         400        0      400        1  0.04075    1.329 -1.91524 -1.46490 
         .25      .50      .75      .90      .95 
    -0.79310  0.03148  0.76989  1.64146  2.05359 
    
    lowest : -3.48062 -2.94694 -2.88416 -2.87052 -2.3299 
    highest: 2.70477  2.76557  2.93471  3.31567  3.64434 
    --------------------------------------------------------------------------------
    XB 
           n  missing distinct     Info     Mean      Gmd 
         400        0        1        0        0        0 
                  
    Value        0
    Frequency  400
    Proportion   1
    --------------------------------------------------------------------------------
    error 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         400        0      400        1  0.04075    1.329 -1.91524 -1.46490 
         .25      .50      .75      .90      .95 
    -0.79310  0.03148  0.76989  1.64146  2.05359 
    
    lowest : -3.48062 -2.94694 -2.88416 -2.87052 -2.3299 
    highest: 2.70477  2.76557  2.93471  3.31567  3.64434 
    --------------------------------------------------------------------------------
    Y_mar 
           n  missing distinct     Info     Mean      Gmd      .05      .10 
         340       60      340        1  -0.1855    1.172  -1.9403  -1.4960 
         .25      .50      .75      .90      .95 
     -0.8918  -0.1460   0.4887   1.1045   1.6409 
    
    lowest : -3.48062 -2.94694 -2.88416 -2.87052 -2.3299 
    highest: 2.05285  2.06771  2.07109  2.4065   2.93471 
    --------------------------------------------------------------------------------


Plot the variables of interest with a scatter plot matrix from package GGally.



```R
library(GGally)
library(tidyverse)
df1 <- df %>% dplyr::select(Group, Time, Y_comp, XB, error, Y_mar)
ggpairs(df1)

```

    Warning message:
    "[1m[22mRemoved 60 rows containing non-finite values (`stat_boxplot()`)."
    Warning message:
    "[1m[22mRemoved 60 rows containing non-finite values (`stat_boxplot()`)."
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    Warning message in cor(x, y):
    "Standardabweichung ist Null"
    Warning message in ggally_statistic(data = data, mapping = mapping, na.rm = na.rm, :
    "Removed 60 rows containing missing values"
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    Warning message in cor(x, y):
    "Standardabweichung ist Null"
    Warning message in ggally_statistic(data = data, mapping = mapping, na.rm = na.rm, :
    "Removed 60 rows containing missing values"
    Warning message in cor(x, y):
    "Standardabweichung ist Null"
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    Warning message in ggally_statistic(data = data, mapping = mapping, na.rm = na.rm, :
    "Removed 60 rows containing missing values"
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    Warning message:
    "[1m[22mRemoved 60 rows containing non-finite values (`stat_bin()`)."
    [1m[22m`stat_bin()` using `bins = 30`. Pick better value with `binwidth`.
    Warning message:
    "[1m[22mRemoved 60 rows containing non-finite values (`stat_bin()`)."
    Warning message:
    "[1m[22mRemoved 60 rows containing missing values (`geom_point()`)."
    Warning message:
    "[1m[22mRemoved 60 rows containing missing values (`geom_point()`)."
    Warning message:
    "[1m[22mRemoved 60 rows containing missing values (`geom_point()`)."
    Warning message:
    "[1m[22mRemoved 60 rows containing non-finite values (`stat_density()`)."
    


    
![png](img_screenhots/output_10_1.png)
    


USUBJID omitted, because this variable has more than 15 levels.

## Assumptions mixed model repeated measure

- 


TODO: Check completeness of the assumptions and add example code for the checks.

## Coding of categorical variables

- Group
- Time

If using character variables, the first level of the alphabetic sort order will be regardes as reference level.

For some functions the character variable has to be converted to a factor, e.g., for the contrasts() function.



```R
table(df$Group)
table(df$Time)
```


    
    Group_1 Group_2 
        200     200 



    
    Time_1 Time_2 Time_3 Time_4 
       100    100    100    100 



```R
contrasts(as.factor(df$Group))
contrasts(as.factor(df$Time))
```


<table class="dataframe">
<caption>A matrix: 2 × 1 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Group_2</th></tr>
</thead>
<tbody>
	<tr><th scope=row>Group_1</th><td>0</td></tr>
	<tr><th scope=row>Group_2</th><td>1</td></tr>
</tbody>
</table>




<table class="dataframe">
<caption>A matrix: 4 × 3 of type dbl</caption>
<thead>
	<tr><th></th><th scope=col>Time_2</th><th scope=col>Time_3</th><th scope=col>Time_4</th></tr>
</thead>
<tbody>
	<tr><th scope=row>Time_1</th><td>0</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>Time_2</th><td>1</td><td>0</td><td>0</td></tr>
	<tr><th scope=row>Time_3</th><td>0</td><td>1</td><td>0</td></tr>
	<tr><th scope=row>Time_4</th><td>0</td><td>0</td><td>1</td></tr>
</tbody>
</table>



Dummy coding is the default coding.

## Fit the model



```R
library(SPR)
library(MASS)
library(glmmTMB) # https://cran.r-project.org/web/packages/glmmTMB/vignettes/covstruct.html
library(emmeans)
library(nlme)
library(lme4)
library(broom)
my_gls <- nlme::gls(Y_comp ~ Group + Time + Group*Time,
                       data = df07,
                       correlation = corSymm(form = ~ 1 | USUBJID),    #  unstructured correlation
                       weights = varIdent(form = ~ 1 | Time),          #  estimate variance at subsequent timepoints
                       na.action = na.exclude)
summary(my_gls)
```


    Generalized least squares fit by REML
      Model: Y_comp ~ Group + Time + Group * Time 
      Data: df07 
         AIC      BIC    logLik
      1036.4 1107.883 -500.2002
    
    Correlation Structure: General
     Formula: ~1 | USUBJID 
     Parameter estimate(s):
     Correlation: 
      1     2     3    
    2 0.779            
    3 0.574 0.728      
    4 0.484 0.535 0.763
    Variance function:
     Structure: Different standard deviations per stratum
     Formula: ~1 | Time 
     Parameter estimates:
      Time_1   Time_2   Time_3   Time_4 
    1.000000 1.095606 1.217203 1.499564 
    
    Coefficients:
                                 Value  Std.Error    t-value p-value
    (Intercept)             -0.0693893 0.13692310 -0.5067758  0.6126
    GroupGroup_2             0.3336959 0.19363850  1.7232931  0.0856
    TimeTime_2              -0.0531747 0.09614668 -0.5530579  0.5805
    TimeTime_3               0.0054841 0.14258851  0.0384609  0.9693
    TimeTime_4              -0.0566736 0.18351241 -0.3088272  0.7576
    GroupGroup_2:TimeTime_2  0.0280852 0.13597194  0.2065512  0.8365
    GroupGroup_2:TimeTime_3 -0.1262152 0.20165061 -0.6259106  0.5317
    GroupGroup_2:TimeTime_4 -0.1468045 0.25952574 -0.5656646  0.5719
    
     Correlation: 
                            (Intr) GrpG_2 TmTm_2 TmTm_3 TmTm_4 GG_2:TT_2 GG_2:TT_3
    GroupGroup_2            -0.707                                                
    TimeTime_2              -0.208  0.147                                         
    TimeTime_3              -0.289  0.205  0.573                                  
    TimeTime_4              -0.204  0.144  0.317  0.694                           
    GroupGroup_2:TimeTime_2  0.147 -0.208 -0.707 -0.405 -0.224                    
    GroupGroup_2:TimeTime_3  0.205 -0.289 -0.405 -0.707 -0.491  0.573             
    GroupGroup_2:TimeTime_4  0.144 -0.204 -0.224 -0.491 -0.707  0.317     0.694   
    
    Standardized residuals:
            Min          Q1         Med          Q3         Max 
    -2.55759465 -0.68545835 -0.02754474  0.67478149  2.54110499 
    
    Residual standard error: 0.9681925 
    Degrees of freedom: 400 total; 392 residual


## Summary of the model


```R
summary(my_gls)
```


    Generalized least squares fit by REML
      Model: Y_comp ~ Group + Time + Group * Time 
      Data: df07 
         AIC      BIC    logLik
      1036.4 1107.883 -500.2002
    
    Correlation Structure: General
     Formula: ~1 | USUBJID 
     Parameter estimate(s):
     Correlation: 
      1     2     3    
    2 0.779            
    3 0.574 0.728      
    4 0.484 0.535 0.763
    Variance function:
     Structure: Different standard deviations per stratum
     Formula: ~1 | Time 
     Parameter estimates:
      Time_1   Time_2   Time_3   Time_4 
    1.000000 1.095606 1.217203 1.499564 
    
    Coefficients:
                                 Value  Std.Error    t-value p-value
    (Intercept)             -0.0693893 0.13692310 -0.5067758  0.6126
    GroupGroup_2             0.3336959 0.19363850  1.7232931  0.0856
    TimeTime_2              -0.0531747 0.09614668 -0.5530579  0.5805
    TimeTime_3               0.0054841 0.14258851  0.0384609  0.9693
    TimeTime_4              -0.0566736 0.18351241 -0.3088272  0.7576
    GroupGroup_2:TimeTime_2  0.0280852 0.13597194  0.2065512  0.8365
    GroupGroup_2:TimeTime_3 -0.1262152 0.20165061 -0.6259106  0.5317
    GroupGroup_2:TimeTime_4 -0.1468045 0.25952574 -0.5656646  0.5719
    
     Correlation: 
                            (Intr) GrpG_2 TmTm_2 TmTm_3 TmTm_4 GG_2:TT_2 GG_2:TT_3
    GroupGroup_2            -0.707                                                
    TimeTime_2              -0.208  0.147                                         
    TimeTime_3              -0.289  0.205  0.573                                  
    TimeTime_4              -0.204  0.144  0.317  0.694                           
    GroupGroup_2:TimeTime_2  0.147 -0.208 -0.707 -0.405 -0.224                    
    GroupGroup_2:TimeTime_3  0.205 -0.289 -0.405 -0.707 -0.491  0.573             
    GroupGroup_2:TimeTime_4  0.144 -0.204 -0.224 -0.491 -0.707  0.317     0.694   
    
    Standardized residuals:
            Min          Q1         Med          Q3         Max 
    -2.55759465 -0.68545835 -0.02754474  0.67478149  2.54110499 
    
    Residual standard error: 0.9681925 
    Degrees of freedom: 400 total; 392 residual


## Estimates


```R
# tidy() not supported
df1 <- data.frame("Estimates" = coef(my_gls))
df1

```


<table class="dataframe">
<caption>A data.frame: 8 × 1</caption>
<thead>
	<tr><th></th><th scope=col>Estimates</th></tr>
	<tr><th></th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>-0.069389313</td></tr>
	<tr><th scope=row>GroupGroup_2</th><td> 0.333695897</td></tr>
	<tr><th scope=row>TimeTime_2</th><td>-0.053174683</td></tr>
	<tr><th scope=row>TimeTime_3</th><td> 0.005484079</td></tr>
	<tr><th scope=row>TimeTime_4</th><td>-0.056673629</td></tr>
	<tr><th scope=row>GroupGroup_2:TimeTime_2</th><td> 0.028085170</td></tr>
	<tr><th scope=row>GroupGroup_2:TimeTime_3</th><td>-0.126215247</td></tr>
	<tr><th scope=row>GroupGroup_2:TimeTime_4</th><td>-0.146804525</td></tr>
</tbody>
</table>



## Confidence intervals


```R
df2 <-intervals(my_gls)
df2
```


    Approximate 95% confidence intervals
    
     Coefficients:
                                  lower         est.     upper
    (Intercept)             -0.33858480 -0.069389313 0.1998062
    GroupGroup_2            -0.04700401  0.333695897 0.7143958
    TimeTime_2              -0.24220234 -0.053174683 0.1358530
    TimeTime_3              -0.27484980  0.005484079 0.2858180
    TimeTime_4              -0.41746529 -0.056673629 0.3041180
    GroupGroup_2:TimeTime_2 -0.23924031  0.028085170 0.2954106
    GroupGroup_2:TimeTime_3 -0.52266722 -0.126215247 0.2702367
    GroupGroup_2:TimeTime_4 -0.65704098 -0.146804525 0.3634319
    
     Correlation structure:
                 lower      est.     upper
    cor(1,2) 0.6883582 0.7791464 0.8459000
    cor(1,3) 0.4263403 0.5739051 0.6917156
    cor(1,4) 0.3192077 0.4842732 0.6208203
    cor(2,3) 0.6214156 0.7283397 0.8086294
    cor(2,4) 0.3790272 0.5347039 0.6609483
    cor(3,4) 0.6672953 0.7631717 0.8341672
    
     Variance function:
               lower     est.    upper
    Time_2 0.9673921 1.095606 1.240812
    Time_3 1.0336540 1.217203 1.433346
    Time_4 1.2592235 1.499564 1.785776
    
     Residual standard error:
        lower      est.     upper 
    0.8403933 0.9681925 1.1154262 


## Anova


```R
anova(my_gls)
```


<table class="dataframe">
<caption>A anova.lme: 4 × 3</caption>
<thead>
	<tr><th></th><th scope=col>numDF</th><th scope=col>F-value</th><th scope=col>p-value</th></tr>
	<tr><th></th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>(Intercept)</th><td>1</td><td>0.7268036</td><td>0.39444181</td></tr>
	<tr><th scope=row>Group</th><td>1</td><td>2.7222672</td><td>0.09975837</td></tr>
	<tr><th scope=row>Time</th><td>3</td><td>0.4038847</td><td>0.75028908</td></tr>
	<tr><th scope=row>Group:Time</th><td>3</td><td>0.2918742</td><td>0.83127312</td></tr>
</tbody>
</table>



## Residuals



```R
# augment() not supported
df3 <- data.frame("Residuals" = residuals(my_gls))
df3 %>% head()
```


<table class="dataframe">
<caption>A data.frame: 6 × 1</caption>
<thead>
	<tr><th></th><th scope=col>Residuals</th></tr>
	<tr><th></th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><th scope=row>1</th><td>-0.007346321</td></tr>
	<tr><th scope=row>2</th><td>-0.676279818</td></tr>
	<tr><th scope=row>3</th><td>-1.160301210</td></tr>
	<tr><th scope=row>4</th><td>-0.229472726</td></tr>
	<tr><th scope=row>5</th><td> 0.581892064</td></tr>
	<tr><th scope=row>6</th><td>-0.025340927</td></tr>
</tbody>
</table>



Only the first rows are displayed.
