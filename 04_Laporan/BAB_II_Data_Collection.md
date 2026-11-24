# BAB II: DATA COLLECTION DAN PEMAHAMAN DATASET

## 2.1 Deskripsi Dataset

Dataset yang digunakan dalam penelitian ini berupa data teks komentar dan opini publik yang dihimpun dari tiga platform media sosial, yaitu YouTube, X (Twitter), dan TikTok. Unit data terkecil yang dianalisis adalah per satu baris data yang mewakili satu komentar atau cuitan unik dari pengguna. Seluruh data yang diambil menggunakan bahasa Indonesia, baik ragam bahasa baku maupun ragam bahasa percakapan sehari-hari (bahasa gaul) yang lazim digunakan masyarakat pada kolom interaksi media sosial.

Pengumpulan data difokuskan secara ketat pada rentang waktu **12 November hingga 24 November 2026**. Periode ini dipilih karena bertepatan langsung dengan peristiwa penyerahan surat pengunduran diri Wakil Presiden Republik Indonesia (Gibran Rakabuming Raka) ke pimpinan MPR RI pada 12 November 2026, serta dinamika respons masyarakat yang berkembang pada hari-hari setelahnya. Secara keseluruhan, terkumpul sebanyak 2.767 baris data mentah dari ketiga platform, yang setelah melalui tahap pembersihan dan deduplikasi menghasilkan **2.759 data gabungan unik**. Rincian jumlah sumber, volume data, dan periode waktu pengumpulan disajikan pada Tabel 2.1.

### Tabel 2.1 Ringkasan Dataset per Platform

| Platform | Jumlah Sumber | Jumlah Komentar Mentah | Periode Komentar |
| :--- | :--- | :---: | :---: |
| **YouTube** | 5 video berita resmi | 1.000 komentar | 12 November 2026 – 19 November 2026 |
| **X (Twitter)** | Kueri penelusuran terarah | 750 cuitan unik | 12 November 2026 – 18 November 2026 |
| **TikTok** | 15 video konten viral | 1.017 komentar | 12 November 2026 – 24 November 2026 |
| **Total** | 20 video & kueri penelusuran | **2.767 komentar mentah** | **12 November 2026 – 24 November 2026** |

*Sumber: Data primer penelitian diolah (2026)*

---

## 2.2 Sumber dan Metode Pengumpulan Data

### 2.2.1 YouTube
Data komentar dari platform YouTube dikumpulkan menggunakan skrip Python yang memanfaatkan antarmuka resmi **YouTube Data API v3**, khususnya *endpoint* `commentThreads.list`. Pengambilan data dilaksanakan pada tanggal 19 November 2026.

Kriteria pemilihan video didasarkan pada tayangan berita dari kanal media massa nasional terverifikasi dan kredibel yang memiliki tingkat penayangan (*views*) dan interaksi tinggi seputar peristiwa pengunduran diri Wapres. Sumber video mencakup 5 kanal berita: tvOneNews, Liputan6, BeritaSatu, KOMPASTV Jember, serta tayangan dokumentasi Sekretariat Wakil Presiden RI. Kata kunci penelusuran meliputi: *"pengunduran diri wapres"*, *"Gibran mundur"*, dan *"sidang MPR"*. Dari proses ekstraksi ini, berhasil diperoleh tepat **1.000 baris komentar teratas** beserta metadata jumlah tanda suka (*likes*) dalam rentang tanggal 12 hingga 19 November 2026.

### 2.2.2 X (Twitter)
Pengumpulan data dari platform X dilakukan menggunakan alat bantu penelusuran berbasis otomasi *browser* Chromium dan Node.js, yaitu `tweet-harvest`. Pengambilan data dilaksanakan secara bertahap pada rentang tanggal 14 hingga 18 November 2026.

Penelusuran dilakukan pada linimasa publik menggunakan kueri terarah dengan memadukan tab *Top Tweets* dan *Latest Tweets*. Parameter pencarian menyertakan filter bahasa Indonesia (`lang:id`) dan pengecualian *retweet* (`-is:retweet`) agar tidak terjadi penumpukan postingan dari fitur *retweet* otomatis. Kata kunci penelusuran mencakup variasi: *"gibran mundur"*, *"pengunduran diri wapres"*, *"wapres pengganti"*, *"sidang istimewa mpr"*, *"keabsahan wakil presiden"*, dan *"fufufafa wapres"*.

