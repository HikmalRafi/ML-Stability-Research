# Analisis Stabilitas Model Machine Learning

[🇬🇧 English](README.md) | [🇮🇩 Bahasa Indonesia](README_ID.md)

Penelitian empiris mengenai pengaruh variasi pembagian data training dan testing terhadap performa klasifikasi, stabilitas model, dan konsistensi peringkat model.

---

## Gambaran Umum

Model Machine Learning sering dievaluasi menggunakan satu kali pembagian data training dan testing. Namun, komposisi data training dan testing dapat berubah ketika menggunakan random seed yang berbeda. Perubahan ini dapat menghasilkan estimasi performa yang berbeda dan, dalam beberapa kasus, mengubah kesimpulan mengenai model yang memiliki performa terbaik.

Penelitian ini menganalisis **stabilitas performa algoritma klasifikasi terhadap variasi pembagian data training dan testing**.

Eksperimen membandingkan tiga algoritma klasifikasi pada tiga dataset tabular dengan menggunakan 100 random seed untuk setiap dataset. Selain mengukur variasi performa, penelitian ini juga menganalisis apakah model dengan rata-rata skor tertinggi secara konsisten menempati peringkat pertama pada berbagai pembagian data.

### Pertanyaan Penelitian

> Bagaimana variasi pembagian data training dan testing memengaruhi estimasi performa klasifikasi, stabilitas model, dan konsistensi peringkat model?

## Tujuan Penelitian

- Mengukur performa klasifikasi pada berbagai pembagian data training dan testing.
- Membandingkan variasi performa menggunakan standard deviation dan range.
- Menganalisis apakah model dengan rata-rata performa tertinggi juga merupakan model yang paling stabil.
- Mengamati perubahan peringkat model pada berbagai pembagian data.
- Membandingkan kesimpulan berdasarkan metrik Accuracy dan Macro F1-score.
- Menyediakan alur eksperimen yang dapat direproduksi beserta file hasil yang dapat diakses.

## Desain Eksperimen

| Komponen | Konfigurasi |
|---|---|
| Dataset | 3 |
| Algoritma klasifikasi | 3 |
| Random seed | 100 per dataset |
| Total evaluasi model | 900 |
| Data training | 80% |
| Data testing | 20% |
| Metode pembagian data | Stratified train-test split |
| Metrik utama | Accuracy dan Macro F1-score |

Untuk setiap dataset, nilai seed yang sama digunakan pada proses pembagian data untuk ketiga algoritma. Konfigurasi model dipertahankan tetap sehingga eksperimen berfokus pada pengaruh perubahan komposisi data training dan testing.

### Algoritma Klasifikasi

| Algoritma | Konfigurasi |
|---|---|
| Decision Tree | `random_state=42` pada model |
| K-Nearest Neighbors (KNN) | `n_neighbors=5`, menggunakan `StandardScaler` dalam pipeline |
| Random Forest | `n_estimators=100`, `random_state=42` pada model |

Pada KNN, `StandardScaler` ditempatkan di dalam pipeline scikit-learn agar parameter penskalaan hanya dipelajari dari data training.

Konfigurasi model dipertahankan tetap selama eksperimen. Dengan demikian, penelitian berfokus pada variasi performa akibat perubahan pembagian data, bukan optimasi hyperparameter atau variasi random seed internal model.

## Dataset

Penelitian ini menggunakan tiga dataset klasifikasi publik.

| Dataset | Jumlah Sampel | Jumlah Fitur | Jumlah Kelas | Jenis Klasifikasi |
|---|---:|---:|---:|---|
| Wine | 178 | 13 | 3 | Multikelas |
| Breast Cancer Wisconsin Diagnostic | 569 | 30 | 2 | Biner |
| Raisin | 900 | 7 | 2 | Biner |

Dataset Wine berisi pengukuran kimia dari sampel wine. Dataset Breast Cancer Wisconsin Diagnostic berisi fitur numerik yang berasal dari karakteristik inti sel. Sementara itu, dataset Raisin berisi pengukuran geometris untuk mengklasifikasikan dua varietas kismis.

