# C14250142---Joshua-vincent-UAS-DAE


# README — Analisis Dataset ToyotaCorolla (Step-by-step + Visualisasi)

Dokumen ini adalah README ringkas dan reproducible yang menuntun Anda langkah-per-langkah — dari persiapan lingkungan hingga pembuatan visualisasi — untuk dataset `ToyotaCorolla.csv`. Semua perintah disajikan dalam urutan yang jelas sehingga Anda bisa menjalankannya satu-per-satu.

---

## Output yang sudah tersedia

Letakkan perhatian ke folder output yang saya gunakan: `/mnt/data/knime_report_outputs/`.

File penting:

* `ToyotaCorolla_processed.csv` — dataset yang dibersihkan.
* `ToyotaCorolla_report.pdf` — laporan ringkas (jika tersedia)
* `hist_price.png`, `scatter_price_age.png`, `scatter_price_km.png`, `corr_matrix.png` — visualisasi utama.

---

## 1. Persiapan lingkungan

1. Pastikan Python 3.8+ terpasang.
2. Buat virtual environment (opsional) dan install dependensi:

```bash
python -m venv venv
source venv/bin/activate  # linux / macOS
# atau: venv\Scripts\activate  # Windows
pip install pandas numpy matplotlib scikit-learn reportlab
```

---

## 2. Struktur README ini (apa yang Anda akan lakukan)

1. Muat dataset dan inspeksi singkat.
2. Konversi tipe yang relevan.
3. Bersihkan data (hapus duplikasi).
4. Simpan dataset terproses.
5. Buat visualisasi: histogram, scatter plots, korelasi.
6. (Opsional) Baseline modeling: regresi linear.
7. Simpan semua hasil ke folder output.

---

## 3. Step-by-step code (jalankan di Jupyter atau script `run_analysis.py`)

### A. Muat dataset dan inspeksi cepat

```python
import pandas as pd
from pathlib import Path
DATA = Path('/mnt/data/ToyotaCorolla.csv')
OUT = Path('/mnt/data/knime_report_outputs')
OUT.mkdir(exist_ok=True)

df = pd.read_csv(DATA, encoding='latin1', low_memory=False)
print('Rows, cols:', df.shape)
print(df.head())
print(df.info())
```

### B. Periksa missing values dan tipe data

```python
missing = df.isna().sum().sort_values(ascending=False)
print(missing[missing>0])
print(df.dtypes)
```

### C. Konversi kolom bertipe object yang sebenarnya numeric

```python
for col in df.columns:
    if df[col].dtype == object:
        # hilangkan tanda ribuan dan coba parse
        coerced = pd.to_numeric(df[col].astype(str).str.replace(',',''), errors='coerce')
        if coerced.notna().sum() / len(df) > 0.8:
            df[col] = coerced
```

### D. Hapus duplikasi dan simpan dataset terproses

```python
processed = df.drop_duplicates().reset_index(drop=True)
processed.to_csv(OUT / 'ToyotaCorolla_processed.csv', index=False)
```

### E. Buat visualisasi (simpan ke PNG)

Berikut contoh plotting menggunakan matplotlib. Jalankan tiap blok untuk membuat file gambar.

1. Histogram Price

```python
import matplotlib.pyplot as plt
if 'Price' in processed.columns:
    plt.figure(figsize=(8,5))
    plt.hist(processed['Price'].dropna(), bins=30)
    plt.title('Histogram: Price')
    plt.xlabel('Price')
    plt.ylabel('Count')
    plt.tight_layout()
    plt.savefig(OUT / 'hist_price.png')
    plt.close()
```

2. Scatter Price vs Age_08_04

```python
if {'Price','Age_08_04'}.issubset(processed.columns):
    plt.figure(figsize=(8,5))
    plt.scatter(processed['Age_08_04'], processed['Price'], s=10, alpha=0.6)
    plt.title('Price vs Age_08_04')
    plt.xlabel('Age_08_04 (months)')
    plt.ylabel('Price')
    plt.tight_layout()
    plt.savefig(OUT / 'scatter_price_age.png')
    plt.close()
```

3. Scatter Price vs KM

