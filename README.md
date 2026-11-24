# Tugas Besar Big Data: Analisis Sentimen & Opini Publik

> **Judul:** Analisis Sentimen dan Topik Respons Publik terhadap Pengunduran Diri Wakil Presiden di Berbagai Platform Media Sosial Menggunakan Orange Data Mining  
> **Mata Kuliah:** Big Data (Pertemuan 12–15)  
> **Repositori Utama:** [https://github.com/samythh/tb_bigdata](https://github.com/samythh/tb_bigdata)  
> **Panduan Resmi:** [00_Materi/P12 - Laporan Analisis Data.pdf](00_Materi/P12%20-%20Laporan%20Analisis%20Data.pdf)

---

## 📌 Ringkasan Proyek

Penelitian ini memotret dan menganalisis secara komparatif respons opini masyarakat Indonesia terhadap peristiwa penyerahan surat pengunduran diri Wakil Presiden Republik Indonesia ke MPR RI pada linimasa November 2026. Analisis dilakukan secara lintas platform (*cross-platform*) pada **X (Twitter)**, **YouTube**, dan **TikTok** untuk memetakan polarisasi sentimen, tema pembicaraan utama, serta dinamika persepsi publik.

### 🏛️ Stakeholder Utama & Kebutuhan Informasi
* **Stakeholder:** **Kantor Komunikasi Kepresidenan (Presidential Communication Office / PCO)** & **KPU / Pemerintah**.
* **Kebutuhan Informasi:**
  1. *Apa pola dan sebaran sentimen publik (positif, netral, negatif) terhadap peristiwa ini di media sosial?*
  2. *Narasi atau isu apa yang paling banyak dibicarakan dan memiliki tingkat resonansi (*engagement*) tertinggi?*
  3. *Bagaimana rekomendasi strategi komunikasi publik yang tepat guna meredam disinformasi, spekulasi liar, serta menjaga stabilitas nasional?*

---

## 🔄 Alur Kerja Data (Estafet Pipeline)

Proyek ini dijalankan secara berantai dan terintegrasi antar anggota kelompok:

```
[2️⃣ YouTube]  ──┐
[3️⃣ X (Twitter)] ─┼─► [3️⃣ Gabung Data] ─► [5️⃣ Preprocessing Teks] ─► [6️⃣ Labeling & Model] ─► [7️⃣ Visualisasi & Insight] ─► [1️⃣ Laporan & Slide]
[4️⃣ TikTok]   ──┘   (data_mentah_gabungan)    (preprocessing.ows)        (model_sentimen.ows)      (analisis_visualisasi.ows)     (Laporan Final .docx)
```

---

## 📊 Status & Rekapitulasi Dataset

Seluruh data mentah telah berhasil dikumpulkan, dibersihkan dari duplikat, dan distandarisasi ke dalam format UTF-8 bebas *newline*:

| Platform | Metode Pengumpulan | Target Min. | Data Bersih Unik | Status Kuota | Lokasi File |
|---|---|---|---|---|---|
| **X (Twitter)** | Browser Console Scraping | 500 | **767 baris** | 🟢 Terpenuhi (153%) | `01_Dataset/raw/data_mentah_x.csv` |
| **YouTube** | YouTube Data API v3 | 500 | **1.000 baris** | 🟢 Terpenuhi (200%) | `01_Dataset/raw/data_mentah_youtube.csv` |
| **TikTok** | Assisted Manual Scraping | 500 | **1.017 baris** | 🟢 Terpenuhi (203%) | `01_Dataset/raw/data_mentah_tiktok.csv` |
| **DATA GABUNGAN** | Multi-Platform Consolidation | 1.500 | **2.776 baris** | 🚀 **LENGKAP (185%)** | `01_Dataset/raw/data_mentah_gabungan.csv` |
| **SAMPEL PELABELAN**| Stratified Random Sampling | 750–1.100 | **900 baris** | 🎯 **Siap Dilabel (300/platform)** | `01_Dataset/raw/sampel_pelabelan.csv` |

---

## 📏 Skema Kontrak Data (8 Kolom Baku)

Semua dataset wajib mematuhi skema 8 kolom standar untuk kompatibilitas penuh dengan Orange Data Mining:

| No | Kolom | Tipe | Deskripsi & Format | Contoh |
|---|---|---|---|---|
| 1 | `id` | String | Pengenal unik record (*primary key*) | `X-0001`, `YT-0001`, `TT-0001` |
| 2 | `tanggal` | Date | Tanggal postingan (Format ISO: `YYYY-MM-DD`) | `2026-11-12` |
| 3 | `platform` | Categorical | Asal platform (huruf kecil: `x`, `youtube`, `tiktok`) | `x` |
| 4 | `teks` | Text | Teks opini yang telah dianonimkan (bebas newline) | `Menghormati putusan konstitusi...` |
| 5 | `like` | Numeric | Jumlah apresiasi suka (*likes count*) | `15` |
| 6 | `balas` | Numeric | Jumlah balasan komentar (*replies count*) | `4` |
| 7 | `kata_kunci`| Categorical | Topik penelusuran / sumber data | `pengunduran diri wapres` |
| 8 | `sentimen`  | Categorical | Target klasifikasi (`positif`, `netral`, `negatif`) | *(Diisi saat pelabelan)* |

*🔒 **Etika Data:** Seluruh mention username akun pengguna asli telah disamarkan menjadi `@user`, serta bebas dari nomor telepon pribadi dan URL.*

---

## 📁 Struktur Direktori Repositori

```
tb_bigdata/
├── 00_Materi/               # Panduan tugas besar, slide P12, dan rubrik penilaian
├── 01_Dataset/
│   ├── raw/                 # Dataset mentah 3 platform, gabungan, & sampel pelabelan
│   │   ├── data_mentah_x.csv
│   │   ├── data_mentah_youtube.csv
│   │   ├── data_mentah_tiktok.csv
│   │   ├── data_mentah_gabungan.csv
│   │   └── sampel_pelabelan.csv
│   └── processed/           # Dataset hasil preprocessing & hasil prediksi sentimen
├── 02_Orange_Workflow/      # File alur kerja Orange Data Mining (.ows)
│   ├── preprocessing.ows    # Workflow pembersihan & normalisasi teks
│   ├── model_sentimen.ows   # Workflow training & perbandingan model klasifikasi
│   └── visualisasi.ows      # Workflow wordcloud, sebaran topik, & sentimen
├── 03_Visualisasi/          # Ekspor grafik (PNG) & screenshot workflow Orange
├── 04_Laporan/              # Naskah laporan final tugas besar (diisi setelah kompilasi selesai)
├── 05_Presentasi/           # Slide presentasi Pertemuan 14 & 15
├── 06_Referensi/            # Dokumen jurnal/referensi pendukung (APA 7)
├── Final_Submit/            # Berkas final untuk pengumpulan di Google Classroom
├── IDE_TOPIK.md             # Dokumen eksplorasi awal topik
└── README.md                # Dokumentasi utama proyek
```

---

## 👥 Pembagian Peran & Tanggung Jawab Anggota

| Peran | Tanggung Jawab Utama | Output Utama | Status |
|---|---|---|---|
| **1️⃣ Koordinator & Integrator** | Rumusan masalah, stakeholder, template docx, BAB I & V, kompilasi laporan & slide | `Laporan Final (.docx)`, `Slide Presentasi` | Sedang Berjalan |
| **2️⃣ Pengumpul YouTube** | Crawling komentar YouTube berita via API, timeline peristiwa, BAB I | `data_mentah_youtube.csv`, Timeline | 🟢 Selesai |
| **3️⃣ Pengumpul X & Integrator** | Crawling data X, standarisasi 8 kolom, gabung 3 platform, buat sampel 900 baris, BAB II | `data_mentah_x.csv`, `data_mentah_gabungan.csv`, `sampel_pelabelan.csv`, BAB II (Google Docs) | 🟢 Selesai |
| **4️⃣ Pengumpul TikTok** | Crawling komentar TikTok berita/edukator, log pengumpulan, bagian etika data | `data_mentah_tiktok.csv`, Dokumentasi Etika | 🟢 Selesai |
| **5️⃣ Preprocessing Teks** | Kamus gaul (slang), stopwords, pembersihan teks di Orange (`preprocessing.ows`), BAB III | `preprocessing.ows`, `data_bersih.csv`, `BAB III` | Tahap Selanjutnya |
| **6️⃣ Pelabelan & Pemodelan** | Panduan label, pelabelan sampel 900 baris, training model (SVM, Naive Bayes, LR), BAB IV.1-2 | `model_sentimen.ows`, `data_berlabel.csv`, Evaluasi Model | Tahap Selanjutnya |
| **7️⃣ Analisis & Visualisasi** | Grafik sebaran sentimen, tren waktu, word cloud, rumusan insight & rekomendasi, BAB IV.3-5 | `visualisasi.ows`, Grafik PNG, BAB IV.3-5 | Tahap Selanjutnya |

---

## ✅ Checklist Standar Luaran Minimum (Rubrik P12)

- [x] Sumber data primer digital lintas platform (≥ 500 baris per platform, total 2.776 baris unik).
- [x] Kontrak skema 8 kolom baku dan kepatuhan etika anonimisasi data.
- [ ] Minimal 3 pertanyaan analisis yang terdefinisi jelas.
- [ ] Minimal 3 visualisasi data yang relevan dari Orange Data Mining.
- [ ] Minimal 3 temuan (*insight*) utama berbasis bukti (*evidence-based*).
- [ ] Minimal 2 rekomendasi strategis bagi stakeholder (Kantor Komunikasi Kepresidenan/PCO).
- [ ] Penjelasan keterbatasan dataset dan metodologi analisis.
- [ ] Naskah laporan final sesuai format (A4, TNR 12pt, spasi 1.5, margin 4-3-3-3 cm, sitasi APA 7).
