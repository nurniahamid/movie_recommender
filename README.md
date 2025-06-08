# Laporan Proyek Machine Learning - Nurnia Hamid 

## Project Overview

  Di era digital saat ini, informasi dan pilihan hiburan sangat melimpah , termasuk dalam industri perfilman. Platform seperti Netflix, Disney+, atau layanan streaming lainnya memiliki ribuan judul film yang membuat pengguna kesulitan dalam memilih tontonan yang sesuai dengan preferensi mereka. Oleh karena itu, sistem rekomendasi menjadi hal yang penting untuk membantu pengguna menemukan film relavan dan menarik bagi mereka. 
  
  Selain memberikan pengalaman pengguna yang lebih personal, sistem rekomendasi juga memberikan berbagai keuntungan strategis bagi industri hiburan dan teknologi, antara lain: Meningkatkan Retensi Pengguna, Mendorong Konsumsi Konten, Optimalisasi Promosi dan Distribusi, Pengambilan Keputusan Berbasis Data, Keunggulan Kompetitif. 
  
  Sistem rekomendasi membantu meningkatkan retensi dan konsumsi konten secara signifikan di platform hiburan digital seperti Netflix (Gómez-Uribe & Hunt, 2016). Selain itu, mereka juga memberikan keunggulan kompetitif dan mendukung pengambilan keputusan berbasis data dalam industri (Ricci et al., 2015; Jannach et al., 2021).


## Business Understanding
Banyak pengguna layanan streaming seperti Netflix atau Disney++ merasa kewalahankarena terlalu banyak pilihan film, sehingga mereka sulit menemukan film yang benar-benar sesuai dengan minat mereka. Hal ini dapat menurunkan kepuasan pengguna dan meningkatkan resiko churn (berhenti berlangganan).

Dari sisi bisnis, jika pengguna tidak menonton film secara aktif, maka perusahaan kehilangan potensi keuntungan, baik dari iklan, retensi pelanggan, maupun penggunaan konten.

Oleh karena itu, dibutuhkan sistem rekomendasi yang bisa membantu pengguna memilih film yang relevan berdasarkan preferensi mereka dan riwayat interaksi mereka sebelumnya.

### Problem Statements 
1. Bagaimana memberikan rekomendasi film yang relevan kepada pengguna berdasarkan preferensi genre yang dimilikinya?

2. Bagaimana menyarankan film kepada pengguna dengan mempertimbangkan perilaku pengguna lain yang memiliki preferensi serupa?

### Goals 
1. Membangun sistem rekomendasi berbasis konten (content-based filtering) yang dapat menganalisis informasi film (seperti genre) untuk memberikan saran yang sesuai dengan riwayat tontonan pengguna.
2. Mengembangkan sistem rekomendasi berbasis kolaboratif (collaborative filtering) yang dapat memberikan saran kepada pengguna berdasarkan pola rating dan preferensi dari pengguna lain yang serupa.

### Solution statements
Untuk menjawab masalah yang telah diidentifikasi, proyek ini menggunakan dua pendekatan utama:

1. Content-Based Filtering
Rekomendasi diberikan berdasarkan kemiripan konten film, khususnya genre, menggunakan TF-IDF dan cosine similarity. Cocok untuk pengguna baru karena tidak memerlukan data pengguna lain.

2. Collaborative Filtering (Item-Based)
Rekomendasi berdasarkan kesamaan pola rating antar film menggunakan Pearson correlation. Pendekatan ini memperhitungkan preferensi pengguna lain yang serupa, sehingga lebih personal dan variatif.

Kombinasi kedua pendekatan ini membuat sistem rekomendasi lebih fleksibel dan relevan bagi berbagai tipe pengguna.

## Data Understanding
Proyek ini menggunakan dataset rekomendasi film dari Kaggle yang dapat diakses melalui tautan berikut:
- Link Dataset : https://www.kaggle.com/datasets/gargmanas/movierecommenderdataset
- Data tersebut memiliki dua file csv yaitu : movies dan ratings
1. movies.csv
- Jumlah data : 9742 baris x 3 kolom
- Deskripsi variable : 
  - movieId = ID unik untuk setiap film
  - title = judul film disertai dengan tahun rilis didalam tanda kurung ()
  - genres = berisi genre film, bisa lebih dari satu genre
- Jumlah kombinasi Genre unik :  951
- Kondisi data :
    - Missing values: Tidak ada (0)
    - Data duplikat: Tidak ditemukan (0)
    - Outlier: Tidak relevan pada jenis data ini
 
2. ratings.css
- Jumlah Data : 100,836 baris × 4 kolo
- Deskripsi variable : 
  - userId : id unik dari user /pengguna
  - movieId : ID unik untuk setiap film yang dirating
  - rating : Nilai rating yang diberikan pengguna terhadap film tertentu.
  - timestamp : Waktu ketika rating diberikan
