# Bayesian-Analysis-of-NYC-East-River-Bridge-Bicycle-Traffic
This project performs a Bayesian analysis to predict the number of bicyclists crossing the East River bridges in New York City based on weather conditions and the day of the week. The analysis uses data from April 2016 and builds a multivariate Poisson regression model using JAGS (Just Another Gibbs Sampler).

### Project Overview
The primary goal is to understand the factors influencing bicycle traffic across four major NYC bridges: Brooklyn, Manhattan, Williamsburg, and Queensboro. We aim to predict the total number of cyclists as well as the count for each individual bridge using a Bayesian framework.

The analysis pipeline includes:
- **Data Cleaning and Preprocessing**: Handling duplicates, incorrect data types, and outliers.
- **Exploratory Data Analysis (EDA)**: Univariate and bivariate analysis to understand data distributions and variable relationships.
- **Bayesian Modeling**: Constructing a multivariate Poisson regression model in JAGS.
- **Model Evaluation**: Assessing model convergence and performance using diagnostic tests.
- **Model Comparison**: Comparing a simple model against one with interaction terms using Bayes Factors and DIC.
- **Posterior Predictive Checks**: Validating the model's fit to the data.

### Dataset
The data for this analysis is sourced from the New York City Department of Transportation's daily monitoring program 🚲. This program tracks bicycle traffic across the East River bridges to measure bike utilization, which is a key input for transportation planning. 
The dataset, `data.csv`, provides a daily record of the number of cyclists crossing into or out of Manhattan for the month of April 2016. It includes the bicycle counts for each bridge along with key weather information for that day.

**Resource**: [Kaggle – New York City Bicycle Crossing](https://www.kaggle.com/datasets/new-york-city/nyc-east-river-bicycle-crossings?), which contains daily records for April 2016.

##### Predictor Variables:
- `Day`: Day of the week (numeric, where Monday=1, Sunday=7).
- `High_Temp_F`: Daily high temperature in Fahrenheit.
- `Low_Temp_F`: Daily low temperature in Fahrenheit.
- `Precipitation`: Daily precipitation in inches.

##### Response Variables:
- `Brooklyn.Bridge`: Daily bicycle count on the Brooklyn Bridge.
- `Manhattan.Bridge`: Daily bicycle count on the Manhattan Bridge.
- `Williamsburg.Bridge`: Daily bicycle count on the Williamsburg Bridge.
- `Queensboro.Bridge`: Daily bicycle count on the Queensboro Bridge.
- `Total`: Total daily bicycle count across all four bridges.

### Bayesian Poisson Regression Model
A multivariate Poisson regression model was implemented in JAGS. The number of cyclists for each bridge (and the total) was modeled as a Poisson distribution, where the logarithm of its mean ($\lambda $) is a linear function of the predictors.

### Model Evaluation & Diagnostics
The model's convergence and stability were assessed using standard MCMC diagnostics:
- `Trace Plots`: Visual inspection showed that the MCMC chains were mixing well and had converged to a stable posterior distribution.
- `Geweke Diagnostic`: Z-scores for all parameters were within the standard range (|-1.96, 1.96|), indicating no significant difference between the means of the early and late parts of the chain.
- `Gelman-Rubin Diagnostic`: The potential scale reduction factor (PSRF) was approximately 1.0 for all parameters, suggesting that the variance within chains and between chains was similar, confirming convergence.
- `Effective Sample Size (ESS)`: The ESS for all parameters was sufficiently large, indicating low autocorrelation and an adequate number of independent samples from the posterior.

### Model Comparison
Two models were compared to determine the best fit:
- `M1`: A model with only main effects (Day, Temperature, Precipitation).
- `M2`: A model including an interaction term between High_Temp_F and Precipitation.

The models were compared using:
- `Bayes Factors`: Calculated using the brms package to compare the marginal likelihood of the two models.
- `Deviance Information Criterion (DIC)`: A hierarchical modeling generalization of AIC and BIC, used to assess model fit while penalizing for complexity. The model with the lower DIC is preferred.

### Posterior Predictive Check
A Bayesian p-value was calculated by comparing the distribution of a test statistic (total cyclist count) generated from the posterior predictive distribution with the observed statistic. This check helps validate whether the model provides a good fit for the observed data.

### Results
The Bayesian regression model successfully converged and produced stable posterior distributions for all parameters.
Temperature was confirmed to be a significant positive predictor of cyclist traffic.
Based on the posterior mean of the intercept parameters, the Manhattan Bridge was identified as the most frequently used bridge by cyclists during this period.

### How to Run the Code
#### Prerequisites
- **R** and **RStudio**  
- **JAGS** (Just Another Gibbs Sampler) – [Download here](https://mcmc-jags.sourceforge.io/)

### Installation
1. Clone the repository:
```bash
git clone https://github.com/lynscrat/Bayesian-Analysis-of-NYC-East-River-Bridge-Bicycle-Traffic.git
cd Bayesian-Analysis-of-NYC-East-River-Bridge-Bicycle-Traffic
```
2. Open `NYC_Bicycle.Rmd` in RStudio.
3. Install required R packages:
```bash
install.packages(c("tidyverse", "rjags", "e1071", "corrplot", "coda", "brms", "Rcpp"))
```
