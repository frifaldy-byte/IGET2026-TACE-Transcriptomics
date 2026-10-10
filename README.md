# Pathway-Constrained Transcriptomic Endotyping and Interpretable Machine Learning for Pretreatment TACE Response in Hepatocellular Carcinoma

**Conference:** IGET LIVE 2026  
**Research area:** Interventional Oncology — Embolotherapy  
**Study type:** Retrospective secondary transcriptomic analysis and computational model development  
**Dataset:** [GEO GSE104580](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104580)

## 1. Project Overview

Transarterial chemoembolization (TACE) is an established locoregional treatment for hepatocellular carcinoma (HCC). Treatment response varies among patients, motivating research into biological features that may help characterize response before intervention.

This project investigates a pathway-constrained computational framework integrating transcriptomic pathway activity, unsupervised molecular endotyping, statistical association testing, and interpretable machine learning. The objective is to identify pretreatment biological signals associated with TACE response and explore their potential for patient-level response stratification.

This is an exploratory computational study using publicly available transcriptomic data. The reported findings do not establish clinical validity or demonstrate that model-guided treatment improves patient outcomes.

## 2. Research Objectives

1. Quantify predefined biological pathway activity from pretreatment tumor transcriptomic profiles.
2. Investigate transcriptomic endotypes and evaluate their clustering stability.
3. Assess associations between pathway activity and observed TACE response.
4. Evaluate the discrimination, calibration, and interpretability of a regularized logistic regression model.
5. Identify methodological requirements for future independent validation.

## 3. Dataset and Study Population

The analysis uses the public Gene Expression Omnibus (GEO) dataset GSE104580.

The submitted notebook reports the following study population:

| Characteristic | Reported value |
|---|---:|
| Total specimens | 147 |
| Responders | 81 |
| Nonresponders | 66 |
| Hallmark pathways analyzed | 14 |
| Transcriptomic endotypes | 2 |

Probe-level expression measurements were mapped to gene-level expression using platform annotation. The resulting expression profiles were summarized into pathway-level features.

**Data source:** [NCBI GEO — GSE104580](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104580)

The response labels and sample counts should be verified against the original GEO sample metadata and associated publication before claiming independent reproduction. The original dataset should be obtained from GEO rather than redistributed without authorization.

## 4. Analytical Workflow

The analysis follows these stages:

1. **Data acquisition:** Obtain the transcriptomic dataset and inspect sample annotations.
2. **Expression processing:** Map probes to genes and prepare the expression matrix.
3. **Pathway scoring:** Estimate activity across 14 predefined Hallmark pathways using single-sample Gene Set Enrichment Analysis (ssGSEA).
4. **Transcriptomic endotyping:** Apply consensus clustering with bootstrap resampling.
5. **Cluster evaluation:** Assess stability using silhouette statistics and adjusted Rand indices.
6. **Pathway association testing:** Compare responders and nonresponders using Mann–Whitney tests, rank-biserial correlations, and false discovery rate (FDR) correction.
7. **Predictive modeling:** Fit L2-regularized logistic regression using an elastic-net hyperparameter-tuning framework.
8. **Model evaluation:** Examine discrimination, calibration, permutation performance, coefficient stability, decision curves, and leave-one-pathway-out sensitivity.
9. **Response stratification:** Summarize observed response rates across groups defined by out-of-fold predicted probabilities.

The accompanying Jupyter notebook contains the implemented computational workflow.

## 5. Statistical and Machine-Learning Methods

### 5.1 Pathway activity

Predefined Hallmark gene sets were used to summarize transcriptomic profiles into pathway-level features. This approach provides a biologically interpretable representation of the expression data.

### 5.2 Unsupervised endotyping

Consensus clustering with bootstrap resampling was used to investigate potential molecular subgroups. Clustering stability was assessed using adjusted Rand indices and silhouette statistics.

These clusters are exploratory molecular groupings and should not be interpreted as established clinical subtypes without independent replication.

### 5.3 Pathway association analysis

Mann–Whitney tests were used to compare pathway activity between response groups. Rank-biserial correlation was used to quantify association, and FDR correction was applied to account for multiple pathway comparisons.

The direction of an effect must be interpreted according to the group coding and effect-size convention implemented in the notebook.

### 5.4 Predictive modeling

The primary classifier was regularized logistic regression, with hyperparameter tuning performed within the repeated stratified cross-validation framework.

Reported performance measures include:

- Area under the receiver operating characteristic curve (AUROC).
- Area under the precision-recall curve (AUPRC).
- Brier score and calibration slope.
- Sensitivity, specificity, and balanced accuracy.
- Permutation testing.
- Bootstrap coefficient stability.
- Decision-curve analysis.
- Leave-one-pathway-out sensitivity analysis.

The primary classifier uses pathway-level features. Internal cross-validation is not equivalent to external validation in an independent patient cohort.

## 6. Results

**Important:** The values below are transcribed from the notebook outputs previously recorded for this project. They have not been independently reproduced from raw data during preparation of this README.

### 6.1 Transcriptomic endotypes

Two transcriptomic endotypes were reported, with a mean subsampling adjusted Rand index of 0.883.

| Endotype | Reported response rate |
|---|---:|
| Endotype 1 | 62.0% |
| Endotype 2 | 48.7% |

