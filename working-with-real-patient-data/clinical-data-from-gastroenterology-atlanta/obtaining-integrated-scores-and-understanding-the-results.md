# Obtaining integrated scores and understanding the results

The previous model that was created using Plink2 PRS and clinical data from the UKB was used to find the predicted scores, which described the probability of getting the disease for each patient.

```r
predicted_probabilities <- predict(model, type = "response", newdata = merged_df)
```

The IDs have not been shown in the following results to maintain confidentiality.

| Polygenic risk percentile | Integrated Risk Probability (Predicted) | Confidence Interval (Lower Bound) | Confidence Interval (Upper Bound) |
| :-----------------------: | :-------------------------------------: | :-------------------------------: | :-------------------------------: |
|           90.38           |                0.4930510                |             0.4579438             |             0.5281582             |
|           63.44           |                0.5397638                |             0.4145773             |             0.6649503             |
|           21.51           |                0.2689487                |             0.2462624             |             0.2916351             |
|            0.46           |                0.3130315                |             0.2028478             |             0.4232151             |
|           36.86           |                0.3324632                |             0.3021140             |             0.3628124             |
|           14.12           |                0.5318100                |             0.4048787             |             0.6587413             |
|           69.82           |                0.4207799                |             0.3711769             |             0.4703828             |
|           80.12           |                0.4507590                |             0.4201634             |             0.4813546             |

These results show a marked difference between percentile (only PRS) and probability (integrated) values for the same individuals. This is because of the inclusion of all risk factors. This goes on to say that a person may have the SNPs required to contract the disease, but their demographics and habits can reduce their risk, and the absence of SNPs does not necessarily make you immune to the disease.

This data can be used to advise patients about what they can change in their lifestyle to reduce their risk of getting colorectal cancer in the future. We hope that this tool can be used to understand the disease and reduce the risk of getting it, for each individual, in the foreseeable future.
