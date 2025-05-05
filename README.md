# dMRI Ensemble Visualization Directory

This repository documents the structure and content of the dMRI ensemble model visualization outputs for multiple sclerosis (MS) diagnosis.

---

## 📁 Directory Structure

```
BASE_DIR
│
├── DTI/
│   ├── M0067/
│   │   ├── 3DFLAIR_CE.nii
│   │   ├── ad_dti.nii.gz
│   │   ├── fa_dti.nii.gz
│   │   ├── md_dti.nii.gz
│   │   ├── rd_dti.nii.gz
│   │   ├── non_brain_mask.nii.gz
│   │   ├── wm_mask.nii.gz
│   │   ├── Salient.nii.gz
│   │   └── Normalized_Salient.nii.gz
│
├── SMI/
│   └── ...
│
├── WDKI/
│   └── ...
│
├── dti.txt
├── smi.txt
├── wdki.txt
│
├── prediction_DTI.csv
├── prediction_SMI.csv
└── prediction_WDKI.csv
```

---

## 📄 File Descriptions

### 1. Text Files (`dti.txt`, `smi.txt`, `wdki.txt`)

Each text file provides a qualitative description of the corresponding modality, including examples grouped by prediction correctness and saliency characteristics:

* **Typical MS case**: True Positive — correctly classified MS cases with strong model attention (i.e., high `weighted_prob_sum` > 100).
* **Atypical Non-MS case**: False Positive — incorrectly predicted as MS, despite being non-MS.
* **Typical Non-MS case**: True Negative — correctly classified non-MS with little or no false activation.
* **Atypical MS case**: False Negative — missed MS diagnoses, often due to subtle or atypical features.

Samples in each group are ordered by `weighted_prob_sum`, which reflects the overall activation level in the saliency map. Higher values appear earlier, indicating stronger model attention.

---

### 2. CSV Files (`prediction_*.csv`)

Each CSV file contains detailed prediction results from the ensemble model.

| Column Name         | Description                                                            |
| ------------------- | ---------------------------------------------------------------------- |
| `m_id`              | Unique patient/study identifier                                        |
| `Age`               | Patient age                                                            |
| `Sex`               | Patient sex                                                            |
| `ms`                | Ground truth MS label (1 = MS, 0 = Non-MS)                             |
| `ms_prob`           | Predicted MS probability, computed as `weighted_prob_sum / sa_map_sum` |
| `accurate`          | Prediction correctness (True if `ms_prob > 0.5` matches label)         |
| `weighted_prob_sum` | Total voxel-wise probability sum from the saliency map                 |
| `sa_map_sum`        | Total saliency map attention sum                                       |

---

### 3. NIfTI Files (`.nii` / `.nii.gz`)

Each patient directory (e.g., `M0067`) contains relevant imaging files:

| File Name                   | Description                                                         |
| --------------------------- | ------------------------------------------------------------------- |
| `3DFLAIR_CE.nii`            | Structural reference MRI used for alignment                         |
| `*_dti.nii.gz`              | DTI-derived maps (e.g., FA, MD, AD, RD)                             |
| `wm_mask.nii.gz`            | White matter mask                                                   |
| `non_brain_mask.nii.gz`     | Non-brain mask                                                      |
| `Salient.nii.gz`            | Raw model saliency output                                           |
| `Normalized_Salient.nii.gz` | Min-max normalized saliency map — **recommended for visualization** |

---

## ✅ Notes

* Visualization is best performed using `Normalized_Salient.nii.gz`.
* Prediction accuracy is assessed at a threshold of `ms_prob = 0.5`.
* Saliency maps provide voxel-level interpretability of model attention.

---

Feel free to open issues or pull requests if you have questions or improvements!
