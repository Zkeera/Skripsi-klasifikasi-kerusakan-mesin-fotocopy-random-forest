# Laporan Proyek Machine Learning - Klasifikasi Kerusakan Mesin Fotocopy Canon iRA4051

## Domain Proyek

Notebook ini bertujuan untuk membangun model *machine learning* untuk mengklasifikasikan kerusakan mesin fotocopy Canon iRA4051 berdasarkan kode *error*, gejala kerusakan, dan komponen mesin. Model yang digunakan adalah *Random Forest Classifier* untuk membantu teknisi dalam mengidentifikasi jenis kerusakan secara sistematis.

## Business Understanding

### Problem Statements

* Bagaimana cara mengklasifikasikan jenis kerusakan mesin fotocopy berdasarkan data *error* dan gejala yang muncul?
* Fitur apa saja yang paling berkontribusi dalam menentukan kategori kerusakan mesin?

### Goals

* Membangun model klasifikasi untuk menentukan jenis kerusakan mesin fotocopy.
* Mengidentifikasi fitur (kode *error*, komponen, gejala) yang paling relevan dengan jenis kerusakan.

### Solution Statements

* Membangun model klasifikasi menggunakan algoritma **Random Forest Classifier**.
* Melakukan *preprocessing* data, *encoding* fitur kategorikal, dan ekstraksi fitur teks (*TF-IDF*).
* Menilai performa model menggunakan metrik klasifikasi seperti **Akurasi**, **Precision**, **Recall**, dan **F1-Score**.

## Data Understanding

Dataset yang digunakan berasal dari Canon Service Manual yang berisi daftar kode *error* mesin fotocopy Canon iRA4051. Dataset mencakup 568 entri dengan 10 atribut. Atribut utama yang digunakan sebagai fitur adalah `kode_error`, `gejala`, dan `komponen_utama`, sedangkan `jenis_kerusakan` (label) digunakan sebagai target klasifikasi.

Berikut adalah seluruh fitur yang tersedia dalam dataset:

| Nama Fitur | Deskripsi |
| --- | --- |
| id | Identitas unik data |
| **tanggal** | Tanggal pencatatan |
| **tipe_mesin** | Model mesin fotocopy |
| **kode_error** | Kode *error* spesifik |
| **gejala** | Deskripsi gejala kerusakan |
| **jenis_kerusakan** | Label target klasifikasi |
| **komponen_utama** | Komponen yang mengalami kerusakan |
| **tindakan_perbaikan** | Prosedur perbaikan |
| **konfirmasi_teknisi** | Status konfirmasi (Y/N) |
| **catatan** | Catatan tambahan |

## Data Preparation

Berikut adalah langkah-langkah *data preparation* yang dilakukan:

1. **Pembersihan Data**
Melakukan penyederhanaan nama kolom (contoh: `"gejala (pisahkan dengan ';')"` menjadi `"gejala"`) agar lebih mudah diproses dalam analisis.


2. **Encoding Fitur**

* **Label Encoding**: Digunakan untuk mengubah data kategorikal pada kolom `kode_error`, `komponen_utama`, `error_prefix`, dan target `jenis_kerusakan` menjadi format numerik.


* **TF-IDF Vectorizer**: Mengubah teks deskriptif pada kolom `gejala` menjadi fitur numerik agar dapat diproses oleh model.



3. **Pembagian Data**
Dataset dibagi menjadi data latih dan data uji dengan rasio **80:20** menggunakan `train_test_split()` dengan `random_state=42`.



## Modeling

### Algoritma yang digunakan:

* **Random Forest Classifier**
Alasan pemilihan:
* Metode *ensemble learning* yang mampu menangani dataset kompleks.
* *Robust* terhadap *noise* dan mengurangi risiko *overfitting*.
* Memberikan informasi *feature importance* yang transparan.



### Proses pelatihan:

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(n_estimators=200, random_state=42)
model.fit(X_train, y_train)

```

## Evaluation

### Model dievaluasi menggunakan metrik klasifikasi:

* **Akurasi: 0.9210 (92,11%)**
* **Cross Validation (CV) Score: Rata-rata 0.885**

Nilai akurasi yang tinggi menunjukkan bahwa model mampu memprediksi jenis kerusakan mesin dengan tingkat ketepatan yang baik. Evaluasi lebih mendalam melalui *classification report* menunjukkan performa yang stabil, meskipun terdapat variasi pada kelas dengan jumlah data yang lebih sedikit (*class imbalance*).

### Analisis Feature Importance:

| Fitur | Importance |
| --- | --- |
| **komponen_encoded** | 0.147398 |
| **error** | 0.076074 |
| **prefix_encoded** | 0.072627 |
| **kode_error_encoded** | 0.065445 |
| **in** | 0.064390 |

Fitur `komponen_encoded` merupakan variabel yang paling berpengaruh terhadap hasil prediksi model, diikuti oleh informasi terkait struktur kode *error*.

## Conclusion

Model *machine learning* berbasis *Random Forest* berhasil dibangun untuk mengklasifikasikan kerusakan mesin fotocopy. Model ini memiliki akurasi yang tinggi sebesar **92,11%** dan terbukti efektif dalam mengenali pola kerusakan.

**Menjawab Problem Statement:**

* Model mampu melakukan klasifikasi jenis kerusakan berdasarkan kode *error*, gejala, dan komponen mesin dengan akurat.


* Fitur **komponen mesin** dan **kode error** merupakan kontributor utama dalam menentukan jenis kerusakan mesin.



Sistem ini berpotensi digunakan sebagai pendukung diagnosis awal bagi teknisi dalam mempercepat proses identifikasi dan perbaikan kerusakan mesin fotocopy.
