# ChurnPrediction — Customer Churn Prediction

## Deskripsi singkat ✨
Project ini berisi pipeline end-to-end untuk memprediksi churn pelanggan (Telecom) menggunakan Python. Skrip utama `Churn.py` melakukan pembersihan data, eksplorasi singkat, encoding fitur, pengecekan multikolinearitas (VIF), pemilihan model (Random Forest & XGBoost) dengan GridSearchCV, serta evaluasi model (Accuracy, Precision, Recall, F1).

---

## Daftar isi
1. [Fitur utama](#fitur-utama)
2. [Dataset](#dataset)
3. [Persyaratan & Instalasi](#persyaratan--instalasi)
4. [Cara menjalankan](#cara-menjalankan)
5. [Rincian cara kerja](#rincian-cara-kerja)
6. [Model & Hyperparameter](#model--hyperparameter)
7. [Evaluasi & Output yang diharapkan](#evaluasi--output-yang-diharapkan)
8. [Hasil singkat / Insight](#hasil-singkat--insight)
9. [Keterbatasan & saran perbaikan](#keterbatasan--saran-perbaikan)
10. [File penting](#file-penting)
11. [Lisensi](#lisensi)

---

## Fitur utama ✅
- Pipeline pra-pemrosesan (pembersihan, imputasi, encoding)
- Deteksi outlier sederhana dan analisis korelasi
- Pengecekan multikolinearitas (VIF) dan penghapusan fitur bermasalah
- Pelatihan model dengan GridSearchCV untuk Random Forest dan XGBoost
- Evaluasi lengkap (Accuracy, Precision, Recall, F1)
- Visualisasi dasar (histogram setiap fitur dan heatmap korelasi)

---

## Dataset 📂
- File yang digunakan: `WA_FnUseC_TelcoCustomerChurn.csv`
- Harus diletakkan di folder yang sama dengan `Churn.py`.

---

## Persyaratan & Instalasi 🔧
Rekomendasi Python: **3.8+**

Instalasi cepat (virtual environment disarankan):

```bash
python -m venv .venv
.venv\Scripts\activate      # Windows
pip install --upgrade pip
pip install numpy pandas seaborn matplotlib scikit-learn imbalanced-learn statsmodels xgboost
```

Atau buat `requirements.txt` dan jalankan:

```bash
pip install -r requirements.txt
```

Paket utama yang dipakai:
- numpy, pandas
- seaborn, matplotlib
- scikit-learn (model, preprocessing, metrics, GridSearchCV)
- xgboost
- statsmodels (VIF)
- imbalanced-learn (SMOTE — diimpor tetapi tidak dipakai secara default)

---

## Cara menjalankan ▶️
Menjalankan skrip utama (interaktif — akan meminta pilihan model):

```bash
python Churn.py
```

Catatan:
- Skrip menggunakan `input()` untuk memilih model (masukkan `1` untuk Random Forest atau `2` untuk XGBoost).
- Untuk menjalankan di Jupyter: buka `Churn.ipynb`.

---

## Rincian cara kerja (ringkas) 🔍
1. Load dataset dan inspeksi singkat (`df.head()`, `df.info()`).
2. Pembersihan: ubah nilai kosong pada `TotalCharges`, konversi ke float, isi dengan median.
3. EDA: plot distribusi setiap kolom dan deteksi outlier untuk `tenure`, `MonthlyCharges`, `TotalCharges`.
4. Encoding fitur:
   - Map manual untuk kolom biner (Yes/No → 1/0) dan beberapa fitur khusus.
   - `LabelEncoder` untuk sisa kolom bertipe object.
5. Split fitur/target (`X`, `y`) dan split train/test (test_size=0.2, random_state=42).
6. Hitung VIF untuk mendeteksi multikolinearitas; berdasarkan hasil, `MonthlyCharges` dan `TotalCharges` di-drop.
7. Pilih model dengan fungsi `pilih_model()` — menyiapkan `GridSearchCV` untuk hyperparameter tuning.
8. Fit model, tampilkan best params & best score, lalu evaluasi pada test set (Accuracy, Precision, Recall, F1).
9. Visualisasi heatmap korelasi dan beberapa histogram selama EDA.

---

## Model & Hyperparameter (yang dipakai di skrip) ⚙️
- RandomForestClassifier (GridSearchCV):
  - n_estimators: [50, 100, 200]
  - max_depth: [5, 10, None]
  - min_samples_split: [2, 5, 10]

- XGBClassifier (GridSearchCV):
  - n_estimators: [50, 100, 200]
  - max_depth: [5, 10, 12]
  - learning_rate: [0.01, 0.1, 0.2]

Keduanya menggunakan `cv=5` dan `scoring='accuracy'`.

---

## Evaluasi & output yang diharapkan 📈
- Output di console: best parameters (GridSearch), best CV score, dan metrik evaluasi pada test set (Accuracy, Precision, Recall, F1).
- Visual: histogram tiap fitur dan heatmap korelasi (ditampilkan saat runtime).

Metrik yang dicetak:
- accuracy_score
- precision_score
- recall_score
- f1_score

---

## Hasil singkat / Insight (dari eksperimen di skrip) 📊
- Accuracy: RF ≈ 0.8105, XGB ≈ 0.8005
- Precision: RF ≈ 0.6827, XGB ≈ 0.6483
- Recall: RF ≈ 0.5308, XGB ≈ 0.5388
- F1-score: RF ≈ 0.5972, XGB ≈ 0.5885

Kesimpulan singkat: model Random Forest menunjukkan performa yang sedikit lebih baik secara keseluruhan dan lebih seimbang menurut F1-score.

---

## Keterbatasan & saran perbaikan 💡
- `SMOTE` di-import tetapi tidak digunakan — pertimbangkan untuk mengatasi class imbalance.
- Gunakan pipeline (`sklearn.pipeline.Pipeline`) agar kode lebih modular dan reproducible.
- Simpan model terbaik (`joblib` / `pickle`) untuk deployment.
- Tambah metric lain (ROC-AUC, PR-AUC), validation strategy (stratified CV), dan analisa feature importance.
- Eksperimen lebih luas pada hyperparameter dan/atau coba model ensemble lain.

---

## File penting dalam repo
- `Churn.py` — skrip utama (pipeline + training + evaluation)
- `Churn.ipynb` — notebook eksplorasi (jika tersedia)
- `WA_FnUseC_TelcoCustomerChurn.csv` — dataset (wajib ada)

---

## Lisensi
Ditentukan oleh pemilik repo — tambahkan `LICENSE` sesuai kebutuhan (MIT direkomendasikan untuk open source).

---

Jika ingin, saya bisa juga:
- Menambahkan `requirements.txt` otomatis ✅
- Menyimpan model terbaik ke `model.joblib` ✅
- Memecah `Churn.py` menjadi modul/fungsi untuk dipanggil dari CLI ✅

Silakan beri tahu tindakan mana yang diinginkan selanjutnya.