```python
if {'Price','KM'}.issubset(processed.columns):
    plt.figure(figsize=(8,5))
    plt.scatter(processed['KM'], processed['Price'], s=10, alpha=0.6)
    plt.title('Price vs KM')
    plt.xlabel('KM')
    plt.ylabel('Price')
    plt.tight_layout()
    plt.savefig(OUT / 'scatter_price_km.png')
    plt.close()
```

4. Correlation matrix (numeric)

```python
import numpy as np
num = processed.select_dtypes(include=[np.number])
if num.shape[1] > 1:
    corr = num.corr()
    plt.figure(figsize=(10,8))
    plt.imshow(corr, aspect='auto', cmap='viridis')
    plt.colorbar(fraction=0.03)
    plt.xticks(range(len(corr.columns)), corr.columns, rotation=90)
    plt.yticks(range(len(corr.index)), corr.index)
    plt.title('Correlation matrix (numeric)')
    plt.tight_layout()
    plt.savefig(OUT / 'corr_matrix.png')
    plt.close()
```

> Setelah menjalankan blok di atas, periksa folder `/mnt/data/knime_report_outputs/` untuk empat file PNG.

---

## 4. Baseline modeling (opsional)

Jika ingin baseline cepat:

```python
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import r2_score, mean_squared_error

if {'Price','Age_08_04','KM'}.issubset(processed.columns):
    df_model = processed[['Price','Age_08_04','KM']].dropna()
    X = df_model[['Age_08_04','KM']]
    y = df_model['Price']
    X_train, X_test, y_train, y_test = train_test_split(X,y,test_size=0.25,random_state=42)
    model = LinearRegression(); model.fit(X_train,y_train)
    y_pred = model.predict(X_test)
    print('R2:', r2_score(y_test,y_pred))
    print('MSE:', mean_squared_error(y_test,y_pred))
    print('Coefficients:', dict(zip(X.columns, model.coef_)))
```

---

## 5. Hasil visualisasi yang saya siapkan (sudah dibuat)

Jika Anda tidak ingin menjalankan skrip sendiri, saya sudah menyimpan visualisasi berikut:

* `/mnt/data/knime_report_outputs/hist_price.png`
* `/mnt/data/knime_report_outputs/scatter_price_age.png`
* `/mnt/data/knime_report_outputs/scatter_price_km.png`
* `/mnt/data/knime_report_outputs/corr_matrix.png`

Anda bisa mengunduhnya langsung.

---

## 6. Tips interpretasi untuk tiap visualisasi

* **Histogram Price**: cari skew (kanan/kiri). Jika skew tinggi, pertimbangkan transformasi log untuk modeling.
* **Scatter Price vs Age**: tren negatif tipikal; cek outlier (umur rendah tapi harga rendah).
* **Scatter Price vs KM**: korelasi negatif diharapkan; variasi besar menandakan fitur tambahan (model/tahun) penting.
* **Correlation matrix**: gunakan untuk memilih fitur yang berkorelasi tinggi satu sama lain; hindari multikolinearitas.

---

## 7. Langkah lanjutan (opsional, rekomendasi)

1. Encoding kategori: `Model`, `Fuel_Type`, `Color`.
2. Imputasi missing values (median untuk numerik, mode atau new category untuk kategorik).
3. Feature selection & regularisasi (Ridge/Lasso).
4. Coba model non-linear (RandomForest / XGBoost) dan gunakan CV untuk tuning.
5. Interpretasi: SHAP untuk menjelaskan pengaruh fitur.

---

## 8. Jika Anda mau saya lakukan sekarang

Ketik salah satu opsi:

* `buat notebook reproducible` — saya buat `analysis.ipynb` dan simpan.
* `jalankan modeling XGBoost` — saya jalankan XGBoost CV dan kirim hasil.
* `ekspor README ke md/pdf` — saya ekspor dan beri link unduhan.

---

Terima kasih — README ini sudah disederhanakan dan difokuskan pada langkah demi langkah serta instruksi pembuatan visualisasi. Jika Anda butuh format lain (mis. ringkasan 1 halaman, atau instruksi KNIME), beri tahu saya.
