# 01 - Simple linear regression

## Data

Source of data: SAS Help

Data set class.csv


```julia
# import packages for handling csv files and dataframes
using CSV
using DataFrames

df_class = CSV.read("data/class.csv", DataFrame)

print(first(df_class, 5))

```

    [1m5×5 DataFrame[0m
    [1m Row [0m│[1m Name    [0m[1m Sex     [0m[1m Age   [0m[1m Height  [0m[1m Weight  [0m
         │[90m String7 [0m[90m String1 [0m[90m Int64 [0m[90m Float64 [0m[90m Float64 [0m
    ─────┼───────────────────────────────────────────
       1 │ Alfred   M           14     69.0    112.5
       2 │ Alice    F           13     56.5     84.0
       3 │ Barbara  F           13     65.3     98.0
       4 │ Carol    F           14     62.8    102.5
       5 │ Henry    M           14     63.5    102.5

## SAS program snippet

The following SAS code will be executed.
proc reg data=sashelp.class;
   model Weight = Height;
run;

## Julia chunk

Packages will be loaded in the chunk where they are first needed.

A similar Julia program might look like this. It uses the lm() function.


```julia
using GLM, StatsBase

# Linear regression using lm()
ols = lm(@formula(Weight ~ Height), df_class)
print(ols)

```

    StatsModels.TableRegressionModel{LinearModel{GLM.LmResp{Vector{Float64}}, GLM.DensePredChol{Float64, LinearAlgebra.CholeskyPivoted{Float64, Matrix{Float64}, Vector{Int64}}}}, Matrix{Float64}}
    
    Weight ~ 1 + Height
    
    Coefficients:
    ───────────────────────────────────────────────────────────────────────────
                      Coef.  Std. Error      t  Pr(>|t|)   Lower 95%  Upper 95%
    ───────────────────────────────────────────────────────────────────────────
    (Intercept)  -143.027     32.2746    -4.43    0.0004  -211.12     -74.9335
    Height          3.89903    0.516094   7.55    <1e-06     2.81017    4.98789
    ───────────────────────────────────────────────────────────────────────────

## Results

The output is divided into blocks to explain it and to reproduce it afterwards in the different languages.

### Block 1
![Block 1](img_screenshots/block_1.png)

Number of observations read is the number of observations in the dataset.

Number of observation used is the number of complete cases regarding the variables used for the SAS program snippet.

### Julia chunk for reproduction


```julia
# Number of rows in dataframe
print(nrow(df_class))
print("\r\n")
# or
print(size(df_class, 1))
print("\r\n")
# remove missings
df_complete = dropmissing(df_class, [:"Height", :"Weight"])
print(nrow(df_complete))
print("\r\n")

```

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


## Julia chunk for reproduction


```julia
using ANOVA
model = fit(LinearModel,
            @formula(Weight ~ Height), 
            df_class)
Anova(model)

```


    UndefVarError: `Anova` not defined

    

    Stacktrace:

     [1] top-level scope

       @ In[29]:5


Package ANOVA.jl does not work in the way in which it is described in the manual

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



## Julia chunk for reproduction

# todo
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


## Julia chunk for reproduction




```julia
model = fit(LinearModel,
            @formula(Weight ~ Height), 
            df_class)
model
```




    StatsModels.TableRegressionModel{LinearModel{GLM.LmResp{Vector{Float64}}, GLM.DensePredChol{Float64, LinearAlgebra.CholeskyPivoted{Float64, Matrix{Float64}, Vector{Int64}}}}, Matrix{Float64}}
    
    Weight ~ 1 + Height
    
    Coefficients:
    ───────────────────────────────────────────────────────────────────────────
                      Coef.  Std. Error      t  Pr(>|t|)   Lower 95%  Upper 95%
    ───────────────────────────────────────────────────────────────────────────
    (Intercept)  -143.027     32.2746    -4.43    0.0004  -211.12     -74.9335
    Height          3.89903    0.516094   7.55    <1e-06     2.81017    4.98789
    ───────────────────────────────────────────────────────────────────────────



Degrees of freedom are missing, but the other columns give the same results.
