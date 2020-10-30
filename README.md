# PD Model for Fixed-Rate Mortgages: Using Transition Matrix Method
 
## Introduction
Beginning in 2013, Fannie Mae released a comprehensive dataset that offers insight into the credit performance of a portion of its single-family book of business. And the dataset now consists of over 22 million records, including a subset of Fannie Mae’s 30-year, fixed-rate, fully documented, single-family amortizing loans that the company owned or guaranteed on or after January 1, 2000.  It provides monthly loan-level detail and helps investors gain a better understanding of the credit performance of single-family loans.

For borrowers with fixed-rate mortgages, which represent the vast majority of U.S. mortgage borrowers, the decrease in monthly payments may take place when the borrower refinances the existing mortgage at a lower prevailing mortgage interest rate or encounters a macroeconomics recession. And this will eventually affect whether the borrower's performance is default. 

The purpose of this report is to predict the probability of default (PD) for a fixed-rate mortgage portfolio. A mortgage portfolio consists of all loans purchased by Fannie Mae from Wells Fargo, and records can only be observed quarterly from 2006 to 2017.

## Data Preparation


To better fit the model and test the fitting effect of the model, we only kept quarterly recorders of the loan performance by extracting the observations at the end of the quarter and also assuming that all loan termination periods occur at that time. Next, we used the simple random sample method to select 70 percent of loan ID along with their records to train the regression model and used the rest 30 percent as the validation set.


According to the status of the unpaid principal balance after each period of payment, we define the current state for each loan as current, delinquent, serious delinquent, and prepayment. And we define default as the serious delinquent, which is three more months past due, and any other status that indicates a loss. Also, we assume the default and prepayment are absorbing processes, which means that once a loan defaulted or prepaid, its state cannot change. 


Next, after trying different segmentations, such as jumbo versus conforming loans, we decided to perform the segmentation based on the FICO score. Based on the idea of maximizing the difference in default performance, we define the ones with FICO lower than 670 are sub-prime, and the rest belong to the prime group.
Also, to eliminate the multicollinearity between macroeconomic drivers, we carried out six transformations of the macroeconomic variables. Then selected those fit the model best for different segmentations.


## Algorithm

We carried out various different analyses on the Data available to use, both the Macro-economics and the Loan-Level Data.

### Exploratory Data Analysis (EDA)
We start with Univariate Analysis. It is the simplest form of analyzing data. The main purpose of this analysis was to describe the data, summarize it and find patterns in the data before we fit the variables to our regression model. We looked at the various statistical measures of the variable i.e Mean, Mode, Median, Range, Variance, Standard Deviation etc. We also checked Histogram plots, Frequency distributions for these variables. We also standardized the variable we felt needed to be standardized after looking at all of these statistics and removed outliers as well.
 
### Binomial Logistic Regression 
We then carried out a Binomial Logistic Regression for all of the predictor variables with the response variable i.e Default or Not- default. Using the regression we estimated the coefficients and standard error of the predictor variables. We looked at the MLE fit and we also took a note of the p-value < 0.025 for these variables which indicated whether they were significant or not. Doing this with each predictor we decided which variables were significant to our model and which were not.
 
 
Now before moving on to Multinomial Logistic Regression, We performed some analysis on the variables that were in line with our main assumptions for multinomial logistic regression. We performed Correlation Analysis, Covariance Analysis and Graphical Analysis on these selected variables to make sure we do not have any multicollinearity in our predictors. We looked at VIF factors for these variables. Multicollinearity refers to a situation in which two or more explanatory variables in a multiple regression model are highly linearly related. We used Pearson Correlation Coefficients as well to look at the correlations between these variables. Based on all of the analysis carried out we selected our variables for our Final Model.
 
 
### Multinomial Logistic Regression
This is a model that is used to predict the probabilities of the different possible outcomes of a Categorically distributed dependent variable, given a set of independent variables (which may be real-valued, binary-valued, categorical-valued, etc.). We fit all of our selected variables to the Regression model. We looked at the Wald Statistic of each variable and its p-value. Compared the coefficients with the Binomial model coefficients and we Cross-Validated the MLE signs with the expected behavior of the predictor variables. 


By analyzing all of these values and the p-value we decided which variables were significant and which were not. We removed the variable which was insignificant and then proceeded to refit the model without it. We again Analyzed the fit of the model and looked at all the interactions of the variables that were significant previously. This way we repeatedly carried out analyzing and refitting the model with significant variables and picked our final explanatory variables. 


We also used a stepwise selection algorithm for our Macroeconomic variables on our model. Stepwise regression is a modification of the forward selection so that after each step in which a variable was added, all candidate variables in the model are checked to see if their significance has been reduced below the specified tolerance level. If a nonsignificant variable is found, it is removed from the model. Stepwise regression requires two significance levels: one for adding variables and one for removing variables.


The cutoff probability for adding variables should be less than the cutoff probability for removing variables so that the procedure does not get into an infinite loop. We looked at the AIC and BIC criterion of the models thus generated as well. AIC is an estimate of a constant plus the relative distance between the unknown true likelihood function of the data and the fitted likelihood function of the model, so that a lower AIC means a model is considered to be closer to the truth. BIC is an estimate of a function of the posterior probability of a model being true, under a certain Bayesian setup, so that a lower BIC means that a model is considered to be more likely to be the true model.
