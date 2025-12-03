# C14250142---Joshua-vincent-UAS-DAE
# README — Analisis Dataset Toyota Corolla (Tanpa Python)

Dokumen ini berisi ringkasan **step-by-step** proses analisis menggunakan:

* **Dataset CSV Toyota Corolla**
* **Workflow KNIME (`KNIME_TOYOTA_2.0.knwf`)**

Ringkasan ini mencakup tiga tahap utama:

1. **Tahap Data Preparation**
2. **Tahap Data Processing & Klasifikasi**
3. **Tahap Visualisasi**

Tidak ada kode Python di dalam dokumen ini.

---

# 1. Tahap Data Preparation

Tahap ini dilakukan baik di KNIME maupun berdasarkan struktur dataset asli.

## 1.1. Memuat Dataset

* Sumber utama: `ToyotaCorolla.csv`
* Gunakan node KNIME:

  * **CSV Reader** → Memuat dataset dan mendeteksi tipe kolom.

## 1.2. Pemeriksaan Struktur Dataset

Hal-hal yang diperiksa:

* Jumlah baris & kolom
* Tipe data setiap kolom (numeric vs kategorik)
* Nilai hilang (missing values)
* Nilai duplikat

Node yang digunakan:

* **Data Explorer**
* **Statistics**
* **Duplicate Row Filter**

## 1.3. Perbaikan Tipe Data

Beberapa kolom pada dataset Toyota Corolla sering terbaca sebagai string (object), misalnya kolom angka dengan koma.

Langkah:

* Mengubah kolom ke tipe angka (Integer/Double) menggunakan:

  * **Column Auto-Type Cast**
  * atau **String to Number** jika butuh konversi spesifik.

## 1.4. Penanganan Missing Values

Jika ada nilai kosong:

* Untuk numerik: gunakan median
* Untuk kategorik: gunakan mode atau kategori baru

Node yang digunakan:

* **Missing Value**

## 1.5. Pembersihan Data

* Menghapus duplikasi
* Menyusun ulang kolom bila diperlukan

Node:

* **Duplicate Row Filter**
* **Column Filter**

Output akhir tahap ini adalah dataset bersih siap diproses.

---

# 2. Tahap Data Processing & Klasifikasi

Fokus tahap ini adalah:

* Transformasi tambahan
* Pemilihan fitur
* Proses klasifikasi (jika dibuat dalam workflow)

## 2.1. Normalisasi / Standarisasi Data (Opsional)

Untuk model tertentu diperlukan normalisasi.
Node yang digunakan:

* **Normalizer**

## 2.2. Feature Engineering (Opsional)

Contoh yang biasa dilakukan:

* Membuat bin kategori: KM, Age
* Mengubah Fuel_Type menjadi One-Hot Encoding

Node:

* **One to Many (One-Hot Encoding)**
* **Rule Engine** untuk membuat label atau kategori

## 2.3. Pembuatan Target (Jika melakukan klasifikasi)

Pada dataset Toyota Corolla, target umum:

* **Klasifikasi harga (High/Low Price)**
* **Prediksi kondisi berdasarkan KM atau Age**

Node:

* **Rule Engine** → membuat kategori target berdasarkan threshold

## 2.4. Membuat Model Klasifikasi

Model yang umum digunakan:

* Decision Tree
* Random Forest
* Logistic Regression

Node KNIME:

* **Decision Tree Learner**
* **Decision Tree Predictor**
* **Random Forest Learner**
* **Random Forest Predictor**
* 
---

# 3. Tahap Visualisasi

Berikut visualisasi *langsung dari dataset CSV* yang telah dihasilkan. Anda dapat membuka file PNG di folder:
`/mnt/data/visuals_readme/`

### 3.1. Histogram Harga (Price)

**File:** `hist_price.png`

* Menunjukkan distribusi harga mobil Toyota Corolla.
* Membantu memahami apakah harga condong (skewed) ke kiri/kanan.

### 3.2. Scatter Plot: Price vs Age_08_04

**File:** `scatter_age.png`

* Menunjukkan hubungan antara umur mobil (bulan) dan harga.
* Biasanya menunjukkan pola menurun (semakin tua → harga turun).

### 3.3. Scatter Plot: Price vs KM

**File:** `scatter_km.png`

* Menunjukkan hubungan antara jarak tempuh dan harga.
* Harga cenderung turun saat KM meningkat.

### 3.4. Korelasi Antar Fitur (Correlation Matrix)

**File:** `corr_matrix.png`

* Memvisualisasikan hubungan antar fitur numerik.
* Membantu memilih fitur terbaik untuk modeling.

Visualisasi dapat dibuat langsung dari KNIME atau dari hasil CSV.

Berikut visualisasi utama yang perlu dihasilkan:

## 3.1. Histogram Harga (Price)

Tujuan:

* Melihat distribusi harga mobil.

Node KNIME:

* **Histogram**
* atau **JavaScript Histogram**

## 3.2. Scatter Plot

Plot yang relevan:

* **Price vs Age_08_04** → melihat penyusutan harga
* **Price vs KM** → melihat hubungan jarak tempuh dan nilai

Node KNIME:

* **Scatter Plot**
* **Interactive Scatter Plot**

## 3.3. Korelasi Antar Fitur

Digunakan untuk melihat korelasi antar variabel numerik.

Node KNIME:

* **Linear Correlation**
* **Correlation Matrix**
* **Heatmap**

Output biasanya berupa visualisasi seperti:

* grafik histogram
* scatter plot
* matriks korelasi

---
## 3.4 preview hasil data
<img width="1364" height="605" alt="image" src="https://github.com/user-attachments/assets/65106f4a-a9c1-4b8e-884c-c25462c36a20" />

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

