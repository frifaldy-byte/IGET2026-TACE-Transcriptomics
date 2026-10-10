# Pathway-Constrained Transcriptomic Endotyping and Interpretable Machine Learning for Pretreatment TACE Response in Hepatocellular Carcinoma

**Conference:** IGET LIVE 2026  
**Research category:** Embolotherapy  
**Research domain:** Interventional Oncology, Computational Biology, and Machine Learning  
**Study design:** Secondary analysis of publicly available transcriptomic data  
**Primary dataset:** [GEO GSE104580](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104580)  
**Programming environment:** Python and Jupyter Notebook

---

## 1. Project Overview

Transarterial chemoembolization (TACE) is an established locoregional treatment for hepatocellular carcinoma (HCC). However, treatment response varies among patients, highlighting the importance of understanding the biological features associated with treatment outcomes.

This project investigates a pathway-constrained computational framework that integrates transcriptomic pathway scoring, unsupervised molecular endotyping, statistical association analysis, and interpretable machine learning to characterize pretreatment biological signals associated with TACE response.

Using publicly available tumor transcriptomic data, the analysis examines whether predefined biological pathway activities can help characterize response heterogeneity and support patient-level response stratification.

The framework combines pathway-level biological interpretation with predictive modeling and internal performance evaluation. Rather than relying exclusively on individual gene expression measurements, it summarizes expression profiles into biologically defined pathway features.

The project is intended to support further research into transcriptomic response heterogeneity in HCC. Its findings remain exploratory and require methodological verification and independent external validation before any clinical application.

## 2. Background and Rationale

HCC exhibits substantial molecular heterogeneity, which may contribute to differences in treatment response. Transcriptomic profiling provides an opportunity to investigate biological programs associated with these differences.

However, high-dimensional gene expression data present several analytical challenges:

- A large number of molecular measurements relative to the available sample size.
- Correlated expression patterns across genes and biological pathways.
- Potential heterogeneity among patients with apparently similar clinical outcomes.
- The need to distinguish predictive performance from biological interpretation.
- The risk of optimistic performance estimates when preprocessing or model selection is not fully contained within validation procedures.

Pathway-level analysis offers a biologically interpretable representation of transcriptomic data. Unsupervised clustering can then be used to explore molecular heterogeneity, while regularized logistic regression provides a relatively interpretable approach to response prediction.

This project combines these complementary approaches within a computational workflow designed to investigate pathway-associated response patterns and evaluate their potential predictive relevance.

## 3. Research Objectives

The study addresses five objectives:

1. To quantify predefined biological pathway activity from pretreatment HCC transcriptomic profiles.
2. To investigate transcriptomic endotypes and assess their clustering stability.
3. To identify pathways associated with observed TACE response using statistical association testing.
4. To evaluate the discrimination and calibration of a regularized logistic regression model.
5. To investigate patient-level response stratification and identify methodological priorities for future validation.

## 4. Dataset and Study Population

### 4.1 Data source

The analysis uses the publicly available Gene Expression Omnibus (GEO) dataset GSE104580.

**Dataset link:**  
https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104580

The dataset provides the transcriptomic data used in the computational analysis. The notebook reports 147 HCC specimens classified according to treatment response.

### 4.2 Reported study population

| Characteristic | Reported value |
|---|---:|
| Total specimens | 147 |
| Responders | 81 |
| Nonresponders | 66 |
| Predefined Hallmark pathways | 14 |
| Transcriptomic endotypes | 2 |

These values are transcribed from the analysis outputs previously recorded in the notebook. The sample annotations, response labels, and associated publication should be checked against the original GEO records before claiming independent reproduction.

### 4.3 Data preparation

The implemented workflow includes probe-to-gene mapping using platform annotation, expression-data preparation, and pathway-level feature construction.

The original deposited expression data may already have undergone preprocessing. The exact preprocessing history, including normalization and any batch correction, should therefore be documented from the original data source rather than assumed from the downstream analysis.

The original GEO data should be obtained directly from the source and used in accordance with its applicable access conditions and terms. The repository does not need to redistribute the original dataset.

## 5. Analytical Framework

The computational workflow comprises the following stages:

