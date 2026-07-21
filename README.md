# Glomerular Basement Membrane (GBM) Thickness Analysis

This repository contains the Python implementation for the automated evaluation, true-scale calibration, and distribution analysis of Glomerular Basement Membrane (GBM) thickness in diabetic patients.

---

## 📌 Project Overview & Objectives
The primary objective of this project is to quantitatively analyze GBM thickness profiles from Transmission Electron Microscopy (TEM) images to investigate structural variations—specifically looking for **bi-modal or multi-modal thickness trends** in diabetic conditions.

### Key Methodology:
* **True Physical Calibration:** Evaluates true nanometer-scale dimensions by calibrating pixel density directly against image scale bars ($1\ \mu\text{m} = 1010\text{ pixels} \implies 0.9901\text{ nm/pixel}$).
* **Fine-Grained Segmentation:** Measures orthogonal thickness continuously along the membrane's centerline, treating each continuous component as an independent physical sample.
* **Image Processing Pipeline:** Utilizes binary thresholding, morphological closing (`scipy.ndimage`), Distance Transform (`distance_transform_edt`), and Skeletonization (`skimage.morphology`) to ensure unbiased orthogonal thickness extraction.

---

## 🚀 Current Implementation & Progress

### 1. Calibrated Processing Pipeline (`gbm_analysis.py`)
* **Scale Calibration:** Updated the pixel-to-nanometer factor to **$0.9901\text{ nm/pixel}$** based on TEM scale bar analysis ($1\ \text{pixel} \approx 1\text{ nm}$).
* **Noise & Artifact Filtering:** Implemented validation bounds ($10.0\text{ nm} - 1500.0\text{ nm}$) to remove boundary artifacts and micro-noise.
* **Automated Data Extraction:** Automatically parses `Patient_ID` and `Glomerulus` metadata from filenames and outputs summary metrics (`Mean`, `Median`, `Std Dev`) to CSV.

### 2. Preliminary Findings & Distribution Analysis
* **Global Distribution:** Calibrated thickness profiles exhibit a characteristic right-skewed log-normal shape.
* **Multi-Modal Trends:** The calibrated global histogram reveals distinct secondary peaks and shoulders, supporting the hypothesis of localized structural membrane thickening in diabetic samples.
* **Calibrated Stratification Scheme:**
  * **Thin:** $< 50.0\text{ nm}$
  * **Normal:** $50.0\text{ nm} - 100.0\text{ nm}$
  * **Thick:** $> 100.0\text{ nm}$

---

## 📁 Repository Structure

```text
├── images/                        # Raw TEM binary masks / dataset
├── gbm_analysis.py                # Main analysis & calibration pipeline
├── gbm_thickness_results.csv      # Exported statistical summary per component
└── global_gbm_distribution.png    # Output global calibrated histogram
