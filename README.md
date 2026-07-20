# Bi-modal-GBM-Thickness-distributions-in-diabetic-patients

Project Overview

This project focuses on the statistical characterization of Glomerular Basement Membrane (GBM) thickness distributions in patients with diabetes using Transmission Electron Microscopy (TEM) images and their corresponding segmentation masks.

The project builds upon the automated GBM segmentation and thickness estimation pipeline developed by Curti et al. The current dataset contains pre-segmented GBM images collected from 11 diabetic patients. The main objective is to analyze the distribution of individual GBM membrane thickness measurements and investigate the presence of uni-modal, bi-modal, or multi-modal patterns.

The analysis will provide an initial characterization of GBM thickness variability before extending the methodology to a larger dataset containing additional GBM components.

Research Objective

The primary objective of this project is to:

Characterize the distribution of GBM thickness measurements in diabetic patients and investigate whether the distributions exhibit significant bi-modal or multi-modal patterns.

The analysis will focus on:

Measuring the thickness of individual segmented GBM membrane components.
Investigating the variability of GBM thickness within and between patients.
Identifying possible uni-modal, bi-modal, or multi-modal distributions.
Characterizing the distribution of GBM thickness at patient and glomerulus levels.
Quantifying localized variations in GBM thickness.
Dataset

The preliminary dataset consists of segmented TEM images collected from 11 diabetic patients.

Each patient contains multiple views of the GBM, sampled from different glomeruli and slice positions. Each image may contain multiple membrane components. Therefore, individual segmented membrane components are treated as independent measurement units for the initial thickness characterization.

The hierarchical structure of the data can be represented as:

Patient
   └── Glomerulus
          └── TEM Image
                 └── Multiple GBM Membrane Components
                        └── Thickness Measurements

The dataset contains:

11 patients
Multiple glomeruli
More than 10 images per patient
Multiple segmented membrane components per image
Research Hypothesis

The underlying hypothesis is that GBM thickness in diabetic patients may exhibit considerable heterogeneity.

Since a single image may contain multiple membrane components with different structural characteristics, the distribution of GBM thickness may not follow a simple unimodal distribution.

The project investigates whether the observed thickness values exhibit:

A unimodal distribution
A bimodal distribution
A multimodal distribution
A prolonged right tail indicating localized membrane thickening
Methodology

The analysis workflow consists of the following stages:

Pre-Segmented TEM Images
          ↓
Segmentation Mask Processing
          ↓
Individual Membrane Component Identification
          ↓
Thickness Measurement
          ↓
Thickness Dataset Construction
          ↓
Exploratory Data Analysis
          ↓
Distribution Analysis
          ↓
Multi-Modality Analysis
          ↓
Patient and Glomerulus-Level Characterization
          ↓
Statistical Interpretation
Thickness Measurement

The project utilizes computer vision-based thickness estimation methods applied to the provided segmentation masks.

The general process includes:

Loading the segmentation mask.
Identifying individual membrane components.
Extracting the geometry of each membrane.
Applying distance-based thickness estimation.
Using skeletonization to obtain representative membrane centerlines.
Estimating local thickness values along the membrane.
Aggregating the measurements to obtain a thickness distribution for each membrane.

Each individual membrane component is treated as a separate measurement sample during the initial analysis.

Statistical Analysis

The extracted thickness measurements will be analyzed using exploratory and statistical methods, including:

Exploratory Analysis
Histograms
Kernel Density Estimation (KDE)
Boxplots
Violin plots
Empirical cumulative distribution functions
Descriptive Statistics
Mean
Median
Standard deviation
Variance
Interquartile range
Skewness
Kurtosis
Minimum and maximum thickness
Multi-Modality Analysis

Potential multi-modal patterns will be investigated using:

Gaussian Mixture Models (GMM)
Model selection criteria such as AIC and BIC
Statistical tests for multi-modality
Density-based analysis
Comparison of one-, two-, and multi-component distributions

The number of distribution components will be evaluated to determine whether the observed thickness distribution is better represented by a single distribution or multiple underlying sub-populations.

Hierarchical Analysis

Although individual membrane components are treated as independent samples for the initial thickness characterization, the data has an inherent hierarchical structure.

Measurements are associated with:

Patient → Glomerulus → Image → Membrane

Therefore, additional analyses will investigate variability at different levels:

Within individual images
Between images
Between glomeruli
Between patients

This will help determine whether observed thickness variations are primarily localized or consistent across individual patients.

Project Structure
Bi-modal-GBM-Thickness-distributions-in-diabetic-patients/
│
├── data/
│   ├── raw/
│   │   └── Original TEM images and segmentation masks
│   │
│   └── processed/
│       └── Extracted thickness measurements
│
├── notebooks/
│   ├── 01_dataset_exploration.ipynb
│   ├── 02_thickness_measurement.ipynb
│   ├── 03_exploratory_data_analysis.ipynb
│   ├── 04_multimodality_analysis.ipynb
│   └── 05_patient_glomerulus_analysis.ipynb
│
├── src/
│   ├── preprocessing/
│   ├── thickness_estimation/
│   ├── statistical_analysis/
│   └── visualization/
│
├── results/
│   ├── figures/
│   ├── statistical_summaries/
│   └── extracted_measurements/
│
├── requirements.txt
├── README.md
└── LICENSE
Expected Outcomes

The expected outcomes of this project include:

A complete dataset of GBM thickness measurements.
Statistical characterization of GBM thickness distributions.
Identification of possible bi-modal or multi-modal patterns.
Quantification of thickness variability among diabetic patients.
Comparison of thickness distributions between patients and glomeruli.
Identification of potential sub-populations of GBM membrane thicknesses.
Future Work

The next phase of the project will extend the analysis to a larger dataset containing additional segmented components of the GBM.

The future work will focus on:

Applying the analysis pipeline to a larger dataset.
Incorporating additional GBM components.
Improving the characterization of membrane structures.
Investigating relationships between different GBM components.
Developing more advanced statistical and computational models for GBM characterization.
Related Work

This project builds upon the automated GBM segmentation and thickness estimation pipeline developed by Curti et al.

The original work introduced an automated pipeline for:

GBM segmentation from TEM images.
Computer vision-based thickness estimation.
Deep learning-based image segmentation.
Automated analysis of GBM morphology.

The current project extends this work by focusing on the statistical characterization of the resulting GBM thickness distributions.

Acknowledgement

This project is conducted under the guidance of Professor Nico Curti and builds upon the existing GBM segmentation and thickness estimation methodology.
