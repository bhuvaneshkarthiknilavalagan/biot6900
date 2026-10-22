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

MODULE 3 ASSIGNMENT
# BIOT 6900 — Module 3 Assignment: Target Triage with Protein Language Models

Disease: Lung Adenocarcinoma (LUAD)

Input:`targets_luad.csv` (top 15 targets from Module 2)

Output:`targets_luad_w3.csv` — re-ranked target list combining Module 2 multi-omics evidence with ESM C tractability priors

Status: Notebook runs cleanly top to bottom (Kernel → Restart & Run All confirmed). All steps completed as planned — no blockers.

Mapping: 15/15 Module 2 targets mapped to `proteome.tsv` (0 unmapped).

## Module 4 — Structure-Based Druggability (LUAD)

**Targets:** FGFR2 (#1 in Module 2) and CYP1A1 (#8)

**Status:** Ran cleanly top to bottom, no issues after installing fpocket.

**Findings:** CYP1A1 is one confident domain (pLDDT 95.5), RMSD 0.33 Å vs. experiment. FGFR2 has 5 domains with an uncertain arrangement between them, but its kinase domain matched experiment well (RMSD 0.65 Å). Both targets produced one strong, confidence-backed pocket, and both matched real bound-ligand sites almost perfectly (100% and ~94% coverage).

**Call for both:** Structure supports a small-molecule pocket.

**Key inference:** FGFR2's second-best pocket by raw geometry got rejected for low confidence, real proof that confidence checks catch misleading pockets geometry alone would miss.