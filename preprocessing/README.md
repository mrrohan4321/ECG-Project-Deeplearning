# ⚙️ Signal Preprocessing & Segmentation

**Member 2 · Ritam Jana** · Code: [`02_preprocessing_v1.py`](02_preprocessing_v1.py) (same logic as [`notebooks/02_preprocessing_v1_Ritam_WORK.ipynb`](../notebooks/02_preprocessing_v1_Ritam_WORK.ipynb))

## 1. Configuration

| Step | Setting |
|---|---|
| Lead | Channel 0 (MLII, Modified Limb Lead II) |
| Segment length | 360 samples (1.0 s window centred on the annotated R-peak: 180 before, 180 after) |
| Bandpass filter | Butterworth, 0.5–45 Hz, order 4 (removes baseline wander and high-frequency noise) |
| Normalisation | Per-segment z-score: `(segment - mean) / std` |

## 2. Records & Patient-Wise Split
The split is strictly by patient/record (not random) to prevent data leakage.

- **Excluded paced records:** 102, 104, 107, 217
- **Total processed records:** 44
- **Train (30 records):** 100, 101, 103, 105, 106, 108, 109, 111, 112, 113, 114, 115, 116, 117, 118, 119, 121, 122, 123, 124, 200, 201, 202, 205, 208, 209, 210, 212, 213, 215
- **Validation (7 records):** 203, 214, 219, 220, 221, 222, 228
- **Test (7 records):** 207, 223, 230, 231, 232, 233, 234

## 3. Label Mapping & Beat Counts
- 🟢 **Normal (0):** `N`, `L`, `R`, `e`, `j`
- 🔴 **Abnormal (1):** `A`, `a`, `J`, `S`, `V`, `E`, `F`, `/`, `f`, `Q`
- **Non-beat annotations dropped:** e.g. `+`, `~`, `|`

| Split | Beats | Normal | Abnormal |
|---|---|---|---|
| Train | 68,386 | 62,790 | 5,596 |
| Validation | 16,399 | 14,553 | 1,846 |
| Test | 15,897 | 12,733 | 3,164 |

## 4. Output File
**`ecg_processed_v1.npz`** (~134 MB) is hosted on Google Drive because of GitHub's file size limit:
[Download from Google Drive](https://drive.google.com/file/d/1TO043QL8KSBiMkkf4Di5MMbLF5wXGgT0/view)

| Array | Shape | Type |
|---|---|---|
| `X_train` | (68386, 360, 1) | float32 |
| `y_train` | (68386,) | int32 |
| `X_val` | (16399, 360, 1) | float32 |
| `y_val` | (16399,) | int32 |
| `X_test` | (15897, 360, 1) | float32 |
| `y_test` | (15897,) | int32 |

## 5. How to Load the Data
```python
import numpy as np

data = np.load('ecg_processed_v1.npz')
X_train, y_train = data['X_train'], data['y_train']
X_val,   y_val   = data['X_val'],   data['y_val']
X_test,  y_test  = data['X_test'],  data['y_test']

print("Train:", X_train.shape, y_train.shape)
print("Val:  ", X_val.shape, y_val.shape)
print("Test: ", X_test.shape, y_test.shape)
```
Expected output:
```
Train: (68386, 360, 1) (68386,)
Val:   (16399, 360, 1) (16399,)
Test:  (15897, 360, 1) (15897,)
```

## 6. Notes
- Beats whose 360-sample window does not fit inside the record are dropped: 100,733 annotated beats → 100,682 segments (68,386 + 16,399 + 15,897).
- The filter is applied to the **whole record first**, then the 360-sample window is cut and z-scored. The demo does the same for raw ECG input; beats taken from `ecg_processed_v1.npz` are already processed and are not filtered again.
