# 04 - Multiple logistic regression

## Data

Source of data: Titanic

www.kaggle.com

https://www.kaggle.com/datasets/hesh97/titanicdataset-traincsv 

Dataset titanic.csv


```R
library(readr)
titanic <- read_csv("data/titanic.csv",
                 show_col_types = FALSE)
head(titanic)

```


<table class="dataframe">
<caption>A tibble: 6 × 12</caption>
<thead>
	<tr><th scope=col>PassengerId</th><th scope=col>Survived</th><th scope=col>Pclass</th><th scope=col>Name</th><th scope=col>Sex</th><th scope=col>Age</th><th scope=col>SibSp</th><th scope=col>Parch</th><th scope=col>Ticket</th><th scope=col>Fare</th><th scope=col>Cabin</th><th scope=col>Embarked</th></tr>
	<tr><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;dbl&gt;</th><th scope=col>&lt;chr&gt;</th><th scope=col>&lt;chr&gt;</th></tr>
</thead>
<tbody>
	<tr><td>1</td><td>0</td><td>3</td><td>Braund, Mr. Owen Harris                            </td><td>male  </td><td>22</td><td>1</td><td>0</td><td>A/5 21171       </td><td> 7.2500</td><td>NA  </td><td>S</td></tr>
	<tr><td>2</td><td>1</td><td>1</td><td>Cumings, Mrs. John Bradley (Florence Briggs Thayer)</td><td>female</td><td>38</td><td>1</td><td>0</td><td>PC 17599        </td><td>71.2833</td><td>C85 </td><td>C</td></tr>
	<tr><td>3</td><td>1</td><td>3</td><td>Heikkinen, Miss. Laina                             </td><td>female</td><td>26</td><td>0</td><td>0</td><td>STON/O2. 3101282</td><td> 7.9250</td><td>NA  </td><td>S</td></tr>
	<tr><td>4</td><td>1</td><td>1</td><td>Futrelle, Mrs. Jacques Heath (Lily May Peel)       </td><td>female</td><td>35</td><td>1</td><td>0</td><td>113803          </td><td>53.1000</td><td>C123</td><td>S</td></tr>
	<tr><td>5</td><td>0</td><td>3</td><td>Allen, Mr. William Henry                           </td><td>male  </td><td>35</td><td>0</td><td>0</td><td>373450          </td><td> 8.0500</td><td>NA  </td><td>S</td></tr>
	<tr><td>6</td><td>0</td><td>3</td><td>Moran, Mr. James                                   </td><td>male  </td><td>NA</td><td>0</td><td>0</td><td>330877          </td><td> 8.4583</td><td>NA  </td><td>Q</td></tr>
</tbody>
</table>



## SAS program snippet

The following SAS code will be executed.
proc logistic data = titanic descending;
  class sex Pclass;
  model survived = sex Pclass age;
run;

The option descending reverses the order of the levels in the dependent variable.

## Results

The output is divided into blocks to explain it and to reproduce it afterwards in the different languages.

The sequence and contents of the blocks differes between simple and multiple logistic regression.

The occurrence of categorical variables also adds other blocks.

### Block 1
![Block 1](img_screenshots/block_1.png)

Row 1 refers to the dataset which was used in this procedure.

Row 2 gives the response variable or dependent variable for the logistic regression.

Row 3 gives the number of response levels equal to the available levels of the dependent variable in the dataset.

Row 4 names the type of model. In this case it is a logistic regression or binary logit as stated here.

Row 5 gives the name of the optimization technique which was used. Here is a source for differences between the statistical programs.
In SAS, the default method is Fisher’s scoring method.
In R, the glm documentation mentions iteratively reweighted least squares (IWLS) as the method.
In Stata, it is the Newton-Raphson algorithm. 
These are the three main methods.

You have to look into the small print in the description of the method.

### Block 2
![Block 2](img_screenshots/block_2.png)

The number of observations used might be less than the number of observations read.
SAS performs a listwise deletion (complete case analysis) if missing values are present.

### Block 3
![Block 3](img_screenshots/block_3.png)

The levels and the frequencies for the dependent variable are provided here.

It is also stated which probability is modeled here. The order was reversed here with the descending option in the proc logistic statement.

By default SAS models the 0 while other statistical programs model the 1. 
Categorical levels would be sorted in alphabetical order and the first level would be modeled.

### Block 4
![Block 4](img_screenshots/block_4.png)

The important information that the model converged can be found here.

Coding of categorical is listed here.

This coding might differ from the coding in other statistic programming languages.


### Block 5
![Block 5](img_screenshots/block_5.png)

The model fit status is described by 
-  AIC (Akaike Information Criterion): Smaller is better.
-  SC (Schwarz Criterion): Smaller is better.
-  -2 Log L (negative two times the log-likelihood)



### Block 6
![Block 6](img_screenshots/block_6.png)

These global tests test the null hypothesis that all regression coefficents are zero.

The tests are different chi-square tests.


### Block 7
![Block 7](img_screenshots/block_7.png)

Column 1 "Effect" lists the variables which are interpreted by the point estimate.

Column 2 "Point Estimate" is interpreted as an odds ratio. 
One unit change in the independent variable changes the probability for the modelled event by the estimated value.

Column 3 and 4 give the confidence interval for the odds ratio.

### Block 8
![Block 8](img_screenshots/block_8.png)

Column 1 "Parameter" lists the intercept and the parameter in the model.

Column 2 "DF" gives the degrees of freedom for every parameter.

Column 3 "Estimate" lists the logit regression estimates for every parameter given that the other parameter are held constant. 

$log(p / (1 - p)) = -6.10 + 0.04 * glucose$ with p as the probability for diabetes.

Column 4 "Standard Error" gives the standard errors of the individual regression coefficients.

Column 5 "Wald Chi-Square" tests the null hypothesis that the regression coefficient is zero given that the other predictors are in the model.

Column 6 "Pr > ChiSq" gives the p-value for the Wald Chi-Square statistic.

### Block 9
![Block 9](img_screenshots/block_9.png)

Column 1 "Effect" lists the variables which are interpreted by the point estimate.

Column 2 "Point Estimate" is interpreted as an odds ratio. 
One unit change in the independent variable changes the probability for the modelled event by the estimated value.

Column 3 and 4 give the confidence interval for the odds ratio.


### Block 10
![Block 10](img_screenshots/block_10.png)

These parameter describe the association between the predicted probabilities and observed responses.



```R

```
