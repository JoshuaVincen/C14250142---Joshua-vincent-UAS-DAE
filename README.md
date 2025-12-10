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

<img width="909" height="494" alt="image" src="https://github.com/user-attachments/assets/087731f7-ec48-43e7-b120-7bb130e602b9" />

<img width="888" height="437" alt="image" src="https://github.com/user-attachments/assets/8d4ecc93-de44-4c78-bcfe-177d151f9b05" />

---

<img width="486" height="200" alt="image" src="https://github.com/user-attachments/assets/8e8d9913-d775-4521-9a96-d5d7dab7903f" />

Proses ini menghasilkan data berupa harga minimum dan maksimal mobil dengan penjualan lebih dari 15

Dan pie chart dibawah berfungsi untuk menunjukan jumlah mobil berdasarkan tipe fuel

<img width="1217" height="704" alt="image" src="https://github.com/user-attachments/assets/c7c30b46-f9de-4ec1-a638-81db815bc524" />

<img width="862" height="420" alt="image" src="https://github.com/user-attachments/assets/6bb5c72f-074d-4bbe-9d3e-4b0dac066f18" />

---

<img width="192" height="104" alt="image" src="https://github.com/user-attachments/assets/f208ab97-9696-4c10-9b73-64ca253ad436" />

Scatter plot ini mengambil data csv dan menampilkan km kendaraan berdasarkan umur

<img width="1259" height="454" alt="image" src="https://github.com/user-attachments/assets/af1cd2f8-cefb-4050-80c7-145d864cc4ec" />

---

<img width="577" height="119" alt="image" src="https://github.com/user-attachments/assets/65ca4438-9032-48d0-a1c7-7f42b0e6e33d" />

Proses ini mengambil data dari csv dan menggunakan desicion tree untuk menampilkan desicion tree mobil manual dan automatic

<img width="411" height="420" alt="image" src="https://github.com/user-attachments/assets/1d5214ac-969e-45ce-94e5-b334cf9c2bb6" />

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

