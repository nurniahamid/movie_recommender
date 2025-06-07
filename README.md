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
1. Movie: merupakan data film 
- Jumlah data film = 9742
- Variabel dalam data movies :
  - movieId = merupakan id unik untuk setiap film
  - title = judul film disertai dengan tahun rilis didalam tanda kurung ()
  - genres = berisi genre film, bisa lebih dari satu genre
- Jumlah kombinasi Genre unik :  951
- Kondisi data :
    - Missing values: Tidak ada (0)
    - Data duplikat: Tidak ditemukan (0)
    - Outlier: Tidak relevan pada jenis data ini
 
2. ratings : merupakan data rating pengguna terhadap film
- Banyaknya data rating :  610
- Banyaknya film yang di rating :  banyaknya data rating
- Variabel dalam data ratings :
  - userId : id unik dari user /pengguna
  - movieId : ID unik untuk setiap film yang dirating
  - rating : Nilai rating yang diberikan pengguna terhadap film tertentu.
  - timestamp : Waktu ketika rating diberikan
- Kondisi Data :
  - Missing values: Tidak ada (0)
  - Data duplikat: Tidak ditemukan (0)
  - Outlier: Tidak ditemukan (semua rating valid antara 0.5 – 5.0)

## Data Preparation 

Pada tahap ini, dilakukan serangkaian proses persiapan data yang penting untuk mendukung dua pendekatan sistem rekomendasi: Content-Based Filtering dan Collaborative Filtering. Proses dilakukan secara terstruktur agar data siap digunakan dalam tahap modeling.

#### Content-Based Filtering 

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

#### Collaborative Filtering

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
Content-Based Filtering merekomendasikan film berdasarkan kemiripan kontennya. Dalam konteks ini, konten yang digunakan adalah genre film.

Langkah-Langkah:
- TF-IDF Vectorization
Genre film diubah menjadi representasi numerik menggunakan teknik TF-IDF. Setiap kombinasi genre diperlakukan sebagai dokumen, lalu dikonversi menjadi vektor berbobot.
- Cosine Similarity
Untuk mengukur tingkat kemiripan antar film, digunakan cosine similarity pada vektor TF-IDF yang telah dibentuk.
- Mendapatkan Rekomendasi Film
Model akan mencari film yang memiliki nilai similarity tertinggi (Top-N) terhadap film input yang diberikan.
Contoh Output : Ketika pengguna menyukai film "Waterboy, The (1998)", sistem akan merekomendasikan 5 film yang memiliki genre paling mirip.
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

Selain pendekatan berbasis konten (Content-Based Filtering), proyek ini juga mengimplementasikan pendekatan Collaborative Filtering, khususnya item-based collaborative filtering menggunakan Pearson Correlation. Pendekatan ini menganalisis pola rating dari pengguna yang sama terhadap film yang berbeda, untuk mengetahui kemiripan antar film.
Langkah-langkah Modeling:
1. Membuat User-Movie Matrix
Pertama-tama, hanya film yang populer (dengan minimal 50 rating) yang dipertahankan. Kemudian dibuat pivot table dengan:
- Baris: userId
- Kolom: title (judul film)
- Nilai: rating yang diberikan user terhadap film
2. Menghitung Similarity antar Film
Selanjutnya, dihitung Pearson correlation antar film, menggunakan fungsi .corr() dengan min_periods=5, agar korelasi hanya dihitung antara film-film yang setidaknya memiliki 5 user yang sama-sama memberi rating.
Nilai korelasi:
- 1.0 → rating dua film sangat mirip
- 0.0 → tidak berkorelasi
- < 0.0 → berkorelasi negatif
3. Mendapatkan Rekomendasi Film
Fungsi recommend_movies() digunakan untuk mencari 5 film yang memiliki korelasi tertinggi terhadap film input. Film yang sama dengan input akan dihapus dari hasil rekomendasi.
4. Contoh Rekomendasi
Sebagai contoh, berikut adalah hasil ketika pengguna menyukai "Forrest Gump (1994)":

![image](https://github.com/user-attachments/assets/fc98a3c4-ac5c-4032-9af1-2acb093b66ec)

#### Kelebihan dan Kekurangan
Aspek	Collaborative Filtering (Item-Based)
- Kelebihan :
  - Tidak bergantung pada konten film
  - Bisa menangkap pola preferensi tersembunyi antar pengguna
- Kekurangan	:
  - Membutuhkan data rating yang cukup banyak
  - Rentan terhadap cold-start problem untuk film baru

  ## Evaluation 

1. Metrik Evaluasi yang Digunakan
Dalam sistem rekomendasi ini , ada beberapa metrik yang digunakan :

a. Cosine Similarity (untuk Content-Based Filtering)
  - Cosine similarity mengukur kemiripan antara dua item berdasarkan vektor fitur (dalam kasus ini: genre TF-IDF).
  - Rumus:

    ![image](https://github.com/user-attachments/assets/cbe87e03-e7e2-44ae-a103-4d4a1b96403e)
  - Nilainya berkisar dari 0 (tidak mirip) hingga 1 (sangat mirip).
  - Dalam proyek ini, digunakan untuk menghitung seberapa mirip film A dengan film B, sehingga bisa direkomendasikan.

b. Pearson Correlation (untuk Collaborative Filtering)
  - Digunakan untuk mengukur kemiripan pola rating antara dua film berdasarkan rating pengguna.
  - Rumus :

    ![image](https://github.com/user-attachments/assets/20315841-03ff-4c73-bb71-6a9c8243edf3)
  - Nilai +1: sangat mirip, 0: tidak berkorelasi, -1: sangat berlawanan.
  - Dalam proyek ini, digunakan untuk melihat apakah dua film memiliki pola rating yang serupa oleh user-user yang sama, dan digunakan sebagai dasar dalam item-based collaborative filtering.

### Hasil Evaluasi 
1. Content-Based Filtering
- Menghasilkan rekomendasi yang relevan secara konten karena menggunakan kemiripan genre.
- Contoh: Film "Waterboy, The (1998)" menghasilkan rekomendasi film dengan genre serupa (comedy, sports).
- Cosine similarity menunjukkan nilai tinggi (> 0.7) untuk film-film yang memang sangat mirip secara konten.

2.  Collaborative Filtering
- Memberikan hasil rekomendasi berdasarkan pola kesukaan banyak pengguna.
- Misal: Untuk film "Forrest Gump (1994)", film-film yang direkomendasikan memiliki korelasi > 0.5, artinya banyak user yang menyukai Forrest Gump juga menyukai film-film tersebut.
- Pearson correlation digunakan untuk memastikan hanya film dengan jumlah user overlap ≥ 5 yang dihitung, demi menghindari overfitting.



























































































































































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
