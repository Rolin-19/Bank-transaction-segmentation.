# Studi Kasus: Segmentasi Transaksi Bank dengan Machine Learning

## Latar belakang

Proyek ini berasal dari submission akhir kursus Belajar Machine Learning untuk Pemula (Dicoding, 2026).
Tujuan: mengelompokkan transaksi bank berdasarkan perilaku, lalu membangun model klasifikasi yang
memprediksi label kelompok tersebut.

> Ini adalah proyek pembelajaran. Kode ditulis dengan bantuan AI dan diverifikasi secara manual.

## Data

- **Sumber:** Google Sheets publik dari materi kursus (tautan di README).
- **Dataset awal:** 2.537 baris transaksi.
- **Setelah preprocessing:** 1.945 baris (penanganan nilai kosong, encoding kategorikal, standardisasi).
- **Fitur:** TransactionAmount, TransactionType, Location, Channel, CustomerAge,
  CustomerOccupation, TransactionDuration, LoginAttempts, AccountBalance, AgeGroup.

> Catatan: satu baris = satu transaksi, bukan satu nasabah. Satu nasabah (`AccountID`) dapat
> memiliki banyak transaksi. Jadi "empat kelompok" merujuk pada kelompok transaksi, bukan segmen
> nasabah unik.

## Metode

### Clustering

1. Standardisasi fitur numerik (StandardScaler).
2. Label encoding fitur kategorikal.
3. K-Means; jumlah cluster ditentukan lewat Elbow Method berbasis Silhouette Score.
4. Visualisasi cluster dalam 2D dengan PCA.
5. Interpretasi 4 kelompok dari agregasi (mean/min/max) fitur numerik dan modus fitur kategorikal
   pada data yang sudah di-inverse transform.

### Klasifikasi

1. Memuat data hasil clustering (`data_clustering_inverse.csv`), one-hot encoding fitur kategorikal.
2. Split 80/20 stratified → 1.556 data latih, 389 data uji.
3. Decision Tree (`random_state=42`).
4. Random Forest (`random_state=42`).
5. Tuning Random Forest dengan GridSearchCV (5-fold, scoring accuracy).
6. Evaluasi: accuracy, precision, recall, F1 (macro).
7. Penyimpanan model dengan joblib.

## Hasil

| Model | Accuracy (uji) | F1 macro (uji) |
|---|---|---|
| Decision Tree | 95,37% | 0,9538 |
| Random Forest | 100% | 1,0000 |
| Random Forest (tuned) | 100% | 1,0000 |

- **Silhouette Score K-Means:** 0,4446.
- Parameter terbaik Random Forest: `min_samples_split=5`, `n_estimators=50`.

## Keterbatasan (wajib dibaca)

1. **Akurasi 100% bukan kemampuan memprediksi dunia nyata.** Target yang diprediksi adalah
   **label hasil K-Means** (cluster ID), bukan label fraud yang diverifikasi. Model klasifikasi
   hanya mempelajari pola segmentasi yang sudah dibuat clustering — ini tugas memprediksi label
   buatan, bukan mendeteksi penipuan.
2. **Silhouette Score 0,4446** menunjukkan pemisahan cluster sedang, bukan sangat jelas.
   Empat kelompok transaksi tumpang tindih pada beberapa fitur.
3. **Dataset kecil** (1.945 baris setelah preprocessing) dan berasal dari data kursus, belum
   tentu mewakili distribusi transaksi bank nyata.
4. **Empat kelompok adalah kelompok transaksi**, bukan segmen nasabah unik.
5. Akurasi 100% pada data uji kecil (389 baris) dengan 4 kelas seimbang berpotensi overfitting;
   belum diuji pada data baru.

## Status publikasi

- Notebook submission asli dan CSV **tidak** disertakan di repositori publik (hak publikasi
  belum terverifikasi).
- Repositori publik berisi: dokumentasi, hasil analisis, studi kasus, requirements.txt.
- Lisensi MIT hanya berlaku untuk materi yang saya tulis sendiri (README, docs, requirements).
- Materi dari Dicoding (template, dataset) tidak dilisensikan ulang.