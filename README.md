# Bi-modal GBM Thickness Distributions in Diabetic Patients

## Overview

This project presents an automated image analysis pipeline for quantifying Glomerular Basement Membrane (GBM) thickness from Transmission Electron Microscopy (TEM) images of diabetic patients. The pipeline combines image processing techniques, morphometric thickness measurement, and statistical modelling to investigate whether GBM thickness distributions are unimodal or multimodal across patients.

Local GBM thickness is estimated from manually refined segmentation masks using skeletonization and the Euclidean Distance Transform. Patient-specific thickness distributions are subsequently analysed using Hartigan's Dip Test and Gaussian Mixture Models (GMMs), with model selection based on the Bayesian Information Criterion (BIC) and Akaike Information Criterion (AIC).

The main objective is to characterize the variability of GBM thickness and investigate whether diabetic GBM thickness distributions show **unimodal or multimodal (bi-modal) behaviour**.

Connected component analysis is performed to identify individual GBM segments. Each connected membrane segment is treated as an independent measurement unit, following the methodology proposed by Curti et al.

---

# Dataset

The dataset consists of TEM images collected from **11 diabetic patients**.

Each patient contains multiple GBM images sampled from different glomeruli and positions.

Patient IDs:
01-24,
02-24,
03-24,
04-23,
05-24,
06-24,
07-25,
08-25,
09-24,
10-24,
11-24

For each image:

- Original TEM image
- Corresponding segmentation mask
- Scale information

are provided.

---

# Analysis Workflow

The complete workflow is:

```text
Raw TEM Images
        │
        ▼
Manual / Refined GBM Masks
        │
        ▼
Medial Axis Transform
        │
        ▼
Euclidean Distance Transform
        │
        ▼
Thickness Measurement (nm)
        │
        ▼
gbm_thickness_summary.csv
        │
        ▼
Per-patient Statistical Analysis
        │
        ├── Hartigan's Dip Test
        ├── Gaussian Mixture Models (1–3 components)
        ├── AIC/BIC Model Comparison
        └── Best Model Selection
        │
        ▼
Outputs
    ├── GMM plots for each patient
    ├── GMM_all_patients_summary.csv
    └── Statistical interpretation
```

---

# 1. Scale Calibration

File: 01_scale_calibration.py

Purpose:

The physical scale of each TEM image is different.

Therefore, the scale bar of each image is detected automatically and converted into: nanometer / pixel

The generated file: scale_calibration.csv

contains:

| Column | Description |
|---|---|
| image_name | TEM image name |
| scale_bar_px | detected scale bar length in pixels |
| nm_per_pixel | physical resolution |

These values are used during thickness conversion.

---

# 2. GBM Thickness Extraction

File: 02_gbm_thickness_extraction.py

Purpose:

Measure GBM thickness from already segmented masks.

The method follows Curti et al.

## Processing steps:

### Binary mask preparation

The segmentation mask is converted into a binary membrane mask.

### Connected component analysis

Each separated membrane component is considered as an independent sample.

### Medial axis extraction

The centreline of each segmented GBM component is extracted using the Medial Axis Transform (skimage.morphology.medial_axis). Unlike simple skeletonization, the medial axis preserves the distance of each centreline pixel to the nearest membrane boundary, making it well suited for local thickness estimation.

### Thickness estimation

The Euclidean Distance Transform (EDT) is computed simultaneously with the medial axis. For each pixel on the medial axis, the EDT provides the shortest distance to the membrane boundary, corresponding to the local membrane radius. Local GBM thickness is then calculated as:

Thickness is calculated as: Thickness (nm) = 2 × EDT × nm_per_pixel

Outputs: gbm_thickness_summary.csv

Contains one row per membrane component.

Example parameters:

- Mean thickness
- Median thickness
- Standard deviation
- Minimum thickness
- Maximum thickness

Raw measurements: gbm_raw_thickness.csv

contains thickness values along the whole membrane skeleton.

---

# 3. Thickness Distribution Analysis

The extracted membrane thickness values are analysed per patient.

The median thickness of each membrane component is considered as one independent measurement.