Pada proses penarikan awal, penggunaan kueri umum seperti *"pengunduran diri wapres"* sempat menjaring cuitan lama dari tahun 2012–2014 (isu pengunduran diri Wapres Boediono pada masa lampau) karena kesamaan frasa kata kunci. Menyadari hal tersebut, peneliti melakukan penyaringan ketat berbasis pencocokan entitas (*Gibran*, *putusan MK*, *Mahfud MD*, *sidang MPR*) dan pemangkasan kronologis waktu secara absolut pasca 12 November 2026. Dari proses kurasi dan eliminasi duplikasi tersebut, diperoleh sebanyak **750 cuitan unik dan bersih** yang sepenuhnya memuat opini publik terkait pengunduran diri Wapres Gibran.

### 2.2.3 TikTok
Pengumpulan data dari TikTok dilakukan terhadap konten video ulasan berita dan analisis opini politik yang viral seputar pengunduran diri wakil presiden. Sumber konten mencakup akun media berita nasional (seperti KompasTV, Detikcom, MetroTV, IDN Times, Inilah.com, Pikiran Rakyat) serta kreator konten opini publik terverifikasi (seperti Raymond Chin, Andovi Da Lopez, Gerald Vincent, dan Sadam Permana).

Ekstraksi data dilakukan terhadap 15 tautan video dengan tingkat interaksi tinggi. Pada penarikan versi awal, ditemukan sejumlah komentar yang tertanggal 10–11 November akibat kalkulasi tanggal relatif pada video yang diunggah mendahului isu resmi. Untuk menjaga validitas ilmiah penelitian, tim melakukan pembaruan dataset secara menyeluruh dengan mengambil ulang data komentar yang terbit pasca peristiwa resmi. Dari pembaruan ini, terkumpul **1.017 baris komentar bersih** yang seluruhnya berada pada rentang tanggal 12 November hingga 24 November 2026.

### 2.2.4 Etika Pengumpulan Data
Proses pengumpulan data dalam penelitian ini sepenuhnya mematuhi prinsip etika penelitian sosial komputasional dan perlindungan privasi data digital warganet, dengan penerapan kaidah sebagai berikut:

1. **Anonimisasi Identitas Pengguna:** Demi menjaga privasi pemilik akun, nama akun (*username*), nama profil (*display name*), nomor ID unik akun, serta tanda *mention* akun (seperti format `@namaakun`) disamarkan secara otomatis menjadi `@user` pada seluruh baris dataset.
2. **Penyaringan Informasi Pribadi:** Kandungan informasi pribadi sensitif (*Personally Identifiable Information / PII*) seperti nomor telepon, alamat email, dan tautan luar (*URL*) disaring dan dihilangkan dari teks komentar.
3. **Kepatuhan terhadap Akses Publik:** Penarikan data YouTube dilakukan melalui API resmi pengembang. Untuk platform X dan TikTok, data yang diambil merupakan percakapan pada linimasa publik terbuka tanpa meretas atau mengakses akun yang bersifat privat (*private account*).
4. **Tujuan Akademik:** Seluruh dataset yang dihimpun semata-mata dimanfaatkan untuk kebutuhan analisis ilmiah tugas besar mata kuliah Big Data dan tidak digunakan untuk tujuan komersial, perundungan siber, maupun *profiling* personal terhadap individu tertentu.

---

## 2.3 Karakteristik dan Struktur Dataset

Data mentah yang diperoleh dari masing-masing platform memiliki perbedaan struktur skema awal. Untuk menyelaraskan data agar siap diproses pada perangkat lunak Orange Data Mining, tim peneliti menetapkan struktur kontrak baku yang terdiri atas **8 atribut kolom seragam**. Rincian struktur kolom dijelaskan pada Tabel 2.2.

### Tabel 2.2 Struktur Kolom Dataset Gabungan

