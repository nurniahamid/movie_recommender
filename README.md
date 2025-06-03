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
Data tersebut memiliki dua file csv yaitu : movies dan ratings
1. Movie: merupakan jumlah data film 
- Jumlah data film = 972
- Variabel dalam data movies = 
  - movieId = id unik film 
  - title = judul film
  - genres = genres film
- Jumlah Jenis Genre film :  951
 
2. ratings : merupakan data rating pengguna terhadap film
- Banyaknya data rating :  610
- Banyaknya film yang di rating :  9724
- Variabel dalam data ratings :
  - userId : id unik dari user /pengguna
  - movieId : id Unik film
  - rating : nilai yang diberikan oleh user
  - timestamp : 



































































































































































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

