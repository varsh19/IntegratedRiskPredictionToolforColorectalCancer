# Integrated risk score

## Generalized linear model

A generalized linear model is a statistical framework used for modeling the relationship between a response variable (phenotype) and one or more explanatory variables (risk factors). It extends the concept of linear regression by allowing the response variable to have a non-normal distribution and heteroscedasticity.&#x20;

The relationship between the response variable and the explanatory variables is described using a link function and probability distribution. We want to obtain this link function and the probability distribution for a model incorporating PRS values and clinical risk factors to quantify the relationship between the risk of disease and the factors affecting it. We then proceed to infer which risk factors have significant effects on the risk.&#x20;

AIC stands for Akaike Information Criterion. It is used to measure the quality of a statistical model and takes into account both the model’s goodness of fit and its complexity. It is primarily used to compare different models and select the one that is the best balance of both these features. It is used to compare different GLMs with varying explanatory variables or link functions. The formula for AIC is:

$$
AIC  =   - 2log(L)  +  2K
$$

Here, L is the maximized likelihood of the model and k is the number of parameters in the model. The AIC value is calculated for each model, and the model with the lowest AIC is considered the best fit among the competing models.

The AIC can be used in GLMs to compare different models with different sets of explanatory variables or different link functions. By comparing the AIC values of different models, we can select the model that strikes the best balance between goodness of fit and complexity, thus aiding in model selection and inference.

## Development and comparative analysis

Accurate risk prediction tools are developed using the derived PRS values and pre-processed phenotypic data, including clinical risk factors. These models are developed separately for PRS and clinical factors. Another integrated model incorporating both is developed.&#x20;

The score follows the following formula:&#x20;

$$
OR_{IRT} = OR_{PRS} * OR_{Clinical}
$$

Here, OR refers to the odds ratio, which refers to the relationship between exposure and disease in a case-control study.&#x20;

PRS-only and clinical models are evaluated against the integrated model to see whether the latter performs better than its stand-alone components using the AUC, R2, and NRI metrics. The model's performance is also assessed for the component ethnicities against a meta-analysis. Finally, the scripts will be integrated with an R Shiny app for public use and available via a webserver (only using public data).
