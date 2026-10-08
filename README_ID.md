# Analisis Stabilitas Model Machine Learning

🇮🇩 Bahasa Indonesia | [🇬🇧 English](README.md)

> Analisis eksperimental terhadap kestabilan Decision Tree, K-Nearest Neighbors, dan Random Forest ketika pembagian data training dan testing divariasikan.

---

## 📌 Gambaran Umum

Model Machine Learning sering kali dievaluasi hanya menggunakan satu kali pembagian data training dan testing.

Namun, perubahan random seed pada proses pembagian data dapat mengubah sampel mana yang masuk ke data training dan mana yang masuk ke data testing.

Akibatnya, performa model dapat berubah walaupun dataset, algoritma, dan konfigurasi model yang digunakan tetap sama.

Project penelitian ini bertujuan untuk menganalisis **kestabilan model Machine Learning terhadap variasi pembagian data training dan testing**.

Pertanyaan utama penelitian ini adalah:

> **Seberapa besar performa model berubah ketika pembagian data training dan testing divariasikan menggunakan random seed yang berbeda?**

Tiga algoritma klasifikasi yang digunakan:

- Decision Tree
- K-Nearest Neighbors (KNN)
- Random Forest

Eksperimen dilakukan menggunakan tiga dataset klasifikasi publik:

- Wine Dataset
- Breast Cancer Wisconsin Diagnostic Dataset
- Raisin Dataset

---

## 🎯 Tujuan Penelitian

Tujuan utama penelitian ini adalah:

1. Mengevaluasi performa model pada berbagai pembagian data training dan testing.
2. Mengukur variasi akurasi model akibat perubahan random seed.
3. Membandingkan kestabilan Decision Tree, KNN, dan Random Forest.
4. Mengetahui apakah model dengan rata-rata akurasi tertinggi juga merupakan model yang paling stabil.
5. Mengamati apakah kestabilan model berubah pada dataset dengan karakteristik yang berbeda.

---

## 🧪 Desain Eksperimen

Setiap dataset diuji menggunakan prosedur eksperimen yang sama.

### Pembagian Data

```text
Data training : 80%
Data testing  : 20%
```

Stratifikasi digunakan untuk menjaga proporsi kelas antara data training dan testing.

### Random Seed

Setiap eksperimen dilakukan menggunakan:

```text
random_state = 1 sampai 30
```

Nilai `random_state` pada proses train-test split divariasikan, sedangkan konfigurasi utama model dibuat tetap.

Dengan metode ini, eksperimen difokuskan pada pengaruh perubahan komposisi data training dan testing.

### Model

| Model | Konfigurasi Utama |
|---|---|
| Decision Tree | Random state internal model dibuat tetap |
| KNN | K = 5 |
| Random Forest | 100 tree, random state internal model dibuat tetap |

`StandardScaler` digunakan pada KNN karena algoritma KNN menggunakan perhitungan jarak dan sensitif terhadap perbedaan skala fitur.

---

## 📊 Dataset

### 1. Wine Dataset

Dataset klasifikasi multiclass yang berisi pengukuran kimia dari sampel wine.

```text
Jumlah sampel : 178
Jumlah fitur  : 13
Jumlah kelas  : 3
```

---

### 2. Breast Cancer Wisconsin Diagnostic Dataset

Dataset klasifikasi biner yang berisi fitur numerik yang berasal dari karakteristik inti sel.

```text
Jumlah sampel : 569
Jumlah fitur  : 30
Jumlah kelas  : 2
```

---

### 3. Raisin Dataset

Dataset klasifikasi biner yang berisi karakteristik geometris dari dua jenis kismis, yaitu **Kecimen** dan **Besni**.

```text
Jumlah sampel : 900
Jumlah fitur  : 7
Jumlah kelas  : 2
```

---

## 📏 Metrik Evaluasi

Performa dan kestabilan model dievaluasi menggunakan:

- Mean Accuracy
- Minimum Accuracy
- Maximum Accuracy
- Median Accuracy
- Standard Deviation
- Accuracy Range

### Standard Deviation

Standard deviation digunakan untuk melihat seberapa besar hasil akurasi menyebar dari nilai rata-rata.

