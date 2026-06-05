# Dataset Exploration Report - EyeMotionID Project

**Prepared by:** Kavya Jain  
**Project:** EyeMotionID- Behavioral Biometric Authentication  
**Week:** 1 - Research and Environment Setup  
**Date:** June 2026  

---

## 1. Overview

This report documents the exploration of four datasets selected for the EyeMotionID behavioral biometric authentication project. Each dataset was explored in terms of folder structure, image count, resolution, class distribution, and suitability for the project tasks.

| Dataset            | Platform Used   | Status    |
|--------------------|-----------------|-----------|
| MRL Eye Dataset    | Local Jupyter   |  Explored |
| MPIIGaze           | Local Jupyter   |  Explored |
| OpenEDS (Subset 1) | Google Colab    |  Explored |
| EyeDentify         | Kaggle Notebook |  Explored |

---

## 2. Dataset 1 - MRL Eye Dataset

### 2.1 Source
- **Origin:** Kaggle - `akashshingha850/mrl-eye-dataset`
- **Storage:** Local laptop
- **Size:** ~500 MB

### 2.2 Folder Structure
```
MRL Eye Dataset/
└── data/
    ├── train/
    │   ├── awake/     <- open eye images
    │   └── sleepy/    <- closed eye images
    ├── val/
    │   ├── awake/
    │   └── sleepy/
    └── test/
        ├── awake/
        └── sleepy/
```

### 2.3 Image Statistics

| Split | Awake | Sleepy | Total |
|-------|-------|--------|-------|
| Train | ~25,770 | ~25,167 | ~50,937 |
| Val | ~8,591 | ~8,389 | ~16,980 |
| Test | ~8,591 | ~8,390 | ~16,981 |
| **Grand Total** | | | **~84,898** |

### 2.4 Image Properties
- **Format:** PNG
- **Resolution:** ~24x24 px to ~36x60 px (small cropped eye images)
- **Color:** Grayscale and RGB variants
- **Labels:** Binary - awake (open) / sleepy (closed)

### 2.5 Key Observations
- Dataset is well balanced - nearly equal awake and sleepy images in all splits
- Images captured under various lighting conditions (indoor, outdoor, low light)
- Subject identity labels not included - suitable for blink detection, not user identification
- File naming convention: `s{subject_id}_{frame_id}_{attributes}.png`

### 2.6 Use in Project
- **Week 2:** Blink detection module training
- **Week 2:** Eye Aspect Ratio (EAR) validation
- **Week 3:** Blink rhythm feature extraction

---

## 3. Dataset 2 - MPIIGaze

### 3.1 Source
- **Origin:** Kaggle - `4quant/eye-gaze`
- **Storage:** Local laptop
- **Size:** ~200 MB

### 3.2 Folder Structure
```
MPIIGAZE/
└── MPIIGaze/
    └── Data/
        └── Original/
            ├── p00/        <- participant 0
            │   ├── day01/
            │   │   ├── *.jpg
            │   │   └── annotation.txt
            │   └── day02/...
            ├── p01/
            └── ... p14/   <- 15 participants total
```

### 3.3 Image Statistics

| Property | Value |
|----------|-------|
| Total participants | 15 |
| Sessions per participant | Multiple days |
| Total images | ~45,000 |
| Images per participant | ~3,000 |

### 3.4 Image Properties
- **Format:** JPEG
- **Resolution:** 60x36 px (cropped eye region)
- **Color:** Grayscale
- **Labels:** Gaze direction (horizontal and vertical angles) in `annotation.txt`

### 3.5 Key Observations
- Captured using laptop webcams in everyday settings - realistic conditions
- Gaze annotation includes head pose and gaze direction vectors
- Organized by participant and day - suitable for temporal sequence creation
- Small image resolution requires upscaling for ResNet18 input (224x224)

### 3.6 Use in Project
- **Week 2:** Gaze tracking module development
- **Week 2:** Gaze transition analysis
- **Week 3:** CNN + LSTM training (15 users)

---

## 4. Dataset 3 - OpenEDS (Subset 1)

### 4.1 Source
- **Origin:** Facebook Research - OpenEDS Challenge 2019
- **Storage:** Google Drive
- **Size:** 1.49 GB (extracted)

### 4.2 Folder Structure
```
openeds/
└── openEDS/
    └── openEDS/
        ├── S_0/        <- subject 0
        │   ├── 0.png   <- eye image
        │   ├── 0.npy   <- segmentation mask
        │   ├── 1.png
        │   └── ...
        ├── S_1/
        └── ... S_124/  <- 27 subjects total
```

### 4.3 Image Statistics

| Property                   | Value |
|----------------------------|-------|
| Total subjects             | 27    |
| Total images (.png)        | 3,919 |
| Total masks (.npy)         | 4,129 |
| Average images per subject | 145   |

### 4.4 Image Properties
- **Format:** PNG (images) + NPY (segmentation masks)
- **Resolution:** 400x640 px
- **Color:** Grayscale
- **Labels:** Pixel-level segmentation - iris, pupil, sclera regions

