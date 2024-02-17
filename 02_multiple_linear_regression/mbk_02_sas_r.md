# 02 - Multivariate linear regression

## Data

Source of data: UCLA

Introduction to regression in R. UCLA: Statistical Consulting Group. 
fromhttps://stats.oarc.ucla.edu/r/seminars/introduction-to-regression-in-r/.

elementary school academic performance index
https://stats.idre.ucla.edu/wp-content/uploads/2019/02/elemapi2v2.csv

Data set elemapi2.csv


```R
library(readr)
elemapi2 <- read_csv("data/elemapi2.csv",
                 show_col_types = FALSE)
head(elemapi2)

```


<table class="dataframe">
<caption>A tibble: 6 × 22</caption>
<thead>
	<tr><th scope=col>snum</th><th scope=col>dnum</th><th scope=col>api00</th><th scope=col>api99</th><th scope=col>growth</th><th scope=col>meals</th><th scope=col>ell</th><th scope=col>yr_rnd</th><th scope=col>mobility</th><th scope=col>acs_k3</th><th scope=col>⋯</th><th scope=col>hsg</th><th scope=col>some_col</th><th scope=col>col_grad</th><th scope=col>grad_sch</th><th scope=col>avg_ed</th><th scope=col>full</th><th scope=col>emer</th><th scope=col>enroll</th><th scope=col>mealcat</th><th scope=col>collcat</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>⋯</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td> 906</td><td>41</td><td>693</td><td>600</td><td>93</td><td>67</td><td> 9</td><td>0</td><td>11</td><td>16</td><td>⋯</td><td> 0</td><td> 0</td><td> 0</td><td> 0</td><td>  NA</td><td> 76</td><td>24</td><td>247</td><td>2</td><td>1</td></tr>
	<tr><td> 889</td><td>41</td><td>570</td><td>501</td><td>69</td><td>92</td><td>21</td><td>0</td><td>33</td><td>15</td><td>⋯</td><td> 0</td><td> 0</td><td> 0</td><td> 0</td><td>  NA</td><td> 79</td><td>19</td><td>463</td><td>3</td><td>1</td></tr>
	<tr><td> 887</td><td>41</td><td>546</td><td>472</td><td>74</td><td>97</td><td>29</td><td>0</td><td>36</td><td>17</td><td>⋯</td><td> 0</td><td> 0</td><td> 0</td><td> 0</td><td>  NA</td><td> 68</td><td>29</td><td>395</td><td>3</td><td>1</td></tr>
	<tr><td> 876</td><td>41</td><td>571</td><td>487</td><td>84</td><td>90</td><td>27</td><td>0</td><td>27</td><td>20</td><td>⋯</td><td>45</td><td> 9</td><td> 9</td><td> 0</td><td>1.91</td><td> 87</td><td>11</td><td>418</td><td>3</td><td>1</td></tr>
	<tr><td> 888</td><td>41</td><td>478</td><td>425</td><td>53</td><td>89</td><td>30</td><td>0</td><td>44</td><td>18</td><td>⋯</td><td>50</td><td> 0</td><td> 0</td><td> 0</td><td>1.50</td><td> 87</td><td>13</td><td>520</td><td>3</td><td>1</td></tr>
	<tr><td>4284</td><td>98</td><td>858</td><td>844</td><td>14</td><td>10</td><td> 3</td><td>0</td><td>10</td><td>20</td><td>⋯</td><td> 8</td><td>24</td><td>36</td><td>31</td><td>3.89</td><td>100</td><td> 0</td><td>343</td><td>1</td><td>2</td></tr>
</tbody>
</table>



## SAS program snippet

The following SAS code will be executed.
proc reg data = elemapi2;
  model api00 = ell meals yr_rnd mobility acs_k3 acs_46 full emer enroll / stb;
run;

The option /stb provides standardized estimates.

## R chunk

Packages will be loaded in the chunk where they are first needed.

A similar R program might look like this. It uses the lm() function.

The tidy() function from the broom-packages formats the output into a tibble for easier processing.


```R
library(broom)
# Linear regression using lm()
my_lm <- lm(api00 ~ ell + meals + yr_rnd + mobility + acs_k3 + acs_46 + full + emer + enroll, data = elemapi2)
tidy(my_lm)

```


