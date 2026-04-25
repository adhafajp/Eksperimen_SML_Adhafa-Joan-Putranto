# Eksperimen_SML_Adhafa-Joan-Putranto

Repository ini merupakan bagian dari submission MLOps Expert IDCamp 2025 kelas **Sistem Machine Learning** Dicoding pada Kriteria 1: Eksperimen dan Otomatisasi Data Preprocessing

---

## Dataset

**Credit Risk Dataset** dari Kaggle (Lao Tse) — data historis peminjam untuk prediksi risiko kredit

Sumber: https://www.kaggle.com/datasets/laotse/credit-risk-dataset/data

Fitur yang tersedia:

* `person_age` — usia peminjam
* `person_income` — pendapatan tahunan
* `person_home_ownership` — status kepemilikan rumah
* `person_emp_length` — lama bekerja
* `loan_intent` — tujuan pinjaman
* `loan_grade` — grade pinjaman
* `loan_amnt` — jumlah pinjaman
* `loan_int_rate` — suku bunga
* `loan_percent_income` — rasio pinjaman terhadap pendapatan
* `cb_person_default_on_file` — riwayat gagal bayar
* `cb_person_cred_hist_length` — panjang riwayat kredit
* `loan_status` — target (0 = non-default, 1 = default)

---

## Struktur Repository
```text
Eksperimen_SML_Adhafa-Joan-Putranto/
├── .github/
│   └── workflows/
│       └── preprocess.yml
├── credit_risk_dataset.csv
├── preprocessing/
│   ├── Eksperimen_Adhafa-Joan-Putranto.ipynb
│   ├── automate_Adhafa-Joan-Putranto.py
│   └── credit_risk_preprocessing/
│       ├── X_train.csv
│       ├── X_test.csv
│       ├── y_train.csv
│       └── y_test.csv
```

* `Eksperimen_Adhafa-Joan-Putranto.ipynb` — eksplorasi dan eksperimen manual
* `automate_Adhafa-Joan-Putranto.py` — konversi preprocessing menjadi script otomatis
* `preprocess.yml` — workflow GitHub Actions

---

## Tahapan Eksperimen

### 1. Data Loading

Dataset dimuat dari file CSV dan diperiksa struktur, tipe data, serta beberapa baris awalnya

### 2. Exploratory Data Analysis (EDA)

* Analisis missing values — `loan_int_rate` (~9.5%) dan `person_emp_length` (~2.8%)
* Distribusi fitur numerik dan kategorikal
* Analisis imbalance target — 78.2% non-default, 21.8% default
* Bivariate analysis numerik dan kategorikal terhadap target
* Korelasi antar fitur dengan heatmap
* Deteksi outlier menggunakan metode IQR

### 3. Data Preprocessing

* Train-test split dengan stratifikasi sebelum preprocessing untuk menghindari data leakage
* Imputasi `loan_int_rate` menggunakan median per `loan_grade` dari data train
* Imputasi `person_emp_length` menggunakan median dari data train
* Capping outlier: `person_age` (max 65), `person_emp_length` (max 40), `loan_percent_income` (max 0.6)
* Log transform + cap 99th percentile pada `person_income`
* Ordinal encoding pada `loan_grade` (urutan A < B < C < D < E < F < G)
* One-Hot encoding pada `person_home_ownership`, `loan_intent`, `cb_person_default_on_file`
* StandardScaler pada semua fitur numerik (fit dari train, transform ke test)

---

## Automatisasi Preprocessing

File `preprocessing/automate_Adhafa-Joan-Putranto.py` merupakan konversi dari notebook dengan fungsi utama:

* `load_data()` — membaca dataset raw dari CSV
* `split_data()` — train-test split sebelum preprocessing
* `preprocess_data()` — membangun dan menjalankan pipeline ColumnTransformer yang mencakup imputasi, capping, log transform, encoding, dan scaling per kolom
* `save_output()` — menyimpan hasil ke folder output
* `main()` — menjalankan seluruh pipeline

Output disimpan di `preprocessing/credit_risk_preprocessing/`

### Output Structure

Setelah script dijalankan, dihasilkan 4 file CSV dengan struktur berikut:

**`X_train.csv` dan `X_test.csv`** — 17 kolom fitur hasil preprocessing:

| # | Kolom | Transformasi |
|---|-------|-------------|
| 1 | `person_age` | Capping (max 65) + StandardScaler |
| 2 | `person_emp_length` | Capping (max 40) + StandardScaler |
| 3 | `loan_percent_income` | Capping (max 0.6) + StandardScaler |
| 4 | `person_income` | Log1p + Cap 99th percentile + StandardScaler |
| 5 | `loan_amnt` | StandardScaler |
| 6 | `cb_person_cred_hist_length` | StandardScaler |
| 7 | `loan_int_rate` | Imputasi median per grade + StandardScaler |
| 8 | `person_home_ownership_OTHER` | OneHotEncoder (drop=first) |
| 9 | `person_home_ownership_OWN` | OneHotEncoder (drop=first) |
| 10 | `person_home_ownership_RENT` | OneHotEncoder (drop=first) |
| 11 | `loan_intent_EDUCATION` | OneHotEncoder (drop=first) |
| 12 | `loan_intent_HOMEIMPROVEMENT` | OneHotEncoder (drop=first) |
| 13 | `loan_intent_MEDICAL` | OneHotEncoder (drop=first) |
| 14 | `loan_intent_PERSONAL` | OneHotEncoder (drop=first) |
| 15 | `loan_intent_VENTURE` | OneHotEncoder (drop=first) |
| 16 | `cb_person_default_on_file_Y` | OneHotEncoder (drop=first) |
| 17 | `loan_grade` | OrdinalEncoder (A=0 … G=6) |

> Kategori yang di-drop (baseline): `person_home_ownership_MORTGAGE`, `loan_intent_DEBTCONSOLIDATION`, `cb_person_default_on_file_N`

**`y_train.csv` dan `y_test.csv`** — 1 kolom target:

| Kolom | Nilai |
|-------|-------|
| `loan_status` | `0` = Non-default, `1` = Default |

---

## GitHub Actions

Workflow `.github/workflows/preprocess.yml` menjalankan script preprocessing otomatis setiap kali ada push atau manual trigger.

Tahapan pipeline:

1. Checkout repository
2. Setup Python 3.12.7
3. Install dependencies
4. Jalankan `automate_Adhafa-Joan-Putranto.py`

---

## Cara Menjalankan

**Lokal:**
```bash
pip install -r requirements.txt
python preprocessing/automate_Adhafa-Joan-Putranto.py
```

**GitHub Actions:** push ke repository, workflow akan berjalan otomatis

---

## Author

Adhafa Joan Putranto

- [GitHub](https://github.com/adhafajp)
- [DagsHub](https://dagshub.com/adhafajp)
- [Docker Hub](https://hub.docker.com/u/adhafajp)