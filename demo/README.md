# 🖥️ Demo

**Built by Member 4 (Rohan Adak) and Member 5 (Rishabh Jha)**

🌐 **Live demo:** [ecg-project-deeplearning.streamlit.app](https://ecg-project-deeplearning-6383kyappwquwvbrmgodag6.streamlit.app/)
🎬 **Demo video:** [youtu.be/or2yRLIMrqw](https://youtu.be/or2yRLIMrqw) (also saved here as `demo_video.mp4`)

A Streamlit app that connects the whole pipeline:

```
input ECG beat → preprocessing → 1D-CNN + CNN-LSTM → predicted class (Normal / Abnormal)
```

> The free Streamlit app goes to sleep when nobody uses it. If you see *"Wake this app up"*, click the button and wait a few seconds.

| File | Description |
|---|---|
| `app.py` | Streamlit app (run from the project root) |
| `sample_beats.npz` | Built-in sample: 400 real test beats (200 Normal, 200 Abnormal) taken from the test split, so the demo works with no upload |
| `demo_video.mp4` | Recorded walkthrough of the demo |
| `requirements.txt` | Package list for the Streamlit deployment (no versions) |
| `../requirements.txt` | Pinned packages: Streamlit, TensorFlow 2.20.0, Keras 3.13.2, NumPy, pandas, Matplotlib, SciPy |

## 🔍 What the app does
1. **Step 1 · Input:** choose a beat from the built-in sample (loads automatically) or, if available, from the full test set of `ecg_processed_v1.npz` (filter: Any / Normal / Abnormal; the true label is shown), or upload your own ECG file (`.csv`, `.txt`, `.npy`) as a raw signal or already preprocessed.
2. **Step 2 · Preprocessing:** Butterworth bandpass 0.5–45 Hz (order 4, 360 Hz), 360-sample window, per-segment z-score. Beats from the `.npz` are already preprocessed, so this step is skipped for them.
3. **Step 3 · Prediction:** both models give one sigmoid output, P(abnormal). Threshold 0.5: P ≥ 0.5 is **Abnormal**, otherwise **Normal**. The app also says whether the two models agree.
4. **Step 4 · Evaluation:** metrics, confusion matrix counts, CPU vs GPU table and speedups are read from `results/*.csv` and `results/hardware_info_v1.json`. No number is typed into the code. The "best model" line is chosen automatically from the abnormal-class F1 in the CSV files.

## 🛠️ How to run

**Easiest:** open the live demo link above. The beats load automatically from the built-in sample, nothing to upload. Click **Run both models** to see the predictions.

**On your own computer:**

1. Install (Python 3.10 to 3.13):
   ```bash
   pip install -r requirements.txt
   ```
2. Run from the project root:
   ```bash
   streamlit run demo/app.py
   ```
   The browser opens at [http://localhost:8501](http://localhost:8501).

**Optional: use the full test set (15,897 beats) instead of the sample**

Download **`ecg_processed_v1.npz`** (~134 MB, too big for GitHub) and either upload it in the app (*"Optional: use the full test set..."* box), or put it in the project root (next to `requirements.txt`) or in a `data/` folder, where the app finds it automatically:
```bash
pip install gdown
gdown 1TO043QL8KSBiMkkf4Di5MMbLF5wXGgT0 -O ecg_processed_v1.npz
```
Or use the [Google Drive link](https://drive.google.com/file/d/1TO043QL8KSBiMkkf4Di5MMbLF5wXGgT0/view).

## 📦 Needed files
- `models/1d_cnn_model.keras` and `models/cnn_lstm_v1.keras`
- `results/` folder (CSV and JSON files)
- `demo/sample_beats.npz` (built-in sample). `ecg_processed_v1.npz` is optional.

## 🧯 Notes and troubleshooting
- Both models were saved with Keras 3.13.2 (`.keras` format) in Colab with TensorFlow 2.20.0. Older TensorFlow (e.g. 2.15, which uses Keras 2) can fail to load them, so keep the versions in `requirements.txt`.
- **Own ECG files:** raw signals need at least 360 samples at 360 Hz. Longer signals are filtered as a whole and the centre 360 samples are used as the beat. Files with fewer than 360 samples give an error message.
- The models were trained on MIT-BIH beats (MLII lead, 360 Hz) with the preprocessing above. Other leads, sampling rates or filters give unreliable predictions.
- The built-in sample is balanced (200 Normal, 200 Abnormal), while the real test set has 19.9% abnormal beats. The Step 4 evaluation numbers always come from the full test run in `results/*.csv`, not from the sample.
- Results are weak for the abnormal class (F1 about 0.45–0.52 on unseen records), so the demo can make mistakes. It is an educational project demo, **not a medical device**.
