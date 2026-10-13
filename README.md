# TB1-JST-Kelompok_Diabetes_Clasification





## Identitas Anggota

| No | Nama | NIM |
|---|---|---|
| 1 | Hendrikus Lanang Ona | 71241114 |
| 2 | Harison Abraham Pribadi | 71241151 |
| 3 | Jochelino Felix Kurniawan | 71241156 |

## Struktur Repository

| Berkas | Isi |
|---|---|
| [`TB1_KelompokXX.ipynb`](./TB1_Kelompok_Diabetes_clasification.ipynb) | Notebook lengkap: pemuatan data, implementasi Perceptron & ADALINE dari nol (NumPy), perhitungan manual, delapan konfigurasi eksperimen, evaluasi, dan visualisasi. Dapat dijalankan ulang dari awal hingga akhir. |
| [`TB1_KelompokXX.pdf`](./TB1_Kelompok_Diabetes_clasification.pdf) | Laporan tertulis (struktur: masalah & tujuan → dataset → artikel/kode pendukung → model neuron → penyiapan data & eksperimen → hasil → pembahasan → kesimpulan → daftar pustaka → lampiran). |
| [`dataset.csv`](./dataset.csv) | Data yang benar-benar dipakai pada eksperimen — lihat **Sumber dan Deskripsi Data** di bawah. |
| [`requirements.txt`](./requirements.txt) | Daftar dependensi Python beserta versi yang dipakai untuk menjalankan dan menguji notebook ini. |
| [`Presentasi_TB1.pdf`](./Presentasi_TB1.pdf) | Ringkasan untuk presentasi pertemuan 7: kasus, metode, eksperimen, hasil utama, keterbatasan, kesimpulan. |
| `README.md` | Berkas ini. |

## Sumber dan Deskripsi Data

