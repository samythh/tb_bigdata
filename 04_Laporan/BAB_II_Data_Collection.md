# BAB II: DATA COLLECTION DAN PEMAHAMAN DATASET

## 2.1 Deskripsi Dataset
Dataset yang digunakan dalam penelitian ini merupakan dataset opini publik berbasis teks (*user-generated text*) yang dihimpun dari tiga platform media sosial terkemuka di Indonesia, yaitu **X (dahulu Twitter)**, **YouTube**, dan **TikTok**. Pengumpulan data difokuskan pada respons, percakapan, kritik, serta reaksi masyarakat terhadap isu pengunduran diri Wakil Presiden Republik Indonesia beserta dinamika politik, hukum, dan etika yang menyertainya (termasuk polemik keabsahan syarat pencalonan, isu pemakzulan, hingga jejak digital akun Fufufafa).

Ketiga platform tersebut dipilih secara sengaja (*purposive*) guna merepresentasikan demografi dan karakteristik warganet yang beragam:
1. **X (Twitter):** Merepresentasikan diskursus opini publik yang dinamis, cepat, sarat akan perdebatan politik, analisis kritis, serta percakapan berbasis teks secara *real-time*.
2. **YouTube:** Merepresentasikan tanggapan audiens terhadap pemberitaan video berdurasi panjang dari kanal-kanal berita resmi dan independen (seperti Kompas TV, CNN Indonesia, dll.), yang cenderung menyajikan argumen lebih panjang dan kontekstual.
3. **TikTok:** Merepresentasikan respons masyarakat dari kalangan generasi muda (Gen-Z dan milenial) yang merespons cuplikan video singkat viral seputar peristiwa politik terkini.

Data yang dikumpulkan mencakup atribut metadata penting seperti tanggal pengunggahan, platform asal, teks komentar/cuitan, metrik keterlibatan (*likes* dan *replies*), serta kata kunci pencarian. Sebelum dilakukan analisis pemodelan, dataset mentah ini melalui standardisasi skema data agar dapat digabungkan secara terpadu.

---

## 2.2 Sumber dan Metode Pengumpulan Data

### 2.2.1 Sumber Data
Sumber data primer diperoleh secara langsung dari media sosial menggunakan teknik penambangan data digital (*digital data scraping*) dan pemanfaatan Application Programming Interface (API) resmi, dengan rincian sebagai berikut:
- **Platform X:** Diambil dari linimasa percakapan publik, cuitan orisinal, serta balasan (*replies*) terhadap isu terkait mulai rentang peristiwa November 2026.
- **Platform YouTube:** Diambil dari kolom komentar pada 5–10 video berita resmi dari stasiun televisi dan media berita nasional terpercaya yang memberitakan pengunduran diri Wapres dan sidang istimewa MPR.
- **Platform TikTok:** Diambil dari kolom komentar video-video viral bertema berita politik terkait.

### 2.2.2 Metode dan Prosedur Pengumpulan Data
Pengumpulan data dilakukan dengan pendekatan terstruktur untuk memastikan validitas dan keterwakilan data:
1. **Penentuan Kata Kunci (Query Formulation):**
   Pengumpulan data menggunakan serangkaian kata kunci terarah dalam bahasa Indonesia guna membatasi cakupan topik agar tetap relevan dan menghindari *noise*. Kata kunci utama yang disepakati bersama meliputi:
   - *"Gibran mundur"*
   - *"pengunduran diri wapres"*
   - *"wapres pengganti"*
   - *"sidang istimewa MPR"*
   - *"keabsahan wakil presiden / ijazah Gibran"*
   - *"akun fufufafa wapres"*

2. **Ekstraksi Data X (Twitter):**
   Pengumpulan data X dilakukan menggunakan *crawling tool* berbasis Node.js/Chromium (`tweet-harvest`) dengan parameter penelusuran spesifik:
   - Filter bahasa: `lang:id` (hanya cuitan berbahasa Indonesia).
   - Filter retweet: `-is:retweet` (menghindari duplikasi masif dari fitur *retweet* otomatis).
   - Penjelajahan linimasa: Mengombinasikan tab *Top Tweets* dan *Latest Tweets* untuk menangkap baik cuitan dengan atensi tinggi maupun dinamika kronologis netizen.
   - Dari 181 variasi pencarian, terkumpul sebanyak **4.558 baris data mentah**, yang kemudian melalui tahap eliminasi duplikat ID sehingga menghasilkan **1.634 cuitan unik**.

3. **Ekstraksi Data YouTube:**
   Pengumpulan komentar video YouTube dilakukan menggunakan skrip otomasi yang memanfaatkan layanan resmi **YouTube Data API v3** (`commentThreads.list`). Data diambil dari video pemberitaan dengan interaksi tertinggi untuk menjaring perspektif pemirsa berita.

4. **Ekstraksi Data TikTok:**
   Pengumpulan komentar TikTok dilakukan dengan teknik *assisted scraping* / ekstraksi manual terstruktur terhadap video berita viral, di mana format tanggal relatif (seperti *"3 hari lalu"*) dikonversi menjadi tanggal absolut (*YYYY-MM-DD*).

