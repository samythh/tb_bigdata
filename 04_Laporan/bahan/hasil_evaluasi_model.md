# Hasil Evaluasi Model Sentimen (bahan BAB IV 4.2), data final 25 Nov

Angka di bawah diambil langsung dari widget **Test & Score** di `model_sentimen.ows`. Saat workflow dibuka ulang, hasilnya sama persis.

- **Data latih**: 829 komentar berlabel (positif 305, netral 297, negatif 227).
  - Sebanyak 31 komentar `tidak_relevan` dikeluarkan.
  - Sampelnya berasal dari `sampel_pelabelan.csv` (900 baris). Yang dipakai hanya 860 baris yang lolos preprocessing.
- **Representasi teks**: kolom `teks_bersih` diubah menjadi Bag of Words dengan bobot Count × IDF dan normalisasi L2 (TF-IDF).
- **Evaluasi**: 10-fold stratified cross validation.

## Tabel 4.1 Perbandingan Kinerja Model Klasifikasi Sentimen

| Model | Akurasi (CA) | Precision | Recall | F1 (tertimbang) | F1 makro | AUC | Recall negatif |
|---|---|---|---|---|---|---|---|
| **Logistic Regression** (class weight seimbang) | 0,663 | 0,683 | 0,663 | **0,664** | **0,659** | 0,828 | **58,1%** |
| Naive Bayes | 0,634 | 0,653 | 0,634 | 0,628 | 0,616 | 0,803 | 40,1% |
| SVM (kernel linear) | 0,651 | 0,659 | 0,651 | 0,648 | 0,640 | 0,807 | 49,3% |
| Random Forest (100 pohon) | **0,674** | **0,714** | **0,674** | 0,662 | 0,650 | **0,828** | 38,8% |

Precision, Recall, dan F1 tertimbang diambil dari Test & Score. F1 makro dan recall negatif dihitung dari confusion matrix masing-masing model.

**Model terpilih: Logistic Regression.**
- Akurasi Random Forest memang sedikit lebih tinggi (0,674 vs 0,663). Tapi itu terjadi karena Random Forest terlalu sering menebak "netral": recall netral 90%, sedangkan komentar negatif hanya 38,8% yang dikenali.
- Logistic Regression memberi F1 tertimbang dan F1 makro tertinggi, dan paling seimbang di ketiga kelas. Komentar negatif yang dikenali 58,1%.
- Untuk stakeholder, kemampuan menangkap kritik (kelas negatif) lebih penting daripada selisih akurasi 1%.

## Confusion Matrix Logistic Regression (10-fold CV)

| Aktual \ Prediksi | negatif | netral | positif | Recall per kelas |
|---|---|---|---|---|
| negatif (227) | **132** | 73 | 22 | 58,1% |
| netral (297) | 46 | **229** | 22 | 77,1% |
| positif (305) | 33 | 83 | **189** | 62,0% |

**Cara membaca:**
- Kesalahan terbanyak adalah komentar positif atau negatif yang diprediksi **netral**.
- Komentar sindiran (misalnya "19 juta lapangan kerja", "login kaskus") dan candaan (misalnya "mau main GTA 6") paling sering salah tebak.
- Ini perlu ditulis sebagai keterbatasan di BAB V.

## Hasil prediksi seluruh data bersih (2.660 komentar)
Label manual dipakai jika ada. Selain itu dipakai hasil prediksi model.

| Platform | negatif | netral | positif | tidak_relevan | Total |
|---|---|---|---|---|---|
| YouTube | 182 (18,4%) | 168 (17,0%) | 639 (64,6%) | 1 | 990 |
| TikTok | 177 (18,5%) | 533 (55,6%) | 248 (25,9%) | 27 | 985 |
| X | 307 (45,0%) | 320 (46,9%) | 55 (8,1%) | 3 | 685 |
| **Total** | 666 | 1.021 | 942 | 31 | 2.660 |

Persentase dihitung tanpa baris `tidak_relevan`.

**Catatan untuk peran 7** sebelum menafsirkan tabel ini:
- Perbedaan antar platform besar: YouTube condong positif, X condong negatif. Perlu dicatat bahwa 250 komentar YouTube berasal dari kanal resmi Wakil Presiden.
- Data X kebanyakan bertanggal 17–24 Nov, sedangkan YouTube menumpuk di 12–13 Nov. Jadi perbedaan platform bisa juga dipengaruhi waktu pengambilan data, bukan hanya karakter penggunanya.
- Komentar `tidak_relevan` sebaiknya dikeluarkan dari grafik.
