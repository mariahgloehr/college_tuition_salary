# Introduction

Recent years have seen growing concerns over the exorbitant college tuition costs in the United States. With student loan debt totaling \$1.74 trillion ("Student Loan Debt Statistics"), many are beginning to consider the benefits of such a pricey secondary education. Does shelling out more money on college really guarantee better economic options post-grad? In this study, we aim to assess whether there is a relationship between college tuition and alumni salaries in the United States, after controlling for school size, whether a school is in the Ivy League, and whether the school is public or private.

# Methods

In order to create our dataset, we pulled multiple datasets from TidyTuesday (which gathered data from payscale.com, Chronicle of Higher Education, Chronicle of Higher Education, and Tuitiontracker.org) and combined them into one dataframe containing our variables of interest: school name, type (public or private), in-state tuition, out-of-state tuition, and estimated mid-career pay of alumni. Only schools that had data on all of these variables were included in the final set. We then created two new variables: ivy_league, an indicator for whether the university is in the Ivy League, and tuition_avg, an average of in-state and out-of-state tuition.

To determine whether the cost of a school impacts the estimated mid-career earnings of its alumni, we created a multiple linear regression model, which follows the equation: $\mu(Y|X)=\beta_0+beta_1x_{i1}+\beta_2x_{i2}+...+\beta_px_{ip}+\epsilon_i$, where $x_{i1}, x_{i2}, ..., x_{ip}$ are predictor variables. Our initial model included predictor variables for tuition, type of school, total enrollment of the school, and whether a school is an Ivy League member. The p-values for all variables were extremely low, but the residuals for enrollment were right-skewed, so we pursued a natural log transformation.

After we log-transformed enrollment, the residuals appeared normally distributed around 0. However, there was still a lot of variation in the residuals, which ranged from -20000 to 40000. We log-transformed our response variable of mid-career earnings to reduce variability in the residuals. The VIF values of this model were all less than 5, so there was no multicollinearity and we did not find any influential points when calculating leverage and Cook's Distance values. Furthermore, analysis of the residuals revealed that the model assumptions of a linearity, identically and normally distributed error terms with mean zero, and constant variance is consistent were sufficiently met. We were unable to assess the model assumption of independent error terms because there was no variable that we could plot the residuals against that may induce dependence, such as time.

After these transformations, and with our conditions satisfied, we went forward with fitting our model, which was as follows: $\log(Y) =10.8+8.332*10^{-6}avgtuition +0.101Type+0.0297\log(totalenrollment)+0.138ivyleague$ In this model, "Type" is an indicator variable whose value is one when a school is public and zero if it is private. The variable, "ivy_league," is also an indicator. Its value is one when a school is an Ivy League member and zero otherwise. We ran a t-test on the coefficient "tuition_avg" to determine whether there is association between the average tuition of a school and the estimated mid-career salary of that school's alumni. We also ran t-tests on the additional predictor variables to confirm that they can be used in the model as controlling variables.

# Results

The results of our t-tests can be seen below in Figure 1:

| Variable               | Coefficient | Standard Error | t-statistic | p-value  |
|------------------------|-------------|----------------|-------------|----------|
| Intercept              | 10.8        | 3.601E-02      | 299.940     | \< 0.001 |
| tuition_avg            | 8.332E-06   | 3.818E-07      | 21.822      | \< 0.001 |
| Type (public)          | 0.101       | 1.505E-02      | 6.699       | \< 0.001 |
| log(total_enrollement) | 0.0297      | 4.793E-03      | 6.228       | \< 0.001 |
| ivy_league             | 0.138       | 4.54E-02       | 3.032       | 0.00253  |

: Figure 1: Table of all the regression coefficients from the fitted model, along with their standard errors and t-test results. The degrees of freedom for each t-test was 635.

In our testing to answer our primary question, we found very strong evidence of an association between the average tuition of a school and the estimated mid-career pay of alumni (t = 21.822, df = 635, p-value \< 0.001). According to our model, a one-thousand dollar increase in the average tuition of a school is associated with the estimated mid-career salary of its alumni increasing by a factor of 1.008 (a 0.8 percent increase), after accounting for the type of school (public vs. private), the total enrollment of the school, and whether the school is in the Ivy League. To understand this association more, we generated a range of possible values for the coefficient of average tuition. From this, we can be very confident that a one-thousand dollar increase in the average tuition of a school is associated with the estimated mid-career salary of its alumni increasing by a factor greater 1.00761 (a 0.761 percent increase) and less than 1.00912 (a 0.912 percent increase).

Additionally, we also found the other variables that we controlled for to have a discernible relationship with the estimated mid career pay, as we expected (Type: t = 6.699, df = 635, p-value \< 0.001, log(total enrollment): t = 6.228, df = 635, p-value \< 0.001, Ivy League: t = 3.032, df = 635, p-value = 0.00253). According to our model, the estimated mid-career salary for alumni from public schools is 10.6 percent greater than that of alumni from private schools, after accounting for the other predictor variables. However, our model also postulates that alumni from Ivy League member schools have an median estimated mid-career salary that is 14.8 percent higher than alumni from non-Ivy schools, after accounting for the other predictor variables. On another note, our model proposes a positive linear relationship between the total enrollment of a school and the median estimated mid-career pay, with a doubling in enrollment being associated with a 3.01 percent increase in the median estimated mid-career pay, holding the other predictors constant.

# Discussion

It is worth noting that our results can only be generalized to the 640 schools sampled, since random sampling was not performed, if the error terms are indeed independent. A future study would have to find information on something such as the order in which the data were collected to determine if the error terms are independent. Also, a future study, with random sampling of all the four year colleges and universities in the United States, if there is shown to be a discernible relationship between average tuition (and each of the other predictors) and estimated mid-career pay, can be used to generalize these results to all of the four-year colleges and universities in the United States.

In conclusion, the answer to our primary question is yes, there is a relationship between the average tuition of a school and the estimated mid-career salary of its alumni, at least for the 640 schools in our dataset. Our study has no way of determining if there is a causal relationship between higher tuition and higher estimated mid-career salary.

Moreover, our study does not tell us why the association exists. One hypothesis we have is that tuition may be highly connected to prestige, which may impact future career and salary opportunities. While prestige of a school is difficult to measure directly, it is feasible that tuition acts as a proxy variable. Another possibility is that higher tuition is discernibly associated with a higher quality of education, which could lead to better career pay. A future study would be needed to look into these theories and to understand our results better.

# Works Cited

"Student Loan Debt Statistics: 2024." NerdWallet, 5 Feb. 2024, <https://www.nerdwallet.com/article/loans/student-loans/student-loan-debt>.

"Tidytuesday/Data/2020/2020-03-10/Readme.Md at Master · Rfordatascience/Tidytuesday." GitHub, <https://github.com/rfordatascience/tidytuesday/blob/master/data/2020/2020-03-10/readme.md>. Accessed 8 Mar. 2024.# college_tuition_salary
