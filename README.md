# BIOT 6900 coursework
Bhuvanesh Karthik Nilavalagan 

MODULE 1 ASSIGNMENT 

Everything worked as expected during setup or running this project. The only issue was with Git authentication using a password, which no longer works with GitHub, this was resolved by switching to SSH authentication instead.

MODULE 2 ASSIGNMENT
## BIOT 6900 — Assignment 2: Independent Multi-Omics Target Discovery

Disease: Lung Adenocarcinoma (LUAD)

Data layers:
- Transcriptomics | CPTAC LUAD (`cptac` package, source: bcm) | Open | https://github.com/PayneLab/cptac
- Proteomics | CPTAC LUAD (`cptac` package, source: umich) | Open | https://github.com/PayneLab/cptac
- Genomics | GWAS Catalog, lung adenocarcinoma (MONDO_0005061) | Open | https://www.ebi.ac.uk/gwas/

RNA and protein are sample-matched (213 CPTAC samples: 111 tumor, 102 normal). GWAS is from independent cohorts, so the integration is gene-level. 154 genes survived the join; the top 15 are in `targets_luad.csv` (all 154 in `targets_luad_all.csv`). See the report for interpretation.

What did not work: the GWAS mapped genes came as pairs (e.g. `GENE1 - GENE2`) and had to be split before joining. Some GWAS hits came from cross-cancer studies rather than LUAD-specific ones, which is flagged in the notebook (Part D) and the report.

MODULE 3 ASSIGNMENT
# BIOT 6900 — Module 3 Assignment: Target Triage with Protein Language Models

Disease: Lung Adenocarcinoma (LUAD)

Input:`targets_luad.csv` (top 15 targets from Module 2)

Output:`targets_luad_w3.csv` — re-ranked target list combining Module 2 multi-omics evidence with ESM C tractability priors

Status: Notebook runs cleanly top to bottom (Kernel → Restart & Run All confirmed). Only issue: after reorganizing the repo, the notebook's paths had to be updated to read the data pack from `shared/Data/BIOT6900_Module3_DataPack` and the targets from `modules/module2/targets_luad.csv`.

Mapping: 15/15 Module 2 targets mapped to `proteome.tsv` (0 unmapped).

## Module 4 — Structure-Based Druggability (LUAD)

