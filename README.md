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


HER2-enriched tumors lie between Basal-like and luminal for PHGDH, PSAT1 and MTHFD1L. SHMT1 was not upregulated in Basal-like tumors (slightly lower), so there was no sign of cytosolic compensation, and SHMT2 was not reduced.

#### 3. Single-cell resolution
- The serine synthesis score was highest in cancer epithelial cells (median per-patient score 0.100) and at or below background in immune and stromal cell types.
- In cancer cells, PHGDH and PSAT1 were higher in TNBC than in ER+ patients (p = 0.001 and < 0.001; 8 vs 9 patients) and highest in the four PAM50 Basal-like patients.
- The importer pattern was not confirmed at single-cell level: SFXN2 did not differ between groups, and SFXN1 is expressed in T cells and other cell types, so its bulk signal is mixed.
  
#### 4. Gene amplification does not explain it
High-level amplification in 1,070 TCGA breast tumors (cBioPortal): PHGDH 2.1% (23 tumors), PSPH 1.3%, MTHFD1L 1.1%, PSAT1 0.2% (2 tumors). The expression difference is present across nearly all Basal-like tumors, so amplification can account for at most a small fraction of it. Subtype-specific frequencies and low-level gains were not assessed.

#### 5. Confounder analysis
Basal vs Luminal B gap in the serine score after adjustment:
Adjustment	SCAN-B	TCGA
None	1.49 SD	1.45 SD
+ proliferation	1.36 (91%)	1.38 (96%)
+ hypoxia	1.30 (87%)	1.33 (92%)
+ MYC targets	1.20 (81%)	1.26 (87%)
+ ATF4 targets	1.14 (77%)	1.15 (79%)
+ 
Within Basal tumors, the joint model gave similar coefficients in both cohorts: ATF4 targets 0.286 (SCAN-B) and 0.285 (TCGA), proliferation 0.226 and 0.174, hypoxia not significant in either. The MTHFD2 and SHMT2 gaps lost more after proliferation adjustment (62% and 75% kept in SCAN-B), indicating that part of their elevation is proliferation-linked, while MTHFD1L kept 96%.

Glycolytic gene expression was also higher in Basal tumors, but only about a third of that gap remained after adjusting for proliferation and hypoxia, and there was no independent coupling between glycolysis and serine synthesis within Basal tumors. The data are therefore described as metabolic reprogramming and are not presented as evidence for a Warburg-type program.

#### 6. What tracks serine synthesis inside Basal tumors
In the WGCNA network built on SCAN-B Basal tumors, PHGDH, PSAT1, PSPH, MTHFD1L, MTHFD2, SHMT2, ATF4, ASNS and MKI67 fell in the same large module (1,094 genes), whose most central members were cell-cycle genes. SFXN1 and SFXN2 were unassigned.

After regressing out proliferation, the genes most associated with the serine score were an amino-acid stress-response set:

Gene	Partial rho, SCAN-B	Partial rho, TCGA
- ASNS	0.58	0.48
- CHAC1	0.41	0.52
- SHMT2	0.42	0.41
- MTHFD2	0.32	0.35
- ATF4	0.37	0.26
- GPT2	0.35	0.28
- MTHFD1L	0.12	0.16
- MKI67	-0.08	-0.12

Of the 40 genes with the highest SCAN-B values, 38 were positive in TCGA and 20 were above 0.2. In TCGA, aminoacyl-tRNA synthetases (GARS, SARS, MARS), SESN2, SLC7A5 and SLC3A2 also ranked near the top.

#### 7. Single-cell coupling of serine synthesis and the stress program
- The stress-response score was not specific to cancer cells: cancer cells did not score higher than myeloid cells, and plasmablasts and T cells scored higher.
- Inside cancer cells, the per-patient correlation between the serine and stress scores (proliferation removed) was higher in TNBC than in ER+ patients (median rho 0.13 vs 0.03, Mann-Whitney p = 0.021, n = 8 vs 9) and heterogeneous across tumors. The pooled regression gave an extra slope in TNBC of +0.20 (95% CI +0.06 to +0.34), but this estimate rests on 17 patients and its clustered standard errors are likely too small.
- Stress-response expression in non-tumor cell types did not correlate with the cancer-cell serine score across patients (|rho| up to 0.12 for T cells, myeloid cells, fibroblasts, endothelial and B cells; n = 15-20 patients).

### What did not replicate or did not hold
- FAO and immune activity. In TCGA Basal tumors, FAO correlated with an immune signature (ICR; rho 0.28-0.31) and with CD8 and M2 macrophage infiltration estimates. In SCAN-B (n = 360 Basal tumors) it did not (rho = 0.05, p = 0.33), although the ICR genes were coherent there (mean pairwise rho 0.73). This finding is unreplicated.
- Serine synthesis vs FAO: unrelated within Basal tumors in both cohorts (rho near 0).
- Serine synthesis vs immune score: a weak negative association disappeared after adjusting for stromal content in TCGA and was not significant in SCAN-B.
- Mitochondrial importers (SFXN1, SFXN2): lower in Basal in both bulk cohorts, but not confirmed in cancer cells at single-cell level.
- Amplification: tested and found too rare to explain the effect.

### Limitations
- All results are RNA expression (plus one copy-number frequency table). Nothing here measures serine production, flux, protein levels or localization.
- Gene-set scores are rough proxies. Small sets (3 to 10 genes) are noisy, and some genes sit near the detection floor in SCAN-B.
- Adjusting for programs that overlap with the subtype (proliferation, MYC, ATF4) can over- or under-correct.
- ATF4 target expression reflects stress signaling in general and does not identify its trigger. The serine synthesis genes are themselves regulated by ATF4 in several systems, so part of that association is expected.
- The single-cell dataset has 26 patients, only 4 PAM50 Basal-like (two with low-confidence calls from pseudobulk PAM50 on 8 samples). Single-cell results give direction and approximate size, not formal replication.
- DNA methylation, which prior work links to PSAT1 silencing in luminal tumors, was not analyzed here, so the mechanism behind the subtype difference is open.
- Several analyses were exploratory, and the number of comparisons run is larger than the number reported with adjusted p-values.
- Bulk tumor expression mixes cell types, so tumor purity and stromal content influence scores in all bulk analyses.

### Tools
- Python: pandas, NumPy, SciPy, statsmodels, Scanpy, AnnData, matplotlib
- R: WGCNA, genefu, org.Hs.eg.db, clusterProfiler, estimate
- Data sources: recount3 (TCGA-BRCA), GEO (GSE176078), SCAN-B, cBioPortal, MitoCarta3.0
Data and code availability


### Possible next steps
- DNA methylation of the serine synthesis gene promoters in TCGA-BRCA (for example PSAT1), as DNA-level support for the expression difference and a direct link to the prior work cited above.
- A larger single-cell breast cancer atlas, to replace the 4-patient Basal-like analysis and to test the stress-response coupling with more patients.
- Functional follow-up (isotope tracing or metabolomics) would be needed to say anything about serine flux.

### References
- Choi et al. 2022. Cell Reports (PSAT1 methylation and serine dependence in luminal breast tumors).
- Wu SZ et al. A single-cell and spatially resolved atlas of human breast cancers. Nature Genetics, 2021.