### 2.2.3 Prinsip Etika dan Privasi Data
Dalam menjaga integritas akademik dan privasi pengguna media sosial, proses pengumpulan data mematuhi prinsip-prinsip etika data:
- **Anonimisasi Identitas:** Seluruh nama akun (*username*), nama tampilan (*display name*), dan ID pengguna tidak disimpan dalam dataset akhir. Mention terhadap akun tertentu (misal `@nama_orang`) disamarkan menjadi `@user`.
- **Penghapusan Data Sensitif:** Tautan (*URL*), nomor telepon pribadi, dan informasi identitas pribadi (*Personally Identifiable Information / PII*) dibersihkan dari isi teks.
- **Tujuan Akademik:** Data yang dikumpulkan bersifat publik dan semata-mata dimanfaatkan untuk kepentingan analisis akademik, bukan untuk profiling individu, penyerangan pribadi, atau komersialisasi.

---

## 2.3 Karakteristik dan Struktur Dataset

### 2.3.1 Struktur Kolom Dataset
Untuk menyatukan data dari ketiga platform yang memiliki struktur asal berbeda, tim menetapkan sebuah kontrak standar yang terdiri dari **8 kolom wajib**. Struktur ini dirancang untuk kompatibilitas penuh dengan perangkat lunak **Orange Data Mining**:

| No | Nama Kolom | Tipe Data | Deskripsi dan Format | Contoh Isi |
|---|---|---|---|---|
| 1 | `id` | String | Pengenal unik berurutan per platform sebagai *primary key* | `X-0001`, `YT-0001`, `TT-0001` |
| 2 | `tanggal` | Date | Tanggal publikasi postingan (Format ISO: `YYYY-MM-DD`) | `2026-10-13`, `2026-11-12` |
| 3 | `platform` | Categorical | Nama platform media sosial sumber (huruf kecil) | `x`, `youtube`, `tiktok` |
| 4 | `teks` | Text | Teks komentar/cuitan bersih dari *line break*, siap diolah | *"Lebih terhormat hormati putusan MK..."* |
| 5 | `like` | Integer | Jumlah tanda suka (*likes/favorites*), default: 0 | `15`, `120` |
| 6 | `balas` | Integer | Jumlah balasan (*replies/comments count*), default: 0 | `2`, `14` |
| 7 | `kata_kunci`| Categorical | Kata kunci pencarian atau topik pemicu pengumpulan | `pengunduran diri wapres` |
| 8 | `sentimen`  | Categorical | Label kelas sentimen (*positif*, *netral*, *negatif*) | *(Diisi saat tahap pelabelan)* |

### 2.3.2 Distribusi dan Volume Data
Berdasarkan hasil pengumpulan dan pra-pembersihan tahap awal, rekapitulasi volume data antarmedia dijabarkan pada tabel berikut:

| Platform | Metode Pengumpulan | Target Minimal | Data Mentah Terkumpul | Data Bersih & Unik | Status Keterpenuhan |
|---|---|---|---|---|---|
| **X (Twitter)** | `tweet-harvest` (Node.js) | 500 baris | 750 | **750 baris** | **Terpenuhi (150%)** |
| **YouTube** | YouTube Data API v3 | 500 baris | 1000 | **1000 baris** | **Terpenuhi (200%)** |
| **TikTok** | Assisted Manual Scraping | 500 baris | 1191 | **1191 baris** | **Terpenuhi (238%)** |
| **Total Gabungan** | Multi-platform merge | 1.500 baris | 2941 | **2923 baris** | **Lengkap (194%)** |

---

## 2.4 Relevansi Dataset terhadap Permasalahan

Keberhasilan analisis data sangat bergantung pada derajat kesesuaian antara data yang dihimpun dengan rumusan masalah yang dihadapi oleh *stakeholder* utama, yaitu **Kantor Komunikasi Kepresidenan (Presidential Communication Office / PCO)** dan **KPU / Pemerintah**. Dataset ini memiliki tingkat relevansi yang sangat tinggi ditinjau dari tiga dimensi:

1. **Memotret Sentimen Publik Secara *Multi-Perspective*:**
   Isu pengunduran diri seorang pimpinan negara merupakan peristiwa politik luar biasa yang memicu polarisasi opini yang tajam. Dengan menghimpun percakapan dari X, YouTube, dan TikTok, dataset ini mampu menangkap beragam spektrum sentimen:
   - Opini kritis/kontra (tuntutan pertanggungjawaban etika, desakan pengunduran diri).
   - Opini pro/legitimasi (penghormatan terhadap putusan pemilu, narasi stabilitas nasional).
   - Opini netral/analitis (diskusi tata negara mengenai mekanisme sidang istimewa MPR dan figur pengganti).

2. **Dukungan Terhadap Pengambilan Keputusan Strategis Komunikasi:**
   Melalui atribut metrik interaksi (`like` dan `balas`), stakeholder dapat mengidentifikasi bukan hanya apa yang dibicarakan publik, melainkan **narasi mana yang paling banyak diamplifikasi dan memperoleh resonansi publik paling kuat**. Hal ini krusial bagi Kantor Komunikasi Kepresidenan dalam merancang strategi diseminasi informasi publik yang responsif, menenangkan spekulasi liar, serta mengikis disinformasi.

3. **Kesiapan Teknis untuk Pengolahan di Orange Data Mining:**
   Data teks telah melalui proses standardisasi bebas karakter tersembunyi (*hidden line breaks* dan karakter non-UTF-8), sehingga siap dimasukkan ke dalam *widget* **Corpus**, **Preprocess Text**, **Word Cloud**, hingga **Sentiment Analysis / Classification** di Orange Data Mining tanpa kendala ketidakcocokan tipe data.
