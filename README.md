# Quantitative Analysis of GBM Thickness Distribution and Structural Heterogeneity in Diabetic Patients Using Statistical Modelling

# Quantitative Characterization of Bi-Modal Glomerular Basement Membrane (GBM) Thickness Distributions in Diabetic Patients

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Domain](https://img.shields.io/badge/Domain-Renal%20Pathology%20%7C%20Electron%20Microscopy-red.svg)]()

## Overview

This project implements an automated image-processing and multi-level statistical analysis pipeline for the quantitative characterization of Glomerular Basement Membrane (GBM) thickness from Transmission Electron Microscopy (TEM) images.

The framework progresses from continuous point-wise membrane thickness measurements to membrane-component, glomerulus-level, and patient-level statistical analyses.

A key objective is to characterize the distribution of GBM thickness and investigate structural heterogeneity, including potential bimodal or multimodal thickness distributions in diabetic patients.

The pipeline follows a hierarchical biological structure:

```text
Point-wise Thickness Measurements
              │
              ▼
     Membrane Components
              │
              ▼
          Glomeruli
              │
              ▼
           Patients
```

This hierarchical organization prevents patients with a larger number of segmented membrane components from being artificially overrepresented in population-level analyses.

---

## Key Features

* **Continuous medial-axis thickness measurement**
  Computes perpendicular Euclidean distances along membrane skeletons rather than relying on sparse manual measurements.

* **Spatial calibration**
  Converts pixel-based measurements into physical GBM thickness values in nanometers using image-specific calibration factors.

* **Hierarchical biological aggregation**
  Aggregates measurements from membrane components to glomeruli and then to patients.

* **Multimodality analysis**
  Uses Hartigan's Dip Test to evaluate whether patient-level GBM thickness distributions significantly deviate from unimodality.

* **Gaussian Mixture Model (GMM) analysis**
  Fits 1-, 2-, and 3-component Gaussian mixture models and selects the preferred model using the Bayesian Information Criterion (BIC).

* **Glomerulus-level statistical analysis**
  Calculates quantitative thickness statistics for individual glomeruli.

* **Patient-level statistical analysis**
  Compares glomerular thickness distributions across patients while maintaining the biological hierarchy.

* **Glomerulus thickness stratification**
  Classifies glomeruli into quantitative thickness groups such as Thin, Intermediate, and Thick.

* **Principal Component Analysis (PCA)**
  Reduces multidimensional glomerular morphometric measurements into principal components to investigate structural variation and heterogeneity.

* **Automated visualization**
  Produces box plots, GMM distributions, stratification plots, and PCA visualizations for interpretation.

---

# Hierarchical Data Structure

The analysis is organized into four biological levels:

```text
Level 1
Point-wise Thickness Measurements
          │
          ▼
Level 2
Membrane Components
          │
          ▼
Level 3
Glomeruli
          │
          ▼
Level 4
Patients
```

### Level 1 — Point-wise measurements

Individual thickness measurements obtained continuously along the medial axis of each segmented GBM membrane component.

### Level 2 — Membrane components

Point-wise measurements are summarized for each connected membrane component.

### Level 3 — Glomeruli

Multiple membrane components belonging to the same glomerulus are aggregated to obtain glomerulus-level measurements.

### Level 4 — Patients

Multiple glomeruli belonging to the same patient are summarized to obtain patient-level statistics.

This structure is important because measurements from the same patient are not statistically independent.

For example, a patient containing many segmented membrane fragments should not automatically contribute more weight to a population-level analysis simply because more fragments were available.

---

# Repository Structure

The recommended repository structure is:

```text
GBM_Analysis/
│
├── 01_thickness_extraction.py
├── 02_multimodality_GMM.py
├── 03_glomerulus_analysis.py
├── 04_global_statistics.py
├── 05_global_patient_statistics.py
├── 06_glomerulus_stratification.py
├── 07_PCA_dimensionality_reduction.py
│
├── refined_masks/
│   └── Binary TEM segmentation masks (.png)
│
├── scale_calibration.csv
│
├── gbm_thickness_summary.csv
├── gbm_raw_thickness.csv
├── gbm_thickness_analysis.xlsx
│
├── Glomerulus_analysis/
│   ├── gbm_glomerulus_summary.csv
│   ├── gbm_patient_summary.csv
│   ├── global_glomerulus_boxplot.png
│   ├── patient_glomerulus_boxplot.png
│   ├── glomerulus_stratification.png
│   ├── PCA_results.csv
│   └── PCA_plots/
│
└── GMM_plots/
    ├── GMM_patient_01-24.png
    ├── ...
    └── gbm_multimodality_summary.csv
```

---

# Detailed Workflow

The complete analysis pipeline is:

```text
TEM Images
    │
    ▼
Segmented GBM Masks
    │
    ▼
Scale Calibration
    │
    ▼
Continuous GBM Thickness Measurement
    │
    ├──────────────► Raw Point-wise Measurements
    │
    ▼
Membrane-Level Summary
    │
    ▼
Glomerulus-Level Summary
    │
    ├──────────────► Global Glomerulus Statistics
    │
    ▼
Patient-Level Summary
    │
    ├──────────────► Patient Statistics
    │
    ├──────────────► Box Plots
    │
    └──────────────► Multimodality / GMM Analysis
    │
    ▼
Glomerulus Thickness Stratification
    │
    ▼
PCA Dimensionality Reduction
    │
    ▼
Patient- and Glomerulus-Level Interpretation
```

---

# 1. Continuous GBM Thickness Extraction

### Script

```text
01_thickness_extraction.py
```

This is the first stage of the pipeline.

For each segmented GBM membrane component, the script:

1. Loads the binary segmentation mask.
2. Fills internal holes within the segmented membrane.
3. Removes very small segmentation artifacts.
4. Identifies connected membrane components.
5. Computes the medial-axis skeleton.
6. Calculates the Euclidean distance transform.
7. Determines the local radius from the medial axis to the membrane boundary.
8. Converts radius into full membrane thickness.
9. Applies the image-specific spatial calibration.
10. Saves continuous point-wise and component-level thickness measurements.

The full membrane thickness is calculated as:

```text
Thickness = 2 × Radius
```

After spatial calibration:

```text
Thickness (nm) =
Thickness (pixels) × nm_per_pixel
```

The result is a continuous series of thickness measurements along the membrane rather than a small number of manually selected points.

---

# 2. Multimodality and GMM Analysis

### Script

```text
02_multimodality_GMM.py
```

This stage investigates whether patient-level GBM thickness distributions are unimodal or contain multiple sub-populations.

## Hartigan's Dip Test

Hartigan's Dip Test is used to evaluate deviation from unimodality.

The interpretation is:

```text
p < 0.05
    │
    ▼
Evidence against unimodality
    │
    ▼
Possible multimodal distribution
```

A non-significant result does not prove that the distribution is perfectly unimodal; it indicates that the test does not provide sufficient evidence against unimodality at the selected significance level.

---

## Gaussian Mixture Models

Gaussian Mixture Models are fitted using:

```text
k = 1
k = 2
k = 3
```

components.

The models are compared using the Bayesian Information Criterion (BIC).

The preferred model is the one with the lowest BIC:

```text
Lower BIC
   =
Preferred model
```

The BIC is calculated as:

```text
BIC = k ln(n) - 2 ln(L)
```

where:

* `k` = number of estimated model parameters
* `n` = number of observations
* `L` = maximized likelihood of the model

A two-component GMM can be useful for identifying a potential thin and thick GBM sub-population.

However, the identification of biological sub-populations should be interpreted together with the statistical results rather than based only on visual appearance.

---

# 3. Glomerulus-Level Analysis

### Script

```text
03_glomerulus_analysis.py
```

This stage aggregates membrane-level measurements according to their corresponding glomerulus.

For each glomerulus, the analysis can include:

* Mean GBM thickness
* Median GBM thickness
* Standard deviation
* Minimum thickness
* Maximum thickness
* Number of membrane components
* Number of measurements
* Inter-component variability

The glomerulus becomes an important intermediate biological unit between individual membrane measurements and patients.

---

# 4. Global Glomerulus Statistics

### Script

```text
04_global_statistics.py
```

This stage summarizes the complete glomerulus-level population.

The analysis provides population-wide statistical descriptions of GBM thickness across all analyzed glomeruli.

Typical outputs include:

* Overall distribution
* Mean thickness
* Median thickness
* Standard deviation
* Quartiles
* Minimum and maximum values
* Number of analyzed glomeruli
* Distribution plots

This provides a global view of GBM structural variation before focusing on individual patient differences.

---

# 5. Global Patient Statistics

### Script

```text
05_global_patient_statistics.py
```

This stage moves the analysis from the glomerulus level to the patient level.

Each patient contributes information through their glomerular measurements rather than through every individual point measurement.

The analysis can include:

* Patient-level mean thickness
* Patient-level median thickness
* Inter-glomerular variability
* Number of glomeruli per patient
* Patient-level box plots
* Comparison of GBM thickness distributions between patients

This step is particularly important for preventing patients with different numbers of glomeruli or membrane components from dominating the analysis.

---

# 6. Glomerulus Thickness Stratification

### Script

```text
06_glomerulus_stratification.py
```

This stage divides glomeruli into quantitative thickness groups.

The current approach uses thickness tertiles:

```text
Thin
Intermediate
Thick
```

The purpose is to provide an interpretable classification of glomerular GBM thickness.

Conceptually:

```text
Lowest thickness
       │
       ▼
     THIN
       │
       ▼
 INTERMEDIATE
       │
       ▼
     THICK
       │
       ▼
Highest thickness
```

The classification is based on the observed distribution of glomerular thickness values.

This stratification allows subsequent analyses to investigate whether structural features differ between relatively thin and thick GBM groups.

---

# 7. PCA Dimensionality Reduction

### Script

```text
07_PCA_dimensionality_reduction.py
```

Principal Component Analysis (PCA) is used to reduce multiple correlated glomerular morphometric variables into a smaller number of principal components.

The PCA transforms the original feature space into new orthogonal axes:

```text
Original features
       │
       ▼
      PCA
       │
       ├──► PC1
       ├──► PC2
       ├──► PC3
       └──► ...
```

## Important interpretation

The x-axis labelled:

```text
Principal Components
```

does **not** represent patient IDs.

For example:

```text
PC1
PC2
PC3
```

are mathematical combinations of the original input features.

PC1 explains the largest amount of variance in the dataset.

PC2 explains the next largest amount of variance, subject to being orthogonal to PC1.

---

## Explained Variance

The PCA analysis reports:

* Individual explained variance
* Cumulative explained variance
* PCA scores
* PCA loading vectors

The cumulative explained variance increases as additional principal components are included.

For example:

```text
PC1     → 40%
PC1+PC2 → 65%
PC1+PC2+PC3 → 80%
```

Therefore, the cumulative variance curve should generally move upward as more principal components are added.

---

# Input Data

## Segmentation Masks

The `refined_masks/` directory contains binary TEM segmentation masks.

The filenames follow a structured naming convention such as:

```text
PATIENT-ID_GLOMERULUS-ID_IMAGE.png
```

Example:

```text
01-24_03_1g.png
```

where:

```text
Patient ID     = 01-24
Glomerulus ID  = 03_1g
```

The exact naming convention should remain consistent because patient and glomerulus identifiers are extracted from the filenames.

---

# Spatial Calibration

Spatial calibration information is stored in:

```text
scale_calibration.csv
```

Each image is associated with an `nm_per_pixel` conversion factor.

The conversion is:

```text
Thickness (nm) =
Thickness (pixels) × nm_per_pixel
```

This converts image-derived distances into physically meaningful GBM thickness measurements.

---

# Output Files

| File                            | Biological Level | Description                                                       |
| ------------------------------- | ---------------- | ----------------------------------------------------------------- |
| `gbm_raw_thickness.csv`         | Point-wise       | Continuous thickness measurements along the medial axis           |
| `gbm_thickness_summary.csv`     | Membrane         | Per-component thickness statistics                                |
| `gbm_glomerulus_summary.csv`    | Glomerulus       | Aggregated measurements for individual glomeruli                  |
| `gbm_patient_summary.csv`       | Patient          | Patient-level thickness and variability statistics                |
| `gbm_multimodality_summary.csv` | Patient          | Hartigan's Dip Test and GMM results                               |
| `PCA_results.csv`               | Multivariate     | PCA scores, explained variance, cumulative variance, and loadings |

---

# Main Figures

The pipeline generates several figures for interpretation.

## Global Glomerulus Box Plot

```text
global_glomerulus_boxplot.png
```

Shows the distribution of GBM thickness across the analyzed glomeruli.

## Patient-Level Box Plot

```text
patient_glomerulus_boxplot.png
```

Shows the distribution of glomerular thickness for individual patients.

Each patient can therefore be compared while preserving the glomerulus-level structure of the data.

## Glomerulus Stratification

```text
glomerulus_stratification.png
```

Visualizes the classification of glomeruli into:

```text
Thin
Intermediate
Thick
```

## PCA Figures

PCA visualizations are stored in:

```text
PCA_plots/
```

These figures can include:

* Explained variance
* Cumulative explained variance
* PCA score plots
* PCA loading plots

## GMM Figures

Patient-level GMM plots are stored in:

```text
GMM_plots/
```

These figures show the observed thickness distribution together with the fitted Gaussian mixture components.

---

# Installation

The project requires Python 3.8 or newer.

Install the required packages using:

```bash
pip install numpy pandas scipy scikit-image scikit-learn matplotlib seaborn diptest openpyxl
```

---

# Execution Guide

Run the scripts sequentially from the project root directory.

## Step 1 — Thickness Extraction

```bash
python 01_thickness_extraction.py
```

Extracts continuous GBM thickness measurements from the segmentation masks.

---

## Step 2 — Multimodality and GMM Analysis

```bash
python 02_multimodality_GMM.py
```

Performs:

* Hartigan's Dip Test
* 1-component GMM
* 2-component GMM
* 3-component GMM
* BIC comparison
* Patient-level distribution analysis

---

## Step 3 — Glomerulus Analysis

```bash
python 03_glomerulus_analysis.py
```

Aggregates membrane components into glomerulus-level measurements.

---

## Step 4 — Global Statistics

```bash
python 04_global_statistics.py
```

Calculates population-wide glomerulus-level statistics and generates global visualizations.

---

## Step 5 — Patient Statistics

```bash
python 05_global_patient_statistics.py
```

Performs patient-level comparisons and generates patient-level box plots.

---

## Step 6 — Glomerulus Stratification

```bash
python 06_glomerulus_stratification.py
```

Classifies glomeruli into Thin, Intermediate, and Thick groups using thickness tertiles.

---

## Step 7 — PCA

```bash
python 07_PCA_dimensionality_reduction.py
```

Performs PCA-based dimensionality reduction and generates:

* Explained variance plots
* Cumulative explained variance plots
* PCA score plots
* PCA loading information

---

# Complete Pipeline

The complete analysis can therefore be summarized as:

```text
01_thickness_extraction.py
          │
          ▼
Continuous GBM Thickness
          │
          ▼
02_multimodality_GMM.py
          │
          ├── Hartigan's Dip Test
          └── GMM / BIC
          │
          ▼
03_glomerulus_analysis.py
          │
          ▼
Glomerulus-Level Measurements
          │
          ▼
04_global_statistics.py
          │
          ▼
Global Glomerulus Statistics
          │
          ▼
05_global_patient_statistics.py
          │
          ▼
Patient-Level Statistics
          │
          ▼
06_glomerulus_stratification.py
          │
          ▼
Thin / Intermediate / Thick
          │
          ▼
07_PCA_dimensionality_reduction.py
          │
          ▼
Principal Component Analysis
          │
          ▼
Structural Heterogeneity Interpretation
```

---

# Scientific Interpretation

The pipeline is designed to investigate GBM structural heterogeneity at multiple biological levels.

The analysis begins with continuous measurements extracted directly from segmented TEM images. These measurements are then progressively summarized into membrane components, glomeruli, and patients.

This allows the analysis to distinguish between:

```text
Measurement-level variation
        ↓
Membrane-level variation
        ↓
Glomerular variation
        ↓
Patient-level variation
```

The multimodality analysis investigates whether GBM thickness distributions can be described by a single population or whether multiple statistical sub-populations may be present.

GMM analysis provides a quantitative way of characterizing these potential sub-populations, while glomerulus-level stratification provides an interpretable classification based on relative thickness.

Finally, PCA summarizes multidimensional glomerular morphology and provides a lower-dimensional representation of structural heterogeneity.

Together, these analyses provide a quantitative framework for studying GBM thickness variation in diabetic renal pathology using transmission electron microscopy.

---

# Reference 

Curti et al. "Automated evaluation of glomerular basement membrane thickness using deep learning segmentation and medial axis analysis." 

GitHub repository: https://github.com/Nico-Curti/glomerular-basement-membrane 

Author Name: Ishini Langappuli 
Institution: University of Bologna Master Degree in Physics

