<table class="dataframe">
<caption>A tibble: 10 × 5</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>estimate</th><th scope=col>std.error</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>(Intercept)</td><td>758.94179320</td><td>62.28600730</td><td> 12.1847880</td><td>4.134890e-29</td></tr>
	<tr><td>ell        </td><td> -0.86007067</td><td> 0.21063175</td><td> -4.0832907</td><td>5.402931e-05</td></tr>
	<tr><td>meals      </td><td> -2.94821634</td><td> 0.17034524</td><td>-17.3073006</td><td>4.715844e-50</td></tr>
	<tr><td>yr_rnd     </td><td>-19.88874706</td><td> 9.25844226</td><td> -2.1481742</td><td>3.232329e-02</td></tr>
	<tr><td>mobility   </td><td> -1.30135168</td><td> 0.43620533</td><td> -2.9833466</td><td>3.032785e-03</td></tr>
	<tr><td>acs_k3     </td><td>  1.31870017</td><td> 2.25268291</td><td>  0.5853909</td><td>5.586278e-01</td></tr>
	<tr><td>acs_46     </td><td>  2.03245622</td><td> 0.79832127</td><td>  2.5459126</td><td>1.128813e-02</td></tr>
	<tr><td>full       </td><td>  0.60971500</td><td> 0.47582046</td><td>  1.2813972</td><td>2.008254e-01</td></tr>
	<tr><td>emer       </td><td> -0.70661916</td><td> 0.60540863</td><td> -1.1671772</td><td>2.438612e-01</td></tr>
	<tr><td>enroll     </td><td> -0.01216405</td><td> 0.01679211</td><td> -0.7243903</td><td>4.692661e-01</td></tr>
</tbody>
</table>



## Results

The output is divided into blocks to explain it and to reproduce it afterwards in the different languages.

### Block 1
![Block 1](img_screenshots/block_1.png)

Number of observations read is the number of observations in the dataset.

Number of observation used is the number of complete cases regarding the variables used for the SAS program snippet.

Number of observations with missing values in the model variables.

## R chunk for reproduction