| Nama Kolom | Tipe Data | Format / Aturan | Keterangan dan Contoh |
| :--- | :---: | :--- | :--- |
| `id` | String / Teks | Prefix platform + nomor urut | Identitas unik per record data (Contoh: `X-0001`, `YT-0001`, `TT-0001`) |
| `tanggal` | Date / Tanggal | ISO 8601 (`YYYY-MM-DD`) | Tanggal komentar dipublikasikan (Rentang: `2026-11-12` s.d. `2026-11-24`) |
| `platform` | Kategorikal | Huruf kecil | Nama media sosial sumber data (`x`, `youtube`, `tiktok`) |
| `teks` | String / Teks | Teks bersih tanpa line break | Isi komentar opini publik setelah dibersihkan dari mention dan URL |
| `like` | Numerik (Integer) | Angka bulat non-negatif | Jumlah tanda suka yang diperoleh komentar (indikator persetujuan publik) |
| `balas` | Numerik (Integer) | Angka bulat non-negatif | Jumlah tanggapan balasan pada komentar (indikator interaktivitas) |
| `kata_kunci` | Kategorikal | Topik penelusuran | Frasa query penelusuran data (Default: `pengunduran diri wapres`) |
| `sentimen` | Kategorikal | Target klasifikasi | Label polaritas sentimen (*Positif*, *Netral*, *Negatif*) yang diisi pada tahap pelabelan |

*Sumber: Spesifikasi rancangan data kelompok (2026)*

Selain menyusun dataset gabungan utuh sebanyak 2.759 baris, peneliti juga menyiapkan sampel pelabelan (`sampel_pelabelan.csv`) sebanyak **900 baris data** melalui metode *stratified random sampling*. Sampel ini dibagi secara proporsional dan berimbang, yaitu masing-masing tepat **300 baris dari platform X, 300 baris dari YouTube, dan 300 baris dari TikTok**. Seluruh baris sampel berada dalam rentang tanggal 12 hingga 24 November 2026, yang diperuntukkan bagi proses anotasi sentimen manual (*ground truth*) oleh anggota tim sebelum digunakan untuk melatih algoritma klasifikasi di Orange.

---

## 2.4 Relevansi Dataset terhadap Permasalahan

Penggunaan dataset teks opini publik dari tiga platform media sosial ini dinilai sangat relevan dan tepat guna untuk menjawab rumusan masalah penelitian serta menyediakan wawasan strategis bagi pemangku kepentingan (*stakeholder*) utama, yaitu **Kantor Komunikasi Kepresidenan (Presidential Communication Office / PCO)** dan **Komisi Pemilihan Umum (KPU)**. Kesesuaian ini didasarkan pada empat pertimbangan analitis:

1. **Menjangkau Demografi Opini yang Luas dan Berimbang:**  
   Peristiwa pengunduran diri seorang pimpinan negara menyedot perhatian lintas generasi. Pemanfaatan tiga platform mencegah bias opini: audiens YouTube mewakili masyarakat yang menyimak ulasan berita mendalam berdurasi panjang dengan argumen analitis; pengguna X mewakili kelompok yang aktif dalam diskursus perdebatan politik, konstitusi, dan kritik kebijakan *real-time*; sedangkan pengguna TikTok merepresentasikan suara kelompok pemilih muda (Gen-Z dan milenial) dengan gaya bahasa lugas.

2. **Memotret Sentimen Riil dan Organik:**  
   Komentar warganet di media sosial diunggah secara sukarela dan spontan tanpa rekayasa kuesioner formal. Data ini memberikan potret nyata mengenai penerimaan masyarakat terhadap alasan pengunduran diri, spekulasi figur pengganti, serta isu keabsahan hukum tata negara pasca peristiwa.

3. **Mendukung Keputusan Strategis Komunikasi Publik:**  
   Atribut metrik interaksi (*likes* dan *balas*) memungkinkan pemangku kepentingan memetakan narasi mana yang memperoleh resonansi atau persetujuan tertinggi dari masyarakat. Wawasan ini sangat dibutuhkan oleh Kantor Komunikasi Kepresidenan untuk merumuskan narasi klarifikasi resmi, meredam spekulasi yang memicu kepanikan politik, dan menjaga stabilitas nasional selama masa transisi.

4. **Kesiapan Alur Pengolahan pada Orange Data Mining:**  
   Dataset yang telah dibersihkan dari format *line break* acak, karakter non-standar, dan telah dianonimkan dapat langsung dihubungkan ke *widget* **Corpus**, **Preprocess Text**, **Word Cloud**, hingga algoritma klasifikasi sentimen di Orange Data Mining tanpa menimbulkan kendala *parsing* tipe data.
