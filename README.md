# Serine-synthesis-in-Basal-like-breast-cancer : replication, cell-type resolution and confounder analysis

An expression-based study of the de novo serine synthesis pathway across three public breast cancer datasets. It started from a bulk TCGA-BRCA analysis of mitochondrial pathways and followed one finding, high serine synthesis in Basal-like tumors, into an independent cohort and into single-cell data. All results are RNA expression plus one copy-number summary; no metabolic flux was measured.

### Summary
- Serine synthesis genes (PHGDH, PSAT1, PSPH) are much higher in Basal-like than in luminal tumors in two independent bulk cohorts (TCGA-BRCA, SCAN-B), and the signal sits in cancer cells in single-cell data.
- About four fifths of the Basal vs Luminal B gap remains after adjusting for proliferation, hypoxia, MYC-target and ATF4-target scores (77% in SCAN-B, 79% in TCGA).
- High-level gene amplification is too rare to explain it (PHGDH 2.1%, PSAT1 0.2% of 1,070 breast tumors).
- Within Basal tumors, serine synthesis tracks an amino-acid stress-response program (ASNS, CHAC1, TRIB3, SHMT2, MTHFD2 and others) after removing proliferation, in both cohorts.
- Several hypotheses did not hold and are reported below, including a fatty acid oxidation (FAO) to immune-score association from TCGA that did not replicate in SCAN-B.

### Background
Breast tumors are classified into intrinsic molecular subtypes (PAM50): Luminal A, Luminal B, HER2-enriched, Basal-like and Normal-like. Basal-like tumors are mostly triple-negative (ER-, PR-, HER2-) and tend to be more proliferative and aggressive.

Cancer cells reprogram their metabolism. One branch of glycolysis feeds de novo serine synthesis (PHGDH, then PSAT1, then PSPH). Serine supplies one-carbon metabolism, which runs in the cytosol (SHMT1, MTHFD1) and in mitochondria (SHMT2, MTHFD2, MTHFD1L). Members of the sideroflexin family (SFXN1, SFXN2, SFXN3) have been described as mitochondrial serine transporters. ATF4 is a stress-response transcription factor that regulates several amino-acid metabolism genes.

##### Prior work
High expression of serine synthesis genes in basal-like breast cancer has been reported before. Choi et al. (2022, Cell Reports) reported that luminal breast tumors silence PSAT1 by promoter DNA methylation and consequently depend on exogenous serine. This repository does not repeat that work. It asks complementary questions at the population scale: whether the expression pattern replicates across large independent cohorts, which cells carry it, how much of it is explained by proliferation, hypoxia and stress programs, what it co-varies with inside Basal tumors, and whether gene amplification contributes.

### Questions
- Is serine synthesis higher in Basal-like tumors, and does it replicate across cohorts?
- Which cells carry the signal: cancer cells, immune cells or stroma?
- Is the difference explained by proliferation, hypoxia, MYC or ATF4 programs?
- Could gene amplification explain it?
- What co-varies with serine synthesis inside Basal tumors?
- Does it relate to fatty acid oxidation (FAO) or immune activity, the earlier TCGA finding that motivated the work?

### Data
Dataset	Use	Notes
- Discovery cohort: TCGA-BRCA RNA-seq (via recount3),  1,109 primary tumors and 112 solid-tissue normals; 112 patients with matched pairs; PAM50 labels from the TCGA subtype table (Basal 197, LumB 209, LumA 569, Her2 82 tumors)
- Replication cohort: SCAN-B breast cancer RNA-seq, log2 expression matrix with PAM50 calls (Basal 360, LumB 767, LumA 1,709, Her2 348)
- Wu et al. 2021 single-cell atlas (GEO GSE176078), Cell-type resolution	26 primary tumors (ER+, HER2+, TNBC), cell-type annotations provided by the authors
- cBioPortal TCGA breast cancer (PanCancer Atlas)	Copy number	Gene-level alteration frequencies across 1,070 tumors
- MitoCarta3.0	Mitochondrial pathway gene sets	MitoPathways

### Methods
Paired tumor vs normal (TCGA). Patients with both a Primary Tumor and a Solid Tissue Normal sample were selected (112). For patients with several tumor samples, the sample with the largest library size was kept. Expression was normalized to log2 CPM. Pathway scores are z-scores of gene expression averaged over the genes in each set, calculated within the paired cohort. Paired differences per patient were tested with the Wilcoxon signed-rank test, with Benjamini-Hochberg correction across pathways. Normal-tissue fat content was approximated by an adipocyte-marker score (ADIPOQ, PLIN1, LEP, CIDEC, FABP4, LIPE).

