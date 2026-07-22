# Quantitative Analysis of Glomerular Basement Membrane (GBM) Thickness Distributions in Diabetic Patients 

## 🎯 What Does This Project Mean? (Project Context)
Glomerular Basement Membrane (GBM) thickening is a key structural indicator of Diabetic Kidney Disease (DKD). Standard clinical evaluations often rely on basic manual spot-checks, which miss local variations across the membrane. 

**The core goal of this project** is to automatically measure the orthogonal thickness of the GBM along its entire length from Transmission Electron Microscopy (TEM) images. By doing so, we aim to statistically test whether diabetic GBM thickening follows a **bi-modal or multi-modal distribution** (having multiple distinct peaks/thickness populations) rather than a uniform single-peak distribution.

---

## ✅ What Has Been Done So Far

### 1. Physical Scale Calibration
* Calibrated the pixel density against high-magnification TEM scale bars ($1\ \mu\text{m} = 1010\text{ pixels}$).
* Established the true resolution scale: **$1\text{ pixel} \approx 0.9901\text{ nm}$**.

### 2. Automated Image Processing Pipeline (`gbm_analysis.py`)
* **Preprocessing:** Cleaned binary TEM masks using morphological closing operations.
* **Skeletonization:** Extracted the centerline of the membrane structures.
* **Orthogonal Measurement:** Applied Euclidean Distance Transform along the skeleton to compute accurate perpendicular thickness values in nanometers.
* **Data Processing:** Automatically extracted patient and glomerulus metadata from image filenames and exported thickness statistics to CSV.

### 3. Preliminary Analysis
* Observed that calibrated thickness distributions show a right-skewed profile with potential secondary peaks/shoulders, supporting the hypothesis of localized membrane thickening.

---
![Global GBM Thickness Distribution](global_gbm_distribution.PNG)

## 🔮 What Needs To Be Done Next (Tasks Ahead)

- [ ] **Patient-Level Data Aggregation:** Group thickness statistics per `Patient_ID` and specific glomeruli to separate within-patient variability from between-patient differences.
- [ ] **Statistical Multi-Modality Testing:** Apply Kernel Density Estimation (KDE) and Gaussian Mixture Model (GMM) fitting to mathematically confirm bi-modal or multi-modal trends.
- [ ] **Dataset Expansion:** Run the calibrated pipeline across the remaining cohort of patient TEM images.
- [ ] **Data Visualization:** Generate per-patient comparison charts and heatmaps for thesis documentation.

---

