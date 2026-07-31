# Quantitative Analysis of GBM Thickness Distribution and Structural Heterogeneity in Diabetic Patients Using Statistical Modelling

# Quantitative Characterization of Bi-Modal Glomerular Basement Membrane (GBM) Thickness Distributions in Diabetic Patients

[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Domain](https://img.shields.io/badge/Domain-Renal%20Pathology%20%7C%20Electron%20Microscopy-red.svg)]()

## Overview

This project implements an automated image-processing and multi-level statistical analysis pipeline for the quantitative characterization of Glomerular Basement Membrane (GBM) thickness from Transmission Electron Microscopy (TEM) images. 

The framework transitions from isolated segmented membrane measurements to a multi-level hierarchical statistical analysis, explicitly distinguishing between point-wise measurements, individual membrane fragments, glomeruli, and patient-level aggregates.

---

## 📖 Key Features

* **Continuous Medial-Axis Distance Measurement:** Computes perpendicular Euclidean distance along membrane skeletons rather than relying on sparse manual point selections.
* **Hierarchical Biological Aggregation:** Aggregates individual membrane components into their parent glomerulus and patient IDs to avoid sample size bias.
* **Multimodality & Bimodality Testing:** Evaluates thickness distributions using **Hartigan's Dip Test** to test for non-unimodality ($p < 0.05$).
* **Gaussian Mixture Model (GMM) Characterization:** Fits 1-, 2-, and 3-component GMMs selected via the Bayesian Information Criterion (BIC) to identify thickening sub-populations.
* **Glomerulus Thickness Stratification:** Classifies glomeruli into quantitative thickness groups (Thin, Intermediate, Thick).
* **Dimensionality Reduction (PCA):** Maps multidimensional glomerular morphometrics into principal component space to identify structural heterogeneity.

---

## 🏗 Hierarchical Data Structure

A central feature of this pipeline is that measurements are not treated as independent across patients. The analysis strictly adheres to a 4-level biological hierarchy:

```text
Point-wise Thickness Measurements (Level 1)
                  │
                  ▼
      Membrane Components (Level 2)
                  │
                  ▼
         Glomeruli (Level 3)
                  │
                  ▼
          Patients (Level 4) ```

