Nilai standard deviation yang lebih kecil menunjukkan bahwa performa model lebih konsisten pada berbagai variasi train-test split.

### Accuracy Range

```text
Range = Maximum Accuracy - Minimum Accuracy
```

Range yang lebih kecil menunjukkan bahwa perbedaan antara performa terbaik dan terburuk lebih kecil.

---

# 📈 Hasil Sementara

## Wine Dataset

| Model | Mean Accuracy | Minimum | Maximum | Std. Dev. | Range |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 90.93% | 80.56% | 97.22% | ~3.86% | 16.66% |
| KNN | 96.57% | 91.67% | 100.00% | 2.15% | 8.33% |
| Random Forest | **98.61%** | 94.44% | 100.00% | **1.59%** | **5.56%** |

Pada Wine Dataset, Random Forest menghasilkan rata-rata akurasi tertinggi sekaligus menunjukkan kestabilan terbaik.

---

## Breast Cancer Wisconsin Diagnostic Dataset

| Model | Mean Accuracy | Minimum | Maximum | Std. Dev. | Range |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 92.92% | 86.84% | 97.37% | 3.17% | 10.53% |
| KNN | **97.16%** | 92.98% | 99.12% | 1.41% | 6.14% |
| Random Forest | 96.32% | **93.86%** | 99.12% | **1.39%** | **5.26%** |

KNN memiliki rata-rata akurasi tertinggi, sedangkan Random Forest menunjukkan kestabilan yang sedikit lebih baik.

---

## Raisin Dataset

| Model | Mean Accuracy | Minimum | Maximum | Std. Dev. | Range |
|---|---:|---:|---:|---:|---:|
| Decision Tree | 80.74% | 75.00% | 84.44% | 2.46% | 9.44% |
| KNN | 85.00% | 81.11% | 89.44% | **2.24%** | **8.33%** |
| Random Forest | **85.85%** | 81.11% | **90.00%** | 2.39% | 8.89% |

Random Forest menghasilkan rata-rata akurasi tertinggi, sedangkan KNN menunjukkan kestabilan yang sedikit lebih baik.

---

## 🔎 Pengamatan Sementara

Hasil eksperimen menunjukkan bahwa performa model dapat berubah ketika pembagian data training dan testing berubah.

Eksperimen juga menunjukkan bahwa:

> **Model dengan rata-rata akurasi tertinggi belum tentu merupakan model yang paling stabil.**

Pengamatan sementara:

```text
Wine
Random Forest → Akurasi tertinggi + stabilitas terbaik

Breast Cancer
KNN           → Rata-rata akurasi tertinggi
Random Forest → Stabilitas terbaik

Raisin
Random Forest → Rata-rata akurasi tertinggi
KNN           → Stabilitas terbaik
```

Hasil ini masih bersifat sementara dan tidak dapat digunakan sebagai kesimpulan universal mengenai algoritma yang diuji.

Hasil hanya menggambarkan dataset dan konfigurasi eksperimen yang digunakan pada penelitian ini.

---

## 📁 Struktur Project

```text
ML-Stability-Research/
│
├── data/
│
├── notebooks/
│   ├── wine/
│   │   ├── 01_wine_exploration.ipynb
│   │   ├── 02_wine_decision_tree.ipynb
│   │   ├── 03_wine_knn.ipynb
│   │   ├── 04_wine_random_forest.ipynb
│   │   └── 05_wine_comparison.ipynb
│   │
│   ├── breast_cancer/
│   │   ├── 01_breast_cancer_exploration.ipynb
│   │   ├── 02_breast_cancer_decision_tree.ipynb
│   │   ├── 03_breast_cancer_knn.ipynb
│   │   ├── 04_breast_cancer_random_forest.ipynb
│   │   └── 05_breast_cancer_comparison.ipynb
│   │
│   └── raisin/
│       ├── 01_raisin_exploration.ipynb
│       ├── 02_raisin_decision_tree.ipynb
│       ├── 03_raisin_knn.ipynb
│       ├── 04_raisin_random_forest.ipynb
│       └── 05_raisin_comparison.ipynb
│
├── results/
│   ├── wine/
│   ├── breast_cancer/
│   └── raisin/
│
├── src/
│
├── README.md
├── README_ID.md
├── requirements.txt
└── .gitignore
```

