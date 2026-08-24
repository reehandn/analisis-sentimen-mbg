# Analisis Sentimen Program Makan Bergizi Gratis (MBG)

Proyek ini bertujuan melakukan analisis sentimen terhadap komentar YouTube yang terkait Program Makan Bergizi Gratis (MBG). 

## Installation

Untuk menjalankan proyek ini, pastikan Anda memiliki Python 3.7+ dan pip terinstall. Install library yang diperlukan dengan perintah berikut:

```
pip install pandas numpy scikit-learn Sastrawi nltk wordcloud matplotlib seaborn evaluate transformers iterative-stratification torch deep_translator langdetect tqdm google-api-python-client
```

Untuk scraping komentar YouTube, Anda perlu mendapatkan API Key dari [Google Cloud Console](https://console.cloud.google.com/) dengan mengaktifkan YouTube Data API v3.

Pipeline utama:

1. Scraping komentar YouTube
2. Preprocessing teks
3. Membagi dataset untuk pelatihan dan inference
4. Pelabelan sentimen, emosi, intensitas, dan aspek
5. Fine-tuning IndoBERT untuk setiap label
6. Prediksi data inference dan visualisasi hasil

## Struktur Proyek

- `DSP_Scraping.ipynb`
  - Scraping komentar YouTube dengan YouTube Data API
  - Menyimpan hasil ke `data_komentar_mbg_scraping.xlsx`

- `DSP_Preprocessing.ipynb`
  - Membersihkan teks komentar
  - Normalisasi slang dan kata tidak baku
  - Menghasilkan `data_komentar_mbg_preprocessing.xlsx`
  - Menyiapkan data untuk pelatihan dan inference

- `DSP_Sentimen_Analisis.ipynb`
  - Memuat dataset preprocessing
  - Membagi data: 2000 baris untuk pelatihan model
  - Menyimpan dataset inference ke `data_komentar_mbg.csv`
  - Melabeli komentar dan menyimpan ke `data_komentar_mbg_labeled.xlsx`
  - Melakukan fine-tuning IndoBERT untuk setiap label
  - Melakukan prediksi pada data inference dan visualisasi hasil

## Definisi Dataset

- `data_komentar_mbg_scraping.xlsx`
  - Dataset hasil scraping komentar YouTube mentah
  - Berisi komentar awal sebelum preprocessing

- `data_komentar_mbg_preprocessing.xlsx`
  - Dataset hasil preprocessing dari `data_komentar_mbg_scraping.xlsx`
  - Komentar sudah dibersihkan dan dinormalisasi

- `data_komentar_mbg_train` (tidak berupa file tunggal di repositori, melainkan subset)
  - 2000 baris diambil dari hasil preprocessing untuk digunakan sebagai data pelatihan model
  - Digunakan untuk fine-tuning IndoBERT pada setiap label

- `data_komentar_mbg.csv`
  - Dataset inference yang dipisahkan dari data training
  - Data ini digunakan untuk prediksi setelah model selesai dilatih

- `data_komentar_mbg_labeled.xlsx`
  - Dataset hasil pelabelan komentar MBG
  - Berisi label:
    - `sentimen_global` (3 kelas: `positif`, `negatif`, `netral`)
    - `emosi` (4 kelas: `Senang`, `Marah`, `Cemas`, `Netral`)
    - `intensitas` (3 kelas: `Tinggi`, `Sedang`, `Rendah`)
    - `aspek` (contoh: `Anggaran`, `Kualitas Makanan`, `Program`, `Distribusi`, `Pemerintahan`)

- Prediksi akhir adalah data komentar MBG yang melewati inference menggunakan model yang sudah dilatih.

## Pipeline Detail

1. Scraping
   - Jalankan `DSP_Scraping.ipynb`
   - Ambil komentar dari video YouTube terkait MBG
   - Simpan keluaran ke `data_komentar_mbg_scraping.xlsx`

2. Preprocessing
   - Jalankan `DSP_Preprocessing.ipynb`
   - Bersihkan teks dan normalisasi slang
   - Hasil preprocessing disimpan ke `data_komentar_mbg_preprocessing.xlsx`
   - Dari dataset preprocessing, pisahkan:
     - 2000 baris untuk pelatihan model (`data_komentar_mbg_train` subset)
     - sisanya untuk inference ke `data_komentar_mbg.csv`

3. Pelabelan
   - Jalankan `DSP_Sentimen_Analisis.ipynb`
   - Lakukan pelabelan 4 label:
     - sentimen global (`positif`, `negatif`, `netral`)
     - emosi (`Senang`, `Marah`, `Cemas`, `Netral`)
     - intensitas (`Tinggi`, `Sedang`, `Rendah`)
     - aspek (topik komentar)
   - Simpan hasil label ke `data_komentar_mbg_labeled.xlsx`

4. Training / Fine-tuning
   - Gunakan 2000 data training untuk fine-tuning IndoBERT pada setiap label
   - Buat model terpisah untuk:
     - sentimen global
     - emosi
     - intensitas
     - aspek (jika ada)
   - Simpan model hasil training untuk digunakan di tahap inference

5. Inference
   - Gunakan model yang sudah dilatih untuk memprediksi data di `data_komentar_mbg.csv`
   - Hasil prediksi adalah dataset komentar MBG prediction

6. Visualisasi
   - Tampilkan evaluasi model dengan classification report dan confusion matrix
   - Visualisasikan distribusi label dan performa prediksi

## Tujuan

- Mengumpulkan komentar MBG dari YouTube
- Membersihkan dan menormalisasi dataset teks Indonesia
- Menyiapkan dataset training dan inference secara terpisah
- Melabeli data dengan keempat label utama
- Melatih model IndoBERT untuk tiap label
- Menghasilkan prediksi data komentar MBG untuk analisis lebih lanjut

## Dependensi

Notebook ini menggunakan paket berikut:

- pandas
- numpy
- scikit-learn
- Sastrawi
- nltk
- wordcloud
- matplotlib
- seaborn
- evaluate
- transformers
- torch
- datasets
- iterative-stratification
- googleapiclient
- deep_translator
- langdetect
- tqdm

## Cara Menjalankan

1. Siapkan environment Python/Colab dengan dependensi di atas.
2. Jalankan `DSP_Scraping.ipynb` untuk scraping komentar dan simpan ke `data_komentar_mbg_scraping.xlsx`.
3. Jalankan `DSP_Preprocessing.ipynb` untuk membuat `data_komentar_mbg_preprocessing.xlsx`.
4. Dari `data_komentar_mbg_preprocessing.xlsx`, ambil 2000 baris untuk training dan simpan data inference terpisah ke `data_komentar_mbg.csv`.
5. Jalankan `DSP_Sentimen_Analisis.ipynb` untuk pelabelan, fine-tuning, dan visualisasi.

> Catatan: `data_komentar_mbg_train` adalah subset 2000 data dari preprocessing dan bukan nama file tunggal dalam repositori. File inference yang digunakan adalah `data_komentar_mbg.csv`.

## Output Hasil
<img width="1661" height="971" alt="image" src="https://github.com/user-attachments/assets/31a57e2d-a53b-4251-95f9-d5b618f5daaf" />

Berdasarkan 10.653 komentar, sentimen negatif menjadi kategori terbanyak dengan 4.797 komentar (45,03%), diikuti positif 4.523 (42,46%) dan netral 1.333 (12,51%). Pada distribusi emosi, Marah mendominasi dengan 3.808 komentar, sedangkan berdasarkan intensitas, kategori Rendah menjadi yang paling banyak dengan 6.085 komentar. Dari sisi aspek, Anggaran paling sering muncul dengan 2.758 komentar, diikuti Program (2.697) dan Program (2.517). Hasil ini menunjukkan bahwa pembahasan MBG didominasi isu terkait anggaran, program, distribusi, dan pemerintahan.
