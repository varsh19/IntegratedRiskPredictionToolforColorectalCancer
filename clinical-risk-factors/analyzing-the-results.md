# Analyzing the results

## R-squared (R2)

R-squared can only be obtained for a linear model, so we perform linear regression in R as follows:

{% code overflow="wrap" %}
```r
# Create a model using linear regression
clinical_model <- lm(PHENO ~ Age + Sex + height + weight + physical_activity + meat + smoking + alcohol + father_cancer + mother_cancer + sibling_cancer + polyps + crohns_disease + ulcerative_colitis, data = merged_df, family = binomial)

# Compute R2
clinical_rsq <- summary(clinical_model)$r.squared

# Print R2
print(clinical_rsq)
```
{% endcode %}

## Area under the curve (AUC)

AUC is computed in R as follows:

{% code overflow="wrap" %}
```r
# Get predicted probabilities
probs_clinical <- predict(model_clinical, type = "response", newdata = merged_df)

# Compute AUC
auc_clinical <- roc(merged_df$PHENO, predict(model_clinical, type = "response"))$auc

# Print AUC
print(auc_clinical)
```
{% endcode %}

## Evaluation metrics

| R2         |    AUC |
| ---------- | -----: |
| 0.01503821 | 0.5708 |