**Targets:** FGFR2 (#1 in Module 2) and CYP1A1 (#8)

**Status:** Ran cleanly top to bottom, no issues after installing fpocket.

**Findings:** CYP1A1 is one confident domain (pLDDT 95.5), RMSD 0.33 Å vs. experiment. FGFR2 has 5 domains with an uncertain arrangement between them, but its kinase domain matched experiment well (RMSD 0.65 Å). Both targets produced one strong, confidence-backed pocket, and both matched real bound-ligand sites almost perfectly (100% and ~94% coverage).

**Call for both:** Structure supports a small-molecule pocket.

**Key inference:** FGFR2's second-best pocket by raw geometry got rejected for low confidence, real proof that confidence checks catch misleading pockets geometry alone would miss.

## Module 5 — Variant Interpretation on CYP2C9

**Gene:** CYP2C9 (warfarin pharmacokinetic gene)

**Status:** Notebook runs cleanly top to bottom. AlphaGenome Atlas ran live (36 variants scored); the re-run loads those cached results, so no API key is needed and none is committed. Atlas results are joined on rsID, so the few variants without one get no Atlas score.

**Fetch:** 41 variants, both anchor alleles (\*2, \*3) found. Frequency filter MAX_AF = 0.10, justified since both anchors sit at 4.9% and 8.8%, well above rare-disease thresholds.

**Calibration:** Attempted on CADD but disclosed as unreliable, only 8 labelled variants, likelihood ratios ran backwards. 

**Atlas:** AlphaGenome setup completed successfully (key configured, package installed, live query run). 

**Major finding:** CYP2C9 has excellent missense and anchor-allele coverage but too few labelled ClinVar variants to meaningfully calibrate any predictor, a clear example that data coverage and statistical power are not the same thing.


## Module 6 — PRS Portability on Real Data

**Score:** PGS Catalog PGS000334 (Alzheimer's, 22 SNPs incl. APOE) applied to 1000 Genomes phase 3 genotypes (2,336 people, 5 ancestry groups). Data from the course `data.zip` (open). The phenotype is simulated.

**Status:** Notebook runs cleanly top to bottom on real data (`USE_DEMO = False`). 22/22 variants used, 13 flipped, 0 dropped. Figures in `modules/module6/figures/`. No issues.

## Module 7 — Knowledge Graph (LUAD, Neo4j)

**Disease:** Lung adenocarcinoma (LUAD)

**Status:** Notebook runs top to bottom on Neo4j AuraDB Free. Graph has 44 nodes (19 genes, 6 drugs, 6 variants, 6 pathways, 5 phenotypes, 2 diseases) and 51 relationships. Genes come from my Module 2 top 15 (`data/targets_luad.csv`) plus EGFR, KRAS and ALK as drug-target genes. Related disease: idiopathic pulmonary fibrosis. Outputs: `my_knowledge_graph.png`, `nodes.csv`, `edges.csv`.

**Credentials:** kept in `modules/module7/.env` (git-ignored, see `.env.example`).

**What didn't work:** My Module 5 variants were CYP2C9 (warfarin) and have no link to LUAD, so I used LUAD GWAS lead SNPs and EGFR L858R instead. MyGene gave an alternate Ensembl id for CLPTM1L, fixed by hand. The starter's node count query only showed the shared `Entity` label, so I changed it to count each node type. No patient layer.

# Module 9 — Clinical NLP on Lung Adenocarcinoma

**Name:** Bhuvanesh Karthik Nilavalagan
**Disease:** Lung adenocarcinoma
**Seed:** 2054404 (student ID 002054404)

**Configuration**
- Signals: malignant_effusion, hypoxia, cachexia
- Consequences (declared leakage): thoracentesis, intubation, comfort_care
- Hidden leak caught: renal_replacement

**Results**
- Cohort: 400 synthetic patients, 22% mortality
- Naive vs assertion-aware extraction (malignant_effusion): 189 vs 111
- AUROC: 0.949 all features → 0.802 declared leakage removed → 0.693 all leakage removed

**Things I noticed**
- comfort_care only scored 0.009 even though it is obvious leakage. I think the leakage features overlap too much, since patients who get comfort care have usually been intubated too.
- Both injected wildcards are sepsis concepts. Lactate clearance and dialysis have nothing to do with lung cancer, so the generator seems to add the same two regardless of the disease you configure.
- lactate_not_clearing came out at −0.002, the lowest of anything. I expected it to leak but left it in since removing it would change nothing.

# Module 8 — Single-Cell & Spatial Omics

**Name:** Bhuvanesh Karthik Nilavalagan
**Target gene:** EPCAM
**Disease:** Breast cancer (both datasets provided)

status everything runs from top to bottom cleanly - all todo cells completed 

**Part 1 — Single-cell**
10 donors pulled from the CZ CELLxGENE Census, 18,388 cells after QC. Ran scVI and Harmony, compared both against no integration using batch and biology silhouettes. Went with scVI: it preserves biology better (0.034 vs Harmony's 0.007), though Harmony mixes donors better (−0.087 vs −0.025). EPCAM came out almost entirely in the Epithelial/Tumour compartment, 29% of those cells versus 0–8% everywhere else.

**Part 2 — Spatial**
10x breast cancer Visium section, 3,798 spots. Nine spatial domains, Moran's I, and neighborhood enrichment all computed. EPCAM reads as a tumour core gene. Worth noting in the Moran's I list — IGHG3, IGLC2 and IGKC are all in the top eight, so there are spatially clustered plasma cells even though no immune domain separated out at resolution 0.6.

**What I struggled with**

Most of my time went on environment problems rather than analysis. `highly_variable_genes` with `flavor="seurat_v3"` crashed on a loess convergence error, which took a while to understand since the message points at the fitting routine rather than the data. Switching to the default flavor on log-normalised data fixed it and keeps the batch-aware step.


Smaller things: the starter references `cell_type_broad` in the plotting cells but never creates it, so I added it from the Census `cell_type` column.

 And scVI ran on CPU rather than GPU, 60 epochs instead of 200, so the integration is probably weaker than it should be.