## Metrik Evaluasi

### Metrik Performa

- **Accuracy:** proporsi sampel yang berhasil diklasifikasikan dengan benar.
- **Macro F1-score:** rata-rata F1-score dari seluruh kelas dengan bobot yang sama untuk setiap kelas.

### Metrik Stabilitas

- **Mean:** rata-rata performa dari 100 pembagian data.
- **Median:** nilai tengah performa.
- **Standard deviation (SD):** mengukur variasi performa antarpembagian data.
- **Minimum dan maximum:** performa terendah dan tertinggi yang diamati.
- **Range:** selisih antara performa tertinggi dan terendah.

Standard deviation atau range yang lebih kecil menunjukkan variasi yang lebih rendah berdasarkan metrik tersebut. Kedua ukuran ini menggambarkan aspek stabilitas yang berbeda sehingga perlu ditafsirkan bersama rata-rata performa.

### Analisis Peringkat Model

Analisis final juga mencakup:

- **Winner counts:** frekuensi setiap model memperoleh skor tertinggi pada satu pembagian data, dengan hasil seri dicatat secara terpisah.
- **Rank reversal:** kondisi ketika pemenang pada satu pembagian data tidak mencakup model dengan rata-rata skor keseluruhan tertinggi.
- **Performance gap:** selisih antara skor tertinggi dan skor tertinggi kedua pada satu pembagian data.
- **Metric agreement:** kesesuaian model atau kelompok model dengan skor tertinggi berdasarkan Accuracy dan Macro F1.

## Hasil Eksperimen

Tabel berikut merangkum hasil eksperimen final yang tersedia saat ini. Performa, standard deviation, dan range ditampilkan dalam persen atau poin persentase, sesuai dengan jenis ukurannya.

### Hasil Accuracy

| Dataset | Model | Mean | SD | Range |
|---|---|---:|---:|---:|
| Breast Cancer | Decision Tree | 93,11% | 2,67 pp | 10,53 pp |
| Breast Cancer | KNN | **96,62%** | **1,49 pp** | **7,02 pp** |
| Breast Cancer | Random Forest | 96,18% | 1,62 pp | **7,02 pp** |
| Raisin | Decision Tree | 81,27% | 2,66 pp | 13,89 pp |
| Raisin | KNN | 85,17% | **2,14 pp** | **10,00 pp** |
| Raisin | Random Forest | **86,09%** | 2,25 pp | 11,11 pp |
| Wine | Decision Tree | 90,17% | 4,74 pp | 19,44 pp |
| Wine | KNN | 96,25% | 2,51 pp | 13,89 pp |
| Wine | Random Forest | **98,31%** | **1,89 pp** | **5,56 pp** |

### Hasil Macro F1-score

| Dataset | Model | Mean | SD | Range |
|---|---|---:|---:|---:|
| Breast Cancer | Decision Tree | 92,62% | 2,82 pp | 11,13 pp |
| Breast Cancer | KNN | **96,32%** | **1,64 pp** | 7,89 pp |
| Breast Cancer | Random Forest | 95,89% | 1,74 pp | **7,35 pp** |
| Raisin | Decision Tree | 81,24% | 2,67 pp | 14,10 pp |
| Raisin | KNN | 85,14% | **2,14 pp** | **10,00 pp** |
| Raisin | Random Forest | **86,07%** | 2,25 pp | 11,12 pp |
| Wine | Decision Tree | 90,27% | 4,75 pp | 20,18 pp |
| Wine | KNN | 96,35% | 2,48 pp | 13,70 pp |
| Wine | Random Forest | **98,36%** | **1,84 pp** | **5,59 pp** |

*pp = poin persentase. Angka tebal menunjukkan rata-rata tertinggi atau variasi terendah pada dataset dan metrik terkait, jika berlaku.*

### Temuan Utama

