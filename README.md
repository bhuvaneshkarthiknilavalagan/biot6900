# BIOT 6900 coursework
Bhuvanesh Karthik Nilavalagan 

MODULE 1 ASSIGNMENT 

Everything worked as expected during setup or running this project. The only issue was with Git authentication using a password, which no longer works with GitHub, this was resolved by switching to SSH authentication instead.

MODULE 2 ASSIGNMENT
# BIOT 6900 — Assignment 2: Independent Multi-Omics Target Discovery

Disease: Lung Adenocarcinoma (LUAD)

# layers
TRANSCRIPTOMICS - CPTAC (via `cptac` Python package, source: bcm) | Open | https://github.com/PayneLab/cptac 
Proteomics | CPTAC (via `cptac` Python package, source: umich) | Open | https://github.com/PayneLab/cptac 
Genomics | GWAS Catalog | Open | https://www.ebi.ac.uk/gwas/

Pipeline: harmonize on gene symbol → sign-agreement concordance → equal-weighted multi-evidence score → rank → export to `targets_luad.csv`.

RNA and protein layers are sample-matched (213 CPTAC patients: 111 tumor, 102 matched-normal, split via `.N` sample ID suffix in patient IDs). GWAS layer is from independent case-control cohorts, so overall three-layer integration is gene-level/unmatched — the same integration category as the course's Alzheimer's exercise.

Multi-evidence ranking recovered CLPTM1L (a well-established lung cancer GWAS locus near TERT) and CYP1A1 (a tobacco-carcinogen metabolism gene) among the top-scoring targets 
Please Refer targets_luad.csv for the full ranked list and the accompanying report for interpretation.