1. Data acquisition and sample annotation review.
2. Probe-to-gene expression mapping and expression-matrix preparation.
3. Single-sample pathway activity scoring.
4. Unsupervised transcriptomic endotyping.
5. Statistical pathway association analysis.
6. Regularized logistic regression and hyperparameter tuning.
7. Internal predictive performance evaluation.
8. Robustness and sensitivity analyses.
9. Patient-level response probability stratification.

The following sections describe the principal analytical components.

## 6. Methods

### 6.1 Transcriptomic preprocessing

Expression measurements were mapped from probe-level to gene-level representation using platform annotation.

The analysis notebook also includes filtering and missing-value handling during preparation of the expression matrix and pathway-level features.

For rigorous predictive validation, preprocessing operations that estimate parameters from the cohort should be fitted using the training data only and then applied to the corresponding validation data. Preprocessing performed across the full cohort before cross-validation may introduce optimistic bias, even when the preprocessing does not directly use outcome labels.

### 6.2 Pathway activity estimation

Single-sample Gene Set Enrichment Analysis (ssGSEA) was used to summarize gene expression profiles into pathway-level activity scores.

The analysis considered 14 predefined Hallmark pathways. This feature representation was intended to reduce dimensionality while retaining biologically interpretable information.

The resulting pathway scores served as inputs for downstream association analysis and predictive modeling.

### 6.3 Unsupervised transcriptomic endotyping

Unsupervised consensus clustering with bootstrap resampling was used to investigate potential transcriptomic endotypes.

Cluster stability was examined using:

- Silhouette statistics.
- Adjusted Rand indices.
- Subsampling-based stability assessment.

The resulting clusters represent exploratory transcriptomic groupings. Their existence does not establish clinically distinct subtypes, and any clinical interpretation requires independent replication.

### 6.4 Statistical pathway association analysis

Pathway activity was compared between responders and nonresponders using Mann–Whitney tests.

Rank-biserial correlation was used to quantify the direction and magnitude of the association. False discovery rate (FDR) correction was applied to account for multiple pathway comparisons.

The direction of each association must be interpreted according to the group coding and effect-size convention implemented in the notebook. Statistical association alone does not establish a causal relationship between a pathway and treatment response.

### 6.5 Predictive modeling

The primary predictive model was L2-regularized logistic regression within an elastic-net hyperparameter-tuning framework.

The notebook implemented repeated stratified cross-validation with an outer five-fold split repeated ten times. Hyperparameter selection was performed within the cross-validation framework for the primary classifier.

The pathway-level representation was used as the model input. No claim is made that the resulting model has been validated in an independent clinical cohort.

### 6.6 Model performance evaluation

The reported evaluation included the following metrics:

- Area under the receiver operating characteristic curve (AUROC).
- Area under the precision-recall curve (AUPRC).
- Brier score.
- Calibration slope.
- Sensitivity and specificity.
- Balanced accuracy.

AUROC summarizes discrimination across classification thresholds, while AUPRC evaluates precision-recall performance. The Brier score measures the accuracy of probabilistic predictions, and the calibration slope provides information about the relationship between predicted probabilities and observed outcomes.

These metrics assess different aspects of model performance and should be interpreted together.

### 6.7 Robustness and sensitivity analyses

The notebook additionally includes:

- Permutation testing.
- Bootstrap coefficient stability analysis.
- Decision-curve analysis.
- Leave-one-pathway-out sensitivity analysis.
- Patient-level stratification using out-of-fold predicted probabilities.

Some secondary analyses use hyperparameters selected on the full dataset rather than repeating the complete model-selection procedure within every resampling iteration. These analyses should therefore be interpreted as exploratory.

For a rigorous permutation test, the complete model-selection process should be repeated for each permuted outcome. Confidence intervals should also be checked to establish whether they account for model fitting and selection uncertainty.

### 6.8 Patient-level probability stratification

Out-of-fold predicted probabilities were used to group patients into three response-probability strata.

This analysis examines whether observed response rates differ across groups defined by model predictions. The resulting groups are descriptive and should not be interpreted as validated clinical thresholds.

Their generalizability and potential clinical utility require evaluation in independent data.

## 7. Results

