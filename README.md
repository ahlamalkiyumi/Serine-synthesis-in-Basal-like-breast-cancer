# Serine-synthesis-in-Basal-like-breast-cancer
replication, cell-type resolution and confounder analysis

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

*Prior work*. High expression of serine synthesis genes in basal-like breast cancer has been reported before. Choi et al. (2022, Cell Reports) reported that luminal breast tumors silence PSAT1 by promoter DNA methylation and consequently depend on exogenous serine. This repository does not repeat that work. It asks complementary questions at the population scale: whether the expression pattern replicates across large independent cohorts, which cells carry it, how much of it is explained by proliferation, hypoxia and stress programs, what it co-varies with inside Basal tumors, and whether gene amplification contributes.

### Questions
- Is serine synthesis higher in Basal-like tumors, and does it replicate across cohorts?
- Which cells carry the signal: cancer cells, immune cells or stroma?
- Is the difference explained by proliferation, hypoxia, MYC or ATF4 programs?
- Could gene amplification explain it?
- What co-varies with serine synthesis inside Basal tumors?
- Does it relate to fatty acid oxidation (FAO) or immune activity, the earlier TCGA finding that motivated the work?





