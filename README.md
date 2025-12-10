* C14250142---Joshua-vincent
* C14250126---Jason Heinrich Zelim
# README — Analisis Dataset Toyota Corolla

READ ME ini berisi ringkasan **step-by-step** proses analisis menggunakan:

* **Dataset CSV Toyota Corolla**
* **Workflow KNIME (`KNIME_TOYOTA_3.1.knwf`)**

---

# 1. Tahap Data Preparation
## 1.1. Memuat Dataset

* Sumber utama: `ToyotaCorolla.csv`
* Gunakan node KNIME:

  * **CSV Reader** → Memuat dataset dan mendeteksi tipe kolom.

## 1.2. Penanganan Missing Values

Jika ada nilai kosong:

* Untuk value 0 diubah menjadi 0

Node yang digunakan:

* **Missing Value**
---

# 2. Tahap Data Processing

<img width="495" height="244" alt="image" src="https://github.com/user-attachments/assets/907e6933-db5c-4f3e-a2fd-188f46a1fb6d" />

Dalam proses ini file data csv yang digunakan di filter dan di visualisasikan 

Bagian di atas menggunakan table view untuk menunjukan jumlah produksi per tahun 1998-2004

Dan bagian di bawah menggunakan pie chart untuk menampilkan jumlah produksi per bulan tahun 1998-2004

<img width="486" height="200" alt="image" src="https://github.com/user-attachments/assets/8e8d9913-d775-4521-9a96-d5d7dab7903f" />

Proses ini menghasilkan data berupa harga minimum dan maksimal mobil dengan penjualan lebih dari 15

Dan pie chart dibawah berfungsi untuk menunjukan jumlah mobil berdasarkan tipe fuel
---

# 4. Ringkasan Alur KNIME (Step-by-Step)

Berikut langkah KNIME paling ringkas yang dapat langsung direplikasi:

1. **CSV Reader** → memuat `ToyotaCorolla.csv`
2. **Data Explorer** → lihat kualitas data
3. **Missing Value** → perbaikan missing value
4. **String to Number / Auto-Type Cast** → memperbaiki tipe data
5. **Duplicate Row Filter** → menghapus duplikasi
6. **One to Many (One-Hot Encoding)** → encode kolom kategorikal
7. **Rule Engine** → membuat target klasifikasi (opsional)
8. **Normalizer** → normalisasi numerik (opsional)
9. **Decision Tree / Random Forest Learner** → model klasifikasi
10. **Predictor Node** → menghasilkan prediksi
11. **Scorer** → mengevaluasi kualitas model
12. **Histogram / Scatter Plot / Heatmap** → membuat visualisasi

---

# 5. Output

File yang dapat dihasilkan:

* Dataset bersih (`ToyotaCorolla_processed.csv`)
* Visualisasi:

  * Histogram harga
  * Scatter Price vs Age
  * Scatter Price vs KM
  * Matriks korelasi
* Model klasifikasi + Confusion Matrix

---

