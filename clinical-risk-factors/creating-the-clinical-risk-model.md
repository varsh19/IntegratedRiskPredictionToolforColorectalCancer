# Creating the clinical risk model

## Command

The clinical risk score was computed using generalized linear models (GLM) in R as follows:

```r
glm(PHENO ~ Age + Sex + height + weight + physical_activity + meat + smoking + alcohol + father_cancer + mother_cancer + sibling_cancer + polyps + crohns_disease + ulcerative_colitis, data = merged_df, family = binomial)
```
