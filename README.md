# Quantitative Analysis of GBM Thickness Distribution and Structural Heterogeneity in Diabetic Patients Using Statistical Modelling

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Domain](https://img.shields.io/badge/Domain-Renal%20Pathology%20%7C%20Electron%20Microscopy-red.svg)]()

An automated image-processing and multi-level statistical framework for quantifying **Glomerular Basement Membrane (GBM)** thickness from Transmission Electron Microscopy (TEM) images. This pipeline aggregates continuous point-wise distance measurements across a nested biological hierarchy (**Point → Membrane → Glomerulus → Patient**) to detect structural heterogeneity, fit Gaussian Mixture Models (GMM), and perform PCA dimensionality reduction.

---

## 📖 Key Features

* **Continuous Medial-Axis Thickness Extraction:** Computes perpendicular Euclidean distance measurements along membrane center-lines rather than relying on sparse manual annotations.
* **Hierarchical Biological Aggregation:** Groups individual membrane fragments into their originating glomerulus and parent patient ID, preventing sample size bias.
* **Multimodality & Bimodality Testing:** Implements **Hartigan's Dip Test** to formally evaluate statistical non-unimodality ($p < 0.05$).
* **Sub-population Decomposition:** Fits 1-, 2-, and 3-component **Gaussian Mixture Models (GMM)** selected via Bayesian Information Criterion (BIC) to identify thickening sub-populations.
* **Multivariate Stratification & PCA:** Conducts Principal Component Analysis (PCA) and tertile stratification across glomerular morphometrics.

---

## 🏗 Hierarchical Data Workflow

Unlike traditional per-image analyses, this framework structures measurements across four nested biological levels to ensure statistical integrity:

```text
      TEM Binary Segmentation Masks (refined_masks/)
                            │
                            ▼
           Center-line Skeletonization & Scale Calibration
                            │
                            ▼
         [Level 1] Point-wise Raw Distance Measurements
                            │
                            ▼
         [Level 2] Membrane Component Summary Statistics
                            │
                            ▼
         [Level 3] Glomerulus-Level Aggregation & Stratification
                            │
                            ▼
         [Level 4] Patient-Level Population Statistics & GMM

Reference

The thickness estimation method follows:

Curti et al.

"Automated evaluation of glomerular basement membrane thickness using deep learning segmentation and medial axis analysis."

GitHub repository:

https://github.com/Nico-Curti/glomerular-basement-membrane 

Author

Name: Ishini Langappuli
Institution: University of Bologna
             Master Degree in Physics
