```R
library(tidyverse)
# Number of observations read
nrow(elemapi2)
# Number of observations used
sum(elemapi2 %>% 
    select(api00, ell, meals, yr_rnd, mobility, acs_k3, acs_46, full, emer, enroll) %>%
    complete.cases())

# Alternative for number of observations used.
nobs(my_lm)
```

    ── [1mAttaching core tidyverse packages[22m ──────────────────────────────────────────────────────────────── tidyverse 2.0.0 ──
    [32m✔[39m [34mdplyr    [39m 1.1.2     [32m✔[39m [34mpurrr    [39m 1.0.1
    [32m✔[39m [34mforcats  [39m 1.0.0     [32m✔[39m [34mstringr  [39m 1.5.0
    [32m✔[39m [34mggplot2  [39m 3.4.4     [32m✔[39m [34mtibble   [39m 3.2.1
    [32m✔[39m [34mlubridate[39m 1.9.2     [32m✔[39m [34mtidyr    [39m 1.3.0
    ── [1mConflicts[22m ────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    [36mℹ[39m Use the conflicted package ([3m[34m<http://conflicted.r-lib.org/>[39m[23m) to force all conflicts to become errors
    


400



395



395


The number of observations is the number of rows in the dataset.

The number of observations used is either the number of complete cases regarding the variables in the model or the number returned from the nobs() function.

### Block 2
![Block 2](img_screenshots/block_2.png)

An analysis of variance was performed for the data.

#### Source
The column source in this table presents the sources of variance. They are divided into

-  Model,
-  Residual, and
-  Total.

Model stands for the variance which is explained by the independent variables.

Total stands for the total variance which can be divided into the variance explained from the model and the variance not explained from the model called residual or error.

Sum of squares of model plus sum of squares of error is equal to the total sum of squares.

#### DF

The degrees of freedom are calculated as follows:

The df for total is the number of used observations minus one.

The df for the total is the number of variables in the model minus one. The intercept is counting as one variable if not explicitely omitted.

The for for the error is the difference of $df_{total} - df_{model}$.

#### Sum of squares

Calculation of sum squares might be added here later.

It can be found in several other tutorials.

#### Mean square

The mean square is the sum of squares divided by the degrees of freedom.

#### F-Value

The F-value is the mean square model divided by the mean square error. The degrees of freedom are $df_{model}$ and $df_{error}$.

#### Pr > F

The null hypothesis tested is that there is no linear relationship between the independent and the dependent variables.

The alternative hypothesis states that there is a linear relationship.


## R chunk for reproduction



```R
my_aov<- aov(api00 ~ ell + meals + yr_rnd + mobility + acs_k3 + acs_46 + full + emer + enroll, data = elemapi2)
tidy(my_aov)
```


<table class="dataframe">
<caption>A tibble: 10 × 6</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>df</th><th scope=col>sumsq</th><th scope=col>meansq</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>ell      </td><td>  1</td><td>4677256.605</td><td>4677256.605</td><td>1451.3842990</td><td>1.116668e-132</td></tr>
	<tr><td>meals    </td><td>  1</td><td>1890980.592</td><td>1890980.592</td><td> 586.7840428</td><td> 2.049987e-79</td></tr>
	<tr><td>yr_rnd   </td><td>  1</td><td>  50911.559</td><td>  50911.559</td><td>  15.7982006</td><td> 8.415214e-05</td></tr>
	<tr><td>mobility </td><td>  1</td><td>  16615.912</td><td>  16615.912</td><td>   5.1560297</td><td> 2.371755e-02</td></tr>
	<tr><td>acs_k3   </td><td>  1</td><td>   7182.540</td><td>   7182.540</td><td>   2.2287907</td><td> 1.362788e-01</td></tr>
	<tr><td>acs_46   </td><td>  1</td><td>  20061.047</td><td>  20061.047</td><td>   6.2250782</td><td> 1.301343e-02</td></tr>
	<tr><td>full     </td><td>  1</td><td>  71776.875</td><td>  71776.875</td><td>  22.2728489</td><td> 3.316001e-06</td></tr>
	<tr><td>emer     </td><td>  1</td><td>   4225.835</td><td>   4225.835</td><td>   1.3113050</td><td> 2.528699e-01</td></tr>
	<tr><td>enroll   </td><td>  1</td><td>   1691.041</td><td>   1691.041</td><td>   0.5247414</td><td> 4.692661e-01</td></tr>
	<tr><td>Residuals</td><td>385</td><td>1240707.781</td><td>   3222.618</td><td>          NA</td><td>           NA</td></tr>
</tbody>
</table>



The model here is split into the individual parameters. It is not complete model in one row compared to SAS.

Error here is called residuals.

The row with the total is missing. It could be appended easily as the sum of the df column and as the sum of the sum of squares column.

The columns and their contents are similar.

### Block 3
![Block 3](img_screenshots/block_3.png)

### Root MSE

Root MSE is the standard deviation of the error term.

It is the square root of the mean square error (or residual).

### Dependent mean

The dependent mean is the mean of the dependent variable of those observations which were used and not omitted.

### Coeff Var

The coefficient of variation is the root MSE divided by the dependent mean. It is a measure of variation in the data.

### R-square

R-square is the proportion of the explained variance based on the total variance. Sum of square model divided by sum of square total.

### Adj R-Sq

Adjusted R-square adjusts for the relation between the number of variables (k) in the model and the number of observations (N) in the dataset.

$R_{adj} = 1 – ((1 – Rsq)((N – 1) / (N – k – 1))$



## R chunk for reproduction


```R
glance(my_lm)
```


<table class="dataframe">
<caption>A tibble: 1 × 12</caption>
<thead>
	<tr><th scope=col>r.squared</th><th scope=col>adj.r.squared</th><th scope=col>sigma</th><th scope=col>statistic</th><th scope=col>p.value</th><th scope=col>df</th><th scope=col>logLik</th><th scope=col>AIC</th><th scope=col>BIC</th><th scope=col>deviance</th><th scope=col>df.residual</th><th scope=col>nobs</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;int&gt;</th><th scope=col>&lt;int&gt;</th></tr>
</thead>
<tbody>
	<tr><td>0.8445503</td><td>0.8409164</td><td>56.7681</td><td>232.4095</td><td>1.183585e-149</td><td>9</td><td>-2150.811</td><td>4323.623</td><td>4367.39</td><td>1240708</td><td>385</td><td>395</td></tr>
</tbody>
</table>



The glance() function provides

Root MSE: sigma
R-square: r.squared
Adjusted R-Square: adj.r.squared
Dependent mean and Coeff Var can be calculated.


```R
df_complete <- elemapi2[complete.cases(elemapi2 %>% select(api00, ell, meals, yr_rnd, mobility, acs_k3, acs_46, full, emer, enroll)), ]

DependentMean <- mean(df_complete$api00)
DependentMean
CoeffVar <- glance(my_lm)$sigma / DependentMean * 100
CoeffVar
```


648.650632911392



8.75172256915632


### Block 4
![Block 4](img_screenshots/block_4.png)

#### Variable

This column refers to the name of the variable in the model.

#### DF

The degrees of freedom are one for continous and binary variable. For categorial variables they are equal to the number of levels minus.

#### Parameter estimate

This columns containts the values which are the $b_i$ in the model.

$\hat{y} = b_0 + b_1 * x_1 + b_2 * x_2 + ... + b_n * x_n$

$api00 = 758.94 - 0.86 * ell - 2.95 * meals ...$

#### Standard error

The standard errors are provided for each variable.

They can be used for calculating the t-value: Parameter estimate divided by standard error.   

#### t value

The null hypothesis tested is that the coefficient is zero.

The alternative hypothesis is that the coefficient is unequal to zero.

#### Pr > |t|

p is provided for a two-sided test. It can be divided by two for a one-sided test.

#### Standardized estimate

This estimates results after standardizing all continous variables before including them into the model.


## R chunk for reproduction



```R
my_lm <- lm(api00 ~ ell + meals + yr_rnd + mobility + acs_k3 + acs_46 + full + emer + enroll, data = elemapi2)
tidy(my_lm)
```


<table class="dataframe">
<caption>A tibble: 10 × 5</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>estimate</th><th scope=col>std.error</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>(Intercept)</td><td>758.94179320</td><td>62.28600730</td><td> 12.1847880</td><td>4.134890e-29</td></tr>
	<tr><td>ell        </td><td> -0.86007067</td><td> 0.21063175</td><td> -4.0832907</td><td>5.402931e-05</td></tr>
	<tr><td>meals      </td><td> -2.94821634</td><td> 0.17034524</td><td>-17.3073006</td><td>4.715844e-50</td></tr>
	<tr><td>yr_rnd     </td><td>-19.88874706</td><td> 9.25844226</td><td> -2.1481742</td><td>3.232329e-02</td></tr>
	<tr><td>mobility   </td><td> -1.30135168</td><td> 0.43620533</td><td> -2.9833466</td><td>3.032785e-03</td></tr>
	<tr><td>acs_k3     </td><td>  1.31870017</td><td> 2.25268291</td><td>  0.5853909</td><td>5.586278e-01</td></tr>
	<tr><td>acs_46     </td><td>  2.03245622</td><td> 0.79832127</td><td>  2.5459126</td><td>1.128813e-02</td></tr>
	<tr><td>full       </td><td>  0.60971500</td><td> 0.47582046</td><td>  1.2813972</td><td>2.008254e-01</td></tr>
	<tr><td>emer       </td><td> -0.70661916</td><td> 0.60540863</td><td> -1.1671772</td><td>2.438612e-01</td></tr>
	<tr><td>enroll     </td><td> -0.01216405</td><td> 0.01679211</td><td> -0.7243903</td><td>4.692661e-01</td></tr>
</tbody>
</table>



The column with the degrees of freedom is missing.

The other columns are similar.

The summary() function gives an overview of the results.


```R
summary(my_lm)
```


    
    Call:
    lm(formula = api00 ~ ell + meals + yr_rnd + mobility + acs_k3 + 
        acs_46 + full + emer + enroll, data = elemapi2)
    
    Residuals:
         Min       1Q   Median       3Q      Max 
    -171.934  -39.294   -2.973   36.096  158.440 
    
    Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
    (Intercept) 758.94179   62.28601  12.185  < 2e-16 ***
    ell          -0.86007    0.21063  -4.083  5.4e-05 ***
    meals        -2.94822    0.17035 -17.307  < 2e-16 ***
    yr_rnd      -19.88875    9.25844  -2.148  0.03232 *  
    mobility     -1.30135    0.43621  -2.983  0.00303 ** 
    acs_k3        1.31870    2.25268   0.585  0.55863    
    acs_46        2.03246    0.79832   2.546  0.01129 *  
    full          0.60972    0.47582   1.281  0.20083    
    emer         -0.70662    0.60541  -1.167  0.24386    
    enroll       -0.01216    0.01679  -0.724  0.46927    
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    
    Residual standard error: 56.77 on 385 degrees of freedom
      (5 Beobachtungen als fehlend gelöscht)
    Multiple R-squared:  0.8446,	Adjusted R-squared:  0.8409 
    F-statistic: 232.4 on 9 and 385 DF,  p-value: < 2.2e-16
    



```R

```
