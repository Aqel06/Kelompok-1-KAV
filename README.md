# Kelompok-1-KAV

# Key Detection
## 1. Representasi Chroma
Data audio diubah menjadi fitur chroma 12 dimensi yang mewakili distribusi energi untuk setiap pitch class (C, C#, D, …, B). Untuk mendeteksi kunci global, diambil nilai rata-rata chroma sepanjang lagu.

## 2. Profil Skala Krumhansl-Schmuckler
Penggunaan 24 profil referensi (12 mayor dan 12 minor) yang berdasarkan dari bobot empiris penelitian Krumhansl-Schmuckler. Kemudian profil ini digeser dengan melingkar (circular shift) untuk dapat mencakup semua kemungkinan kunci dasar.

## 3. Cosine Similarity dengan Profil
Membandingkan rata-rata chroma dengan setiap profil dengan menggunakan cosine similarity. Nilai kesamaan tertinggi kemudian menunjukkan kunci yang paling mendekati distribusi pitch dari lagu.

## 4. Estimasi 
Hasil menunjukkan kunci dengan skor yang tertinggi yang menunjukkan bahwa kunci lagu dikenali  adalah C mayor yang memiliki skor kemiripan 0.880.

# Chord Recognition
## 1. Pembangunan Template Akor
Template akor triad dibuat sebanyak 24 (12 mayor dan 12 minor). Masing-masing dari template dberikan representasi sebagai vektor 12 dimensi dengan bobot root (tinggi), fifth (sedang), dan third (sedikit lebih rendah). Kemudian template dasar digeser dengan melingkar agar dapat mencakup semua akor dari C hingga B.

## 2. Normalisasi Template
Setiap template dilakukan normalisasi agar perbandingan dari cosine similarity dapat lebih akurat dan tidak akan dipengaruhi oleh besar dan kecilnya energi.

## 3. Pencocokan per Beat
Dilakukan perbandingan representasi chroma per beat dengan seluruh template akor. Proses dari pencocokan dilakukan menggunakna cosine similarity yang kemudian akor yang memiliki nilai tertinggi dipilih sebagai hasil prediksi.

## 4. Estimasi
Hasil menunjukkan beat pertama dikenali sebagai C mayor yang memiliki skor 0.93, kemudian beat kedua yang dikenali sebagai F mayor yang memiliki skor 0.94. Hal ini menunjukkan bahwa sistem dapat mendeteksi progresi akor dan dapat dilakukan secara otomatis dengan tingkat keyakinan yang tinggi.

# 🎵 Cover Song Identification with Chroma Features, OTI, and DTW

## 🔎 Analisis Metode

### 1. Preprocessing & Simulasi Cover
Dataset yang digunakan adalah sebuah lagu (`yA`) kemudian dibuat cover versi modifikasi (`yB`) dengan **pitch shift +2 semitone** dan sedikit **time-stretch (0.95x)**. Hal ini meniru situasi nyata di mana sebuah lagu cover biasanya tidak persis sama dengan versi asli, baik dari segi nada maupun tempo.

### 2. Beat-Synchronous Chroma
Fitur utama yang digunakan adalah **Chroma CQT** (12 dimensi yang merepresentasikan pitch class: C, C#, D, …, B). Untuk membuat representasi lebih stabil, fitur ini disinkronkan dengan beat menggunakan median agregasi. Hasilnya adalah matriks **12 x T** yang menyimpan distribusi energi nada per beat.

### 3. Optimal Transposition Index (OTI)
Karena cover sering kali ditransposisi, sistem mencari **shift terbaik (0–11 semitone)** yang memaksimalkan kesamaan rata-rata antar frame. Pada eksperimen ini, OTI mendeteksi perbedaan **+10 semitone-class**, sesuai dengan perubahan pitch yang disimulasikan.

### 4. Dynamic Time Warping (DTW)
Untuk mengukur kesamaan sekuens chroma antara lagu asli dan cover, digunakan **DTW subsequence matching**. Metode ini memungkinkan cover yang lebih pendek atau tempo yang berbeda tetap bisa dibandingkan. Hasil skor kemiripan sederhana menunjukkan nilai **similarity score ≈ 0.667**, yang berarti kedua lagu cukup mirip meski ada perbedaan tempo.

### 5. Pitch Transition Matrix (PTR)
Selain chroma, dibuat juga matriks transisi **12x12** yang merepresentasikan probabilitas perpindahan antar pitch-class dominan dari beat ke beat. Matriks ini membentuk “pola harmoni” lagu. Dengan menghitung jarak Frobenius antara matriks lagu asli dan cover (dengan mempertimbangkan rotasi transposisi), didapat nilai **dist ≈ 1.414**, yang menunjukkan bahwa pola harmoni antara kedua lagu relatif serupa.

---

## 📊 Hasil Utama
- **OTI:** +10 semitone-class (cover terdeteksi digeser tonalitasnya).
- **DTW Similarity:** ≈ 0.667 (kemiripan cukup tinggi meskipun ada time-stretch).
- **PTR Frobenius Distance:** ≈ 1.414 (struktur harmoni tetap mirip).

---
