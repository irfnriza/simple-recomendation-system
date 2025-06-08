# Laporan Proyek Machine Learning - Nama Anda

## Project Overview
Pada bagian ini, akan dijelaskan latar belakang yang relevan dengan proyek sistem rekomendasi film ini. Proyek ini berfokus pada pengembangan sistem rekomendasi untuk membantu pengguna menemukan film yang sesuai dengan preferensi mereka.

Sistem rekomendasi memiliki peran krusial dalam ekosistem digital modern, khususnya pada platform hiburan seperti layanan streaming film, karena mereka membantu pengguna menavigasi sejumlah besar pilihan konten. Dengan menyediakan rekomendasi yang dipersonalisasi, sistem ini dapat meningkatkan pengalaman pengguna secara signifikan, membantu mereka menemukan film yang mungkin tidak akan mereka temukan sendiri, dan pada akhirnya meningkatkan kepuasan serta keterlibatan pengguna.

Proyek ini memanfaatkan **dataset MovieLens ml-latest-small**. Dataset ini berasal dari **MovieLens**, sebuah layanan rekomendasi film yang dioperasikan oleh **GroupLens Research Group** dari University of Minnesota. Dataset `ml-latest-small` secara spesifik mengandung **100.836 rating** dan **3.683 aplikasi tag** yang mencakup **9.742 film**. Data ini dikumpulkan dari **610 pengguna** yang memberikan rating dan tag antara 29 Maret 1996 hingga 24 September 2018.

Penggunaan dataset MovieLens ini relevan karena MovieLens sendiri adalah sumber data untuk riset di bidang sistem rekomendasi. Untuk mengakui penggunaan dataset ini dalam publikasi, disarankan untuk mengutip makalah berikut: F. Maxwell Harper dan Joseph A. Konstan. 2015. The MovieLens Datasets: History and Context. ACM Transactions on Interactive Intelligent Systems (TiiS) 5, 4: 19:1–19:19.

**Referensi:**
 F. Maxwell Harper dan Joseph A. Konstan. (2015). The MovieLens Datasets: History and Context. ACM Transactions on Interactive Intelligent Systems (TiiS), 5(4), 19:1–19:19.

