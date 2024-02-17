# 01 - Simple linear regression

## Data

Source of data: SAS Help

Data set class.csv


```R
# Library with function read_csv
library(readr)
class <- read_csv("data/class.csv",
                 show_col_types = FALSE)
head(class)

```


<table class="dataframe">
<caption>A tibble: 6 × 5</caption>
<thead>
	<tr><th scope=col>Name</th><th scope=col>Sex</th><th scope=col>Age</th><th scope=col>Height</th><th scope=col>Weight</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Alfred </td><td>M</td><td>14</td><td>69.0</td><td>112.5</td></tr>
	<tr><td>Alice  </td><td>F</td><td>13</td><td>56.5</td><td> 84.0</td></tr>
	<tr><td>Barbara</td><td>F</td><td>13</td><td>65.3</td><td> 98.0</td></tr>
	<tr><td>Carol  </td><td>F</td><td>14</td><td>62.8</td><td>102.5</td></tr>
	<tr><td>Henry  </td><td>M</td><td>14</td><td>63.5</td><td>102.5</td></tr>
	<tr><td>James  </td><td>M</td><td>12</td><td>57.3</td><td> 83.0</td></tr>
</tbody>
</table>



## SAS program snippet

The following SAS code will be executed.
proc reg data=sashelp.class;
   model Weight = Height;
run;

## R chunk

Packages will be loaded in the chunk where they are first needed.

A similar R program might look like this. It uses the lm() function.

The tidy() function from the broom-packages formats the output into a tibble for easier processing.


```R
library(broom)
# Linear regression using lm()
my_lm <- lm(Weight ~ Height, data = class)
tidy(my_lm)

```


<table class="dataframe">
<caption>A tibble: 2 × 5</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>estimate</th><th scope=col>std.error</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>(Intercept)</td><td>-143.02692</td><td>32.2745913</td><td>-4.431564</td><td>3.655789e-04</td></tr>
	<tr><td>Height     </td><td>   3.89903</td><td> 0.5160939</td><td> 7.554885</td><td>7.886816e-07</td></tr>
</tbody>
</table>



## Results

The output is divided into blocks to explain it and to reproduce it afterwards in the different languages.

### Block 1
![Block 1](img_screenshots/block_1.png)

Number of observations read is the number of observations in the dataset.

Number of observation used is the number of complete cases regarding the variables used for the SAS program snippet.

### R chunk for reproduction


```R
library(tidyverse)
# Number of observations read
nrow(class)
# Number of observations used
sum(class %>% 
    select(Height, Weight) %>%
    complete.cases())

# Alternative for number of observations used.
nobs(my_lm)
```

    ── [1mAttaching core tidyverse packages[22m ──────────────────────────────────────────────────────────────── tidyverse 2.0.0 ──
    [32m✔[39m [34mdplyr    [39m 1.1.2     [32m✔[39m [34mpurrr    [39m 1.0.1
    [32m✔[39m [34mforcats  [39m 1.0.0     [32m✔[39m [34mstringr  [39m 1.5.0
    [32m✔[39m [34mggplot2  [39m 3.4.2     [32m✔[39m [34mtibble   [39m 3.2.1
    [32m✔[39m [34mlubridate[39m 1.9.2     [32m✔[39m [34mtidyr    [39m 1.3.0
    ── [1mConflicts[22m ────────────────────────────────────────────────────────────────────────────────── tidyverse_conflicts() ──
    [31m✖[39m [34mdplyr[39m::[32mfilter()[39m masks [34mstats[39m::filter()
    [31m✖[39m [34mdplyr[39m::[32mlag()[39m    masks [34mstats[39m::lag()
    [36mℹ[39m Use the conflicted package ([3m[34m<http://conflicted.r-lib.org/>[39m[23m) to force all conflicts to become errors
    


19



19



19


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
my_aov <- aov(Weight ~ Height, data = class)
tidy(my_aov)
```


<table class="dataframe">
<caption>A tibble: 2 × 6</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>df</th><th scope=col>sumsq</th><th scope=col>meansq</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>Height   </td><td> 1</td><td>7193.249</td><td>7193.2491</td><td>57.07628</td><td>7.886816e-07</td></tr>
	<tr><td>Residuals</td><td>17</td><td>2142.488</td><td> 126.0287</td><td>      NA</td><td>          NA</td></tr>
</tbody>
</table>



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



## R-chunk for reproduction




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
	<tr><td>0.7705068</td><td>0.7570072</td><td>11.22625</td><td>57.07628</td><td>7.886816e-07</td><td>1</td><td>-71.85003</td><td>149.7001</td><td>152.5334</td><td>2142.488</td><td>17</td><td>19</td></tr>
</tbody>
</table>



The glance() function provides 

-  Root MSE: sigma
-  R-square: r.squared
-  Adjusted R-Square: adj.r.squared

Dependent mean and Coeff Var can be calculated.


```R
df_complete <- class[complete.cases(class %>% select(Height, Weight)), ]

DependentMean <- mean(df_complete$Weight)
DependentMean
CoeffVar <- glance(my_lm)$sigma / DependentMean * 100
CoeffVar
```


100.026315789474



11.2232965255547


### Block 4
![Block 4](img_screenshots/block_4.png)

#### Variable

This column refers to the name of the variable in the model.

#### DF

The degrees of freedom are one for continous and binary variable. For categorial variables they are equal to the number of levels minus.

#### Parameter estimate

This columns containts the values which are the $b_i$ in the model.

$\hat{y} = b_0 + b_1 * x_1 + b_2 * x_2 + ... + b_n * x_n$

$Weight = -143.03 + 3.89 * Height$

#### Standard error

The standard errors are provided for each variable.

They can be used for calculating the t-value: Parameter estimate divided by standard error.   

#### t value

The null hypothesis tested is that the coefficient is zero.

The alternative hypothesis is that the coefficient is unequal to zero.

#### Pr > |t|

p is provided for a two-sided test. It can be divided by two for a one-sided test.


## R-chunk for reproduction




```R
tidy(my_lm)
```


<table class="dataframe">
<caption>A tibble: 2 × 5</caption>
<thead>
	<tr><th scope=col>term</th><th scope=col>estimate</th><th scope=col>std.error</th><th scope=col>statistic</th><th scope=col>p.value</th></tr>
	<tr><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th></tr>
</thead>
<tbody>
	<tr><td>(Intercept)</td><td>-143.02692</td><td>32.2745913</td><td>-4.431564</td><td>3.655789e-04</td></tr>
	<tr><td>Height     </td><td>   3.89903</td><td> 0.5160939</td><td> 7.554885</td><td>7.886816e-07</td></tr>
</tbody>
</table>



The column with the degrees of freedom is missing.

The other columns are similar.

The summary() function gives an overview of the results.


```R
summary(my_lm)
```


    
    Call:
    lm(formula = Weight ~ Height, data = class)
    
    Residuals:
         Min       1Q   Median       3Q      Max 
    -17.6807  -6.0642   0.5115   9.2846  18.3698 
    
    Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
    (Intercept) -143.0269    32.2746  -4.432 0.000366 ***
    Height         3.8990     0.5161   7.555 7.89e-07 ***
    ---
    Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    
    Residual standard error: 11.23 on 17 degrees of freedom
    Multiple R-squared:  0.7705,	Adjusted R-squared:  0.757 
    F-statistic: 57.08 on 1 and 17 DF,  p-value: 7.887e-07
    



```R

```
