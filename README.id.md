[🇬🇧 English](./README.md) | **🇮🇩 Bahasa Indonesia**

# 📊 Walmart Retail Operations Analysis — Validated Business Insights

[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)](#)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](#)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](#)

Analisis data transaksi cabang Walmart menggunakan SQL + Python untuk mengungkap pola operasional dalam penjadwalan staf, musiman, dan performa cabang — dengan fokus khusus pada kesimpulan yang teruji secara statistik.

**Sumber data:** Dataset transaksi ritel Walmart (Kaggle)

---

## 📑 Daftar Isi

- [Sekilas Hasil](#-sekilas-hasil)
- [Tentang Proyek Ini](#-tentang-proyek-ini)
- [Tech Stack](#-tech-stack)
- [Kedalaman Analitis](#-kedalaman-analitis)
- [Temuan Utama](#-temuan-utama)
- [Sampel Hasil](#-sampel-hasil)
- [Cara Menjalankan](#️-cara-menjalankan)
- [Kontak](#-kontak)

---

## 📸 Sekilas Hasil

<img src="./assets/01_revenue_by_category.png" alt="Grafik batang total revenue per kategori produk, dengan Fashion accessories dan Home and lifestyle memimpin" width="720">

<img src="./assets/02_hourly_trend.png" alt="Grafik garis volume transaksi per jam, menunjukkan puncak tajam antara jam 15:00-20:00" width="720">

<img src="./assets/03_monthly_seasonality.png" alt="Grafik batang volume transaksi bulanan 2020-2023, menunjukkan lonjakan drastis di bulan November dan Desember" width="720">

---

## 📌 Tentang Proyek Ini

**Tujuan**  
Mengidentifikasi tren revenue, mengevaluasi performa kategori produk, dan memetakan pola belanja pelanggan di berbagai cabang Walmart — menerjemahkan data transaksi mentah menjadi rekomendasi actionable untuk penjadwalan staf, perencanaan inventori, dan penyelarasan kalender marketing.

**Metodologi**  
Data diekstrak dan diagregasi menggunakan query PostgreSQL untuk menjawab pertanyaan bisnis operasional yang spesifik, lalu diproses dengan Pandas dan divisualisasikan dengan Matplotlib/Seaborn di Python.

<details>
<summary><strong>📁 Struktur Repo</strong> (klik untuk lihat)</summary>

```
walmart-retail-analytics-validated/
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── assets/
│   ├── 01_revenue_by_category.png
│   ├── 02_hourly_trend.png
│   └── 03_monthly_seasonality.png
├── data/
│   ├── Walmart.csv
│   └── walmart_clean_data.csv
└── Walmart_sales_analysis_project.ipynb
```

Kredensial database dimuat dari file `.env` lokal (tidak ikut di-commit ke repo ini) menggunakan `python-dotenv` — lihat [Cara Menjalankan](#️-cara-menjalankan) untuk setup-nya.

</details>

---

## 🧩 Tech Stack

| Kategori | Tools |
|---|---|
| **Database** | PostgreSQL |
| **Bahasa** | Python |
| **Libraries** | Pandas (manipulasi data), SQLAlchemy (koneksi DB), Matplotlib & Seaborn (visualisasi), python-dotenv (manajemen kredensial) |
| **Environment** | Jupyter Notebook |

---

## 🔍 Kedalaman Analitis

Selama proses analisis, ditemukan dan diperbaiki dua isu metodologi sebelum kesimpulan difinalisasi:

| Isu yang Ditemukan | Masalah | Perbaikan yang Diterapkan |
|---|---|---|
| **Sample size terlalu kecil di perbandingan YoY per cabang** | Beberapa cabang tampak mengalami "penurunan revenue 60%", tapi jumlah transaksi yang mendasarinya cuma 7-19 per tahun — terlalu sedikit untuk jadi sinyal yang reliable | Ditambahkan ambang batas sample size minimum (≥20 transaksi/tahun) untuk menandai penurunan mana yang benar-benar bermakna secara statistik vs. yang kemungkinan besar cuma noise |
| **Bias data partial-year di analisis musiman bulanan** | Data 2019 cuma tersedia untuk Januari-Maret; kalau ikut digabung ke agregat bulanan, angka Q1 jadi menggelembung secara artifisial dibanding bulan lain | Data 2019 dikeluarkan dari analisis musiman bulanan, hanya memakai tahun yang lengkap (2020-2023) untuk perbandingan antar bulan yang adil |

Perbaikan kedua ini juga yang mengungkap **temuan paling kuat di seluruh analisis** — lonjakan volume transaksi ~9x lipat di bulan November-Desember.

---

## 📊 Temuan Utama

### 1️⃣ Musiman Bulanan
**Metodologi:** 
Volume transaksi per bulan, diagregasi dari 2020-2023 (2019 dikeluarkan karena data parsial).

**Temuan:** 
November dan Desember menunjukkan lonjakan drastis, sekitar **9x lebih tinggi** dibanding musim sepi (Jan-Jul). Agustus-Oktober menunjukkan kenaikan moderat, kemungkinan periode persiapan menjelang musim liburan.

**Insight:** 
Ini pola musiman terkuat di seluruh dataset — jauh lebih menonjol dibanding pola harian (per jam) — dan kuat mengindikasikan efek musim belanja liburan (Black Friday, Natal), konsisten di semua tahun yang diamati.

**Rekomendasi:** 
Mulai tingkatkan stok dan tenaga kerja temporer sejak Oktober, dengan kesiapan puncak di 1 November. Fokuskan kampanye promosi dan budget iklan di window Agustus-Desember, bukan disebar merata sepanjang tahun.

### 2️⃣ Pola Transaksi per Jam
**Metodologi:** 
Volume transaksi per jam, diagregasi dari seluruh dataset.

**Temuan:** 
Volume stabil rendah (300-400 transaksi) dari jam 06:00-14:00, lalu melonjak tajam ke ~1.200 transaksi mulai jam 15:00, tetap tinggi sampai jam 20:00.

**Insight:** 
Aktivitas pelanggan terkonsentrasi di window sore-malam selama lima jam, membantah asumsi bahwa toko paling ramai saat jam makan siang. Kemungkinan besar ini didorong kebiasaan belanja sepulang kerja.

**Rekomendasi:** 
Kurangi jumlah staf pagi ke level minimum fungsional; konsentrasikan tenaga kerja dan jadwal restocking di window 15:00-20:00 untuk mencegah antrean panjang di kasir.

### 3️⃣ Revenue & Profit per Kategori
**Metodologi:** 
Total revenue dan profit per kategori produk.

**Temuan:** 
Fashion Accessories dan Home & Lifestyle menghasilkan total profit tertinggi — didorong oleh volume transaksi yang jauh lebih tinggi (10x+ lebih banyak transaksi dibanding kategori dengan performa rendah), bukan karena margin profit yang lebih tinggi. Margin sebenarnya mirip (~39-40%) di semua kategori.

**Insight:** 
"Dominasi" kategori di sini adalah soal volume, bukan soal margin — pembedaan penting untuk keputusan alokasi ruang display dan inventori.

**Rekomendasi:** 
Perluas ruang display fisik untuk kategori dengan volume tinggi; jangan berasumsi margin lebih tinggi sebagai pendorong saat realokasi investasi inventori.

### 4️⃣ Penurunan Revenue YoY per Cabang
**Metodologi:** 
Perbandingan revenue year-over-year per cabang (2022 vs. 2023), dengan flag sample size minimum (≥20 transaksi/tahun) untuk membedakan sinyal yang reliable dari noise.

**Temuan:** 
Beberapa cabang menunjukkan penurunan revenue yang tampak signifikan, tapi sebagian besar punya jumlah transaksi tahunan yang sangat rendah (sering di bawah 20), membuat persentase perubahannya tidak reliable secara statistik di level cabang individual.

**Insight:** 
Tanpa pengecekan ini, cabang dengan volume rendah bisa saja salah dilaporkan sebagai "krisis" operasional hanya berdasarkan segelintir transaksi.

**Rekomendasi:** 
Prioritaskan cabang yang memenuhi ambang batas sample size minimum untuk investigasi; perlakukan temuan dari cabang bervolume rendah sebagai sinyal awal yang butuh lebih banyak data sebelum diambil tindakan.

### 5️⃣ Metode Pembayaran & Kepuasan Pelanggan
**Temuan:** 
Kartu kredit memimpin secara keseluruhan (4.200+ transaksi), tapi e-wallet jadi pilihan teratas di level cabang untuk sebagian besar lokasi — transaksi tunai tertinggal jauh dari keduanya. Rating kepuasan sangat bergantung pada lokasi (misal kategori "Health and beauty" mendapat skor 9.8 di satu cabang vs. 6.9 di cabang lain untuk kategori yang sama).

**Insight:** 
Infrastruktur pembayaran digital sebaiknya diprioritaskan di seluruh perusahaan, sementara isu kualitas layanan tampaknya spesifik per cabang, bukan sistemik — mendukung pendekatan audit layanan yang disesuaikan per cabang, bukan kebijakan yang seragam.

---

## 📄 Sampel Hasil

| File | Isi |
|---|---|
| [`walmart_sales_analysis_project.ipynb`](./walmart_sales_analysis_project.ipynb) | Notebook lengkap: pembersihan data, query SQL, visualisasi, dan interpretasi bisnis untuk setiap temuan di atas |

---

## 🛠️ Cara Menjalankan

1. Clone repo ini.
2. Install dependencies: `pip install -r requirements.txt`
3. Siapkan database PostgreSQL lokal dan muat dataset dari folder `data/`.
4. Buat file `.env` di root proyek (file ini tidak disertakan di repo — lihat `.gitignore`) dengan variabel berikut:
   ```
   DB_USER=username_postgres_kamu
   DB_PASSWORD=password_postgres_kamu
   DB_HOST=localhost
   DB_PORT=5432
   DB_NAME=nama_database_kamu
   ```
5. Buka `walmart_sales_analysis_project.ipynb` dan jalankan semua cell secara berurutan.

---

## 📬 Kontak

Terbuka untuk diskusi, feedback, atau peluang kolaborasi terkait proyek ini.

- **LinkedIn:** [linkedin.com/in/gloryanisveronicalase](https://linkedin.com/in/gloryanisveronicalase)
- **Email:** gloryanislase@gmail.com