The difference in response rates was not statistically significant (P = 0.146). Therefore, these endotypes should not be described as independently established response classes.

### 6.2 Pathway associations

| Pathway | Rank-biserial correlation | FDR-adjusted P value |
|---|---:|---:|
| Glycolysis | -0.545 | 1.97 × 10^-7 |
| Oxidative phosphorylation | 0.282 | 9.36 × 10^-3 |

The reported results indicate associations between pathway activity and treatment response. Biological interpretation of the association direction requires confirmation of the response-group coding used in the analysis.

### 6.3 Predictive performance

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

These values describe internal model evaluation in the analyzed cohort. The confidence intervals and performance estimates should be interpreted cautiously until the full pipeline and uncertainty calculations have been rerun and checked.

### 6.4 Patient-level response stratification

The notebook reports response rates of 22.4%, 63.3%, and 79.6% across three groups formed from out-of-fold predicted probabilities (P = 3.53 × 10^-8).

This is an exploratory description of stratification within the analyzed cohort. It does not establish validated clinical thresholds or prove that the model can guide treatment decisions in other populations.

## 7. Robustness and Model Evaluation

The notebook includes several complementary evaluation procedures:

- Repeated stratified cross-validation with nested hyperparameter tuning for the primary classifier.
- AUROC and AUPRC for discrimination.
- Brier score and calibration slope for probabilistic prediction.
- Permutation testing.
- Bootstrap coefficient stability analysis.
- Decision-curve analysis.
- Leave-one-pathway-out sensitivity analysis.
- Out-of-fold patient-level probability stratification.

Some preprocessing and secondary sensitivity analyses were performed outside the complete cross-validation loop or used hyperparameters selected on the full dataset. These choices may lead to optimistic estimates and should be addressed in a refined analysis.

In particular, a rigorous permutation procedure should repeat the full model-selection process within each permutation. Confidence intervals should also be checked to determine whether they account for model fitting and selection uncertainty.

## 8. Repository Contents

The repository is intended to contain the following materials as they are added:

- Jupyter notebook containing the implemented analysis.
- Figures illustrating pathway associations, transcriptomic endotypes, model performance, and response stratification.
- Machine-readable summary tables.
- Documentation of data provenance and methodological decisions.
- Python dependency specifications and execution instructions.

Only files actually committed to the repository should be considered available. The original submission notebook should be preserved separately from revised versions.

## 9. Reproducibility

The analysis was developed using Python and Jupyter Notebook.

A reproducible release should document:

1. Python and package versions.
2. The exact GEO download and preprocessing procedures.
3. Platform annotation and probe-to-gene mapping rules.
4. Sample exclusions and response-label mapping.
5. Random seeds and cross-validation configuration.
6. The commands required to execute the notebook.
7. The provenance of all figures, tables, and reported metrics.

Uploading a notebook does not by itself demonstrate reproducibility. Reproduction requires running the documented workflow from a clean environment and checking the resulting outputs.

## 10. Limitations

Several limitations should be considered:

- The study uses retrospective public transcriptomic data.
- The reported analysis includes 147 specimens and requires validation in additional cohorts.
- Internal cross-validation does not establish external generalizability.
- Cohort-level preprocessing or imputation before cross-validation may introduce optimistic bias.
- Some secondary analyses do not repeat all model-selection steps within each resampling iteration.
- Predicted-probability groups are descriptive and are not validated clinical thresholds.
- Transcriptomic associations do not establish causality or demonstrate treatment benefit.
- Response-label mapping and reported performance should be independently checked against the original data and analysis code.

These limitations motivate further methodological refinement and independent evaluation.

## 11. Future Directions

Future work should prioritize:

1. Verifying sample annotations and response labels against the original GEO records and source publication.
2. Reassessing preprocessing and imputation within training folds where appropriate.
3. Repeating permutation tests with the full model-selection procedure.
4. Recalculating confidence intervals using a clearly documented uncertainty-estimation procedure.
5. Evaluating performance in an independent cohort.
6. Assessing whether transcriptomic features add value beyond established clinical predictors.

## 12. Clinical Interpretation

This project investigates whether pretreatment transcriptomic pathway features may help characterize biological variation associated with TACE response in HCC.

The framework remains a research-stage computational analysis. It is not a clinically validated prediction tool and should not be used to determine treatment eligibility or replace multidisciplinary clinical decision-making.

## 13. Data Availability

The primary dataset is publicly available through NCBI GEO:

- **GSE104580:** https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE104580

The notebook and supporting materials are made available through this repository as they are committed.

## 14. Research Integrity and AI-Assisted Coding

This repository documents the computational workflow, reported outputs, data source, and methodological limitations of the project.

Any AI-assisted coding or debugging should be disclosed in accordance with the relevant conference, institutional, and publication policies. Scientific claims and numerical results remain the responsibility of the researcher and should be verified against the executable analysis.

## 15. Citation

When referencing this project, cite the repository using its final URL and the specific version or commit used. Cite the original GEO-associated publication separately using the bibliographic information provided by NCBI GEO.

---

**Disclaimer:** This repository presents research-stage computational analyses. The reported findings do not establish prospective clinical effectiveness. Independent external validation is required before any clinical application.
