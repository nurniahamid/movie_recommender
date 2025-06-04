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

## Data Understanding
Data yang digunakan adalah data movierecommenderdataset 
Link Dataset : https://www.kaggle.com/datasets/gargmanas/movierecommenderdataset
Data tersebut memiliki dua file csv yaitu : movies dan ratings
1. Movie: merupakan data film 
- Jumlah data film = 9742
- Variabel dalam data movies :
  - movieId = merupakan id unik untuk setiap film
  - title = judul film disertai dengan tahun rilis didalam tanda kurung ()
  - genres = berisi genre film, bisa lebih dari satu genre
- Jumlah Jenis Genre film :  951
 
2. ratings : merupakan data rating pengguna terhadap film
- Banyaknya data rating :  610
- Banyaknya film yang di rating :  banyaknya data rating
- Variabel dalam data ratings :
  - userId : id unik dari user /pengguna
  - movieId : ID unik untuk setiap film yang dirating
  - rating : Nilai rating yang diberikan pengguna terhadap film tertentu.
  - timestamp : Waktu ketika rating diberikan

## Data Preparation 

Pada tahapan data preparation tahapan yang dilakukan dalam prject ini adalah :
- Menggabungkan data movies dan data ratings ini dilakukan untuk menyatukan infoermasi dari dua tabel yang saling melengkapi untuk analissis dan pemodelan
- Memeriksa jika terdapat null dalamdata, dan di data ini sudah tidak terdapat lagi data null
- dalam kolom genre terdapat multi-kata, agar pada saat tahapan vektorizer multi kata tersebut tidak dianggap sebagai genre yang berbeda, jadi kita akan mengganti tanda (-) menjadi (_) dan mengubah format Adventure|Animation|Children menjadi Adventure Animation Children atau mengubah tanda pemisal menjadi spasi.
- menghapus data duplikasi yang memiliki movieId yang sama. karena 1 film bisa muncul berkali-kali(karena user berbeda dalam ratings), sedangkan kita hanya ingin satu data unik untuk per film
- mengonversi kolom DataFrame kedalam bentuk list untuk proses lanjutan ekstraksi fitur
- setelah itu buat dictionary untuk menentukan pasangan key-value pada data movie_id, movie_title, dan movie_genres yang telah kita siapkan sebelumnya
- sehingga menghasilkan data frame baru yaitu movie_new yang akan kita gunakan untuk tahapan modeling

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
















































































































































































📚 Referensi yang bisa kamu cantumkan:
Gómez-Uribe, C. A., & Hunt, N. (2016).
The Netflix Recommender System: Algorithms, Business Value, and Innovation.
ACM Transactions on Management Information Systems (TMIS), 6(4), 13.
👉 https://dl.acm.org/doi/10.1145/2843948
✅ Membahas langsung tentang bagaimana sistem rekomendasi meningkatkan engagement, retensi, dan personalisasi di Netflix.

Ricci, F., Rokach, L., & Shapira, B. (Eds.). (2015).
Recommender Systems Handbook.
Springer.
✅ Referensi utama dalam bidang sistem rekomendasi, mencakup manfaat strategis bagi bisnis.

Jannach, D., Adomavicius, G., & Tuzhilin, A. (2021).
Recommender Systems: Challenges, Insights and Research Opportunities.
ACM Transactions on Intelligent Systems and Technology.
👉 https://dl.acm.org/doi/10.1145/3457607
✅ Menyoroti manfaat sistem rekomendasi untuk pengalaman pengguna dan bisnis secara keseluruhan.


Gómez-Uribe, Carlos A., and Neil Hunt. (2015). "The Netflix Recommender System: Algorithms, Business Value, and Innovation." ACM Transactions on Management Information Systems (TMIS).

Ricci, F., Rokach, L., & Shapira, B. (2011). Introduction to Recommender Systems Handbook. Springer.