1. **Performa rata-rata tertinggi tidak selalu berarti variasi terendah.** Pada dataset Raisin, Random Forest memiliki rata-rata skor tertinggi, sedangkan KNN memiliki standard deviation dan range lebih rendah pada Accuracy maupun Macro F1.
2. **Hasil bergantung pada dataset.** Random Forest menghasilkan rata-rata skor tertinggi sekaligus variasi terendah pada dataset Wine. Sementara itu, KNN memperoleh rata-rata Accuracy tertinggi pada Breast Cancer.
3. **Penilaian stabilitas dapat bergantung pada metrik yang digunakan.** Pada Breast Cancer, KNN memiliki standard deviation lebih rendah, sedangkan Random Forest memiliki range Macro F1 yang sedikit lebih kecil.
4. **Peringkat model dapat berubah antarpembagian data.** Model dengan rata-rata skor keseluruhan tertinggi tidak selalu menjadi pemenang pada setiap pembagian data.

Temuan tersebut hanya berlaku untuk dataset dan konfigurasi yang diuji dalam penelitian ini. Hasilnya tidak dapat langsung digunakan untuk menetapkan peringkat universal bagi algoritma yang dibandingkan.

## Analisis Rank Reversal

Tabel berikut menunjukkan jumlah pembagian data ketika pemenang pada pembagian tersebut tidak mencakup model dengan rata-rata skor keseluruhan tertinggi.

| Dataset | Accuracy | Macro F1 |
|---|---:|---:|
| Breast Cancer | 31 / 100 | 46 / 100 |
| Raisin | 25 / 100 | 27 / 100 |
| Wine | 5 / 100 | 8 / 100 |

Jumlah tersebut tidak memasukkan kasus seri yang masih melibatkan model dengan rata-rata skor keseluruhan tertinggi. Hasil ini menunjukkan bahwa kesimpulan mengenai model terbaik dapat bergantung pada pembagian data tertentu, terutama pada beberapa dataset dan metrik evaluasi.

## Struktur Repository

```text
ML-Stability-Research/
├── data/
│   ├── raisin/
│   └── wine/
├── notebooks/
│   ├── final_experiment/
│   │   ├── 01_final_experiment.ipynb
│   │   └── 02_final_analysis.ipynb
│   ├── breast_cancer/
│   ├── raisin/
│   └── wine/
├── results/
│   ├── final/
│   ├── breast_cancer/
│   ├── raisin/
│   └── wine/
├── LICENSE
├── README.md
└── README_ID.md
```

Notebook pada masing-masing dataset berisi eksplorasi dan eksperimen sebelumnya. Notebook di dalam folder `final_experiment` menjadi titik awal utama untuk menjalankan dan menganalisis eksperimen final.

## Menjalankan Ulang Eksperimen

### 1. Clone Repository

```bash
git clone https://github.com/HikmalRafi/ML-Stability-Research.git
cd ML-Stability-Research
```

### 2. Buat Virtual Environment

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Pada Windows, aktifkan environment menggunakan:

```powershell
.venv\Scripts\activate
```

### 3. Instal Dependensi

```bash
python -m pip install --upgrade pip
python -m pip install numpy pandas scikit-learn matplotlib jupyter
```

### 4. Jalankan Notebook Secara Berurutan

1. Buka [`01_final_experiment.ipynb`](notebooks/final_experiment/01_final_experiment.ipynb), lalu jalankan seluruh sel untuk melakukan eksperimen.
2. Buka [`02_final_analysis.ipynb`](notebooks/final_experiment/02_final_analysis.ipynb), lalu jalankan seluruh sel untuk menganalisis hasil eksperimen.

Jalankan Jupyter Notebook dengan perintah:

```bash
jupyter notebook
```

Jalankan notebook eksperimen sebelum notebook analisis ketika ingin menghasilkan ulang hasil penelitian. Notebook analisis bergantung pada file output eksperimen.

Untuk reproduksibilitas yang lebih ketat, gunakan konfigurasi yang terdokumentasi serta versi dependensi yang sama dengan lingkungan eksperimen asli. Repository saat ini belum menyertakan file dependensi dengan versi yang dikunci.