- **Sumber:** *Pima Indians Diabetes Dataset* (dataset publik), sama dengan dataset yang dipakai dan dianalisis pada Tugas 1.
- **Ukuran:** 768 observasi (baris) × 9 kolom (8 fitur numerik + 1 target `Outcome`).
- **Fitur (x):** `Pregnancies`, `Glucose`, `BloodPressure`, `SkinThickness`, `Insulin`, `BMI`, `DiabetesPedigreeFunction`, `Age` — seluruhnya numerik, tidak diperlukan konversi kategori.
- **Target (y):** `Outcome` — `1` = diabetes (kelas positif), `0` = tidak diabetes. Pada notebook, label dikonversi menjadi `{-1, +1}` sesuai ketentuan tugas (`+1` = diabetes).
- **Distribusi kelas:** 500 (65,1%) tidak diabetes, 268 (34,9%) diabetes — tidak seimbang, sehingga precision/recall/F1 turut dilaporkan.
- **Kualitas data:** tidak ada nilai `NaN`/*missing* maupun baris duplikat pada `dataset.csv`. Pada versi asli dataset ini, nilai `0` di kolom `Glucose`/`BloodPressure`/`SkinThickness`/`Insulin`/`BMI` merepresentasikan nilai hilang; berkas `dataset.csv` di repository ini **tidak** memiliki nilai nol pada kolom-kolom tersebut, sehingga data yang dipakai sudah melalui praproses/imputasi sebelum dibagikan untuk tugas ini.
- **Transformasi yang diterapkan notebook:** pembagian data latih/uji 80:20 (stratifikasi kelas, `random_state=42`), serta standardisasi fitur (mean/std dipelajari hanya dari data latih, lalu diterapkan ke data latih dan data uji).

## Instalasi dan Cara Menjalankan

1. **Clone repository** lalu masuk ke direktori:
   ```bash
   git clone <URL-repository-ini>
   cd TB1-JST-KelompokXX
   ```
2. **Buat virtual environment** (disarankan) dan instal dependensi:
   ```bash
   python3 -m venv venv
   source venv/bin/activate   # Windows: venv\Scripts\activate
   pip install -r requirements.txt
   ```
3. **Jalankan notebook:**
   ```bash
   jupyter lab TB1_KelompokXX.ipynb
   ```
   lalu pilih **Run → Run All Cells**. Notebook membaca `dataset.csv` dari direktori yang sama, sehingga tidak perlu mengubah path apa pun. Seluruh tabel, kurva, dan angka pada laporan akan dihasilkan ulang secara otomatis.

## Versi Python dan Reproduksibilitas

- **Python:** 3.12.3
- **Seed acak:** `42` (dipakai pada `train_test_split` dan `np.random.seed`, dicatat secara eksplisit pada notebook agar pembagian data dan hasil dapat direproduksi persis).
- **Urutan data latih** tidak diacak ulang antar-epoch (tetap), agar efek murni dari learning rate dan standardisasi dapat dibandingkan secara adil antar konfigurasi.
- Versi pustaka yang dipakai untuk menguji notebook ini tercantum lengkap pada [`requirements.txt`](./requirements.txt).

## Ringkasan Hasil

Delapan pelatihan dijalankan (Perceptron & ADALINE, masing-masing η ∈ {0,001; 0,01; 0,1} dengan standardisasi + 1 konfigurasi tanpa standardisasi per model, 50 epoch). Akurasi uji berkisar **0,63–0,75**; detail lengkap (tabel konfigurasi, confusion matrix, kurva pelatihan, bobot/bias akhir) ada pada `TB1_KelompokXX.ipynb` dan `TB1_KelompokXX.pdf` (Bagian 6).

Tiga temuan utama:
1. **Data tidak terindikasi linearly separable** — Perceptron tidak pernah mencapai nol kesalahan klasifikasi pada 50 epoch (konsisten di ketiga nilai η).
2. **Perceptron invarian terhadap skala η** (dengan inisialisasi bobot nol dan urutan data tetap): mengubah η hanya menskalakan besar bobot akhir secara proporsional (persis 10× dan 100×), tanpa mengubah prediksi maupun akurasi. ADALINE tidak menunjukkan sifat ini karena memakai error kontinu (Delta Rule).
3. **Standardisasi krusial untuk ADALINE** — tanpa standardisasi, MSE meledak hingga ~10¹⁴ dalam 3 epoch pada η=0,01, karena skala fitur yang jauh berbeda (mis. `Insulin` vs `DiabetesPedigreeFunction`).

## Referensi

Rosenblatt, F. (1958). The perceptron: A probabilistic model for information storage and organization in the brain. *Psychological Review, 65*(6), 386–408. https://doi.org/10.1037/h0042519

Widrow, B., & Hoff, M. E. (1960). Adaptive switching circuits. *IRE WESCON Convention Record*, Part 4, 96–104. https://isl.stanford.edu/~widrow/papers/c1960adaptiveswitching.pdf

Raschka, S. (t.t.). Adaline: Adaptive Linear Neuron Classifier. mlxtend documentation. https://rasbt.github.io/mlxtend/user_guide/classifier/Adaline/ (diakses 6 Oktober 2026)

Da Silva, I. N., Spatti, D. H., Flauzino, R. A., Liboni, L. H. B., & dos Reis Alves, S. F. (2017). *Artificial Neural Networks: A Practical Course.* Springer.

Haykin, S. (2009). *Neural Networks and Learning Machines* (3rd ed.). Pearson.

Materi kuliah Jaringan Syaraf Tiruan (IT6053) Pertemuan 1–5, Matahari Bhakti Nendya, S.Kom., M.T., FTI UKDW (2026).

Daftar pustaka lengkap dengan konteks pemakaian tiap sumber ada pada `TB1_KelompokXX.pdf`, Bagian 3 dan 9.

## Bantuan AI
data testing, pengelolaan data, pencarian refrensi dan efisiensi kode

