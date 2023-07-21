# Generalized linear models

A generalized linear model is a statistical framework used for modeling the relationship between a response variable (phenotype) and one or more explanatory variables (risk factors). It extends the concept of linear regression by allowing the response variable to have a non-normal distribution and heteroscedasticity.&#x20;

The relationship between the response variable and the explanatory variables is described using a link function and probability distribution. We want to obtain this link function and the probability distribution for a model incorporating PRS values and clinical risk factors to quantify the relationship between risk of disease and the factors affecting it. We then proceed to infer which risk factors have significant effects on the risk.&#x20;

AIC stands for Akaike Information Criterion. It is used to measure the quality of a statistical model and takes into account both the model’s goodness of fit and its complexity. It is primarily used to compare different models and select the one that is a best balance of both these features. It is used to compare different GLMs with varying explanatory variables or link functions.&#x20;

The formula for AIC is:&#x20;

**AIC = -2ln(L) + 2K**

where

* K = number of estimated parameters in the model
* L = maximum value of the likelihood function for the model