---

## 🔄 Alur Penelitian

```text
Dataset
   │
   ▼
Eksplorasi Data
   │
   ▼
Pemisahan Fitur dan Label
   │
   ▼
30 Train-Test Split
(random_state 1–30)
   │
   ├───────────────┬───────────────┐
   ▼               ▼               ▼
Decision Tree     KNN        Random Forest
   │               │               │
   └───────────────┴───────────────┘
                   │
                   ▼
             Hasil Akurasi
                   │
                   ▼
           Analisis Statistik
                   │
        ┌──────────┼──────────┐
        ▼          ▼          ▼
       Mean      Std Dev     Range
                   │
                   ▼
        Perbandingan Stabilitas
```

---

## ✅ Progress

### Eksperimen Dataset

- [x] Wine Dataset
  - [x] Eksplorasi data
  - [x] Decision Tree
  - [x] KNN
  - [x] Random Forest
  - [x] Perbandingan model

- [x] Breast Cancer Wisconsin Diagnostic Dataset
  - [x] Eksplorasi data
  - [x] Decision Tree
  - [x] KNN
  - [x] Random Forest
  - [x] Perbandingan model

- [x] Raisin Dataset
  - [x] Eksplorasi data
  - [x] Decision Tree
  - [x] KNN
  - [x] Random Forest
  - [x] Perbandingan model

### Analisis Penelitian

- [ ] Perbandingan lintas dataset
- [ ] Visualisasi gabungan
- [ ] Penambahan metrik evaluasi
- [ ] Analisis signifikansi statistik
- [ ] Interpretasi kestabilan model
- [ ] Perbandingan dengan penelitian terdahulu

### Artikel Ilmiah

- [ ] Pendahuluan
- [ ] Tinjauan pustaka
- [ ] Metodologi
- [ ] Hasil
- [ ] Pembahasan
- [ ] Kesimpulan
- [ ] Formatting manuskrip
- [ ] Review akhir
- [ ] Submission jurnal

---

## 🛠 Teknologi

Project ini menggunakan:

- Python
- Pandas
- NumPy
- Scikit-learn
- Matplotlib
- Jupyter Notebook
- Visual Studio Code
- Git
- GitHub

---

## ⚙️ Setup Environment

Membuat virtual environment:

```bash
python3 -m venv .venv
```

Aktifkan virtual environment pada Linux:

```bash
source .venv/bin/activate
```

Install dependency:

```bash
pip install -r requirements.txt
```

Jalankan Jupyter Lab:

```bash
jupyter lab
```

---

## ♻️ Reproducibility

Eksperimen dirancang agar dapat direproduksi.

Parameter penting seperti:

- Rasio train-test
- Random seed
- Konfigurasi model
- Prosedur preprocessing

didokumentasikan dan dibuat konsisten selama eksperimen.

Hasil mentah eksperimen juga disimpan dalam format CSV di dalam folder `results/`.

---

## 🚧 Status Penelitian

> **Status: Penelitian Aktif / Work in Progress**

Hasil saat ini masih bersifat sementara dan dapat dikembangkan atau diperbaiki sebelum penyusunan manuskrip ilmiah final.

Tahap selanjutnya dapat mencakup penambahan metrik evaluasi, pengujian statistik, analisis lintas dataset, dan validasi metodologi penelitian.

---

## 👤 Penulis

**Fatih Hikmal Rafi**

Mahasiswa S1 Teknik Informatika

Bidang minat:

- Machine Learning
- Embedded Systems
- TinyML
- Internet of Things
- Reproducible Machine Learning

---

## ⚠️ Disclaimer

Repository ini dibuat untuk kepentingan akademik dan penelitian.

Breast Cancer Wisconsin Diagnostic Dataset hanya digunakan sebagai benchmark Machine Learning.

Eksperimen dalam repository ini **tidak ditujukan untuk memberikan diagnosis medis maupun rekomendasi klinis**.
