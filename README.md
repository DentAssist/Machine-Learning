# DentAssist

## Detecting your mouth and teeth diseases

Kriteria tambahan yang kami kerjakan :

1. Mengimplementasikan Callback
2. Gambar-gambar pada dataset memiliki resolusi yang tidak seragam.
3. Dataset yang digunakan berisi lebih dari 10000 gambar.
4. Akurasi pada training set dan validation set minimal 94%.
5. Memiliki 5 buah kelas atau lebih.
6. Melakukan inference menggunakan salah satu model (TF-Lite, TFJS atau savedmodel dengan tf serving).

# Penjelasan Proyek

Proyek ini merupakan proyek untuk membuat sebuah model yang dapat melakukan klasifikasi gambar. Diberikan kebebasan untuk memilih dataset yang ingin digunakan.

## Dataset

Dataset merupakan data yang diambil dari **Kaggle** [Link1](https://www.kaggle.com/datasets/salmansajid05/oral-diseases/data) [Link2](https://www.kaggle.com/datasets/alielhenidy/tooth-dataset). Dataset memiliki total 5 penyakit gigi yang terbagi menjadi 5 kelas berbeda. Secara default resolusi dari gambar adalah 256x256, namun untuk memenuhi kriteria maka dataset Gambar-gambar yang valid akan diubah ukurannya secara acak dengan lebar dan tinggi antara 400 hingga 512 piksel

## Distribusi Gambar

Dari 5 kelas penyakit gigi dengan distribusi masing-masing kelas sebagai berikut:

| Condition     | Number of Images |
| ------------- | ---------------- |
| Calculus      | 2497             |
| Caries        | 2454             |
| Healthy_Tooth | 2500             |
| Hypodontia    | 2349             |
| Mouth_Ulcer   | 2500             |
| **Total**     | **12300**        |

# Model Evaluasi

## Arsitektur Model

1. **MobileNetV2 Pre-trained**:

   - Menggunakan MobileNetV2 yang telah dilatih pada ImageNet dengan menghapus top layer (`include_top=False`).
   - Ukuran input model adalah `(150, 150, 3)`.

2. **Layer Beku**:

   - Semua layer MobileNetV2 dibekukan (`pre_trained_model.trainable = False`) untuk mempertahankan bobot dan fitur yang telah dilatih.

3. **Layer Tambahan**:

   - Layer `Conv2D` dengan 32 filter, kernel size 3x3, dan aktivasi ReLU, diikuti oleh `MaxPooling2D` dengan pool size 2x2.
   - Layer `Conv2D` dengan 64 filter, kernel size 3x3, dan aktivasi ReLU, diikuti oleh `MaxPooling2D` dengan pool size 2x2.

4. **Layer Flatten dan Fully Connected**:
   - Fitur yang didapatkan dari layer sebelumnya di-flatten menggunakan `Flatten`.
   - Layer `Dropout` dengan rate 0.3 untuk mencegah overfitting.
   - Layer `Dense` dengan 128 unit dan aktivasi ReLU.
   - Layer output `Dense` dengan 5 unit dan aktivasi softmax untuk klasifikasi multi-kelas.

## Grafik Akurasi dan Loss

<img src="https://github.com/DentAssist/Machine-Learning/blob/main/images/akurasi.png" width="700">
<img src="https://github.com/DentAssist/Machine-Learning/blob/main/images/grafik.png" width="700">

## Predict

| No  | True          | Predicted     |
| --- | ------------- | ------------- |
| 1   | Calculus      | Calculus      |
| 2   | Caries        | Caries        |
| 3   | Healthy_Tooth | Healthy_Tooth |
| 4   | Hypodontia    | Hypodontia    |
| 5   | Mouth_Ulcer   | Mouth_Ulcer   |
