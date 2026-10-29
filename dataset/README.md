# 🗂️ Dataset · Group 7 ECG Project

**Member 1 · Ronit Jana**

## 📍 Source
[MIT-BIH Arrhythmia Database](https://physionet.org/content/mitdb/1.0.0/) (PhysioNet), downloaded with the WFDB Python package (`wfdb.dl_database`).

## 📁 Folder structure
The raw records are **not** stored in this GitHub repo (download code is in the [main README](../README.md#️-dataset)). In Colab, `dataset/mitdb/` holds 48 records with 3 files each (144 files in total):

| Extension | Content |
|---|---|
| `.dat` | ECG signal |
| `.hea` | Header (sampling rate, leads, gain) |
| `.atr` | Beat annotations |

## 📡 Signal information
| Item | Value |
|---|---|
| Sampling rate | 360 Hz |
| Leads | MLII (channel 0) and V5 (channel 1) in record 100 |
| Record length | ~30 min (650,000 samples per record) |
| Lead used in this project | Channel 0 (MLII) |

## 🏷️ Labels
Annotation symbols sit at each R-peak. Non-beat symbols (such as `+`) are ignored.

- 🟢 **Normal (0):** `N`, `L`, `R`, `e`, `j` (AAMI class N)
- 🔴 **Abnormal (1):** `A`, `a`, `J`, `S`, `V`, `E`, `F`, `/`, `f`, `Q`

## 📊 Statistics (paced records excluded)

| Item | Value |
|---|---|
| Records used | 44 (excluded paced records: 102, 104, 107, 217) |
| Total beats | 100,733 |
| Normal | 90,125 |
| Abnormal | 10,608 (10.5%) |

**Beat symbol counts**

| Symbol | N | L | R | V | A | F | j | a | E | J | e | Q | S | f | / |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Count | 74546 | 8075 | 7259 | 6903 | 2546 | 803 | 229 | 150 | 106 | 83 | 16 | 15 | 2 | 0 | 0 |

Per-record counts: [`results/dataset_stats_per_record.csv`](../results/dataset_stats_per_record.csv)

## ⚠️ Known issues
- **Class imbalance:** normal beats far outnumber abnormal beats.
- **Split must be record-wise** (patient-wise) so beats from one record never appear in both train and test.
- Model output is a classification result, not a clinical diagnosis.

## 🔗 Files from Member 1
- [`graphs/class_distribution.png`](../graphs/class_distribution.png), `ecg_normal.png`, `ecg_abnormal_V.png`, `ecg_abnormal_A.png`
- [`results/dataset_stats_per_record.csv`](../results/dataset_stats_per_record.csv)
- [`notebooks/01_dataset_ecg_analisis_Ronit_WORK.ipynb`](../notebooks/01_dataset_ecg_analisis_Ronit_WORK.ipynb)

## ➡️ Used later
Member 2 turned the 44 usable records into `ecg_processed_v1.npz` (see [`preprocessing/README.md`](../preprocessing/README.md)). The demo (`demo/app.py`) uses beats from that processed file, not the raw records.
