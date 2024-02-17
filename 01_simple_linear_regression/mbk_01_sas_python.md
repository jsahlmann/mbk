# 01 - Simple linear regression

## Data

Source of data: SAS Help

Data set class.csv


```python
# Load pandas
import pandas as pd

# Read CSV file into DataFrame df
df_class = pd.read_csv('data/class.csv', index_col=0)

# Show dataframe
print(df_class.head(n = 10))

# class could not be used as a dataframe name because it is a reserved word.
```

            Sex  Age  Height  Weight
    Name                            
    Alfred    M   14    69.0   112.5
    Alice     F   13    56.5    84.0
    Barbara   F   13    65.3    98.0
    Carol     F   14    62.8   102.5
    Henry     M   14    63.5   102.5
    James     M   12    57.3    83.0
    Jane      F   12    59.8    84.5
    Janet     F   15    62.5   112.5
    Jeffrey   M   13    62.5    84.0
    John      M   12    59.0    99.5
    

## SAS program snippet

The following SAS code will be executed.
proc reg data=sashelp.class;
   model Weight = Height;
run;

## Python chunk

Modules will be loaded in the chunk where they are first needed.

A similar python program might look like this. It uses the lm() function.

The tidy() function from the broom-packages formats the output into a tibble for easier processing.


```python
import numpy as np
import statsmodels.api as sm

x = np.array(df_class["Height"]).reshape((-1, 1))
y = np.array(df_class["Weight"])

x = sm.add_constant(x)

model = sm.OLS(y, x)
results = model.fit()
print(results.summary())

```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.771
    Model:                            OLS   Adj. R-squared:                  0.757
    Method:                 Least Squares   F-statistic:                     57.08
    Date:                Sat, 17 Feb 2024   Prob (F-statistic):           7.89e-07
    Time:                        16:25:22   Log-Likelihood:                -71.850
    No. Observations:                  19   AIC:                             147.7
    Df Residuals:                      17   BIC:                             149.6
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const       -143.0269     32.275     -4.432      0.000    -211.120     -74.933
    x1             3.8990      0.516      7.555      0.000       2.810       4.988
    ==============================================================================
    Omnibus:                        1.303   Durbin-Watson:                   2.071
    Prob(Omnibus):                  0.521   Jarque-Bera (JB):                0.838
    Skew:                          -0.020   Prob(JB):                        0.658
    Kurtosis:                       1.972   Cond. No.                         784.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

    C:\Users\user\AppData\Roaming\Python\Python312\site-packages\scipy\stats\_stats_py.py:1806: UserWarning: kurtosistest only valid for n>=20 ... continuing anyway, n=19
      warnings.warn("kurtosistest only valid for n>=20 ... continuing "
    

## Results

The output is divided into blocks to explain it and to reproduce it afterwards in the different languages.

### Block 1
![Block 1](img_screenshots/block_1.png)

Number of observations read is the number of observations in the dataset.

Number of observation used is the number of complete cases regarding the variables used for the SAS program snippet.

### Python chunk for reproduction


```python
# Number of observations in dataframe
# (rows, columns)
print(df_class.shape)
# Number of observations used
print(results.nobs)
```

    (19, 4)
    19.0
    

The number of observations is the number of rows in the dataset.

The number of observations used is the number returned from the nobs property.


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


## Python chunk for reproduction


```python
import statsmodels.api as sm
from statsmodels.formula.api import ols

mod = ols('Weight ~ Height',
                data=df_class).fit()
                
aov_table = sm.stats.anova_lm(mod, typ=2)
print(aov_table)
```

                   sum_sq    df          F        PR(>F)
    Height    7193.249119   1.0  57.076283  7.886816e-07
    Residual  2142.487723  17.0        NaN           NaN
    

Error here is called residuals.

The column with the mean square is missing. It can be calculated as the difference between the sum of squares and the degrees of freedom.

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



## Python chunk for reproduction



```python
model = sm.OLS(y, x)
results = model.fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.771
    Model:                            OLS   Adj. R-squared:                  0.757
    Method:                 Least Squares   F-statistic:                     57.08
    Date:                Sat, 17 Feb 2024   Prob (F-statistic):           7.89e-07
    Time:                        16:25:22   Log-Likelihood:                -71.850
    No. Observations:                  19   AIC:                             147.7
    Df Residuals:                      17   BIC:                             149.6
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const       -143.0269     32.275     -4.432      0.000    -211.120     -74.933
    x1             3.8990      0.516      7.555      0.000       2.810       4.988
    ==============================================================================
    Omnibus:                        1.303   Durbin-Watson:                   2.071
    Prob(Omnibus):                  0.521   Jarque-Bera (JB):                0.838
    Skew:                          -0.020   Prob(JB):                        0.658
    Kurtosis:                       1.972   Cond. No.                         784.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

    C:\Users\user\AppData\Roaming\Python\Python312\site-packages\scipy\stats\_stats_py.py:1806: UserWarning: kurtosistest only valid for n>=20 ... continuing anyway, n=19
      warnings.warn("kurtosistest only valid for n>=20 ... continuing "
    

The summary() function provides 

-  R-square: r.squared
-  Adjusted R-Square: adj.r.squared

Root mean square error (RMSE), dependent mean and Coeff Var can be calculated.


```python
import math
df_complete = df_class.dropna() 
DependentMean = df_complete.mean(numeric_only = True, axis = 0)["Weight"]
print("Dependent Mean: ", DependentMean)
print(aov_table.shape)
print(type(aov_table))
# root of sum of squares divided by df
RMSE = math.sqrt(aov_table.iloc[1, 0] / aov_table.iloc[1, 1])
print("RMSE: ", RMSE)
CoeffVar = RMSE / DependentMean * 100
print("CoeffVar: " , CoeffVar)

```

    Dependent Mean:  100.02631578947368
    (2, 4)
    <class 'pandas.core.frame.DataFrame'>
    RMSE:  11.226250024640356
    CoeffVar:  11.223296525554684
    

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


## Python chunk for reproduction




```python
model = sm.OLS(y, x)
results = model.fit()
print(results.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                      y   R-squared:                       0.771
    Model:                            OLS   Adj. R-squared:                  0.757
    Method:                 Least Squares   F-statistic:                     57.08
    Date:                Sat, 17 Feb 2024   Prob (F-statistic):           7.89e-07
    Time:                        16:25:22   Log-Likelihood:                -71.850
    No. Observations:                  19   AIC:                             147.7
    Df Residuals:                      17   BIC:                             149.6
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const       -143.0269     32.275     -4.432      0.000    -211.120     -74.933
    x1             3.8990      0.516      7.555      0.000       2.810       4.988
    ==============================================================================
    Omnibus:                        1.303   Durbin-Watson:                   2.071
    Prob(Omnibus):                  0.521   Jarque-Bera (JB):                0.838
    Skew:                          -0.020   Prob(JB):                        0.658
    Kurtosis:                       1.972   Cond. No.                         784.
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

    C:\Users\user\AppData\Roaming\Python\Python312\site-packages\scipy\stats\_stats_py.py:1806: UserWarning: kurtosistest only valid for n>=20 ... continuing anyway, n=19
      warnings.warn("kurtosistest only valid for n>=20 ... continuing "
    

The column with the degrees of freedom is missing.

The other columns are similar.




```python

```