**Reporting note:** The numerical values in this section were transcribed from the analysis outputs previously recorded in the notebook. They have not been independently reproduced from the raw dataset during preparation of this README.

### 7.1 Transcriptomic endotypes

The analysis identified two transcriptomic endotypes, with a reported mean subsampling adjusted Rand index of 0.883.

| Endotype | Reported response rate |
|---|---:|
| Endotype 1 | 62.0% |
| Endotype 2 | 48.7% |

The difference in response rates was not statistically significant (P = 0.146).

These findings suggest that the identified transcriptomic groupings should be treated as exploratory molecular endotypes rather than established response classes.

### 7.2 Pathway associations

| Pathway | Rank-biserial correlation | FDR-adjusted P value |
|---|---:|---:|
| Glycolysis | -0.545 | 1.97 × 10^-7 |
| Oxidative phosphorylation | 0.282 | 9.36 × 10^-3 |

Glycolysis showed a reported rank-biserial correlation of -0.545, while oxidative phosphorylation showed a correlation of 0.282.

Both associations were reported as statistically significant after FDR correction.

The biological direction of these associations should be confirmed against the exact response-group coding and effect-size calculation in the notebook before assigning either pathway a specific interpretation in terms of response or nonresponse.

### 7.3 Predictive model performance

| Metric | Reported result |
|---|---:|
| AUROC | 0.784 |
| AUROC 95% confidence interval | 0.706–0.856 |
| AUPRC | 0.788 |
| AUPRC 95% confidence interval | 0.693–0.878 |
| Brier score | 0.187 |
| Calibration slope | 1.109 |
| Sensitivity | 79.0% |
| Specificity | 72.7% |
| Balanced accuracy | 75.9% |

The reported internal evaluation indicates discriminatory performance within the analyzed cohort.

However, the confidence intervals and performance estimates remain provisional until the complete pipeline, preprocessing strategy, and uncertainty calculations have been rerun and checked. These values should not be interpreted as evidence of external generalizability or clinical readiness.

### 7.4 Patient-level response stratification

The notebook reports the following observed response rates across three groups defined by out-of-fold predicted probabilities:

| Probability group | Reported response rate |
|---|---:|
| Group 1 | 22.4% |
| Group 2 | 63.3% |
| Group 3 | 79.6% |

The reported difference across groups had P = 3.53 × 10^-8.

This finding describes response-rate differences within the analyzed cohort. It does not establish clinically actionable probability thresholds or demonstrate that model-guided treatment selection improves patient outcomes.

## 8. Interpretation of Findings

The analysis explores the relationship between pathway-level transcriptomic features and observed TACE response in HCC.

Three observations are of particular interest:

1. The clustering analysis identified two transcriptomic endotypes with a reported stability measure of 0.883, although their response rates were not significantly different.
2. Glycolysis and oxidative phosphorylation showed reported associations with treatment response after FDR correction.
3. The regularized logistic regression model demonstrated reported internal discrimination, while the out-of-fold probability groups showed differences in observed response rates.

Together, these findings motivate further investigation of pathway-level biological features as candidate markers of response heterogeneity.

They do not establish causality, demonstrate prospective predictive performance, or support immediate clinical implementation.

## 9. Reproducibility and Computational Environment

The analysis was developed using Python and Jupyter Notebook.

A complete reproducibility release should document:

- Python version and package versions.
- Exact dataset retrieval procedure.
- Platform annotation and probe-to-gene mapping rules.
- Expression preprocessing and missing-value handling.
- Pathway definitions and scoring configuration.
- Response-label mapping and sample exclusions.
- Random seeds and cross-validation settings.
- Model hyperparameter search space.
- The commands needed to execute the notebook from a clean environment.
- The provenance of reported metrics, tables, and figures.

The notebook is provided as the primary record of the implemented analysis. A successful upload alone does not demonstrate reproducibility; the workflow must be rerun and the outputs compared with the reported results.

## 10. Repository Structure

The repository is intended to organize the analysis and supporting materials as follows:

```text
IGET2026-TACE-Transcriptomics/
├── README.md
├── .gitignore
├── notebooks/
│   ├── IGET2026_submission_original.ipynb
│   └── IGET2026_revised.ipynb
├── figures/
├── results/
├── docs/
└── requirements.txt