Example:
Membrane 1 -> 320 nm
Membrane 2 -> 420 nm
Membrane 3 -> 650 nm
...

Two approaches are used:

## Hartigan's Dip Test

Tests whether the distribution is:

- Unimodal
- Multimodal


Interpretation: p-value < 0.05

indicates multimodal behaviour

---

## Gaussian Mixture Model (GMM)

Gaussian Mixture Models with one, two and three Gaussian components are fitted to each patient's thickness distribution.

The best model is selected using:

- Bayesian Information Criterion (BIC)
- Akaike Information Criterion (AIC)


A lower BIC indicates better model fitting.

---

# 5. GMM Visualization

File: 04_plot_gmm_fits.ipynb

Each generated figure includes:

• Observed thickness histogram
• Kernel Density Estimate (KDE)
• Overall fitted Gaussian Mixture Model
• Individual Gaussian components
• Estimated Gaussian means
• Dip Test statistics
• BIC and AIC values

Example output: gmm_fit_07-25.png

---

# Results

The analysis produces quantitative GBM thickness measurements for every segmented membrane component and evaluates each patient's thickness distribution for evidence of multimodality using Hartigan's Dip Test and Gaussian Mixture Models.

## Thickness Results
gbm_thickness_summary.csv

Contains:

- Patient ID
- Image name
- Membrane component ID
- Median thickness
- Mean thickness
- Standard Deviation of thickness
- Minimum thickness
- Maximum thickness
- Number of measurement
- Resolution in nm per pixel


## Multimodality Results
gbm_multimodality_summary.csv

Contains:

- Dip test p-value
- Multimodality classification
- Best GMM component number
- Estimated thickness peaks
- BIC
- AIC
- Gaussian peaks (1,2,3)
- Gaussian weights
- Multimodality strength classification


## Visualization Outputs

Generated figures:
GMM_patient_01-24.png
GMM_patient_02-24.png
GMM_patient_03-24.png
GMM_patient_04-23.png
GMM_patient_05-24.png
GMM_patient_06-24.png
GMM_patient_07-25.png
GMM_patient_08-25.png
GMM_patient_09-24.png
GMM_patient_10-24.png
GMM_patient_11-24.png
---

## Repository Structure

```text
Bi-modal-GBM-Thickness-Distributions-in-Diabetic-Patients/
│
├── data/
│   ├── TEM_images/                    # Original TEM images
│   ├── refined_masks/                 # Refined binary GBM segmentation masks
│   └── calibration/                   # Image scale information
│
├── results/
│   ├── scale_calibration.csv          # Image-specific calibration (nm/pixel)
│   ├── gbm_raw_thickness.csv          
│   ├── gbm_thickness_summary.csv      
│   ├── gbm_thickness_analysis.csv
|   ├── gbm_multimodality_summary.csv
|   ├── GMM_all_patients_summary.csv   # Dip Test and GMM summary for all patients
|   └── GMM_plots/
│       ├── GMM_patient_01-24.png
│       ├── GMM_patient_02-24.png
│       ├── ...
│       └── GMM_patient_11-24.png
│
├── scripts/
│   ├── 01_scale_calibration.ipynb
│   │   └── Automatic extraction of image-specific scale calibration (nm/pixel)
│   │
│   ├── 02_gbm_thickness_extraction.ipynb
│   │   └── Medial axis extraction and GBM thickness measurement
│   │
│   ├── 03_gbm_thickness_distribution.ipynb
│   │   └── Hartigan's Dip Test, GMM fitting and BIC/AIC model selection
│   │
│   └── 04_plot_gmm_fits.py
│       └── Generates GMM visualizations for every patient
|
├── README.md

```

# Requirements

Python packages:
numpy
pandas
opencv-python
scipy
scikit-image
scikit-learn
diptest
matplotlib

# How to Run

Run scripts in order:

Step 1
01_scale_calibration.ipynb

Step 2
02_gbm_thickness_extraction.ipynb

Step 3
03_gbm_thickness_distribution.ipynb

Step 4
04_plot_gmm_fits.ipynb

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
















