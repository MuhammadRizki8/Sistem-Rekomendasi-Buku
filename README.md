# Laporan Proyek Machine Learning - Muhammad Rizki

## 📘 Project Overview
![image](https://github.com/user-attachments/assets/d3bfee7b-e05e-49ba-b032-a48e35d4f3ad)

Buku merupakan salah satu media yang sangat penting dalam penyebaran pengetahuan dan hiburan. Dengan jutaan judul buku yang tersedia di seluruh dunia, pembaca sering mengalami kesulitan dalam menemukan buku baru yang sesuai dengan minat mereka. Fenomena ini dikenal sebagai *information overload*, yaitu ketika terlalu banyak pilihan tersedia sehingga proses pengambilan keputusan menjadi sulit dan membingungkan.

Di era digital saat ini, platform daring untuk penjualan dan ulasan buku seperti **Goodreads**, **Amazon**, maupun platform lokal Indonesia seperti **Gramedia Digital** menghadapi tantangan besar dalam memberikan rekomendasi yang tepat dan personal kepada setiap penggunanya. Sistem rekomendasi yang efektif tidak hanya memudahkan pembaca menemukan buku sesuai preferensi, tetapi juga meningkatkan pengalaman membaca dan loyalitas pengguna terhadap platform.

Penelitian oleh Isinkaye et al. (2015) menunjukkan bahwa sistem rekomendasi yang dirancang dengan baik dapat meningkatkan kepuasan pengguna hingga **27%** dan mendorong kenaikan konversi penjualan sebesar **35%** [1]. Selain itu, laporan dari McKinsey & Company menyatakan bahwa sekitar **35% dari pembelian di Amazon** berasal dari sistem rekomendasi otomatis mereka [2]. Fakta-fakta ini memperkuat pentingnya pengembangan sistem rekomendasi dalam konteks literatur digital.

Proyek ini menjadi relevan dan penting untuk diselesaikan karena berpotensi memberikan solusi terhadap masalah pencarian buku yang relevan dan personal bagi pengguna. Dengan mempertimbangkan aspek-aspek seperti **rating**, **genre**, **penulis**, dan **pola interaksi pengguna sebelumnya**, sistem ini diharapkan mampu meningkatkan keterlibatan pengguna (*user engagement*) sekaligus mendukung pertumbuhan bisnis platform buku digital melalui peningkatan penjualan.

### 🔖 Referensi

[1] Isinkaye, F. O., Folajimi, Y. O., & Ojokoh, B. A. (2015). Recommendation systems: Principles, methods and evaluation. *Egyptian Informatics Journal*, 16(3), 261–273. [https://doi.org/10.1016/j.eij.2015.06.005](https://doi.org/10.1016/j.eij.2015.06.005)

[2] McKinsey & Company. (2013). *How retailers can keep up with consumers*. [https://www.mckinsey.com/industries/retail/our-insights/how-retailers-can-keep-up-with-consumers](https://www.mckinsey.com/industries/retail/our-insights/how-retailers-can-keep-up-with-consumer)

---

## 🧠 Business Understanding

Bagian ini menjelaskan proses klarifikasi masalah yang mendasari proyek sistem rekomendasi buku, serta tujuan dan pendekatan solusi yang akan digunakan.

### 🔍 Problem Statements

1. **Bagaimana cara membantu pembaca menemukan buku baru yang sesuai dengan minat mereka di tengah jutaan pilihan buku yang tersedia?**
2. **Bagaimana cara memberikan rekomendasi buku yang personal berdasarkan preferensi historis pembaca dan pembaca lain yang memiliki selera serupa?**

### 🎯 Goals

1. **Membangun sistem rekomendasi** yang dapat menyarankan buku-buku yang mungkin disukai oleh pembaca berdasarkan kesamaan konten atau fitur dari buku yang telah mereka baca atau sukai sebelumnya.
2. **Mengembangkan sistem rekomendasi** yang dapat memanfaatkan pola rating dari banyak pengguna untuk memberikan rekomendasi yang lebih personal dan relevan.

### 🛠️ Solution Statements

Untuk mencapai tujuan dan menjawab permasalahan di atas, dua pendekatan sistem rekomendasi yang akan digunakan dalam proyek ini adalah:

#### 1. Content-Based Filtering

- Menganalisis atribut atau fitur buku seperti **genre**, **penulis**, **jumlah halaman**, dan **tahun terbit** untuk memberikan rekomendasi yang serupa dengan buku-buku yang telah disukai pengguna.
- Menggunakan teknik seperti **TF-IDF** dan **Cosine Similarity** untuk menghitung kesamaan antar buku.
- Cocok untuk **pengguna baru (cold-start problem)** karena tidak membutuhkan data pengguna lain.
- Digunakan untuk menjawab **Problem Statement #1** dan mendukung **Goal #1**.

#### 2. Collaborative Filtering

- Menggunakan data rating dari banyak pengguna untuk menemukan **pola kesamaan preferensi** antar pengguna.
- Menerapkan algoritma **Matrix Factorization** seperti **SVD (Singular Value Decomposition)** atau pendekatan berbasis **Neural Collaborative Filtering**.
- Efektif dalam memberikan rekomendasi berbasis perilaku kolektif pengguna, bahkan jika tidak ada kesamaan fitur antar item.
- Digunakan untuk menjawab **Problem Statement #2** dan mendukung **Goal #2**.


Dengan kombinasi dua pendekatan ini, sistem diharapkan mampu memberikan rekomendasi yang **personal, relevan, dan akurat** kepada pengguna, serta meningkatkan pengalaman dan keterlibatan mereka dalam platform literasi digital.


---

## 📊 Data Understanding

Untuk proyek ini, saya menggunakan dataset **"Goodreads Books"** yang tersedia di [Kaggle](https://www.kaggle.com/datasets/zygmunt/goodbooks-10k). Dataset ini berisi informasi mengenai lebih dari 11.000 buku yang disertai dengan detail metadata dan rating dari pengguna di platform Goodreads.
```
# Download dataset
!kaggle datasets download -d jealousleopard/goodreadsbooks
```
Dataset terdiri dari **11.123 entri** dan memiliki **12 kolom fitur** yang relevan untuk keperluan sistem rekomendasi, baik berbasis konten (content-based) maupun kolaboratif (collaborative filtering).

### 📄 Struktur dan Deskripsi Variabel

Berikut adalah deskripsi dari masing-masing kolom dalam dataset:

- **bookID**: ID unik untuk masing-masing buku.
- **title**: Judul buku.
- **authors**: Nama penulis atau penulis bersama dari buku tersebut.
- **average_rating**: Rata-rata rating dari semua pengguna Goodreads.
- **isbn**: ISBN versi 10 digit.
- **isbn13**: ISBN versi 13 digit.
- **language_code**: Kode bahasa buku (misalnya 'eng' untuk bahasa Inggris).
- **num_pages**: Jumlah halaman dalam buku.
- **ratings_count**: Jumlah total rating yang diterima oleh buku.
- **text_reviews_count**: Jumlah total ulasan teks yang ditinggalkan pengguna.
- **publication_date**: Tanggal publikasi buku (format teks).
- **publisher**: Nama penerbit buku.

### 🔍 Hasil Eksplorasi Awal (EDA)
<table>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/76c45878-708b-43c9-9d95-079d3ca5c0c7" width="100%"></td>
    <td><img src="https://github.com/user-attachments/assets/3abacdb2-0e7b-4f7f-85b7-d69e851cc271" width="100%"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/1b33dcaa-aa7e-4a9f-acc2-45fdd5f8a45c" width="100%"></td>
    <td><img src="https://github.com/user-attachments/assets/c3981da8-7d16-472f-a910-81eb3d3463a8" width="100%"></td>
  </tr>
  <tr>
    <td><img src="https://github.com/user-attachments/assets/7eeb0d04-39d9-40e3-b9b5-f7f4a52ad23e" width="100%"></td>
    <td><img src="https://github.com/user-attachments/assets/c5166c6f-e93a-48f3-90fb-b53afe59c4bc" width="100%"></td>
  </tr>
</table>

Dari hasil eksplorasi data yang dilakukan, diperoleh beberapa insight penting:

1. Dataset berisi **lebih dari 11.000 buku** dengan fitur yang lengkap seperti judul, penulis, rating, dan jumlah ulasan.
2. **Tidak ditemukan missing values yang signifikan**, sehingga dataset siap digunakan tanpa pembersihan data besar.
3. Mayoritas buku dalam dataset menggunakan **bahasa Inggris** (berdasarkan `language_code`), menunjukkan adanya potensi bias bahasa.
4. Rata-rata rating buku cenderung **tinggi**, dengan nilai sekitar **3.9/5**, menunjukkan kecenderungan bias positif pada sistem rating.
5. Penulis terkenal seperti **Stephen King** dan **P.G. Wodehouse** memiliki banyak entri dalam dataset, yang bisa memengaruhi popularitas buku di sistem rekomendasi.
6. Tidak terdapat korelasi yang kuat antara **jumlah rating** dan **rating rata-rata**, yang mengindikasikan bahwa popularitas tidak selalu berbanding lurus dengan kualitas persepsi.
7. Sebagian besar buku diterbitkan **setelah tahun 2000**, menunjukkan dataset relatif modern.

### 📌 Kesimpulan

Dataset "Goodreads Books" ini memiliki cakupan data yang cukup kaya dan berkualitas untuk digunakan dalam pengembangan sistem rekomendasi buku. Kombinasi atribut kuantitatif dan kualitatif seperti rating, penulis, dan metadata lainnya memungkinkan untuk diterapkannya baik **content-based filtering** maupun **collaborative filtering**.


---
## 🧹 Data Preparation
| 🔢 Step | 🛠️ Tahapan                     | ✨ Deskripsi Singkat                                                                 |
|--------|----------------------------------|--------------------------------------------------------------------------------------|
| 1️⃣     | **Handling Missing Values**      | Menghapus data yang memiliki nilai kosong untuk menjaga kualitas dan konsistensi.   |
| 2️⃣     | **Feature Selection**            | Memilih fitur penting agar model lebih efisien dan tidak terlalu kompleks.          |
| 3️⃣     | **Feature Engineering**          | Membuat fitur baru seperti kategori panjang buku dan skor popularitas.              |
| 4️⃣     | **Text Cleaning & Preprocessing**| Membersihkan teks (lowercase, hapus karakter aneh, lemmatization, stopwords).       |
| 5️⃣     | **Feature Encoding**             | Mengubah teks ke vektor numerik dengan TF-IDF agar bisa digunakan oleh model ML.    |
| 6️⃣     | **Dataset Splitting**            | Membagi data jadi training dan testing set untuk evaluasi model secara objektif.    |

Pada tahap ini, dilakukan berbagai teknik persiapan data sebelum digunakan untuk melatih model rekomendasi. Teknik data preparation disusun secara berurutan sesuai implementasi pada notebook dan laporan:

### 1. Handling Missing Values
hanya ditemukan 2 missing values pada data publication year.

- **Alasan**:  
Missing value dihapus karena jumlahnya sangat sedikit dan tidak berdampak signifikan terhadap dataset. Tidak ditemukan data duplikat.


### 2. Feature Selection

- **Alasan**:
- Memilih fitur yang paling relevan untuk model dan meningkatkan efisiensi komputasi.
- Untuk **content-based filtering**, digunakan fitur seperti judul, penulis, bahasa, dll.
- Untuk **collaborative filtering**, dibuat **simulasi data rating** berdasarkan `average_rating` dan `ratings_count`, dengan penambahan noise acak untuk menciptakan variasi antar pengguna.

### 3. Feature Engineering

- **Tujuan**: Membuat fitur baru yang menangkap informasi penting yang tidak eksplisit.

- **Langkah-langkah**:
- Ekstraksi `publication_year` dari `publication_date`.
- Kategorisasi panjang buku: `length_category` (Very Short – Very Long).
- Kategorisasi rating: `rating_category` (Low, Medium, High).
- Ekstraksi `main_language` dari `language_code`.
- Identifikasi `potential_genre` melalui kata kunci pada judul.
- Perhitungan `popularity` berdasarkan `average_rating` dan `ratings_count`, dengan skala logaritmik dan pengelompokan kategori.

### 4. Text Cleaning and Preprocessing

- **Langkah-langkah**:
- Konversi teks ke **lowercase**.
- Menghapus karakter non-alfabet dan angka.
- **Tokenisasi** menggunakan `RegexpTokenizer`.
- Menghapus **stopwords** dalam bahasa Inggris.
- **Lemmatization** untuk menyederhanakan bentuk kata.
- Penggabungan fitur teks (judul, penulis, penerbit, genre, dsb.) ke dalam satu kolom: `book_content`, dengan pemberian **bobot khusus** untuk fitur penting.

- **Alasan**:  
Menstandarisasi data teks dan menghilangkan noise demi meningkatkan akurasi sistem rekomendasi berbasis konten.

### 5. Feature Encoding

- **Metode**: TF-IDF Vectorization  
> Shape of TF-IDF matrix: (11121, 5000)

- **Parameter penting**:
- `stop_words='english'`
- `max_features=5000`
- `min_df=5`
- `max_df=0.8`
- `ngram_range=(1, 2)`
- `norm='l2'`
- `sublinear_tf=True`

- **Alasan**:
- Mengubah teks menjadi bentuk numerik yang bisa diproses algoritma machine learning.
- Mengurangi noise dari kata umum dan kata langka.
- Menangkap konteks kata melalui bigram dan normalisasi vektor fitur.

### 6. Dataset Splitting

- **Jumlah data**:
- Training ratings: 10.100
- Testing ratings: 2.526

- **Alasan**:
- **Dataset dibagi** dengan rasio 80:20 (train:test) untuk evaluasi performa model secara objektif.
- `random_state=42` digunakan untuk menjaga konsistensi dan reproducibility.


---
## 🧠 Modeling

Pada tahap ini, sistem rekomendasi dibangun menggunakan dua pendekatan algoritma yang berbeda, yaitu **Content-Based Filtering** dan **Collaborative Filtering berbasis SVD (Singular Value Decomposition)**. Output dari sistem ini adalah rekomendasi top-N buku yang disesuaikan dengan preferensi pengguna atau konten buku.

### 1. Content-Based Filtering

Pendekatan ini menggunakan informasi dari fitur konten buku (seperti judul, penulis, genre, dll) dan menghitung kemiripan antar buku menggunakan **Cosine Similarity** dari representasi TF-IDF.
```python
cosine_sim = cosine_similarity(tfidf_matrix, tfidf_matrix)
content_based_recommendations = get_content_based_recommendations("harry potter order phoenix harry potter")
```
#### Tahapan:
- Melakukan pembersihan dan ekstraksi fitur dari data buku.
- Menghitung **Cosine Similarity Matrix** antar buku berdasarkan TF-IDF.
- Menggunakan nama buku sebagai input untuk menghasilkan rekomendasi buku yang paling mirip.
- Menambahkan fitur diversifikasi, seperti memilih buku dari penulis yang berbeda.

#### Output:
Top-10 rekomendasi buku berdasarkan kemiripan konten dengan buku input.
![image](https://github.com/user-attachments/assets/eeecc62f-5625-4dea-8dac-1fda9af3feb7)

**Kelebihan**:
- Tidak tergantung pada data pengguna lain.
- Cocok untuk pengguna baru (cold-start user).

**Kekurangan**:
- Tidak bisa merekomendasikan buku yang sangat berbeda dari yang pernah dipilih pengguna.
- Bergantung pada kualitas metadata buku (judul, deskripsi, dll).

### 2. Collaborative Filtering (SVD)
Pendekatan ini merekomendasikan buku berdasarkan **pola rating pengguna** lain menggunakan **Singular Value Decomposition (SVD)**.
```
model_data = build_svd_model(train_df)
recommendations_df, _ = get_improved_collaborative_recommendations(user_id=123, model_data=model_data, train_df=train_df, books_df=books_df)
```
#### Tahapan:
- Membangun matriks user-item dari data rating.
- Sentralisasi rating untuk mengurangi bias pengguna.
- Menggunakan TruncatedSVD untuk menemukan dimensi laten.
- Menghitung prediksi rating untuk buku yang belum diberi rating oleh pengguna.
- Menghasilkan top-N rekomendasi berdasarkan prediksi tertinggi, dengan penyesuaian untuk diversifikasi (berbagai penulis dan genre).

#### Output:
Top-10 rekomendasi buku berdasarkan prediksi rating tertinggi untuk pengguna tertentu.
![image](https://github.com/user-attachments/assets/aa93d7d1-1b97-463c-ad72-86d5d4701403)


**Kelebihan**:
- Bisa memberikan rekomendasi yang lebih personal dan mengejutkan.
- Efektif jika data interaksi pengguna cukup banyak.

**Kekurangan**:
- Tidak cocok untuk pengguna baru dengan interaksi sedikit (cold-start problem).
- Memerlukan data interaksi yang besar dan terstruktur.



---

## 📊 Evaluation

Evaluasi dilakukan untuk menilai performa dari dua pendekatan sistem rekomendasi: **Collaborative Filtering** dan **Content-Based Filtering**. Setiap pendekatan dievaluasi dengan metrik yang relevan berdasarkan output yang dihasilkan.

###  🧠 Collaborative Filtering Evaluation
#### a. Root Mean Square Error (RMSE)

RMSE adalah metrik yang mengukur perbedaan antara nilai prediksi dan nilai sebenarnya:

$$
\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2}
$$

- $y_i$ adalah rating sebenarnya  
- $\hat{y}_i$ adalah rating hasil prediksi  
- $n$ adalah jumlah total prediksi

RMSE memberikan bobot lebih tinggi pada error yang besar karena error dikuadratkan sebelum dirata-ratakan. Semakin kecil nilai RMSE, semakin akurat model.

#### b. Mean Absolute Error (MAE)

MAE mengukur rata-rata absolut dari error:

$$
\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|
$$

MAE lebih mudah diinterpretasi karena berada dalam skala yang sama dengan data asli. Seperti RMSE, nilai MAE yang lebih rendah menunjukkan model yang lebih baik.

### 2. 📚 Content-Based Filtering Evaluation
#### a. Cosine Similarity Score

Metrik ini mengukur kesamaan antara item yang direkomendasikan dengan item referensi:

$$
\text{Cosine Similarity} = \frac{\vec{A} \cdot \vec{B}}{||\vec{A}|| \cdot ||\vec{B}||}
$$

Nilai berkisar dari 0 hingga 1, di mana nilai yang lebih tinggi menunjukkan kesamaan yang lebih besar.

#### b. Coverage

Coverage mengukur proporsi katalog yang dapat direkomendasikan oleh sistem:

$$
\text{Coverage} = \frac{\text{Jumlah item yang direkomendasikan}}{\text{Total jumlah item}}
$$

Semakin tinggi nilai coverage, semakin banyak variasi item yang dapat dijangkau oleh sistem.

#### c. Diversity

Diversity mengukur keberagaman rekomendasi:

$$
\text{Diversity} = 1 - \frac{\sum_{i=1}^{n-1} \sum_{j=i+1}^{n} \text{similarity}(i, j)}{\binom{n}{2}}
$$

- $\text{similarity}(i, j)$ adalah kesamaan antara item *i* dan *j*  
- $n$ adalah jumlah rekomendasi

Semakin tinggi nilai diversity, semakin beragam rekomendasi yang diberikan sistem kepada pengguna.

### 📌 Ringkasan Evaluasi

| Pendekatan              | Metrik              | Nilai     |
|-------------------------|---------------------|-----------|
| Collaborative Filtering | RMSE                | 0.5753    |
|                         | MAE                 | 0.4631    |
| Content-Based Filtering | Avg Cosine Similarity | 0.0132  |
|                         | Coverage            | 0.0008    |
|                         | Author Diversity    | 0.3333    |
|                         | Genre Diversity     | 0.1111    |


### 📎 Kesimpulan

Hasil evaluasi menunjukkan perbedaan karakteristik dan performa dari dua pendekatan sistem rekomendasi yang digunakan:

#### ✅ Collaborative Filtering
- Memiliki performa prediktif yang **cukup akurat**, ditunjukkan oleh nilai **RMSE = 0.5753** dan **MAE = 0.4631**.
- Nilai ini menunjukkan bahwa sistem mampu memperkirakan rating pengguna terhadap buku dengan tingkat kesalahan yang relatif rendah.
- Cocok digunakan ketika tersedia data interaksi pengguna yang cukup, seperti banyaknya rating yang telah diberikan.

#### ⚠️ Content-Based Filtering
- Menunjukkan **kesamaan antar item (cosine similarity)** yang rendah (**0.0132**) dan **coverage yang sangat terbatas** (**0.0008**), artinya hanya sebagian kecil buku yang berhasil direkomendasikan.
- Hal ini bisa disebabkan oleh fitur konten yang terbatas atau kurang representatif, misalnya hanya berdasarkan judul atau deskripsi pendek.
- Meskipun begitu, pendekatan ini memiliki **nilai keberagaman (diversity)** yang relatif baik:
  - **Author Diversity = 0.3333**
  - **Genre Diversity = 0.1111**
- Artinya, ketika sistem berhasil memberikan rekomendasi, rekomendasi tersebut **tidak monoton dan cenderung beragam**.

#### 📝 Implikasi dan Saran
- **Collaborative Filtering** lebih direkomendasikan untuk konteks dengan data rating pengguna yang cukup, karena memberikan hasil prediksi yang lebih stabil dan akurat.
- **Content-Based Filtering** masih perlu ditingkatkan melalui:
  - Pengayaan fitur (seperti menggunakan metadata tambahan: genre, sinopsis, review, dll).
  - Teknik NLP untuk memahami konten deskripsi buku secara lebih mendalam.
  - Hybrid approach (menggabungkan kedua metode) untuk menyeimbangkan akurasi dan jangkauan rekomendasi.


---

**---Ini adalah bagian akhir laporan---**

_Catatan:_
- _Anda dapat menambahkan gambar, kode, atau tabel ke dalam laporan jika diperlukan. Temukan caranya pada contoh dokumen markdown di situs editor [Dillinger](https://dillinger.io/), [Github Guides: Mastering markdown](https://guides.github.com/features/mastering-markdown/), atau sumber lain di internet. Semangat!_
- Jika terdapat penjelasan yang harus menyertakan code snippet, tuliskan dengan sewajarnya. Tidak perlu menuliskan keseluruhan kode project, cukup bagian yang ingin dijelaskan saja.
