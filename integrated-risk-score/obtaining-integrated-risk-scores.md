# Obtaining integrated risk scores

## Command

The integrated risk scores were computed using generalized linear models (GLM) in R.

{% code overflow="wrap" %}
```r
model <- glm(PHENO ~ PRS + Age + Sex + height + weight + physical_activity + meat + smoking + alcohol + father_cancer + mother_cancer + sibling_cancer + polyps + crohns_disease + ulcerative_colitis, data = merged_df, family = binomial)
summary(model)
```
{% endcode %}

## Results

When using the polygenic risk scores obtained from the PGS000074 scoring file

<figure><img src="../.gitbook/assets/CRF + PRS1.png" alt=""><figcaption></figcaption></figure>

When using the polygenic risk scores obtained from the PGS000785 scoring file

<figure><img src="../.gitbook/assets/CRF + PRS2.png" alt=""><figcaption></figcaption></figure>

## Observation

The last column represents the p-value of the risk factor. When the p-value is <0.05, it means that the null hypothesis is to be rejected. When the p-value is >0.05, it means that there is no observed effect of the covariate on the hypothesis.

From this, covariates that significantly affect the risk of colorectal cancer include PRS, sex (male), body weight, meat consumption, physical activity, meat and alcohol consumption, family history of cancer, and history of polyps and ulcerative colitis.

Since the AIC value is lesser for PGS000785 (and the previously obtained AUC values are more), it can be inferred that the scoring file PGS000785 provides a better fit.
