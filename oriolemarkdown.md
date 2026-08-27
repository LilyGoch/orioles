Oriole GWAS in GEMMA
================
Lily Goch
2023-10-23

# GWAS in GEMMA \##EDITED August 25 2026

prep beagle files - impute missing data

``` bash
java -Xmx96g -jar /programs/beagle41/beagle41.jar gt=Orioles_filtered_final_093020_55individuals.recode.vcf nthreads=20 out=oriole_beagle_output impute=true
```

Aug 26 2026: Finished 6 hrs 37 min 30 sec

# create PLINK files

``` bash
vcftools --gzvcf oriole_beagle_output.vcf.gz --plink --out oriole_outputPlinkformat
```

# make .bed files

``` bash
/programs/plink-1.9-x86_64-beta5/plink --file oriole_outputPlinkformat --make-bed --chr-set 28 no-mt --allow-extra-chr 0 --out oriole_output_bed
```

# enter phenotypic information into the .fam file

KEEP TRACK OF COLUMN NUMBERS
Column 6 is phenotype information
Coded as 0 (BUOR) or 1 (BAOR), based on mitochondrial haplotype network
N1 = trait one \#haplotype

# RUN GEMMA

# generate relatedness matrix

# 

\#gk: specify which type of kinship/relatedness matrix to generate

(default 1; valid value 1-2; 1: centered matrix; 2: standardized
matrix.) \##miss: specify missingness threshold (default 0.05) \##maf:
specify minor allele frequency threshold (default 0.01) \##r2: specify
r-squared threshold (default 0.9999) \##hwe: specify HWE test p value
threshold (default 0; no test)

``` bash
gemma -bfile oriole_output_bed -gk 1 -miss 1 -maf 0 -r2 1 -hwe 0 -o orioles
```

output=GEMMA_HZ

# run GEMMA: univariate linear models

\#lmm: linear mixed model (also can run lm or bslmm. read about models
and determine the best option for your data) \#lmm (cont): specify
frequentist analysis choice (default 1; valid value 1-4; 1: Wald test;
2: likelihood ratio test; 3: score test; 4: all 1-3.) \##n: specify
phenotype column in the phenotype file (default 1); or to specify which
phenotypes are used in the mvLMM analysis

\#Trait one (Haplotype)

``` bash
gemma -bfile /workdir/lpg34/oriole_output_bed -k /workdir/lpg34/output/orioles.cXX.txt -lmm 4 -n 1 -o GWAS_HZ_lmm_trait1
```
