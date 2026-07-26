# Bi-modal GBM Thickness Distributions in Diabetic Patients

## Overview

This project investigates the distribution of Glomerular Basement Membrane (GBM) thickness in diabetic patients using Transmission Electron Microscopy (TEM) images.

The main objective is to characterize the variability of GBM thickness and investigate whether diabetic GBM thickness distributions show **unimodal or multimodal (bi-modal) behaviour**.

Each segmented membrane component is considered as an independent sample, following the methodology proposed by Curti et al.

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
TEM Images
|
|
v
Scale Calibration
(automatic scale bar detection)
|
|
v
GBM Segmentation Masks
|
|
v
Thickness Extraction
(Medial Axis + Distance Transform)
|
|
v
Thickness Distribution Analysis
|
|
v
Multimodality Testing
(Hartigan Dip Test + Gaussian Mixture Model)
|
|
v
Visualization
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

The centre line of each membrane is obtained using: skimage.morphology.medial_axis

### Thickness estimation

The distance transform gives the local radius.

Thickness is calculated as: Thickness = 2 × distance_transform × nm_per_pixel

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

Gaussian Mixture Models with:
1 components
2 components
3 components 
are fitted.

The best model is selected using:

- Bayesian Information Criterion (BIC)
- Akaike Information Criterion (AIC)


A lower BIC indicates better model fitting.

---

# 5. GMM Visualization

File: 04_plot_gmm.py

This script shows:

- Observed thickness histogram
- Gaussian components
- Estimated peaks

Example output: gmm_fit_07-25.png



---

# Results

## Thickness Results
gbm_thickness_summary.csv

Contains:

- Patient ID
- Image name
- Membrane component ID
- Median thickness
- Mean thickness


## Multimodality Results
gbm_multimodality_summary.csv

Contains:

- Dip test p-value
- Multimodality classification
- Best GMM component number
- Estimated thickness peaks


## Visualization Outputs

Generated figures:
gbm_median_boxplot.png

gbm_kde_distribution.png

gmm_fit_patientID.png

---

## Repository Structure

```text
Bi-modal-GBM-Thickness-distributions-in-diabetic-patients/

│
├── 01_scale_calibration.py
│   └── Automatic extraction of image-specific scale calibration (nm/pixel)
│
├── 02_gbm_thickness_extractor.py
│   └── GBM component separation, medial axis extraction, and thickness measurement
│
├── 03_thickness_distribution_analysis.py
│   └── Patient-wise thickness distribution visualization
│
├── 04_multimodality_analysis.py
│   └── Hartigan's Dip Test and Gaussian Mixture Model (GMM) analysis
│
├── 05_gmm_visualization.py
│   └── Visualization of GMM peak decomposition
│
├── scale_calibration.csv
│   └── Image-specific nm/pixel calibration values
│
├── gbm_thickness_summary.csv
│   └── Mean, median, and statistical thickness values for each membrane component
│
├── gbm_raw_thickness.csv
│   └── Individual thickness measurements along the membrane medial axis
│
├── gbm_multimodality_summary.csv
│   └── Statistical results of multimodality analysis
│
├── figures/
│   ├── gbm_distribution_boxplot.png
│   ├── gbm_kde_distribution.png
│   └── gmm_fit_examples.png
│
└── README.md
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
seaborn

Install:

```bash
pip install numpy pandas opencv-python scipy scikit-image scikit-learn diptest matplotlib seaborn
```

# How to Run

Run scripts in order:

Step 1
python 01_scale_calibration.py

Step 2
python 02_gbm_thickness_extraction.py

Step 3
python 03_multimodality_analysis.py

Step 4
python 04_plot_gmm.py

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
















