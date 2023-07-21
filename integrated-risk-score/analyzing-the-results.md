# Analyzing the results

## Downsizing the number of predictors

We perform stepwise regression to generate the best model with only the most necessary predictors in R:

{% code overflow="wrap" %}
```r
# Create a new GLM model to bypass the error about number of rows changing
model1 <- glm(PHENO ~ PRS + Age + Sex + height + weight + physical_activity + meat + smoking + alcohol + father_cancer + mother_cancer + sibling_cancer + polyps + crohns_disease + ulcerative_colitis, data = model_1$model, family = binomial)

# Run stepwise regression
sml1 <- step(model1)

# View summary of model
summary(sml1)

# Print formula
print(formula(sml1))
```
{% endcode %}

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Here, alcohol consumption has a lesser effect on the results than the other factors, so that can also be omitted. This could be due to the nature of the data in the UK Biobank for this column.

Similarly, smoking seems to have had predominantly "Never" in its entries, making it look like NOT smoking significantly affects the results. So when tied together with other factors, it looks like not smoking could increase the risk of colorectal cancer, which is a strange statement.

## Finding the amount of extra information added by PRS

### Incremental R-squared

We use "incremental r-squared" to compute the difference in R2 values for the model with and without PRS. R-squared can only be obtained for a linear model, so we perform linear regression. We already have [R2 for the clinical model](../clinical-risk-factors/analyzing-the-results.md#r-squared-r2).

{% code overflow="wrap" %}
```r
# Create a model using linear regression
integrated_model <- lm(PHENO ~ PRS + Age + Sex + height + weight + physical_activity + meat + smoking + alcohol + father_cancer + mother_cancer + sibling_cancer + polyps + crohns_disease + ulcerative_colitis, data = merged_df, family = binomial)

# Compute integrated R2
integrated_rsq <- summary(integrated_model)$r.squared

# Print incremental R2
incremental_rsq <- integrated_rsq - clinical_rsq
```
{% endcode %}

| R2                      |  PGS000074 |  PGS000785 |
| ----------------------- | ---------: | ---------: |
| R2 for integrated model | 0.03116385 | 0.04671455 |
| Incremental R2          | 0.01612564 | 0.03167634 |

The positive values say that PRS has indeed added information and increased the clinical model's accuracy instead of worsening it.&#x20;

### Net reclassification index (NRI)

The net reclassification index (NRI) is a measure used to assess the improvement in risk prediction models when new predictors are added. It quantifies the net correct reclassification of individuals into higher or lower-risk categories based on adding new predictors. To calculate the NRI, you must compare the risk prediction model with and without the PRS variable.

**NRI = P(up|event) − P(down|event) + P(down|nonevent)−P(up|nonevent)**

“Up” means that the new risk model places a person into a higher risk category than the old model. Similarly, “down” means the new model places a person into a lower-risk category.

In other words, if you create a confusion matrix

**NRI = P(true positive) + P(false negative) - \[(P(false positive) + P(true negative)]**

The NRI ranges from -2 to +2, with a value of 0 indicating no improvement or change in risk prediction.

<pre class="language-r" data-overflow="wrap"><code class="lang-r"># Predict probabilities for integrated model
probs_integrated &#x3C;- predict(model, type = "response", newdata = merged_df)

# Categorize individuals into risk categories based on the predicted probabilities
categories_without_PRS &#x3C;- cut(probs_clinical, breaks = c(-Inf, 0.5, Inf), labels = c("Low", "High"))
categories_with_PRS &#x3C;- cut(probs_integrated, breaks = c(-Inf, 0.5, Inf), labels = c("Low", "High"))

# Create a contingency table of predicted risk categories
contingency_table &#x3C;- table(categories_clinical, categories_integrated)

# Calculate NRI
<strong>NRI &#x3C;- (sum(contingency_table[1] + contingency_table[2]) - sum(contingency_table[3] + contingency_table[4])) / sum(contingency_table)
</strong>
# Print NRI
print(NRI)
</code></pre>

The NRI value obtained is **0.853516**. It suggests a substantial positive improvement in the model's ability to correctly classify individuals into their respective risk categories upon including PRS. This value implies that, on average, there is a considerable increase in the proportion of correctly reclassified individuals into higher or lower-risk categories with the inclusion of PRS as a new predictor. The higher the NRI value, the stronger the improvement in risk prediction.

### deltaAUC

The area under the curve is computed for the model with and without PRS, and the difference between the two values is obtained. We already have [AUC for the clinical model](../clinical-risk-factors/analyzing-the-results.md#area-under-the-curve-auc).

{% code overflow="wrap" %}
```r
# Compute AUC for the integrated model
auc_integrated <- roc(merged_df$PHENO, predict(model), type = "response"))$auc

# Compute deltaAUC
delta_AUC <- auc_integrated - auc_clinical

# Print deltaAUC
print(delta_AUC)
```
{% endcode %}

| AUC                      |  PGS000074 |  PGS000785 |
| ------------------------ | ---------: | ---------: |
| AUC for integrated model |     0.6028 |     0.6262 |
| deltaAUC                 | 0.03202171 | 0.05543415 |

Just as the previous two metrics, deltaAUC also tells us that the model's accuracy has increased thanks to the addition of PRS as one of the variables.&#x20;

This answers our [research questions](../#question) and tells us that the addition of polygenic risk scores has indeed increased the accuracy of predicting the risk of colorectal cancer.
