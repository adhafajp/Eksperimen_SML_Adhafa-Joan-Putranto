# Eksperimen_SML_Adhafa-Joan-Putranto

## Deskripsi Proyek

Repository ini merupakan bagian dari submission kelas **Sistem Machine Learning (Dicoding)**, khususnya pada **Kriteria 1: Eksperimen dan Otomatisasi Data Preprocessing**.

Tujuan utama dari proyek ini adalah:

* Melakukan eksplorasi dataset (EDA)
* Mendesain preprocessing pipeline
* Mengubah proses manual menjadi otomatis
* Mengimplementasikan automation menggunakan GitHub Actions

---

## Dataset

Dataset yang digunakan adalah **Credit Risk Dataset (Kaggle - Lao Tse)**, yang berisi data terkait risiko kredit individu.

Dataset ini mencakup fitur seperti:

* Usia (`person_age`)
* Pendapatan (`person_income`)
* Status kepemilikan rumah (`person_home_ownership`)
* Lama bekerja (`person_emp_length`)
* Tujuan pinjaman (`loan_intent`)
* Jumlah pinjaman (`loan_amnt`)
* Suku bunga (`loan_int_rate`)
* Status pinjaman (`loan_status`) → target
* dan fitur lainnya ([Kaggle][1])

Dataset ini umum digunakan untuk:

* Analisis risiko kredit
* Prediksi default pinjaman
* Machine Learning classification problem ([Gigasheet][2])

---

## Struktur Repository

```
Eksperimen_SML_Adhafa-Joan-Putranto/
├── .github/
│   └── workflows/
│       └── preprocess.yml
├── credit_risk_dataset.csv
├── preprocessing/
│   ├── Eksperimen_Adhafa-Joan-Putranto.ipynb
│   ├── automate_Adhafa-Joan-Putranto.py
│   └── credit_risk_preprocessing/
```

Keterangan:

* **Eksperimen_Adhafa-Joan-Putranto.ipynb** → eksplorasi & eksperimen manual
* **automate_Adhafa-Joan-Putranto.py** → hasil konversi preprocessing menjadi script otomatis
* **preprocess.yml** → workflow GitHub Actions untuk automation

---

## Tahapan Eksperimen

### 1. Data Loading

Dataset dimuat dari file CSV dan diperiksa struktur awalnya.

### 2. Exploratory Data Analysis (EDA)

Meliputi:

* Analisis missing values
* Distribusi fitur numerik & kategorikal
* Korelasi antar fitur
* Deteksi outlier
* Analisis imbalance pada target

### 3. Data Preprocessing

Langkah preprocessing meliputi:

* Handling missing values (imputation)
* Encoding fitur kategorikal
* Scaling fitur numerik
* Feature engineering (jika diperlukan)
* Handling imbalance (opsional)

---

## Automatisasi Preprocessing

File:

```
preprocessing/automate_Adhafa-Joan-Putranto.py
```

Script ini memiliki fungsi utama:

* `load_data()`
* `preprocess()`
* `split_data()`
* `save_output()`
* `main()`

Output:

```
preprocessing/credit_risk_preprocessing/
├── X_train.csv
├── X_test.csv
├── y_train.csv
├── y_test.csv
```

---

## GitHub Actions (Automation)

Workflow:

```
.github/workflows/preprocess.yml
```

Fungsi:

* Menjalankan preprocessing otomatis saat:

  * push
  * manual trigger (workflow_dispatch)

Pipeline:

1. Checkout repository
2. Setup Python environment
3. Install dependencies
4. Jalankan script preprocessing

---

## Cara Menjalankan

### Lokal

```bash
pip install -r requirements.txt
python preprocessing/automate_Adhafa-Joan-Putranto.py
```

### GitHub Actions

* Push ke repository
* Workflow akan berjalan otomatis

---

## Tujuan Akhir

Repository ini dirancang untuk memenuhi:

* Basic → eksperimen manual
* Skilled → automation preprocessing
* Advanced → workflow GitHub Actions

---

## Catatan Penting

* Repository ini hanya fokus pada **preprocessing stage**
* Tahap modelling, CI/CD, dan monitoring dilakukan pada repository terpisah sesuai ketentuan Dicoding

---

## Author

Adhafa Joan Putranto

[1]: https://www.kaggle.com/datasets/laotse/credit-risk-dataset/data "Credit Risk Dataset"
