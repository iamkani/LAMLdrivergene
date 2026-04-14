# LAMLdrivergene

Identifying candidate driver genes in Acute Myeloid Leukemia (LAML) using machine learning classifiers on publicly available mutation and expression data.

Built during my M.S. at the University of Louisiana at Lafayette (2014–2016) as part of my work at the Centre for Advanced Computer Studies.

## Background

Most mutations in cancer genomes are "passengers" — they don't actually contribute to tumor growth. The ones that do matter are called driver mutations, and figuring out which is which is a hard problem, especially in cancers like AML where the mutation landscape is heterogeneous.

The idea here was to take known driver/passenger annotations from [DriverDB](http://driverdb.tms.cmu.edu.tw/) and mutation data from [dbSNP](https://www.ncbi.nlm.nih.gov/snp/), extract features from the surrounding genomic context, and train classifiers to distinguish drivers from passengers.

## Approach

1. **Data collection** — Pulled LAML mutation records from DriverDB and cross-referenced with dbSNP annotations. Features included mutation type, conservation scores, functional impact predictions, gene expression levels, and pathway membership.

2. **Feature engineering** — Built feature vectors from raw genomic annotations. Handled a lot of missing data (not every mutation has every annotation available) and class imbalance (way more passengers than drivers).

3. **Classification** — Trained and compared several models:
   - Decision Trees (baseline)
   - Bayesian Networks (to capture dependencies between genomic features)
   - Hidden Markov Models (for sequential patterns along the genome)
   - Random Forest (ended up performing best overall)

4. **Evaluation** — Used stratified k-fold cross-validation. Focused on precision-recall rather than accuracy because of the class imbalance. A classifier that just predicts "passenger" every time gets 95%+ accuracy but is useless.

## Results

Random Forest gave the best F1 score (~0.78) for driver gene classification. Bayesian Networks were interesting because they surfaced feature dependencies that the tree-based methods didn't — for instance, the interaction between conservation score and expression level was more predictive than either feature alone. HMMs underperformed for this particular task, likely because the sequential structure along the chromosome wasn't the dominant signal.

The top features for classification were: cross-species conservation score, predicted functional impact (SIFT/PolyPhen), number of known mutations at the same locus in other cancer types, and gene expression level in AML samples vs. normal tissue.

## Tools used

- Python (Scikit-learn, Pandas, NumPy, BioPython)
- R (for some of the statistical analysis and plots)
- MySQL (local copy of DriverDB subset)
- Data sourced from DriverDB, dbSNP, COSMIC

## Files

_(Cleaning up and uploading the original scripts — the code was written during grad school and needs some tidying before it's presentable. Check back soon.)_