- Statistik tambahan :
  - Jumlah pengguna unik : 610
  - Jumlah film unik yang diberi rating : 9724
- Kondisi Data :
  - Missing values: Tidak ada (0)
  - Data duplikat: Tidak ditemukan (0)
  - Outlier: Tidak ditemukan (semua rating valid antara 0.5 – 5.0)

## Data Preparation 

Pada tahap ini, dilakukan serangkaian proses persiapan data yang penting untuk mendukung dua pendekatan sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering. Proses dilakukan secara terstruktur agar data siap digunakan dalam tahap modeling.

### Content-Based Filtering 

1. Menggabungkan data
Dataset movies.csv dan ratings.csv digabungkan berdasarkan movieId untuk menyatukan informasi film (title, genres) dengan interaksi pengguna (userId, rating).
2. Menghapus nilai null dan duplikasi
- Diperiksa dan tidak ditemukan nilai null pada data hasil penggabungan.
- Data duplikat berdasarkan movieId dihapus agar setiap film hanya muncul satu kali (hanya satu data unik per film untuk TF-IDF).
3. Membersihkan kolom genres
- Tanda pemisah | diganti dengan spasi ( ) agar genre dikenali sebagai token terpisah oleh TF-IDF.
- Genre seperti sci-fi diubah menjadi sci_fi untuk menjaga integritas kata majemuk.
4. TF-IDF Vectorization
- Mengubah kolom genres menjadi representasi numerik dengan TfidfVectorizer.
- Hasilnya adalah TF-IDF matrix dengan ukuran (n_film, n_genres) yang mewakili bobot genre untuk tiap film.
5. Membuat Dictionary dan DataFrame film unik
- Kolom movieId, title, dan genres dikonversi menjadi list untuk memudahkan pemetaan.
- Dibuat DataFrame baru movie_new yang hanya menyimpan film unik beserta informasi genre-nya.

### Collaborative Filtering

1. Filtering film populer
- Hanya film yang telah diberi rating oleh minimal 50 pengguna yang dipertahankan.
- Tujuannya adalah untuk menjaga keandalan perhitungan korelasi antar film.
2. Membuat User-Movie Matrix
- Dibentuk matriks dengan:
  - Baris: userId
  - Kolom: title (judul film)
  - Nilai: rating
Dilakukan dengan pivot_table().
3. Data hasil matrix digunakan untuk perhitungan similarity antar film dengan Pearson Correlation.




## Modeling 

