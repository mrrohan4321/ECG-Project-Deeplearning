# 📓 Notebooks

Run all notebooks in **Google Colab**. File naming: `name_v1.ipynb`, `name_v2.ipynb`.

| # | File | Member | Description |
|---|---|---|---|
| 1 | `01_dataset_ecg_analisis_Ronit_WORK.ipynb` | Member 1 | MIT-BIH download, label counts, ECG plots |
| 2 | `02_preprocessing_v1_Ritam_WORK.ipynb` | Member 2 | Filtering, segmentation, z-score, record-wise split |
| 3 | `03_1d_cnn_Rishi_WORK.ipynb` | Member 3 | 1D-CNN training, evaluation, metrics, graphs, saved model (loads `ecg_processed_v1.npz`, asks for upload in Colab if not found) |
| 4 | `04_cnn_lstm_gpu_v1_Rohan_WORK.ipynb` | Member 4 | CNN-LSTM training, CPU vs GPU timing, hardware info (run on a Colab **T4 GPU** runtime) |

## ▶️ Running notebook 04 (CNN-LSTM + GPU)
1. Upload `ecg_processed_v1.npz` to Colab (or let the notebook download it from Drive).
2. Select a GPU runtime: *Runtime → Change runtime type → T4 GPU*.
3. *Runtime → Run all*.

It saves models, results and graphs in `/content/Group_7_ECG_Project/` and downloads them as one zip.

## 🖥️ Demo
The Streamlit demo is `demo/app.py` (Member 5), not a notebook. The notebooks produce the models and results that the demo loads: `models/*.keras` and `results/*.csv`.
