# Plink2

Plink is a widely used software package for working with genetic data, particularly in the context of GWAS and PRS analyses. Plink2 is an updated version of Plink which adds support for genotype likelihoods, phase, multiallelic variants, and reference vs. alternate alleles.

Plink2 was also used to compute the polygenic risk scores using the following code:

{% code overflow="wrap" %}
```bash
plink2 
--pgen /home/sharedFolder/referenceData/ukb/imputed_genotypes_plink_binary_merged/ukb_chr1-22_v2.pgen
--psam /home/sharedFolder/referenceData/ukb/imputed_genotypes_plink_binary_merged/ukb_chr1-22_v2.psam
--pvar 
/home/sharedFolder/referenceData/ukb/imputed_genotypes_plink_binary_merged/ukb_chr1-22_v2.pvar
--score /home/vsrinivasan75/ukb_prs/PGS000785.txt 
1 4 6 no-mean-imputation 
--threads 4 
--out ukb_chr1-22_v1_nomean_PGS000785 
--memory 88304
```
{% endcode %}