### Content-Based Filtering
Sistem rekomendasi berbasis konten menggunakan informasi genre film untuk menentukan kesamaan antar film.
Langkah-Langkah:
- Fitur: Representasi film berdasarkan genre telah diubah menjadi bentuk numerik menggunakan TF-IDF Vectorizer (lihat bagian Data Preparation).
- Algoritma: Kemiripan antar film dihitung menggunakan Cosine Similarity terhadap vektor TF-IDF. Cosine Similarity mengukur sudut antara dua vektor genre film.
- Output Rekomendasi:
Fungsi movie_recommendations() mengembalikan Top-N film yang paling mirip dengan film yang diberikan sebagai input.
Misalnya, untuk input "Waterboy, The (1998)", sistem akan menampilkan 5 film lain dengan skor kemiripan tertinggi berdasarkan genre-nya
![image](https://github.com/user-attachments/assets/99f4b052-485f-48a9-85fa-354e22f303b7)

#### Kelebihan dan Kekurangan
Aspek	Content-Based Filtering
- Kelebihan	
  - Tidak memerlukan data pengguna lain (berbasis konten film itu sendiri)
  - Dapat memberikan rekomendasi pada pengguna baru (asalkan ada preferensi awal)
  - Tidak terpengaruh oleh sparsity (data rating yang jarang)
- Kekurangan 
  - Cenderung memberikan rekomendasi yang sempit (kurang bervariasi atau terlalu mirip)
  - Tidak bisa menangkap selera kolektif dari pengguna lain (misalnya tren)
  - Membutuhkan representasi fitur yang relevan dan baik dari item (dalam hal ini genre)




### Collaborative Filtering 

Selain pendekatan berbasis konten (Content-Based Filtering), proyek ini juga mengimplementasikan pendekatan Collaborative Filtering, khususnya item-based collaborative filtering menggunakan Pearson Correlation.Pendekatan ini merekomendasikan film berdasarkan pola perilaku pengguna lain.

- Fitur: Matriks interaksi antara user dan film telah dibentuk sebelumnya (lihat Data Preparation), di mana setiap sel menunjukkan rating yang diberikan user terhadap film.
- Algoritma: Sistem menggunakan Pearson Correlation untuk mengukur kemiripan antar film berdasarkan pola rating dari user-user yang sama.
Film yang sering diberi rating serupa oleh banyak pengguna dianggap mirip.
- Output Rekomendasi: Fungsi recommend_movies() akan mengembalikan daftar film dengan skor korelasi tertinggi terhadap film input.
Contoh: Jika pengguna menyukai "Forrest Gump (1994)", sistem merekomendasikan film lain seperti "Mr. Holland’s Opus (1995)" dan "Pocahontas (1995)" dengan skor korelasi > 0.5.

![image](https://github.com/user-attachments/assets/fc98a3c4-ac5c-4032-9af1-2acb093b66ec)

#### Kelebihan dan Kekurangan
Aspek	Collaborative Filtering (Item-Based)
- Kelebihan :
  - Tidak bergantung pada konten film
  - Bisa menangkap pola preferensi tersembunyi antar pengguna
- Kekurangan	:
  - Membutuhkan data rating yang cukup banyak
  - Rentan terhadap cold-start problem untuk film baru
 
#### Kesimpulan Modeling
Kedua pendekatan menunjukkan hasil yang konsisten dengan tujuan awal:
- Content-Based Filtering mampu menyarankan film yang serupa secara konten.
- Collaborative Filtering dapat menangkap preferensi kolektif pengguna dan memberikan saran berdasarkan selera umum.

  ## Evaluation

Metrik Evaluasi yang digunakan untuk mengukur performa sistem rekomendasi, digunakan metrik kuantitatif yang relavan terhadap task top_N recommendation, yaitu : 

1. Precision
   Precision mengukur proporsi rekomendasi yang relevan terhadap total rekomendasi yang diberikan. Contoh: Jika dari 5 film yang direkomendasikan, 4 memiliki genre yang relevan, maka Precision = 0.80
2. Recall
   Recall mengukur cakupan genre relevan yang berhasil direkomendasikan dari keseluruhan genre film input.

### Hasil Evaluasi 
1. Content-Based Filtering
- Evaluasi dilakukan terhadap 20 film acak dengan evaluate_genre_precision_recall().
- Rata-rata hasil evaluasi:
  - Precision = 0.96
  - Recall = 1.00

Hasil ini menunjukkan bahwa mayoritas film yang direkomendasikan memiliki genre yang sangat relevan dengan film input. Content-Based Filtering bekerja sangat baik untuk menyarankan film yang mirip dari segi konten.

2.  Collaborative Filtering
Evaluasi dilakukan terhadap beberapa film populer, salah satunya "Forrest Gump (1994)", dengan menggunakan pendekatan item-based collaborative filtering berbasis Pearson Correlation.
- Contoh hasil evaluasi:
  - Precision = 1.00
  - Recall = 0.75
- Hasil ini menunjukkan bahwa dari 5 rekomendasi yang diberikan:
  - 100% memiliki setidaknya satu genre yang cocok dengan genre film input.
  - Sistem berhasil mencakup 75% genre dari film Forrest Gump (1994) dalam rekomendasi yang diberikan.

Model Collaborative Filtering mampu memberikan rekomendasi yang relevan berdasarkan pola kesukaan pengguna lain, dan dalam kasus ini juga cukup sesuai secara konten (genre). Meskipun CF tidak melihat isi film secara langsung (seperti genre), sistem masih bisa menangkap hubungan implisit antar film berdasarkan pola rating. 

#### Hubungan dengan Business Understanding
- Evaluasi ini menjawab Problem Statement dan menunjukkan pencapaian Goals:
  - Content-Based Filtering mampu memberikan rekomendasi yang personal dan relevan, menjawab kebutuhan pengguna untuk menemukan film serupa yang disukai.
  - Collaborative Filtering menawarkan diversifikasi rekomendasi berdasarkan kebiasaan pengguna lain, yang memberi nilai tambah dari sisi pengalaman penemuan film baru.

- Secara keseluruhan, dua pendekatan ini saling melengkapi:
  - CBF baik untuk personalisasi berdasarkan selera konten.
  - CF kuat untuk eksplorasi tren komunitas dan selera kolektif.



























































































































































Referensi : 

1. Gómez-Uribe, C. A., & Hunt, N. (2016).
The Netflix recommender system: Algorithms, business value, and innovation.
ACM Transactions on Management Information Systems (TMIS), 6(4), 13.
https://doi.org/10.1145/2843948

2. Ricci, F., Rokach, L., & Shapira, B. (Eds.). (2015).
Recommender systems handbook.
Springer. https://doi.org/10.1007/978-1-4899-7637-6

3. Jannach, D., Adomavicius, G., & Tuzhilin, A. (2021).
Recommender systems: Challenges, insights and research opportunities.
ACM Transactions on Intelligent Systems and Technology (TIST), 12(1), 1–42.
https://doi.org/10.1145/3457607