Tersedia di: [https://dl.acm.org/doi/10.1145/2827872](https://dl.acm.org/doi/10.1145/2827872)

## Business Understanding
Bagian laporan ini akan menguraikan proses klarifikasi masalah yang menjadi dasar proyek sistem rekomendasi film ini. Ini mencakup pernyataan masalah (Problem Statements), tujuan (Goals), dan pendekatan solusi (Solution Approach) untuk mencapai tujuan tersebut.

### Problem Statements
Sistem rekomendasi menjadi **sangat penting dalam ekosistem digital modern, terutama pada platform hiburan seperti layanan streaming film**. Problem statement utama yang coba diatasi oleh proyek ini adalah:

*   **Tantangan Pengguna dalam Menemukan Film yang Relevan**: Pengguna sering kali kesulitan untuk menavigasi sejumlah besar pilihan konten film yang tersedia, sehingga mereka mungkin melewatkan film yang sebenarnya sesuai dengan preferensi mereka. Tanpa sistem rekomendasi yang efektif, pengalaman pengguna dapat menjadi kurang optimal karena mereka harus mencari film secara manual di antara ribuan judul.

### Goals
Berdasarkan pernyataan masalah di atas, tujuan dari proyek sistem rekomendasi ini adalah:

*   **Meningkatkan Pengalaman Pengguna Melalui Rekomendasi yang Dipersonalisasi**: Mengembangkan sebuah sistem rekomendasi yang mampu menyediakan rekomendasi film yang dipersonalisasi kepada pengguna, memastikan bahwa film yang disarankan sesuai dengan preferensi dan riwayat tontonan mereka.
*   **Meningkatkan Kepuasan dan Keterlibatan Pengguna**: Dengan membantu pengguna menemukan film yang mungkin tidak akan mereka temukan sendiri, sistem ini diharapkan dapat meningkatkan kepuasan pengguna dan mendorong keterlibatan yang lebih tinggi dengan platform.

### Solution Approach
Untuk meraih tujuan-tujuan tersebut, proyek ini akan menguraikan dan menerapkan dua pendekatan solusi sistem rekomendasi:

1.  **Content-Based Filtering (CBF)**:
    *   Pendekatan ini akan merekomendasikan film kepada pengguna berdasarkan **kemiripan konten (genre, sinopsis, fitur teks)** dengan film yang pernah disukai atau ditonton oleh pengguna.
    *   Implementasinya melibatkan pembangunan representasi konten film (`feature_soup`) menggunakan informasi seperti genre, aggregated tags, tahun rilis (dekade), rating tier, dan popularity score. Matriks TF-IDF akan dibangun dari `feature_soup`, dan **cosine similarity** akan digunakan untuk mengukur kemiripan antar film.

2.  **Collaborative Filtering (CF)**:
    *   Pendekatan ini akan merekomendasikan film kepada pengguna berdasarkan **pola kesukaan (interaksi historis)** dari pengguna yang memiliki selera serupa.
    *   Model akan dibuat menggunakan algoritma **SVD (Singular Value Decomposition)** dari pustaka Surprise. Model ini akan belajar dari pola rating pengguna untuk memprediksi preferensi film yang belum dinilai oleh seorang pengguna.

Kedua pendekatan ini akan disajikan dan dibandingkan untuk menunjukkan solusi rekomendasi yang berbeda dalam mengatasi permasalahan yang ada.

## Data Understanding
Bagian ini menyajikan informasi detail mengenai data yang digunakan dalam proyek sistem rekomendasi film, termasuk jumlah data, kondisi data, tautan sumber, serta uraian variabel dan analisis eksplorasi data (EDA) baik secara univariat maupun multivariat.

Data yang digunakan berasal dari **dataset MovieLens ml-latest-small**. Dataset ini merupakan koleksi rating dan aplikasi tag dari **MovieLens**, sebuah layanan rekomendasi film yang dioperasikan oleh GroupLens Research Group dari University of Minnesota.

*   **Tautan Sumber Data:** Dataset ini dapat diunduh secara publik dari situs [web GroupLens Research](https://grouplens.org/datasets/movielens/).
*   **Informasi Umum Data:**
    *   Dataset ini berisi **100.836 rating** dan **3.683 aplikasi tag**.
    *   Mencakup **9.742 film**.
    *   Dibuat oleh **610 pengguna** antara 29 Maret 1996 dan 24 September 2018.
    *   Pengguna dipilih secara acak, dan **setiap pengguna yang disertakan telah memberi rating setidaknya 20 film**.
    *   **Tidak ada informasi demografis** yang disertakan untuk pengguna.
    *   Dataset ini adalah *development dataset* dan dapat berubah seiring waktu.

### Variabel atau Fitur pada Data
Data tersimpan dalam empat file CSV terpisah: `links.csv`, `movies.csv`, `ratings.csv`, dan `tags.csv`. Setiap file memiliki struktur sebagai berikut:

*   **`ratings.csv`**: Berisi data rating yang diberikan oleh pengguna untuk film.
    *   `userId`: ID unik pengguna yang telah dianonimkan. ID ini konsisten antara `ratings.csv` dan `tags.csv`.
    *   `movieId`: ID unik film. ID ini konsisten di antara semua file data.
    *   `rating`: Skor rating yang diberikan pengguna untuk film, dalam skala 5-bintang dengan kelipatan setengah bintang (0.5 hingga 5.0).
    *   `timestamp`: Waktu ketika rating diberikan, dalam detik sejak tengah malam Coordinated Universal Time (UTC) 1 Januari 1970.

*   **`tags.csv`**: Berisi metadata tag yang diterapkan pengguna pada film.
    *   `userId`: ID unik pengguna.
    *   `movieId`: ID unik film.
    *   `tag`: Metadata yang dibuat pengguna tentang film, biasanya berupa kata tunggal atau frasa pendek.
    *   `timestamp`: Waktu ketika tag diterapkan.

*   **`movies.csv`**: Berisi informasi dasar tentang film.
    *   `movieId`: ID unik film.
    *   `title`: Judul film, termasuk tahun rilis dalam tanda kurung.
    *   `genres`: Daftar genre film yang dipisahkan oleh karakter pipa (`|`), seperti Action, Adventure, Animation, Children's, Comedy, Crime, Documentary, Drama, Fantasy, Film-Noir, Horror, Musical, Mystery, Romance, Sci-Fi, Thriller, War, Western, atau `(no genres listed)`.

*   **`links.csv`**: Berisi ID yang dapat digunakan untuk menghubungkan ke sumber data film eksternal.
    *   `movieId`: ID unik film.
    *   `imdbId`: ID film dari IMDb (Internet Movie Database).
    *   `tmdbId`: ID film dari TMDb (The Movie Database).

### Exploratory Data Analysis (EDA)
Sebelum analisis lebih lanjut, data telah melalui proses pembersihan dan pemfilteran. Pengguna yang memberi rating kurang dari 10 film dan film yang memiliki rating kurang dari 5 telah dihapus untuk memastikan kualitas data yang memadai. Setelah pemfilteran, jumlah rating berkurang dari 100.836 menjadi **90.274**, dan jumlah film valid menjadi **3.650** dari 9.742 film awal. Pemeriksaan nilai hilang dan duplikasi pada dataset menunjukkan **tidak ada nilai hilang** yang signifikan dan **tidak ada entri duplikat** berdasarkan kolom kunci.

#### Univariat Analysis
Analisis ini berfokus pada distribusi dan karakteristik masing-masing variabel secara terpisah:

*   **Distribusi Genre Film**:
    * ![Distribusi Genre Film](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/genre.png?raw=true)
    *   **Drama** adalah genre film yang paling banyak diproduksi, diikuti oleh **Comedy** dan **Thriller**. Hal ini mengindikasikan bahwa kedua genre tersebut sangat dominan dalam industri perfilman.
    *   Genre seperti Action, Romance, Crime, Adventure, dan Horror juga memiliki jumlah film yang cukup banyak, berkisar antara 1.000 hingga 2.000 film.
    *   Genre seperti Film-Noir, IMAX, Western, dan Musical memiliki jumlah film yang relatif sedikit (di bawah 500 film), kemungkinan karena pasar yang lebih spesifik atau *niche*.

*   **Distribusi Tahun Rilis Film**:
    * ![Distribusi Tahun Rilis Film](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/tahun.png?raw=true)
    *   Pada awal abad ke-20 (1900-1950), jumlah film yang dirilis masih sedikit dan mengalami pertumbuhan lambat.
    *   Setelah tahun 1950, terutama mulai 1970-an, terjadi peningkatan signifikan dalam jumlah film yang dirilis setiap tahunnya.
    *   Produksi film mencapai puncaknya pada rentang **1995-2010**, dengan sekitar 800-900 film per tahun.
    *   Terlihat adanya tren penurunan jumlah film yang dirilis setiap tahunnya setelah 2005-2010, dan penurunan drastis terlihat setelah tahun **2020**, yang mungkin disebabkan oleh pandemi COVID-19.
    *   Visualisasi ini membantu memahami evolusi industri film secara historis dan peran penting platform distribusi modern.

*   **Distribusi Rating Film**:
    * ![DistribusiRating Film](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/rating.png?raw=true)
    *   Rata-rata rating keseluruhan adalah sekitar **3.53**. Rating **4.0** adalah yang paling umum, dengan lebih dari 25.000 entri.
    *   Rating 3.0 dan 5.0 juga memiliki frekuensi tinggi.
    *   Mayoritas rating cenderung bersifat **positif** (antara 3 hingga 5), menunjukkan bahwa sebagian besar pengguna cukup puas dengan film yang mereka tonton.

*   **Distribusi Jumlah Rating per Pengguna**:
    * ![Distribusi Jumlah Rating per Pengguna](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/ratingperuser.png?raw=true)
    *   Rata-rata pengguna memberikan sekitar **147.99 rating**, dengan median 68 rating.
    *   Distribusi sangat **miring ke kanan (right-skewed)**, menunjukkan bahwa sebagian besar pengguna (lebih dari 250) memberikan kurang dari 50 rating, sementara ada beberapa **pengguna sangat aktif (power user)** yang memberikan rating ribuan film (outlier).
    *   Ini perlu menjadi perhatian khusus dalam pengembangan sistem rekomendasi berbasis kolaborasi, karena ketidakseimbangan kontribusi pengguna.

*   **Distribusi Jumlah Rating per Film**:
    * ![Distribusi Jumlah Rating per Film](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/ratingperfilm.png?raw=true)
    *   Rata-rata film menerima sekitar **24.73 rating**, dengan median 13 rating.
    *   Lebih dari 1.600 film menerima kurang dari 10 rating. Distribusi juga **miring ke kanan (right-skewed)**.
    *   Ini menunjukkan adanya **efek long-tail** pada dataset, di mana sebagian kecil film sangat populer dan menerima banyak rating, sementara sebagian besar film hanya memiliki sedikit data rating. Kondisi ini dapat berdampak pada akurasi model.

*   **Distribusi Aktivitas Rating per Tahun**:
    * ![Distribusi Aktivitas Rating per Tahun](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/ratingpertahun.png?raw=true)
    *   Aktivitas rating mencapai puncaknya pada tahun **2000 dan 2017**.
    *   Tren fluktuatif naik turun signifikan, menunjukkan adanya faktor perubahan platform, jumlah pengguna, atau perilaku pengguna.
    *   Dominasi tahun-tahun modern (2012-2017) kemungkinan terkait dengan peningkatan penggunaan internet dan layanan *streaming*.

*   **Distribusi Jumlah Tag per Pengguna**:
    * ![Distribusi Jumlah Tag per Pengguna](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/tagperuser.png?raw=true)
    *   Rata-rata pengguna memberikan sekitar **58 tag**, dengan median 4 tag.
    *   Distribusi sangat **miring ke kanan**, menunjukkan bahwa sebagian besar pengguna hanya memberikan sedikit tag.
    *   Ada pengguna outlier yang memberikan **1.507 tag**.
    *   Sebagian besar pengguna (75%) memberikan kurang dari 13 tag. Hal ini mengindikasikan bahwa sistem yang bergantung pada tag (misalnya Content-Based Filtering) mungkin kurang efektif bagi sebagian besar pengguna karena data tag yang sedikit.

*   **Distribusi Jumlah Tag per Film**:
    * ![Distribusi Jumlah Tag per Film](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/tagperfilm.png?raw=true)
    *   Rata-rata film menerima sekitar **2.34 tag**, dengan median 1 tag.
    *   **Mayoritas film hanya mendapatkan 1 atau 2 tag**.
    *   Distribusi tidak merata, dengan outlier (film yang menerima 181 tag).
    *   Aktivitas *tagging* terhadap film secara umum terbatas. Film dengan banyak tag dapat mengindikasikan keterlibatan atau daya tarik yang tinggi.

*   **20 Tag Terpopuler**:
    * ![20 Tag Terpopuler](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/tagpopular.png?raw=true)
    *   Tag **"In Netflix queue"** sangat dominan, menunjukkan banyak pengguna menggunakannya sebagai pengingat pribadi.
    *   Tag lain yang populer termasuk "atmospheric", "thought-provoking", dan "superhero", yang cenderung diminati pengguna.
    *   Beberapa tag bersifat deskriptif seperti "surreal", "dark comedy", dan "twist ending".
    *   Informasi tag sangat berharga untuk membangun sistem rekomendasi berbasis konten, tetapi "In Netflix queue" perlu diperlakukan khusus karena sifatnya yang bukan deskriptif konten.

#### Multivariat Analysis
Analisis ini melihat hubungan antara dua atau lebih variabel:

*   **Rata-rata Rating Film Berdasarkan Tahun Rilis**:
    * ![Rata-rata Rating Film Berdasarkan Tahun Rilis](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/rataratatahunrilis.png?raw=true)
    *   Visualisasi menunjukkan **fluktuasi rata-rata rating film berdasarkan tahun rilis**, tanpa tren peningkatan atau penurunan yang jelas secara signifikan.
    *   Rating rata-rata cenderung lebih tinggi pada film-film yang dirilis pada awal abad ke-20 (sebelum 1940-an).
    *   Insight dari analisis ini adalah informasi tag sangat berharga untuk membangun sistem rekomendasi berbasis konten. Tren popularitas tag tertentu menunjukkan minat pengguna terhadap tema, genre, atau suasana film tertentu.

*   **Jumlah Rating vs. Rata-rata Rating per Film**:
    * ![Jumlah Rating vs. Rata-rata Rating per Film](https://github.com/irfnriza/simple-recomendation-system/blob/main/assets/ratingvsperfilm.png?raw=true)
    *   *Plot scatter* ini menunjukkan hubungan antara **jumlah rating yang diterima sebuah film dengan rata-rata rating** film tersebut.
    *   Banyak film memiliki **jumlah rating yang rendah (di bawah 20)**, namun menunjukkan **variasi rata-rata rating yang sangat tinggi** (dari 1 hingga hampir 5). Ini menunjukkan bahwa film-film tersebut sangat dipengaruhi oleh penilaian dari sedikit pengguna.
    *   Semakin tinggi jumlah rating, **rata-rata rating cenderung mengumpul di kisaran 3 hingga 4.5**, menandakan bahwa film dengan banyak penonton memiliki penilaian yang lebih stabil.
    *   **Implikasi**: Kualitas rating suatu film sangat bergantung pada jumlah rating yang diterimanya. Film dengan jumlah rating tinggi umumnya dianggap memiliki kualitas yang lebih kredibel secara umum.
    *   Visualisasi ini secara signifikan membantu menunjukkan bahwa **jumlah rating merupakan faktor penting dalam menilai validitas dari rata-rata rating suatu film**.

## Data Preparation
Bagian ini menjelaskan teknik dan proses data *preparation* yang telah dilakukan, beserta alasan mengapa tahapan tersebut diperlukan untuk membangun sistem rekomendasi film. Teknik yang digunakan pada laporan ini disajikan secara berurutan sesuai dengan proses yang dijalankan.

Proses data *preparation* bertujuan untuk mengubah data mentah menjadi format yang sesuai untuk pemodelan, memastikan kualitas data, dan mengekstrak fitur-fitur yang relevan.

**1. Pemuatan dan Pemfilteran Data Awal**
*   **Proses:** Data awal yang terdiri dari `movies.csv`, `ratings.csv`, dan `tags.csv` dimuat. Selanjutnya, dilakukan proses pemfilteran untuk meningkatkan kualitas data.
    *   Pengguna yang memberikan **rating kurang dari 10 film dihapus**.
    *   Film yang memiliki **rating kurang dari 5 dihapus**.
*   **Jumlah Data Setelah Pemfilteran:**
    *   Jumlah rating awal: **100.836**.
    *   Jumlah rating setelah filter: **90.274**.
    *   Jumlah film valid: **3.650** dari 9.742 film awal.
    *   Jumlah pengguna valid: **610**.
*   **Alasan:** Pemfilteran ini dilakukan untuk memastikan bahwa data yang digunakan memiliki sinyal yang cukup baik dan relevan untuk membangun sistem rekomendasi. Pengguna yang terlalu pasif atau film yang terlalu sedikit di-rating cenderung tidak memberikan informasi yang cukup untuk pola rekomendasi yang akurat.

**2. Pemeriksaan Nilai Hilang (Missing Values)**
*   **Proses:** Dilakukan pemeriksaan nilai hilang pada ketiga *dataframe* utama: `movies_df`, `ratings_df`, dan `tags_df`.
*   **Hasil:** Tidak ditemukan nilai hilang yang signifikan pada dataset setelah pemfilteran awal.
*   **Alasan:** Pemeriksaan ini penting untuk memastikan kelengkapan data. Jika ada nilai hilang, diperlukan strategi penanganan seperti imputasi atau penghapusan baris/kolom, namun pada kasus ini tidak diperlukan.

**3. Pemeriksaan Data Duplikat**
*   **Proses:** Dilakukan pemeriksaan duplikasi pada setiap dataset berdasarkan kolom kunci yang relevan.
    *   Untuk `movies_df`: berdasarkan `movieId`.
    *   Untuk `ratings_df`: berdasarkan kombinasi `userId`, `movieId`, dan `timestamp`.
    *   Untuk `tags_df`: berdasarkan kombinasi `userId`, `movieId`, `tag`, dan `timestamp`.
*   **Hasil:** Tidak ditemukan entri duplikat pada ketiga dataset tersebut.
*   **Alasan:** Memastikan integritas dan keunikan setiap entri data sangat krusial. Duplikasi dapat menyebabkan bias dalam analisis dan pemodelan.

**4. Ekstraksi dan Pra-pemrosesan Fitur (Feature Extraction and Preprocessing)**
Tahap ini berfokus pada penciptaan fitur-fitur baru dan transformasi fitur yang ada agar lebih informatif untuk model rekomendasi.

*   **Ekstraksi Tahun Rilis Film:** Tahun rilis film diekstrak dari kolom `title` pada `movies_df`.
*   **Pemrosesan Genre:** Kolom `genres` dipisah berdasarkan pemisah pipa (`|`) untuk mendapatkan daftar genre individu, kemudian dihitung frekuensi setiap genre.
*   **Statistik Rating per Film:** Dihitung rata-rata rating (`avg_rating`), standar deviasi rating (`rating_std`), dan jumlah *user* yang memberi rating (`user_count`) untuk setiap film.
*   **Skor Popularitas (Weighted Rating):** Skor popularitas dihitung menggunakan formula *weighted rating* untuk film berdasarkan rata-rata rating, jumlah rating, dan rata-rata rating keseluruhan. Fitur ini membantu merekomendasikan film dengan rating tinggi yang juga memiliki banyak penilai.
*   **Pemrosesan Tag (Jika Ada):** Jika data `tags.csv` tersedia, tag diubah ke huruf kecil dan diagregasi untuk setiap film. Fitur ini dapat digunakan untuk sistem rekomendasi berbasis konten (NLP) atau pencarian berbasis teks.
*   **Fitur Dekade:** Tahun rilis film dikelompokkan ke dalam kategori dekade (misalnya 1995 menjadi 1990-an).
*   **Alasan:** Proses ini menciptakan representasi yang kaya dari film, menggabungkan informasi deskriptif (genre, tahun, tag) dan informasi perilaku pengguna (rating, popularitas). Fitur-fitur ini kemudian digunakan untuk membangun model rekomendasi.

**5. Pembuatan Fitur Konten untuk Content-Based Filtering**
*   **Proses:** Sebuah fungsi `build_content_features` digunakan untuk menggabungkan berbagai fitur yang telah diproses (genre, tag agregasi, dekade, kategori rating, dan kategori popularitas) menjadi sebuah "sup fitur" (`feature_soup`). `feature_soup` ini merupakan representasi gabungan dari fitur-fitur tekstual dan kategorikal film.
*   **Alasan:** `feature_soup` ini akan menjadi masukan utama untuk model *Content-Based Filtering* yang akan menghitung kemiripan antar film berdasarkan kontennya.

**6. Pembuatan Profil Preferensi Pengguna untuk Collaborative Filtering**
*   **Proses:** Sebuah fungsi `calculate_user_preferences` digunakan untuk menormalisasi rating yang diberikan oleh setiap pengguna dengan mengurangi rata-rata rating pengguna tersebut. Kemudian, ditentukan apakah pengguna "menyukai" suatu film berdasarkan rating yang telah dinormalisasi (rating >= 0.5).
*   **Alasan:** Normalisasi rating membantu mengatasi perbedaan skala rating antar pengguna. Profil preferensi ini penting untuk model *Collaborative Filtering* agar dapat memprediksi preferensi pengguna terhadap film yang belum pernah di-rating.

## Modeling
Bagian ini membahas model sistem rekomendasi yang dibuat untuk menyelesaikan permasalahan, dengan menyajikan *top-N recommendation* sebagai *output*. Sistem ini menyajikan dua solusi rekomendasi dengan algoritma yang berbeda, serta menjelaskan kelebihan dan kekurangan pada setiap pendekatan yang dipilih.

Untuk mencapai tujuan proyek, dua pendekatan sistem rekomendasi utama digunakan: *Content-Based Filtering* dan *Collaborative Filtering*.

**1. Content-Based Filtering (CBF)**
Pendekatan ini merekomendasikan item (film) yang **serupa dengan apa yang disukai pengguna di masa lalu**, berdasarkan atribut atau konten dari item tersebut.

*   **Konsep:** Sistem ini bekerja dengan memahami fitur-fitur internal dari film dan membandingkannya dengan profil preferensi pengguna. Jika pengguna menyukai film bergenre A, sistem akan merekomendasikan film lain bergenre A.
*   **Pembuatan Fitur Konten (*Feature Engineering*):**
    *   **"Sup Fitur" (*Feature Soup*):** Berbagai fitur yang telah diproses dalam tahap *Data Preparation* digabungkan menjadi sebuah representasi tekstual (`feature_soup`). Fitur-fitur ini meliputi:
        *   **Genre:** Genre film yang sudah dipisahkan.
        *   **Agregasi Tag:** Tag yang diberikan pengguna untuk film tersebut.
        *   **Dekade:** Tahun rilis film dikelompokkan ke dalam kategori dekade (misalnya, 1990-an).
        *   **Kategori Rating:** Film dikategorikan berdasarkan rata-rata ratingnya (misalnya, `low_rated`, `medium_rated`, `excellent_rated`).
        *   **Kategori Popularitas:** Film dikategorikan berdasarkan skor popularitasnya (misalnya, `niche`, `moderate`, `popular`, `blockbuster`).
    *   `feature_soup` ini berfungsi sebagai input untuk proses selanjutnya.
*   **Pembangunan Model:**
    *   **TF-IDF Vectorizer:** `feature_soup` diubah menjadi matriks numerik menggunakan TF-IDF (Term Frequency-Inverse Document Frequency) Vectorizer. Ini mengubah teks menjadi representasi numerik yang menangkap pentingnya kata-kata dalam dokumen.
    *   **Cosine Similarity:** Matriks TF-IDF kemudian digunakan untuk menghitung **kemiripan kosinus antar film**. Skor kemiripan ini mengukur seberapa dekat dua film berdasarkan kontennya.
*   **Generasi Rekomendasi (*Top-N Recommendation*):**
    *   Fungsi `get_content_recommendations` digunakan untuk menghasilkan rekomendasi.
    *   Prosesnya melibatkan pengambilan film-film yang disukai oleh pengguna (berdasarkan rating yang dinormalisasi >= 0.5).
    *   Skor kemiripan konten dari film yang disukai pengguna dihitung dengan semua film lainnya.
    *   Film-film yang belum pernah ditonton pengguna, namun memiliki skor kemiripan tertinggi, akan direkomendasikan.
    *   **Contoh Output:** Untuk *User 1*, rekomendasi *top-N* bisa mencakup "Little Women (1994)" dengan skor kemiripan tinggi.
*   **Kelebihan dan Kekurangan:**
    *   **Kelebihan:**
        *   **Baik untuk Pengguna Baru:** Dapat merekomendasikan film kepada pengguna baru karena tidak memerlukan riwayat interaksi yang banyak, hanya perlu mengetahui film apa yang mereka tonton.
        *   **Mampu Menangkap Preferensi Spesifik:** Sistem mampu menangkap preferensi konten spesifik dari pengguna.
        *   **Tidak Terdampak *Cold Start* Item:** Mampu merekomendasikan film baru yang belum memiliki riwayat rating karena hanya bergantung pada atribut film.
    *   **Kekurangan:**
        *   **Kurang Eksplorasi:** Cenderung merekomendasikan film yang sangat mirip dengan apa yang sudah disukai, sehingga kurang mampu memperkenalkan variasi baru.
        *   **Rentan Terhadap *Overfitting*:** Jika fitur konten tidak cukup beragam, model dapat menjadi terlalu spesifik.
        *   **Keterbatasan Representasi Konten:** Efektivitasnya bergantung pada kualitas dan kelengkapan fitur konten yang tersedia.

**2. Collaborative Filtering (CF)**
Pendekatan ini merekomendasikan item (film) kepada pengguna berdasarkan **pola kesukaan pengguna lain yang memiliki selera serupa**. MovieLens sendiri, sumber data ini, dioperasikan oleh GroupLens Research menggunakan *collaborative filtering*.

*   **Konsep:** Sistem ini mengidentifikasi pengguna dengan preferensi serupa atau film yang disukai oleh pengguna yang mirip, lalu merekomendasikan film yang disukai oleh "tetangga" tersebut yang belum ditonton pengguna.
*   **Pembangunan Model:**
    *   **Algoritma SVD:** Model dibangun menggunakan algoritma **SVD (Singular Value Decomposition)** dari *library* `Surprise` . SVD mempelajari faktor laten (representasi tersembunyi) dari interaksi historis antara pengguna dan film untuk memprediksi preferensi film yang belum dinilai.
    *   **Dataset `Surprise`:** Data rating dikonversi ke format yang kompatibel dengan *library* `Surprise`.
*   **Generasi Rekomendasi (*Top-N Recommendation*):**
    *   Fungsi `get_collaborative_recommendations` digunakan untuk menghasilkan rekomendasi.
    *   Model memprediksi rating untuk film-film yang belum ditonton oleh pengguna target.
    *   Film-film dengan **prediksi rating tertinggi** akan direkomendasikan.
    *   **Contoh Output:** Untuk *User 1*, rekomendasi *top-N* bisa mencakup "Twelve Monkeys (a.k.a. 12 Monkeys) (1995)" dengan prediksi rating 5.0.
*   **Kelebihan dan Kekurangan:**
    *   **Kelebihan:**
        *   **Mampu Menangkap Pola Kompleks:** Mampu menangkap pola preferensi pengguna yang kompleks dan tidak mudah dijelaskan oleh atribut konten saja.
        *   **Prediksi Rating Realistis:** Memiliki prediksi rating yang realistis.
        *   **Potensi Eksplorasi Tinggi:** Dapat merekomendasikan film yang tidak terduga namun relevan, sehingga meningkatkan *serendipity* dan eksplorasi.
    *   **Kekurangan:**
        *   ***Cold Start Problem*:** Kesulitan merekomendasikan item baru atau kepada pengguna baru karena kurangnya data interaksi.
        *   **Masalah Sparsitas Data:** Kinerja dapat menurun pada dataset dengan sedikit interaksi pengguna-item.
        *   **Tidak Sesuai untuk Film yang Jarang Di-rating:** Film yang hanya memiliki sedikit rating mungkin tidak cukup terwakili dalam model.

**Perbandingan Metode Rekomendasi:**
Kedua metode, *Content-Based Filtering* dan *Collaborative Filtering*, memiliki kelebihan dan kekurangannya masing-masing dan **saling melengkapi**. *Content-Based Filtering* ideal untuk pengguna baru atau ketika ingin merekomendasikan konten yang sangat spesifik, sementara *Collaborative Filtering* unggul dalam mengeksplorasi film populer di kalangan pengguna serupa.

## Evaluation
Bagian ini bertujuan untuk **menyebutkan metrik evaluasi yang digunakan** dan **menjelaskan hasil proyek berdasarkan metrik evaluasi tersebut**. Metrik evaluasi yang dipilih harus **sesuai dengan konteks data, pernyataan masalah, dan solusi yang diinginkan**. Penting juga untuk **menjelaskan formula metrik dan cara kerjanya**.

Dalam konteks proyek sistem rekomendasi ini, yang menyajikan *top-N recommendation* sebagai *output*, evaluasi dapat berfokus pada seberapa efektif model dalam memberikan rekomendasi yang relevan kepada pengguna. Meskipun demikian, berdasarkan sumber `notebook.pdf` yang diberikan, bagian "Evaluation" lebih banyak membahas perbandingan kualitatif antara dua pendekatan sistem rekomendasi yang digunakan, yaitu *Content-Based Filtering* (CBF) dan *Collaborative Filtering* (CF), serta kelebihan dan kekurangan masing-masing, dibandingkan menyajikan hasil metrik evaluasi kuantitatif spesifik (seperti RMSE, Precision@K, Recall@K) dan formulanya.

**Perbandingan Metode Rekomendasi (Kualitatif)**
Kedua metode rekomendasi ini **saling melengkapi** dan memiliki karakteristik yang berbeda.

**1. Content-Based Filtering (CBF)**
*   **Kelebihan:**
    *   Sangat **cocok untuk pengguna baru** (*new users*) karena tidak tergantung pada riwayat interaksi pengguna lain.
    *   Mampu **menangkap preferensi konten spesifik** dari pengguna.
    *   Dapat merekomendasikan film bahkan jika **hanya mengetahui film yang ditonton** oleh pengguna.
    *   **Tidak terdampak *cold start* pada item** baru karena bergantung pada atribut film.
*   **Kekurangan:**
    *   Cenderung **kurang eksploratif**, artinya sering menyajikan film yang sangat mirip dengan apa yang sudah disukai pengguna, sehingga kurang memperkenalkan variasi baru.
    *   Rentan terhadap ***overfitting*** jika fitur konten kurang beragam.

**2. Collaborative Filtering (CF)**
*   **Kelebihan:**
    *   Mampu **menangkap pola preferensi pengguna yang kompleks** yang tidak mudah dijelaskan hanya dari atribut konten.
    *   Dapat merekomendasikan film yang **tidak terduga namun relevan** (*serendipity*), sehingga meningkatkan eksplorasi.
    *   Memiliki **prediksi rating yang realistis**.
    *   Mampu **menangkap selera komunitas** dan membantu eksplorasi genre yang belum dikenal.
*   **Kekurangan:**
    *   Mengalami **masalah *Cold Start***, yaitu kesulitan merekomendasikan item baru atau kepada pengguna baru karena kurangnya data interaksi.
    *   Kinerja dapat menurun pada **dataset dengan sedikit interaksi pengguna-item** (*data sparsity*).
    *   **Tidak sesuai untuk film yang jarang di-rating**.

**Kesimpulan Evaluasi (Kualitatif)**

Secara keseluruhan, kedua metode, *Content-Based Filtering* dan *Collaborative Filtering*, **saling melengkapi**. *Content-Based Filtering* ideal untuk pengguna baru atau ketika ingin merekomendasikan konten yang sangat spesifik, sementara *Collaborative Filtering* unggul dalam mengeksplorasi film populer di kalangan pengguna serupa, meskipun bisa lemah untuk pengguna dengan sedikit data. Untuk sistem rekomendasi yang robust, kombinasi atau pendekatan hibrida dari kedua metode ini seringkali menjadi solusi yang paling efektif (Catatan: Informasi ini adalah penjelasan umum tentang sistem rekomendasi dan tidak ada dalam sumber yang diberikan secara eksplisit, tetapi menyiratkan kesimpulan dari "saling melengkapi").
