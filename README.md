# Quantitative Evaluation & Multi-Modal Analysis of Glomerular Basement Membrane (GBM) Thickness Distributions in Diabetic Patients

## 📌 Project Context & Problem Statement
Glomerular Basement Membrane (GBM) thickening is a hallmark pathological feature of Diabetic Kidney Disease (DKD). However, regional variations and multi-modal thickness trends across continuous membrane segments are often obscured in standard single-point estimations. 

This project aims to implement an automated image processing and statistical modeling pipeline to quantify local orthogonal thickness profiles from **Transmission Electron Microscopy (TEM)** images and evaluate structural thickness distributions (specifically identifying bi-modal or multi-modal patterns).

---

## 🔬 Scientific & Technical Methodology

### 1. True Physical Scale Calibration
To guarantee precise physical measurements in nanometers ($\text{nm}$):
* Scale bar calibration was conducted directly on high-magnification ($10,500\times$, $80\text{ kV}$) TEM micrographs.
* A $1\ \mu\text{m}$ ($1000\text{ nm}$) scale bar corresponds to $1010\text{ pixels}$, establishing an exact scale factor of **$0.9901\text{ nm/pixel}$** ($1\text{ pixel} \approx 1\text{ nm}$).

### 2. Automated Image Processing Pipeline (`gbm_analysis.py`)
* **Preprocessing & Binarization:** Converts TEM segment masks into cleaned binary structures using morphological closing (`scipy.ndimage`).
* **Centerline Extraction:** Computes the membrane skeleton (`skimage.morphology.skeletonize`) to trace the medial line.
* **Orthogonal Distance Transform:** Applies Exact Euclidean Distance Transform (`scipy.ndimage.distance_transform_edt`) to derive true local orthogonal thicknesses along every skeleton point ($2 \times \text{distance} \times 0.9901\text{ nm}$).
* **Noise Filtering:** Applies physical validation boundaries ($10.0\text{ nm} - 1500.0\text{ nm}$) to eliminate boundary artifacts.

---

## 📊 Current Progress & Key Findings

1. **Automated Feature Extraction:** Successfully processed TEM image datasets, extracting continuous thickness measurements along thousands of evaluation points.
2. **Global Distribution Profile:** Initial calibrated global distributions demonstrate a right-skewed profile with secondary shoulders/peaks, supporting the hypothesis of heterogeneous, localized membrane thickening in diabetic conditions.
3. **Structured Classification Framework:**
   * **Thin Membrane Segment:** $< 50.0\text{ nm}$
   * **Normal Membrane Segment:** $50.0\text{ nm} - 100.0\text{ nm}$
   * **Thickened Membrane Segment:** $> 100.0\text{ nm}$

---