## File Hasil dan Data

Hasil utama eksperimen tersimpan di folder [`results/final/`](results/final/).

| File | Deskripsi |
|---|---|
| [`final_experiment_results.csv`](results/final/final_experiment_results.csv) | Hasil mentah seluruh evaluasi model |
| [`accuracy_summary.csv`](results/final/accuracy_summary.csv) | Statistik deskriptif Accuracy |
| [`macro_f1_summary.csv`](results/final/macro_f1_summary.csv) | Statistik deskriptif Macro F1 |
| [`accuracy_win_counts.csv`](results/final/accuracy_win_counts.csv) | Jumlah pemenang dan seri berdasarkan Accuracy |
| [`f1_win_counts.csv`](results/final/f1_win_counts.csv) | Jumlah pemenang dan seri berdasarkan Macro F1 |
| [`accuracy_rank_reversal_summary.csv`](results/final/accuracy_rank_reversal_summary.csv) | Konsistensi peringkat berdasarkan Accuracy |
| [`f1_rank_reversal_summary.csv`](results/final/f1_rank_reversal_summary.csv) | Konsistensi peringkat berdasarkan Macro F1 |
| [`accuracy_gap_summary.csv`](results/final/accuracy_gap_summary.csv) | Selisih skor antarmodel teratas berdasarkan Accuracy |
| [`f1_gap_summary.csv`](results/final/f1_gap_summary.csv) | Selisih skor antarmodel teratas berdasarkan Macro F1 |

File CSV tambahan berisi analisis performance gap tanpa hasil seri, ringkasan selisih skor pemenang, dan perbandingan antara kedua metrik evaluasi.

## Keterbatasan Penelitian

Penelitian ini memiliki beberapa keterbatasan:

- Hanya tiga dataset dan tiga algoritma klasifikasi yang diuji.
- Hyperparameter model dipertahankan tetap dan tidak dioptimalkan secara terpisah untuk setiap dataset.
- Hasil mengukur variasi performa akibat perubahan pembagian data pada konfigurasi yang digunakan, bukan menetapkan peringkat algoritma yang berlaku secara universal.
- Pembagian data berulang dapat saling tumpang tindih sehingga hasilnya belum tentu merupakan 100 observasi yang independen. Skor tersebut tidak boleh diperlakukan sebagai 100 observasi independen untuk pengujian inferensial sederhana.
- Temuan dapat berbeda apabila menggunakan dataset, rasio pembagian data, metode preprocessing, konfigurasi model, atau prosedur evaluasi yang berbeda.

Keterbatasan tersebut perlu dipertimbangkan ketika menafsirkan hasil maupun mengembangkan penelitian lebih lanjut.

## Status Penelitian

**Tahap saat ini:** Output eksperimen final dan analisis deskriptif telah tersedia; penyusunan manuskrip ilmiah sedang dipersiapkan.

Ruang lingkup penelitian saat ini berfokus pada variasi performa akibat pembagian data, konsistensi peringkat model, serta hubungan antara rata-rata performa dan stabilitas. Perubahan metodologi selanjutnya sebaiknya dilakukan hanya jika menjawab pertanyaan penelitian yang jelas atau didasarkan pada masukan yang beralasan dari dosen pembimbing maupun reviewer.

## Penulis

**Fatih Hikmal Rafi**

Mahasiswa S1 Teknik Informatika

Bidang minat:
- Machine Learning
- Reproducible Machine Learning
- Embedded Systems
- TinyML
- Internet of Things

## Lisensi

Lihat file [`LICENSE`](LICENSE) untuk mengetahui ketentuan lisensi repository ini. Ketentuan lisensi masing-masing dataset dapat berbeda dari lisensi perangkat lunak repository.

## Disclaimer

Repository ini dibuat untuk kepentingan akademik dan penelitian. Dataset Breast Cancer Wisconsin Diagnostic hanya digunakan sebagai benchmark Machine Learning. Eksperimen ini **tidak ditujukan untuk diagnosis medis maupun pemberian rekomendasi klinis**.
