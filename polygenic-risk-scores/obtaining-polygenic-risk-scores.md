# Obtaining polygenic risk scores

## Commands used to generate the best output (in Linux terminal):

<pre class="language-bash" data-overflow="wrap"><code class="lang-bash">Rscript PRSice/PRSice.R 
--base PGS000074.txt 
--target /home/sharedFolder/referenceData/ukb/imputed_genotypes/ukb_imp_chr#_v3,/home/sharedFolder/referenceData/ukb/imputed_genotypes/ukb_imp_chr1_v3.sample 
--type bgen
--snp 0 --chr 1 --bp 2 --A1 3 --A2 4 --stat 5 --pvalue 10
<strong># --base-maf allelefrequency_effect:0.2
</strong><strong># --maf 0.01
</strong>--binary-target T 
--no-clump 
--fastscore
--ignore-fid 
--pheno /home/vsrinivasan75/cases_controls_new/pheno_white.txt
--cov /home/vsrinivasan75/cases_controls_new/covariates_white.txt
--cov-factor SEX 
--quantile 100 --quant-break 10,20,30,40,50,60,70,80,90,100 --quant-ref 50
--out /ukb_prs/colorectal_cancer/using_prsice/PGS000074/PGS000074_results 
--prsice PRSice/PRSice_linux
</code></pre>

{% code overflow="wrap" %}
```bash
Rscript PRSice/PRSice.R 
--base PGS000785.txt 
--target /home/sharedFolder/referenceData/ukb/imputed_genotypes/ukb_imp_chr#_v3,/home/sharedFolder/referenceData/ukb/imputed_genotypes/ukb_imp_chr1_v3.sample 
--type bgen 
--snp 0 --chr 1 --bp 2 --A1 3 --A2 4 --stat 5 --pvalue 10
# --base-maf allelefrequency_effect:0.2
# --maf 0.01
--binary-target T 
--no-clump 
--fastscore
--ignore-fid 
--pheno /home/vsrinivasan75/cases_controls_new/pheno_white.txt
--cov /home/vsrinivasan75/cases_controls_new/covariates_white.txt
--cov-factor SEX 
--quantile 100 --quant-break 10,20,30,40,50,60,70,80,90,100  --quant-ref 50
--out /ukb_prs/colorectal_cancer/using_prsice/PGS000785/PGS000785_results 
--prsice PRSice/PRSice_linux
```
{% endcode %}