Subtype comparisons in bulk (TCGA tumors, SCAN-B). Gene-level differences between Basal and Luminal B tumors (medians, Mann-Whitney test). Gene sets were fixed before each analysis:
- Serine synthesis: PHGDH, PSAT1, PSPH
- Proliferation: MKI67, TOP2A, MCM2, PCNA, CCNB1, AURKA, BIRC5, CDK1
- Hypoxia: CA9, BNIP3, NDRG1, ADM, VEGFA, ANKRD37, EGLN3, P4HA1, BHLHE40
- MYC targets: NPM1, NCL, CAD, ODC1, CCND2, PPAT, HSPD1, EIF4A1
- ATF4 targets: ASNS, ATF3, DDIT3, TRIB3, CHAC1, SLC7A11, GADD45A
- Glycolysis: SLC2A1, HK2, GPI, PFKP, ALDOA, GAPDH, PGK1, ENO1, PKM, LDHA
- 
Scores were standardized, and the Basal vs Luminal B gap in the serine score was estimated by linear regression with covariates added one at a time. Within Basal tumors, Spearman correlations and a joint regression were used, and variance inflation factors were checked.

##### Copy number
Amplification and deep-deletion frequencies of the serine synthesis genes were read from the cBioPortal "copy-number altered genes" table for the TCGA PanCancer Atlas breast cohort. Frequencies are for all subtypes combined.

##### Co-expression network (SCAN-B Basal tumors)
WGCNA on the 5,000 most variable genes plus selected genes of interest (signed network, soft-threshold power 6, minimum module size 30). The association of each gene with the serine score after regressing out the proliferation score was computed in SCAN-B and repeated in TCGA Basal tumors.

##### Single-cell analysis (Wu 2021)
Counts were library-size normalized and log-transformed in Scanpy. Gene-set scores were computed per cell with scanpy.tl.score_genes. Scores and single-gene expression were averaged per patient and cell type (cell types with fewer than 20 cells in a patient excluded) so that each patient counts once. Basal-like PAM50 status was assigned by calling PAM50 (genefu, pam50.robust) on pseudobulk profiles of the cancer cells of the 8 TNBC patients; 4 were called Basal-like. Within-cancer-cell coupling between the serine and stress-response scores was computed per patient after removing the proliferation score, and compared between TNBC and ER+ patients with a Mann-Whitney test and a pooled regression with patient fixed effects and patient-clustered errors.

##### Statistics
Gene sets and expected directions were fixed before each replication. Many comparisons were exploratory and no global multiplicity correction was applied across the whole study; adjusted p-values are given within each analysis where stated.

### Results
#### 1. Paired tumor vs normal (TCGA, 112 patients)
- The overall mitochondrial expression score was higher in tumor than in matched normal in 78 of 112 pairs (70%; median difference 0.121; Wilcoxon p = 5e-06; effect size 0.33).
- The increase was driven by luminal tumors: Luminal B (n = 22, adjusted p = 1e-4), Luminal A (n = 61, adjusted p = 0.008), HER2-enriched borderline (n = 9, adjusted p = 0.052), Basal-like not detected (n = 17, adjusted p = 0.89).
- Mitochondrial import and biogenesis pathways and respiratory chain expression were higher in tumor, while fatty acid oxidation and several differentiated lipid and xenobiotic pathways were lower (FAO lower in tumor in 87.5% of pairs).
- The FAO decrease was larger in Basal-like than in Luminal B tumors, and the difference remained after adjusting for adipocyte-marker expression in the normal tissue (-0.58 SD, 95% CI -0.85 to -0.31).

#### 2. Serine synthesis is higher in Basal-like tumors in two cohorts
Median log2 difference, Basal minus Luminal B:
Gene	TCGA	SCAN-B
PSAT1	+4.31	+4.39
PHGDH	+2.54	+2.29
MTHFD1L	+1.91	+1.46
SHMT2	+0.42	+0.39
MTHFD2	+0.24	+0.51
SFXN2	-1.04	-1.62
SFXN1	-0.65	-0.48





