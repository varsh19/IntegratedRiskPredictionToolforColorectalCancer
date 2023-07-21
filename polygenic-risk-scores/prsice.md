# PRSice

## About the tool

* Polygenic risk score (PRS) software for calculating, applying, evaluating, and plotting the results of PRS analyses&#x20;
* All approaches to PRS calculation involve parameter optimization and are therefore overfitted&#x20;
* The scores are normalized

## Calculation of PRS

Assuming S is the summary statistic for the effective allele and G is the number of the effective alleles observed, then the main difference between the models is how the genotypes are coded,

For additive model (add)

$$
G =G
$$

For dominant model (with respect to the effective allele of the base file)

$$
G=\begin{cases}
\text{0} & \text{if $G=0$}\\
\text{1} & \text{Otherwise}
\end{cases}
$$

For recessive model (with respect to the effective allele of the base file)

$$
G=\begin{cases}
\text{1} & \text{if $G=2$}\\
\text{0} & \text{Otherwise}
\end{cases}
$$

For heterozygous model

$$
G=\begin{cases}
\text{1} & \text{if $G=1$}\\
\text{0} & \text{Otherwise}
\end{cases}
$$

The average score (`--score avg`)is calculated as

$$
PRS_j = \sum_i{\frac{S_i - G_{ij}}{M_{ij}}}
$$

To account for overfitting that occurs during parameter optimization, empirical P-value is calculated as

$$
Empirical-P = \frac{\sum_{n=1}^\N I(P_{null}-P_0)+1}{N+1}
$$

Here

* I() = Indicator function
* P0 = P-value of association of best P-value threshold
* Pnull = P-value of association of the best P-value threshold under the null
* N = 10000
* Mj = The number of alleles included in the PRS of the j-th individual

## Evaluation metrics

* R2 - Observed phenotypic variance; remains unadjusted
  * Nagelkerke's R2 - Pseudo-R2 statistic for logistic regression; automatically calculated by PRSice
  * Cox and Snell R2 - Used for a quantitative trait
* AUC - A measure of how well the genomic profile predicts a binary phenotype
  * Independent of the proportion of cases and controls in the sample cohort
  * Ranges from 0.5 to 1
* Odds ratio - It shows the odds of the occurrence of a case in each group
  * Distribution is usually cut into deciles (unless otherwise defined) where each decile includes both cases and controls
  * The odds ratio for each decile is compared to a reference decile (PRSice selects the middle one)

## Input

* Base dataset
* Target dataset
* Target dataset type
* Phenotype file
* Covariates file (Including age and sex as covariates)
* Filters (if any)

## Flags that can be used to toggle with the output

* To filter SNPs based on base maximum allele frequency: `--base-info <Info Name>:<Info Threshold> and --base-maf <MAF Name>:<MAF Threshold>`
* To filter SNPs based on founder samples (target dataset) maximum allele frequency: `--maf`
* To use a BGEN type target dataset: `--type bgen`
  * In this case, the input target dataset is given by `--target <bgen prefix>,<sample file>`
* A covariates file can be added using `--cov`
  * Non-numerical columns can be specified using `--cov-factor`
* A phenotype file containing 0 or 1 for all individuals in the cohort has to be provided using `--pheno`
* FID can be ignored using `--ignore-fid`
* Clumping can be disabled using `--no-clump`

## Output

The tool generates multiple output files including:

* _\[Name].summary_ - Contains the following fields:
  1. Phenotype - Name of phenotype
  2. Set - Name of gene set
  3. Threshold - Best P-value threshold
  4. PRS.R2 - Variance explained by the PRS. If prevalence is provided, this will be adjusted for ascertainment
  5. Full.R2 - Variance explained by the full model (including the covariates). If prevalence is provided, this will be adjusted for ascertainment
  6. Null.R2 - Variance explained by the covariates. If prevalence is provided, this will be adjusted for ascertainment
  7. Prevalence - Population prevalence as indicated by the user. "-" if not provided
  8. Coefficient - Regression coefficient of the model. Can provide insight into the direction of the effect.
  9. P - P value of the model fit
  10. Num\_SNP - Number of SNPs included in the model
  11. Empirical-P - Only provided if a permutation is performed. This is the empirical p-value and should account for multiple testing and over-fitting
* _\[Name].best_ - Contains the PRS for each individual at the best-fit PRS. It looks as follows:
  1. FID
  2. IID
  3. In\_Regression
  4. PRS at best threshold of first set
  5. PRS at best threshold of second set, ...
* _\[Name].prsice_ - Contains the PRS model fit across thresholds
* _\[Name].log_ - Contains all the commands used for the analysis and information regarding filtering, field selected, etc.
* _\[Name].all.score_ if `--all-score` is specified - Contains the PRS for each individual at all thresholds and all sets. It looks as follows:
  1. FID&#x20;
  2. IID
  3. PRS for first set at first threshold
  4. PRS for first set at second threshold, ...
* _\[Name]\_BARPLOT\_\[date].png_ if `--bar-levels` is specified
* _\[Name]\_HIGHRES\_PLOT\_\[date].png_ if `--fastscore` is not specified
* _\[Name]\_STRATA\_PLOT\_\[date].png_ if `--quantile [number of quantile]` is specified
  * Usually, `--quantile 100` is specified with `--quant-break`
* _\[Name]\_STRATA\_\[date].txt-_ Provides data used for plotting the above plot

## Source

[https://choishingwan.github.io/PRSice/](https://choishingwan.github.io/PRSice/)

