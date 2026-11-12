# Ide Topik TB Big Data

Status: **belum final**.

✅ Sudah dikonfirmasi dosen: topik **tidak harus** dari tabel contoh di slide P12 (tabel itu hanya contoh).
Jadi aplikasi di luar kategori tabel (Coretax, MyPertamina, dll) dan topik politik juga boleh.

---

## ⭐ Kandidat utama: Analisis Ulasan Aplikasi KAI Access (Transportasi)

- **Kategori slide:** Transportasi, yaitu "pola perjalanan dan waktu permintaan tinggi"
- **Stakeholder:** PT KAI (pengelola transportasi / tim produk aplikasi)
- **Data:** ulasan Google Play Store, di-scrape sendiri (data primer): teks ulasan, rating 1–5, tanggal, versi aplikasi
- **Tools Orange:** Text add-on (Preprocess Text, Word Cloud, Topic Modelling, Sentiment), klasifikasi (rating sebagai label: 1–2 = negatif, 4–5 = positif)

**Menjawab 3 pertanyaan TB:**
1. *Temuan:* kapan keluhan memuncak (Lebaran, Nataru, libur sekolah) dan topik keluhan dominan (gagal bayar, war tiket, server down, dll)
2. *Makna:* sistem belum siap menghadapi lonjakan pengguna saat musim ramai, sehingga kepercayaan pengguna turun
3. *Rekomendasi:* tambah kapasitas server sebelum musim mudik, perbaiki alur pembayaran, terapkan antrean virtual

**Catatan:**
- Sentimen otomatis Orange kurang akurat untuk bahasa gaul Indonesia, jadi pakai rating sebagai label
- Data scraping sebaiknya tidak di-upload mentah ke repo public (sebutkan di laporan)

### Variasi aplikasi lain sesuai kategori slide

| Kategori | Aplikasi | Fokus analisis |
|---|---|---|
| E-commerce | Shopee / Tokopedia / Lazada | Keluhan keterlambatan pengiriman, kurir, paket hilang |
| Kesehatan masyarakat | Mobile JKN (BPJS) | Keluhan antrean online, rujukan, layanan faskes |

### Pengembangan: perbandingan 3 aplikasi layanan publik

Bandingkan keluhan di **Coretax DJP, Mobile JKN, dan KAI Access**. Stakeholder tambahan: Komdigi / KemenPAN-RB.

> Judul usulan: *Analisis Keluhan Pengguna Aplikasi Layanan Publik Digital (Coretax, Mobile JKN, dan KAI Access) Berdasarkan Ulasan Google Play Store untuk Mendukung Perbaikan Layanan Menggunakan Orange Data Mining*

---

## 🔥 Kandidat kedua: Respons Publik terhadap Pengunduran Diri Wakil Presiden

Peristiwa: Wapres menyerahkan surat pengunduran diri ke MPR pada 12 November 2026. MPR punya waktu maksimal 60 hari untuk memilih pengganti.

- **Data:** komentar YouTube (YouTube Data API, data utama), Google Trends (CSV); pembanding TikTok / Instagram / X dikumpulkan manual (±100–200 komentar per platform)
- **Analisis:** sentimen dan topik per platform, perubahan sentimen dari waktu ke waktu (sebelum vs sesudah sidang istimewa MPR), perbandingan framing berita antar media
- **Stakeholder utama (pilih satu):** Kantor Komunikasi Kepresidenan (strategi komunikasi publik) atau KPU & Kemendikdasmen (verifikasi ijazah syarat pencalonan)

> Judul usulan: *Analisis Sentimen dan Topik Respons Publik terhadap Pengunduran Diri Wakil Presiden di Berbagai Platform Media Sosial untuk Mendukung Strategi Komunikasi Publik Pemerintah Menggunakan Orange Data Mining*

**Catatan:**
- Topik sensitif, jadi analisis harus netral (memotret opini, bukan menilai benar atau salah)
- Perlu labeling manual ±300–500 komentar, karena sentimen otomatis lemah untuk sarkasme
- Scraping otomatis X / TikTok / IG melanggar aturan platform, jadi pakai pengumpulan manual
- Jumlah data antar platform tidak seimbang, jadi bandingkan dalam persentase
- Nama akun komentator harus disamarkan
- Data harus diambil secepatnya, karena reaksi awal publik paling banyak di hari-hari pertama

---

## Alternatif lain

| Topik | Sumber data | Catatan |
|---|---|---|
| Harga pangan (beras/cabai) per provinsi | Bapanas di data.go.id (bulanan, 2026) | Data agregat; Orange lemah untuk time series |
| Gempa bumi | Kaggle "Earthquakes in Indonesia" (BMKG) | Data rapi, tapi rekomendasi cenderung umum |
| Keracunan MBG | Hanya dari berita | Tidak disarankan: tidak ada dataset resmi, angka antar-lembaga berbeda |
| Partisipasi pemilih Pilkada 2024 per kab/kota (politik) | KPU (perlu dikompilasi) + indikator BPS | Netral, stakeholder KPU; CSV resmi per kab/kota belum ditemukan |
| Sentimen publik kebijakan (komentar YouTube) | YouTube Data API | Mudah diambil, tapi rawan buzzer/spam dan dianggap memihak |
