# Preparing to use PRSice

## To install PRSice v.2.3.5 (Linux 64 bit)

{% code overflow="wrap" %}
```bash
wget https://github.com/choishingwan/PRSice/releases/download/2.3.5/PRSice_linux.zip
unzip PRSice_linux.zip
```
{% endcode %}

## To get the required scoring files

The following scoring files were taken from [PGS Catalog](https://www.pgscatalog.org/) on the basis of the cohort distribution, year of compilation, and number of variants for the required phenotype. Each of these scoring files had 103 variants for colorectal cancer for white cohorts.

* [PGS000074](https://www.pgscatalog.org/score/PGS000074/)
* [PGS000785](https://www.pgscatalog.org/score/PGS000785/)

These scoring files were loaded using the following command on the Linux terminal:

{% code overflow="wrap" %}
```bash
wget https://ftp.ebi.ac.uk/pub/databases/spot/pgs/scores/PGS000074/ScoringFiles/PGS000074.txt.gz
gunzip PGS000074.txt.gz
```
{% endcode %}

{% code overflow="wrap" %}
```bash
wget https://ftp.ebi.ac.uk/pub/databases/spot/pgs/scores/PGS000785/ScoringFiles/PGS000785.txt.gz
gunzip PGS000785.txt.gz
```
{% endcode %}

## Quality control of the scoring files

To remove metadata (in Linux terminal)

```bash
sed '1,14d' PGS000074.txt > PGS000074.txt
```

```bash
sed '1,14d' PGS000785.txt > PGS000785.txt
```

Since scoring files from the PGS Catalog do not include p-values, it was set to a default value of 0 (in R)

```r
df1 <- read.table("PGS000074.txt", header=TRUE, sep="\t")
df1$P <- '0'
write.table(df1, "PGS000074.txt", quote=FALSE, row.names=FALSE)
```

```r
df2 <- read.table("PGS000785.txt", header=TRUE, sep="\t")
df2$P <- '0'
write.table(df2, "PGS000785.txt", quote=FALSE, row.names=FALSE)
```
