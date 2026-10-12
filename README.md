# TB1-JSTKelompokXX: Perceptron vs ADALINE pada Klasifikasi Diabetes (Pima)

Tugas Besar 1, Jaringan Syaraf Tiruan (IT6053), FTI UKDW.
Klasifikasi dua kelas (diabetes = +1, tidak = −1) dengan Perceptron dan ADALINE (Delta Rule) yang diimplementasikan sendiri dengan NumPy.

## Identitas
71241114 - Hendrikus Lanang Ona
71241151 - Harrison Abraham Pribadi
71241156 - Jochelino Felix Kurniawan



## Data
- **Sumber:** Pima Indians Diabetes Database (NIDDK), via UCI/Kaggle. Smith dkk. (1988). *(Isi URL dan tanggal akses sumber yang Anda unduh, dan verifikasi lisensinya.)*
- **Kolom:** Pregnancies, Glucose, BloodPressure, SkinThickness, Insulin, BMI, DiabetesPedigreeFunction, Age (semua numerik) dan `Outcome` (1 = diabetes, 0 = tidak).
- **Label:** `Outcome` 1 → **+1 (kelas positif)**, 0 → −1. Distribusi: 500 kelas 0, 268 kelas 1 (34,9% positif).
- **Transformasi:** tidak ada perubahan pada CSV. Standardisasi (z-score) dipelajari hanya dari data latih di dalam notebook.
- **Catatan penting:** berkas ini sudah diimputasi sebelum kami terima. Insulin 102,5 (kelas 0) dan 169,5 (kelas 1) serta SkinThickness 27 dan 32 muncul sangat sering. Pola ini menunjukkan imputasi median per kelas (±49% Insulin, ±33% SkinThickness), sehingga ada potensi kebocoran label yang tidak dapat dibatalkan. Lihat laporan Bagian 2, 6.4, dan 7.

## Cara menjalankan
```bash
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter nbconvert --to notebook --execute --inplace TB1_KelompokXX.ipynb
```
Atau buka notebook di Jupyter/VS Code lalu *Restart & Run All*. Notebook membaca `dataset.csv` dari direktori yang sama.

## Lingkungan dan seed
Python 3.12.3, NumPy 2.4.4, pandas 3.0.2, matplotlib 3.10.8, scikit-learn 1.8.0 (hanya untuk `train_test_split` dan pembanding `Perceptron`). **Seed = 42**, split 80:20 berstrata (614 latih / 154 uji). Inisialisasi bobot dan bias = 0; urutan sampel tetap.

## Ringkasan hasil (data uji, 50 epoch)
| Model | η | Std | Acc latih | Acc uji | Recall (+) | F1 (+) |
|---|---|---|---|---|---|---|
| Perceptron | 0,001 / 0,01 / 0,1 | Ya | 0,689 | 0,701 | 0,556 | 0,566 |
| Perceptron | 0,01 | Tidak | 0,661 | 0,630 | 0,074 | 0,123 |
| ADALINE | 0,001 | Ya | 0,792 | 0,734 | 0,704 | 0,650 |
| ADALINE | 0,01 | Ya | 0,793 | 0,753 | 0,704 | 0,667 |
| ADALINE | 0,1 | Ya | 0,800 | 0,727 | 0,537 | 0,580 |
| ADALINE | 0,01 | Tidak | 0,349 | 0,351 | 1,000 | 0,519 (diverge) |

Temuan utama: data tidak menunjukkan keterpisahan linear (Perceptron 1000 epoch tidak pernah mencapai 0 pembaruan); η tidak memengaruhi Perceptron berinisialisasi nol; ADALINE tanpa standardisasi divergen; keunggulan ADALINE pada data uji bersifat indikatif karena 154 sampel uji (±7 poin).

## Referensi
1. Rosenblatt, F. (1958). The perceptron. *Psychological Review, 65*(6), 386–408. https://doi.org/10.1037/h0042519
2. Widrow, B., & Hoff, M. E. (1960). Adaptive switching circuits. *IRE WESCON Convention Record, Part 4*, 96–104. https://isl.stanford.edu/~widrow/papers/c1960adaptiveswitching.pdf
3. Raschka, S. Adaline: Adaptive Linear Neuron Classifier. mlxtend. https://rasbt.github.io/mlxtend/user_guide/classifier/Adaline/
4. Smith, J. W. dkk. (1988). Using the ADAP learning algorithm to forecast the onset of diabetes mellitus. *Proc. Annu. Symp. Comput. Appl. Med. Care*, 261–265.

## Bantuan AI
Pencarian data dan testing 
