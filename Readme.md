Pertalite Sentiment Analysis: TikTok Comments on Indonesian Energy Policy (Pertalite/Bahlil)
Proyek ini melakukan analisis sentimen terhadap komentar publik di TikTok terkait isu energi di Indonesia (khususnya topik Pertalite, Etanol, dan Menteri Bahlil Lahadalia).

Pendekatan yang digunakan adalah Model Distillation: Menggunakan Pre-trained Transformer (RoBERTa) untuk memberikan label sentimen otomatis pada dataset mentah, kemudian melatih model Machine Learning klasik (seperti Random Forest, SVM, dll) menggunakan label tersebut untuk tujuan klasifikasi yang lebih ringan namun akurat.

📋 Daftar Isi
Latar Belakang

Dataset

Metodologi

Tech Stack

Hasil & Performa Model

Instalasi & Cara Menjalankan

📖 Latar Belakang
Reaksi publik terhadap kebijakan pemerintah seringkali terekam jelas di media sosial. Proyek ini bertujuan untuk:

Mengikis (scrape) dan membersihkan data komentar TikTok.

Mengklasifikasikan sentimen komentar menjadi Positif atau Negatif.

Mengatasi ketidakseimbangan data (imbalance dataset) karena komentar cenderung didominasi oleh sentimen negatif.

Membandingkan performa berbagai algoritma klasifikasi teks.

💾 Dataset
Sumber: Komentar TikTok (diambil menggunakan scraper).

File: dataset_tiktok-comments-scraper_2025-11-17_12-14-29-915.csv

Jumlah Data Awal: 2000 baris.

Pembersihan: Menghapus duplikat, nilai kosong (NaN), dan kolom metadata yang tidak relevan (URL, ID, dll).

⚙️ Metodologi
1. Preprocessing & Labeling
Karena data awal tidak memiliki label, proyek ini menggunakan pendekatan semi-supervised/automated labeling:

Labeling: Menggunakan model Hugging Face w11wo/indonesian-roberta-base-sentiment-classifier untuk menghasilkan label sentimen (Positif/Negatif) dan skor kepercayaan. Data dengan skor kepercayaan rendah (< 0.65) dibuang untuk menjaga kualitas label.

Text Cleaning: Lowercasing, menghapus karakter non-alfanumerik (kecuali emoji tertentu), dan normalisasi kata.

Slang Normalization: Mengubah bahasa gaul (e.g., "gk", "bgt", "anj") menjadi bahasa baku menggunakan kamus kustom.

2. Exploratory Data Analysis (EDA)
Analisis distribusi sentimen (Dataset sangat imbalanced: ~88% Negatif vs ~12% Positif).

Visualisasi WordCloud untuk masing-masing sentimen.

Analisis jumlah "Digg" (Likes) berdasarkan sentimen.

3. Feature Extraction
Menggunakan TF-IDF (Term Frequency-Inverse Document Frequency) dengan n-gram (1,2) dan max features 5000.

4. Modeling & Handling Imbalance
Lima algoritma diuji dan di-tuning menggunakan GridSearchCV:

Random Forest (RF)

Linear SVM

Logistic Regression (LR)

Naive Bayes (MultinomialNB)

XGBoost

Teknik SMOTE (Synthetic Minority Over-sampling Technique) diterapkan untuk menangani ketidakseimbangan kelas pada data latih.

## 📊 Hasil & Performa Model

Evaluasi dilakukan menggunakan metrik **Accuracy**, **F1-Score (Macro)**, dan **Confusion Matrix** pada data uji (Test Set).

| Model | Accuracy | Macro F1 | Catatan |
| :--- | :---: | :---: | :--- |
| **Random Forest** | ~88% | ~0.70 | Performa baseline yang kuat. |
| **SVM (Linear)** | ~88% | ~0.57 | Akurasi tinggi tapi struggle di kelas minoritas. |
| **Logistic Regression** | ~86% | ~0.70 | Seimbang setelah tuning. |
| **Naive Bayes** | ~87% | ~0.47 | Kurang efektif mendeteksi kelas positif. |
| **XGBoost** | ~81% | ~0.67 | Memerlukan tuning lebih lanjut pada data kecil. |
| **RF + SMOTE** | ~85% | ~0.70 | Keseimbangan terbaik antara Presisi dan Recall. |

> **Insight:** Meskipun SVM memiliki akurasi tertinggi, **Random Forest dengan SMOTE** memberikan hasil yang lebih *fair* dalam mendeteksi sentimen positif (kelas minoritas), yang sangat penting untuk analisis sentimen yang tidak bias.

---

## 🛠️ Tech Stack

* **Language:** Python 3.12
* **Data Manipulation:** Pandas, NumPy
* **NLP:** Transformers (Hugging Face), NLTK/Regex
* **Machine Learning:** Scikit-Learn, XGBoost, Imbalanced-Learn (SMOTE)
* **Visualization:** Matplotlib, Seaborn, WordCloud

---

## 🚀 Quick Start

Ikuti langkah-langkah berikut untuk menjalankan proyek di lokal komputer Anda.

### 1. Clone Repository
Unduh source code ke direktori lokal Anda:
```bash
git clone [https://github.com/Davino-Edric/pertalite-sentiment-analysis]
cd pertalite-sentiment-analysis

###  2. Setup Environment
Sangat disarankan untuk menggunakan *Virtual Environment* agar dependencies tidak bentrok dengan sistem utama Anda.

```bash
# (Opsional) Buat virtual environment
python -m venv venv

# Aktifkan virtual environment
# Windows:
.\venv\Scripts\activate
# Mac/Linux:
source venv/bin/activate

# Install seluruh library yang dibutuhkan
pip install pandas numpy seaborn matplotlib scikit-learn \
            transformers torch wordcloud imbalanced-learn xgboost

### 3. Jalankan Analysis
jupyter notebook pertalite.ipynb
