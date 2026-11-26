# Analisis Well-Being Global dengan PCA dan K-Means Clustering

[English](README.md) · **Bahasa Indonesia**

Analisis multivariat dengan R yang mereduksi dimensi dan mengelompokkan 145 negara berdasarkan indikator kesejahteraan dari World Happiness Report 2025 (dari 147 negara awal, 2 negara dengan nilai hilang dibuang). Repositori ini memuat presentasi slide interaktif di browser, laporan PDF lengkap, dan skrip R untuk reproduksi.

- **Slide online:** https://febrianggi.github.io/global-well-being/
- **Unduhan:** [`Slide.pdf`](Slide.pdf) · [`Paper.pdf`](Paper.pdf)

## Temuan utama

- **Penyaringan variabel:** Dari enam indikator awal, `Generosity` dikeluarkan karena nilai KMO-nya hanya **0,36** (di bawah ambang kelayakan 0,5). Pada lima indikator tersisa, uji Bartlett signifikan sangat kuat (chi-square **271,634**, `p` ≈ `1,5 × 10^-52`), sehingga korelasinya layak untuk PCA.
- **Tiga komponen utama menjelaskan 88,172% keragaman total** (eigenvalue PC1 sebesar **2,821**):
  - **PC1:** Dimensi kesejahteraan umum (`Log GDP per capita`, `Social support`, `Healthy life expectancy`).
  - **PC2:** Didominasi oleh `Perceptions of corruption`.
  - **PC3:** Didominasi oleh `Freedom to make life choices`.
- **Dua klaster kesejahteraan global (`k = 2`):** Metode Elbow menunjuk `k = 4`, sedangkan Silhouette memilih `k = 2` dengan rata-rata **0,386**. Hasil akhir memakai `k = 2`:
  - **Klaster 1 (67 negara):** Rata-rata lima indikator kesejahteraan lebih rendah.
  - **Klaster 2 (78 negara):** Rata-rata lima indikator kesejahteraan lebih tinggi.

## Cara membaca slide online dan laporan

Buka https://febrianggi.github.io/global-well-being/ (berasal dari [`index.html`](index.html)) untuk menelusuri seluruh **25 slide** (`slides/01.webp` sampai `slides/25.webp`) langsung di browser:

- Gunakan panah di layar (`‹` / `›`) atau tombol panah di keyboard untuk berpindah slide.
- Klik **All slides** untuk membuka tampilan kisi thumbnail dan melompat langsung ke grafik tertentu (uji KMO/Bartlett, scree plot, biplot, pemilihan Elbow/Silhouette, atau profil klaster).
- Klik **Full screen** untuk mode layar penuh, atau gunakan tautan di header untuk mengunduh [`Slide.pdf`](Slide.pdf) dan [`Paper.pdf`](Paper.pdf).

**Catatan bahasa:** Slide presentasi, paper, serta komentar dan keluaran konsol di skrip R ditulis dalam bahasa Indonesia, sedangkan nama variabel dan istilah statistik (`well-being`, `clustering`, `eigenvalue`) tetap memakai istilah Inggris.

## Variabel dan alur analisis

Analisis diawali enam variabel World Happiness Report (`Log GDP per capita`, `Social support`, `Healthy life expectancy`, `Freedom to make life choices`, `Generosity`, dan `Perceptions of corruption`) lalu dilanjutkan dengan lima variabel setelah `Generosity` dikeluarkan.

[`PCA & CLUSTER.R`](PCA%20&%20CLUSTER.R) menjalankan lima tahap dari awal hingga profil klaster:

1. **Pembersihan data:** Membuang baris dengan nilai hilang (147 menjadi 145 negara) dan standardisasi z-score.
2. **Uji asumsi:** KMO, Bartlett's test of sphericity, boxplot, dan jarak Mahalanobis untuk outlier.
3. **PCA:** Tabel eigenvalue, scree plot, loading plot, biplot, dan persamaan tiap komponen.
4. **K-Means pada skor PCA:** Membandingkan `k` dengan metode Elbow dan Silhouette (`set.seed(42)`).
5. **Profil klaster:** Menghitung rata-rata variabel asli per klaster beserta daftar negara anggotanya.

## Cara menjalankan di lokal

### 1. Membuka slide web di lokal

Jalankan server HTTP lokal dari folder utama repositori agar 25 gambar slide WebP dimuat dengan lancar:

```bash
python -m http.server 8000
```

Buka http://localhost:8000 di browser. Hentikan server dengan `Ctrl+C`.

### 2. Mereproduksi analisis R

Pasang R dan paket berikut:

```r
install.packages(c(
  "tidyverse", "factoextra", "FactoMineR",
  "psych", "cluster", "corrplot"
))
```

Dataset mentah tidak disertakan di repositori ini. Unduh data World Happiness Report 2025 dari https://data.worldhappiness.report lalu simpan sebagai CSV dengan pemisah titik koma (`;`) dan desimal koma (`,`) sesuai urutan kolom berikut:

| Kolom | Isi |
|---|---|
| 1 | Year |
| 2 | Rank |
| 3 | Country name |
| 4 sampai 9 | Log GDP, Social support, Healthy life expectancy, Freedom, Generosity, Perceptions of corruption |

Buka [`PCA & CLUSTER.R`](PCA%20&%20CLUSTER.R) di RStudio lalu jalankan. Dialog `file.choose()` akan meminta Anda memilih file CSV tersebut. Dengan `seed = 42`, hasil pembagian 67/78 negara dapat diulang persis.

## Struktur repositori

| Lokasi | Isi |
|---|---|
| [`index.html`](index.html) | Penampil slide di browser |
| `slides/` | 25 gambar slide presentasi (WebP) |
| [`Slide.pdf`](Slide.pdf) | Berkas presentasi lengkap (PDF, bahasa Indonesia) |
| [`Paper.pdf`](Paper.pdf) | Laporan tertulis (PDF, bahasa Indonesia) |
| [`PCA & CLUSTER.R`](PCA%20&%20CLUSTER.R) | Skrip R analisis lengkap |

## Sumber data

Helliwell, J., Layard, R., Sachs, J., De Neve, J.-E., Aknin, L., & Wang, S. (2026). *World Happiness Report*. University of Oxford: Wellbeing Research Centre.

Data asli milik penerbitnya; cek ketentuan penggunaan di situs World Happiness Report sebelum membagikan ulang berkas datanya.

## Keterbatasan

- Data berupa cuplikan satu titik waktu (2025), sehingga perubahan antartahun tidak dianalisis.
- Hanya memakai PCA dan K-Means; metode lain seperti hierarchical clustering atau DBSCAN tidak dibandingkan.
- Nilai rata-rata silhouette 0,386 tergolong sedang, sehingga batas antara kedua klaster tidak terlalu tajam.
