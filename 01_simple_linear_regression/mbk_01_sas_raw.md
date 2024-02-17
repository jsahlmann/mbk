# 01 - Simple linear regression

## Data

Source of data: SAS Help

Data set class.csv


```R
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

## Results

The output is divided into blocks to explain it and to reproduce it afterwards in the different languages.

### Block 1
![Block 1](img_screenshots/block_1.png)

Number of observations read is the number of observations in the dataset.

Number of observation used is the number of complete cases regarding the variables used for the SAS program snippet.

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



```R

```