### 4.5 Segmentation Mask Classes
| Class      | Label Value| Region         |
|------------|------------|----------------|
| Background | 0          | Outside eye    |
| Sclera     | 1          | White of eye   |
| Iris       | 2          | Colored region |
| Pupil      | 3          | Dark center    |

### 4.6 Key Observations
- Each `.png` image has a corresponding `.npy` mask file
- Pixel-level labels allow precise pupil region extraction
- High resolution images - suitable for detailed pupil movement analysis
- Captured using VR headset cameras - slightly different from standard webcam

### 4.7 Use in Project
- **Week 2:** Pupil region extraction using segmentation masks
- **Week 3:** Eye region feature extraction for CNN
- **Week 3:** Pupil movement trajectory calculation

---

## 5. Dataset 4 - EyeDentify

### 5.1 Source
- **Origin:** Kaggle - `vijuls/pupildiameterdatasets`
- **Storage:** Kaggle (accessed via Kaggle Notebooks)
- **Size:** ~5-8 GB (accessed directly on Kaggle - no local storage needed)

### 5.2 Folder Structure
```
eyedentify/
├── left_eyes/
│   ├── 1/              <- subject 1
│   │   ├── 1/          <- session 1
│   │   │   ├── frame_01.png
│   │   │   ├── frame_02.png
│   │   │   └── ...
│   │   └── 2/ ...
│   └── 2/ ... 51/
└── right_eyes/
    └── (same structure)
```

### 5.3 Image Statistics

| Property | Value |
|----------|-------|
| Total subjects | 51 |
| Sessions per subject | 50 |
| Left eye images | 212,073 |
| Right eye images | 212,073 |
| **Total images** | **424,146** |
| Average images per subject | ~4,160 |

### 5.4 Image Properties
- **Format:** PNG (frame sequences)
- **Naming:** `frame_01.png`, `frame_02.png`, ...
- **Labels:** Pupil diameter per frame
- **Eyes:** Left and right eye captured separately

### 5.5 Key Observations
- Largest dataset in the project - 424,146 images across 51 subjects
- Sequential frame naming enables direct temporal sequence creation
- 50 sessions per subject captures behavioral variation over time
- Left and right eye images available - can use either or both for training
- Suitable for user identification with 51 distinct subjects

### 5.6 Use in Project
- **Week 2:** Pupil movement extraction
- **Week 3:** Primary dataset for CNN + LSTM user identification training
- **Week 3:** User identification experiments (51-class classification)

---

## 6. Dataset Comparison Summary

| Property     | MRL Eye         | MPIIGaze      | OpenEDS          | EyeDentify      |
|--------------|-----------------|---------------|------------------|-----------------|
| Subjects     | Multiple        | 15            | 27               | **51**          |
| Total Images | 84,898          | ~45,000       | 3,919            | **424,146**     |
| Resolution   | 24x24-36x60     | 60x36         | 400x640          | Variable        |
| Labels       | Open/Closed     | Gaze angles   | Pixel masks      | Pupil diameter  |
| Storage      | 500 MB          | 200 MB        | 1.49 GB          | 5-8 GB (Kaggle) |
| Platform     | Laptop          | Laptop        | Google Drive     | Kaggle          |
| Best for     | Blink detection | Gaze tracking | Pupil extraction | User ID         |

---

## 7. Plots Generated

| Plot                     | Dataset    | Description                         | Location         |
|--------------------------|------------|-------------------------------------|------------------|
| `mrl_samples.png`        | MRL Eye    | 2x4 grid of awake/sleepy samples    | `results/plots/` |
| `mrl_distribution.png`   | MRL Eye    | Class distribution bar chart        | `results/plots/` |
| `mpii_samples.png`       | MPIIGaze   | 2x4 grid of participant eye images  | `results/plots/` |
| `openeds_samples.png`    | OpenEDS    | 2x4 grid of subject eye images      | `results/plots/` |
| `openeds_mask.png`       | OpenEDS    | Image + segmentation mask + overlay | `results/plots/` |
| `eyedentify_samples.png` | EyeDentify | 2x4 grid of left eye frames         | `results/plots/` |

---

## 8. Notebooks Used

| Notebook                             | Platform     | Datasets Covered           |
|--------------------------------------|--------------|----------------------------|
| `Week1_Dataset_Exploration.ipynb`    | Google Colab | MRL Eye, MPIIGaze, OpenEDS |
| `Week1_EyeDentify_Exploration.ipynb` | Kaggle       | EyeDentify                 |

---

## 9. Conclusion

All four datasets have been successfully explored and are confirmed suitable for the EyeMotionID project. The dataset combination covers all behavioral biometric features required:

- **Blink rhythm** -> MRL Eye Dataset 
- **Gaze transitions** -> MPIIGaze 
- **Pupil movement** -> OpenEDS 
- **User identification** -> EyeDentify (51 users) 

The datasets are stored across three platforms (laptop, Google Drive, Kaggle) to manage storage constraints while maintaining easy access during Week 2 and Week 3 development.

---

*Report prepared as part of Week 1 deliverables for the EyeMotionID 30-day internship project.*
