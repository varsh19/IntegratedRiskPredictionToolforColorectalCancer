# Creating the clinical dataframe

## Command

{% code overflow="wrap" %}
```r
ukb_full_df = read.fst("/home/sharedFolder/referenceData/ukb/metadata/ukb_full.fst")

age_df = subset(ukb_full_df, select=c("f.eid", "f.21022.0.0"))

sex_df = subset(ukb_full_df, select=c("f.eid", "f.31.0.0"))

height_df = subset(ukb_full_df, select = c("f.eid", "f.50.0.0", "f.50.1.0", "f.50.2.0", "f.50.3.0"))
height_df$height = rowMeans(height_df[, c(2,3,4,5)], na.rm=TRUE)
height_df <- subset(height_df, select=c(1,6))
height_df$height = height_df$height/2.54

weight_df = subset(ukb_full_df, select = c("f.eid", "f.21002.0.0", "f.21002.1.0", "f.21002.2.0", "f.21002.3.0"))
weight_df$weight = rowMeans(weight_df[, c(2,3,4,5)], na.rm=TRUE)
weight_df <- subset(weight_df, select=c(1,6))
weight_df$weight = weight_df$weight*2.2

activity_df = subset(ukb_full_df, select = c("f.eid", "f.884.0.0", "f.884.1.0", "f.884.2.0", "f.884.3.0"))
activity_df$activity = ceiling(rowMeans(activity_df[, c(2,3,4,5)], na.rm=TRUE))
activity_df <- subset(activity_df, select=c(1,6))

meat_df <- subset(ukb_full_df, select = c("f.eid", "f.1349.0.0", "f.1349.1.0", "f.1349.2.0", "f.1349.3.0"))
meat_df <- transform(meat_df, meat=ifelse(f.1349.0.0=="Never","No","Yes"))
meat_df <- subset(meat_df, select=c(1,6))

smoking_df <- subset(ukb_full_df, select = c("f.eid", "f.20116.0.0", "f.20116.1.0", "f.20116.2.0", "f.20116.3.0"))
smoking_df <- transform(smoking_df, smoking=f.20116.0.0)
smoking_df <- subset(smoking_df, select=c(1,6))

father_df = subset(ukb_full_df, select=c(1, 8608:8647))
father_df$father <- apply(father_df, 1, function(row) {if(any(grepl("cancer", row))) {return("Yes")} else {return("No")} })
father_df <- subset(father_df, select=c(1,42))

mother_df = subset(ukb_full_df, select=c(1, 8703:8746))
mother_df$mother <- apply(mother_df, 1, function(row) {if(any(grepl("cancer", row))) {return("Yes")} else {return("No")} })
mother_df <- subset(mother_df, select=c(1,46))

sibling_df = subset(ukb_full_df, select=c(1, 8747:8794))
sibling_df$sibling <- apply(sibling_df, 1, function(row) {if(any(grepl("cancer", row))) {return("Yes")} else {return("No")} })
sibling_df <- subset(sibling_df, select=c(1,50))

conditions_df = subset(ukb_full_df, select=c(1, 6729:6863))
conditions_df$polyps <- apply(conditions_df, 1, function(row) {if(any(grepl("1460", row))) {return("Yes")} else {return("No")} })
conditions_df$crohns_disease <- apply(conditions_df, 1, function(row) {if(any(grepl("1462", row))) {return("Yes")} else {return("No")} })
conditions_df$ulcerative_colitis <- apply(conditions_df, 1, function(row) {if(any(grepl("1463", row))) {return("Yes")} else {return("No")} })
conditions_df <- subset(conditions_df, select=c(1,137,138,139))

df_list <- list(height_df, weight_df, activity_df, meat_df, smoking_df, alcohol_df, father_df, mother_df, sibling_df, conditions_df)
clinical_risk_factors <- reduce(function(x, y) merge(x, y, by="f.eid", all=TRUE), df_list)
cohort <- read.table("cohort.tsv", header=TRUE, sep="\t")
clinical_risk_factors <- merge(clinical_risk_factors, cohort_df, by.x="f.eid", by.y="eid")
write.table(clinical_risk_factors, file="clinical_risk_factors.tsv", sep="\t", row.names=FALSE, quote=FALSE)
```
{% endcode %}
