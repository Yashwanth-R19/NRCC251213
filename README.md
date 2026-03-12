#  Automatic Cloud & Shadow Mask Generation

### from Resourcesat-2 / Resourcesat-2A LISS-IV Satellite Images

**ISRO NRSC – Bhoonidhi Challenge Submission**
**Registration ID:** `NRCC251213`

---

##  Project Overview

Clouds and their shadows significantly reduce the usability of satellite imagery in Earth observation tasks such as land-use classification, agriculture monitoring, and disaster management.

This project presents a **deep-learning-based pipeline** to automatically detect and generate **pixel-level cloud and shadow masks** from **Resourcesat-2 / Resourcesat-2A LISS-IV imagery**, which lacks the SWIR band—making traditional threshold-based methods unreliable.

We designed and trained a **U-Net semantic segmentation model with a ResNet encoder** to classify each pixel as:

* `0` → Background (No cloud / shadow)
* `1` → Cloud
* `2` → Shadow

The system supports **end-to-end preprocessing, inference, post-processing, and GIS-ready outputs**, including GeoTIFF masks and ESRI shapefiles.

---

##  Team

* **Yashwanth R.** — SSN College of Engineering
* **Nitheesh K.** — Vellore Institute of Technology
* **Oviya T. S.** — SSN College of Engineering
* **Poojashree S.** — Chennai Institute of Technology

---

##  Dataset

* **Sensor:** LISS-IV
* **Satellites:** Resourcesat-2, Resourcesat-2A
* **Bands Used:**

  * BAND-2 (Green)
  * BAND-3 (Red)
  * BAND-4 (NIR)

### Training Data

* 20 full satellite scenes
* Manually and semi-automatically labeled using **QGIS**
* Mask labels:

  * `0` – Background
  * `1` – Cloud
  * `2` – Shadow

### Evaluation Data

* 10 unseen test scenes provided by NRSC

---

##  Pipeline Overview

###  Preprocessing

* Digital Number → Radiance → TOA Reflectance conversion
* Sun-angle correction using ephemeris calculations
* Normalization to `[0,1]`
* Export as **Cloud Optimized GeoTIFF (COG)**

###  Dataset Labeling

* Cloud masks generated via reflectance thresholding
* Shadow regions manually digitized as polygons in QGIS
* Combined into single multi-class raster masks

###  Model Architecture

* **U-Net** for semantic segmentation
* **ResNet encoder** for strong feature extraction
* Pixel-wise multi-class classification

###  Training Strategy

* Framework: **PyTorch**
* Loss: `CrossEntropyLoss` with class weighting
* Optimizer: Adam
* Scheduler: ReduceLROnPlateau
* Augmentations via **Albumentations**

###  Inference & Post-Processing

* Full-scene inference (no tiling)
* Mask raster generation
* Vectorization into cloud/shadow shapefiles
* GIS-ready outputs

---

##  Model Performance (Best Model)

| Metric    | Value      |
| --------- | ---------- |
| Accuracy  | **95.57%** |
| F1-Score  | **0.977**  |
| Precision | **1.00**   |
| Recall    | **0.956**  |

> Cloud detection performs strongly; shadow detection remains more challenging due to terrain and water-body confusion.

---

##  Repository Structure(Refer to releases for the submission)

```text
.
├── report.pdf
│   └── Detailed technical report (methodology, model, results)
│
├── requirements.txt
│   └── Python dependencies
│
├── Training_Labelled_Data.zip
│   └── 16 manually labelled GeoTIFF training masks
│
├── NRCC251213_Inference_code.zip
│   ├── models/
│   │   └── Trained .pth model weights
│   ├── inference & preprocessing scripts
│   └── helper.txt (step-by-step execution guide)
│
├── NRCC251213_Training_Inference.csv
│   └── Epoch-wise training and validation metrics
│
├── Model.zip
│   └── Best performing trained model
│
├── Masks.zip
│   ├── *.cld  (cloud masks)
│   ├── *.sdw  (shadow masks)
│   └── Dataset folders:
│       ├── cloudshapes.zip
│       ├── shadowshapes.zip
│       ├── mask.tif
│       ├── *.cld
│       └── *.sdw
```

---

##  How to Run Inference

###  Setup Environment

```bash
pip install -r requirements.txt
```

###  Run Inference Pipeline

Follow the step-by-step instructions inside:

```text
NRCC251213_Inference_code/helper.txt
```

###  (Optional) Launch GUI

```bash
streamlit run app.py
```

This opens a browser-based interface for fast visualization and export.

---

##  Key Learnings

* Full-scene inference significantly outperforms tiled inference
* Cloud detection is robust even without SWIR bands
* Shadow detection is harder due to low-reflectance confusion
* Manual labeling remains a bottleneck in remote sensing ML

---

##  Future Improvements

* Expand labeled dataset across seasons and terrains
* Improve shadow detection with contextual constraints
* Automate shadow labeling in QGIS
* Apply morphological post-processing
* Add real-time overlay preview in UI

---

##  Acknowledgements

We sincerely thank **ISRO NRSC** and the **Bhoonidhi portal** for organizing this challenge and providing high-quality satellite datasets and documentation. This project was a valuable hands-on introduction to **remote sensing, geospatial data processing, and deep learning**.

---

##  License

This repository is released **for academic and research purposes only**.

