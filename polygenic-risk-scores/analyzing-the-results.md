# Analyzing the results

## Evaluation metrics

The base-maf (base minor allele frequency) and maf (target minor allele frequency) filters were toggled with. However, there was no difference in the evaluation metrics obtained.

R2 was obtained from the summary file generated by PRSice as described [previously](prsice.md#output).

Using a merged dataframe containing phenotype, age, sex, and PRS, AUC is computed as follows in R:

<pre class="language-r" data-overflow="wrap"><code class="lang-r"># Compute AUC for only PRS
auc_prs &#x3C;- roc(df$PHENO, df$PRS)$auc

<strong># Create GLM for PRS, age, and sex
</strong><strong>prs_model &#x3C;- glm(PHENO ~ PRS + Age + Sex, data = df, family = binomial)
</strong><strong>
</strong><strong># Get predicted probabilities from the model
</strong>predicted_probs &#x3C;- predict(prs_model, type = "response")

# Compute combined AUC
auc_combined &#x3C;- roc(df$PHENO, predicted_probs)$auc

# Print combined AUC
print(auc_combined)
</code></pre>

| Evaluation metrics    | PGS000074 | PGS000785 |
| --------------------- | --------: | --------: |
| R2                    |    0.0505 |    0.0576 |
| AUC (only PRS)        |    0.5783 |    0.6058 |
| AUC (PRS + age + sex) |    0.6976 |    0.7095 |

## Distribution plots

|                PGS000074                |                  PGS000785                  |
| :-------------------------------------: | :-----------------------------------------: |
| ![](<../.gitbook/assets/image (6).png>) | ![](../.gitbook/assets/PGS000785\_dist.png) |

## Odds ratio plots

|                                                PGS000074                                                |                                 PGS000785                                |
| :-----------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------: |
| <img src="../.gitbook/assets/PGS000074_results_STRATA_PLOT_2023-06-27.png" alt="" data-size="original"> | ![](../.gitbook/assets/PGS000785\_results\_STRATA\_PLOT\_2023-06-27.png) |
