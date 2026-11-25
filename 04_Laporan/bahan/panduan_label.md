# Panduan Pelabelan Sentimen
TB Big Data – Respons Publik terhadap Pengunduran Diri Wapres (Peran 6)

## 1. Sentimen diukur terhadap apa?
Sentimen yang dilabeli adalah **sikap penulis komentar terhadap Gibran dan keputusan pengunduran dirinya**.
Bukan suasana hati penulis secara umum, dan bukan sikap terhadap tokoh lain.

Alasannya, satu komentar bisa terdengar "senang" tapi isinya menyerang Gibran. Contoh: "Alhamdulillah akhirnya mundur juga". Kalau yang diukur suasana hati, komentar itu jadi positif. Padahal maksudnya justru negatif terhadap Gibran. Dengan satu acuan yang tetap, label dari semua pelabel bisa konsisten.

## 2. Definisi label

| Label | Definisi | Contoh dari data |
|---|---|---|
| **positif** | Dukungan, simpati, doa, apresiasi, atau pembelaan terhadap Gibran, termasuk menghargai keputusannya mundur dan menyalahkan pihak yang menjatuhkannya | "Langkah yg sangat bijak dari Gibran...Gibran tetap dihati", "Sehat selalu Mas Gibran", "ya allah dzalim banget orang orang" |
| **negatif** | Kritik, kecaman, ejekan, atau sindiran terhadap Gibran (ijazah, nepotisme, fufufafa, tunjangan), desakan pemakzulan, atau rasa senang karena ia jatuh | "Dari awal maju di pilpres sudah cacat hukum", "makzulkan jangan mundur", "Alhamdulillah..sudah mundur wapres yg plonga plongo" |
| **netral** | Berita/informasi, pertanyaan, spekulasi soal pengganti, candaan tanpa sikap jelas, atau keluhan umum soal politik yang tidak mengarah ke Gibran | "siapa yg gantiin?", "materi pkn gen alpha nambah", "ngeri sekali politik di indo" |
| **tidak_relevan** | Tidak membahas peristiwa, Gibran, atau politik terkait sama sekali. **Dikeluarkan dari pelatihan model** | "editingnyaa gileee", "harga sawit di PKS anclok", "udah pada nongki" |

## 3. Aturan untuk kasus sulit
1. **Sindiran.** Label menurut maksud sebenarnya, bukan kata-katanya. "Terima kasih juga atas 19 juta lapangan kerja" adalah sindiran, jadi negatif. Kalau ragu apakah itu sindiran, pilih netral.
2. **Menyerang tokoh lain.**
   - Kalau serangan itu membela Gibran (misalnya "Prabowo habis manis sepah dibuang"), labelnya **positif**.
   - Kalau ikut menyerang Gibran atau keluarganya (misalnya "turun juga lah sekalian, kan 1 paket"), labelnya **negatif**.
   - Kalau tidak ada kaitannya dengan Gibran, labelnya **netral**.
3. **Ucapan pendek yang ambigu** ("Alhamdulillah", "Merdeka!!!", "Mantap") tanpa konteks yang jelas diberi label **netral**.
4. **Campuran pujian dan kritik.** Ambil sikap yang paling dominan. Kalau seimbang, pilih netral.
5. **Berita atau judul artikel** dari X diberi label netral, kecuali judulnya jelas memihak (misalnya "Kabar Baik! ... Pemakzulan Gibran", yang dilabeli negatif).
6. **Emoji** ikut dipertimbangkan dari teks asli (😭 bisa berarti sedih atau simpati). Lihat selalu kolom `teks` (teks asli), bukan `teks_bersih`.

## 4. Status pelabelan saat ini (data final 25 Nov)
- Dari 900 baris di `sampel_pelabelan.csv`, **860 lolos preprocessing** (ada di `data_bersih.csv`) dan semuanya sudah dilabeli.
  - Per platform: YouTube 298, TikTok 294, X 268.
  - 40 baris sisanya dibuang di preprocessing (duplikat, judul berita/link X, spam), jadi tidak dilabeli.
- Hasil: positif 305, netral 297, negatif 227, tidak_relevan 31.
- Label disimpan di kolom `sentimen`, mengikuti format data Lutfi.
- 354 komentar (semua sampel YouTube dan 56 TikTok) teksnya sama persis dengan sampel lama, jadi labelnya dipakai ulang. Sisanya, 506 komentar, dilabeli baru dengan panduan yang sama.

## 5. Uji konsistensi (untuk BAB IV 4.2)
1. Ambil 50–100 baris secara acak dari `data_berlabel.csv`.
2. Minta 1–2 anggota lain melabeli ulang baris tersebut tanpa melihat label yang sudah ada.
3. Hitung persentase kesepakatannya (atau Cohen's kappa), lalu cantumkan di laporan.
4. Kalau ada label yang direvisi, cukup buka ulang `model_sentimen.ows`. Test & Score dan Predictions akan dihitung ulang otomatis.
