# Segmentasi Transaksi Bank dengan Machine Learning

Proyek pembelajaran akhir kursus **Belajar Machine Learning untuk Pemula** (Dicoding, 2026).  
Membangun model clustering untuk mengelompokkan transaksi bank, lalu model klasifikasi untuk
memprediksi label kelompok yang dihasilkan clustering.

> Ini adalah proyek pembelajaran, bukan sistem produksi. Kode ditulis dengan bantuan AI
> dan diverifikasi secara manual.

## Ringkasan

- **Dataset awal:** 2.537 baris transaksi (sumber: Google Sheets publik dari materi kursus).
- **Setelah preprocessing:** 1.945 baris.
- **Clustering (K-Means):** 4 kelompok transaksi, Silhouette Score 0,4446.
- **Klasifikasi (Decision Tree):** akurasi uji 95,37%.
- **Klasifikasi (Random Forest):** akurasi uji 100% (terhadap label hasil K-Means, bukan fraud nyata).

## Struktur repositori (versi aman publik)

```
bank-transaction-segmentation/
├── docs/
│   └── studi-kasus.md              # dokumentasi metode, hasil, keterbatasan
├── README.md
├── requirements.txt
├── LICENSE
└── .gitignore
```

> **Catatan publikasi:** notebook submission asli dan CSV **tidak** disertakan di repositori
> publik karena hak publikasi belum terverifikasi (template Dicoding + dataset dari materi kursus).
> Salinan asli disimpan terpisah di `_submission-ml-dicoding-asli/` (lokal, tidak dipublikasikan).
> Versi publik berisi dokumentasi, hasil analisis, studi kasus, dan requirements.txt.

## Dataset

Dataset berisi atribut transaksi bank: `TransactionAmount`, `TransactionType`, `Location`,
`Channel`, `CustomerAge`, `CustomerOccupation`, `TransactionDuration`, `LoginAttempts`,
`AccountBalance`, `AgeGroup`. Sumber: Google Sheets publik yang dirujuk materi kursus
([tautan](https://docs.google.com/spreadsheets/d/e/2PACX-1vTbg5WVW6W3c8SPNUGc3A3AL-AG32TPEQGpdzARfNICMsLFI0LQj0jporhsLCeVhkN5AoRsTkn08AYl/pub?output=csv)).

> **Catatan sumber & lisensi:** dataset berasal dari Google Sheets publik yang dirujuk
> materi kursus Dicoding. Lisensi republication belum diverifikasi; karena itu dataset
> dan notebook tidak didistribusikan dalam repositori ini. Tautan sumber di atas
> disediakan untuk referensi.

## Metode

### 1. Clustering (notebook clustering, disimpan lokal)

1. Pembersihan data: penanganan nilai kosong, encoding kategorikal, standardisasi fitur numerik.
2. K-Means dengan `n_clusters` ditentukan lewat Elbow Method (Silhouette).
3. Visualisasi cluster dalam 2D menggunakan PCA.
4. Interpretasi 4 kelompok transaksi dari agregasi mean/min/max fitur numerik dan modus fitur kategorikal
   pada data yang sudah di-inverse transform.

### 2. Klasifikasi (notebook klasifikasi, disimpan lokal)

1. Memuat `data_clustering_inverse.csv`, one-hot encoding fitur kategorikal.
2. Split 80/20 (stratified) → 1.556 data latih, 389 data uji.
3. Decision Tree (`random_state=42`).
4. Random Forest (`random_state=42`).
5. Tuning Random Forest dengan GridSearchCV (5-fold).
6. Evaluasi: accuracy, precision, recall, F1 (macro).
7. Penyimpanan model dengan `joblib`.

## Hasil evaluasi (sesuai output notebook)

| Model | Accuracy (uji) | F1 macro (uji) |
|---|---|---|
| Decision Tree | 0,9537 (95,37%) | 0,9538 |
| Random Forest | 1,0000 (100%) | 1,0000 |
| Random Forest (tuned) | 1,0000 (100%) | 1,0000 |

Parameter terbaik Random Forest setelah tuning: `min_samples_split=5`, `n_estimators=50`.

Silhouette Score K-Means: **0,4446**.

## Keterbatasan (penting)

1. **Akurasi 100% bukan kemampuan memprediksi dunia nyata.** Target yang diprediksi adalah
   **label hasil K-Means** (cluster ID), bukan label fraud yang diverifikasi. Model klasifikasi
   hanya "menghafal" pola segmentasi yang sudah dibuat clustering — ini tugas memprediksi label
   buatan, bukan mendeteksi penipuan.
2. **Silhouette Score 0,4446** menunjukkan pemisahan cluster sedang, bukan sangat jelas.
   Empat kelompok transaksi tumpang tindih pada beberapa fitur.
3. **Dataset kecil** (1.945 baris setelah preprocessing) dan berasal dari data sintetis kursus,
   belum tentu mewakili distribusi transaksi bank nyata.
4. **Empat kelompok adalah kelompok transaksi**, bukan segmen nasabah unik — satu nasabah
   (`AccountID`) dapat memiliki banyak transaksi.
5. Akurasi 100% pada data uji kecil (389 baris) dengan 4 kelas seimbang berpotensi overfitting;
   belum diuji pada data baru.

## Reproduksi

Repositori publik ini berisi **dokumentasi**, bukan kode yang dapat dijalankan. Notebook
submission asli, CSV, dan model disimpan lokal dan tidak didistribusikan (hak publikasi
belum terverifikasi). Untuk mereproduksi analisis, dependensi yang dipakai tercantum di
`requirements.txt`; notebook asli dijalankan di lingkungan Python 3.14 dengan pandas,
scikit-learn, dan yellowbrick.

## Sumber & atribusi

- Kursus: Belajar Machine Learning untuk Pemula (Dicoding, 2026).
- Dataset: Google Sheets publik dari materi kursus.
- Lisensi: MIT hanya berlaku untuk materi yang saya tulis sendiri (README, docs, requirements).
  Template Dicoding dan dataset dari materi kursus tidak dilisensikan ulang.

## Status

Proyek kursus · Dicoding · 2026. Repositori publik berisi dokumentasi dan studi kasus.
Notebook, CSV, dan model disimpan lokal.