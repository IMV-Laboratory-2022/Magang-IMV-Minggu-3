# Object Detection
---

## A. Pengantar Object Detection

<p align="center">
    <img src="object detection.gif" width="640" style="vertical-align:middle">
</p>

Secara umum, tujuan dari object detection adalah menentukan `lokasi` dan `kategori objek` di dalam sebuah citra, melabeli objek dengan kotak yang disebut `bounding box` dan menampilkan nilai confidence dalam bentuk persentase.

<p align="center">
    <img src="contents/diff od.png" width="640" style="vertical-align:middle">
</p>

Perbandingan image classification, object localization, dan object detection. `Image classification` hanya berfokus `menentukan kategori objek` dengan menganalisis keseluruhan citra. `Object localization` kemudian `menambahkan penentuan lokasi` dari objek yang ada pada citra. Kedua jenis tugas ini diimplementasikan pada kasus `single object`. Penggabungan classification dan localization untuk multiple objects kemudian disebut sebagai `object detection`. Kemampuan inilah yang membuat object detection sangat cocok digunakan pada pemahaman citra dan analisis video.

## B. Metode Object Detection

<p align="center">
    <img src="https://i.stack.imgur.com/WYQp3.png" width="640" style="vertical-align:middle">
</p>

Pada proses pendeteksian suatu objek, terdapat dua metode dalam menyelesaikan tugas tersebut diantaranya `one-stage` dan `two-stage` detectors.

`One-stage-detector` menggunakan jaringan tunggal `feed-forward fully convolutional` yang secara langsung menyediakan `bounding box` dan `klasifikasi objek`. Algoritma Object Detection yang menggunakan `one-stage-detector` adalah `YOLO` dan `SSD`

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/218655283-fd500572-2156-4493-a953-923c1ea4d174.png" width="640" style="vertical-align:middle">
</p>

`Two-stage-detector` terdiri dari proses `region proposal` dan `tahap klasifikasi`. Algoritma Object Detection yang menggunakan `two-stage-detector` adalah RCNN, Fast RCNN, dan Faster RCNN.

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/218655156-5b9bcac3-841f-4f5e-8a5e-78016e25fb09.png" width="640" style="vertical-align:middle">
</p>

Dibawah ini merupakan perbandingan keakuratan (mAP) dan kecepatan.

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/218655653-c5d21b45-99be-417d-90dd-928458163b9d.png" width="640" style="vertical-align:middle">
</p>

## C. Arsitektur Object Detection (semisal YOLOv8 hanya menunjukkan file .yaml)

Yolov8 (You Only Look Once version 8) adalah salah satu model deteksi objek real-time yang terkenal. Model ini memiliki arsitektur yang sangat efisien dan dapat mendeteksi berbagai jenis objek dengan akurasi yang tinggi.
Arsitektur Yolov8 terdiri dari beberapa komponen utama:

1.	Backbone: Yolov8 menggunakan ResNet-152 sebagai backbone-nya, yaitu suatu jaringan konvolusi dalam yang telah terlatih untuk mengenali fitur-fitur umum pada gambar.
2.	Neck: Yolov8 menggunakan FPN (Feature Pyramid Network) sebagai neck-nya, yaitu suatu jaringan yang menghubungkan output dari berbagai tingkat fitur pada backbone dan menghasilkan fitur yang lebih berkualitas tinggi.
3.	Head: Yolov8 menggunakan Head yang terdiri dari beberapa layer konvolusi dan layer terkait lainnya untuk memproses fitur dari Neck dan menghasilkan bounding boxes serta kelas objek.
4.	Output: Output dari Yolov8 adalah koordinat bounding boxes, skor kepercayaan, dan kelas objek yang terdeteksi.
Dalam Yolov8, input gambar akan dibagi menjadi beberapa bagian dengan ukuran yang sama. Setiap bagian akan dijadikan input untuk model dan model akan mengeluarkan hasil deteksi untuk setiap bagian tersebut. Setelah itu, hasil deteksi akan digabungkan untuk menghasilkan hasil akhir.
Dalam proses pelatihan, Yolov8 menggunakan objektif loss yang disebut "YOLOv5 loss". Objektif loss ini terdiri dari beberapa komponen, termasuk komponen untuk mengukur keakuratan bounding box, keakuratan kelas objek, dan pengurangan false positives.
Dengan menggunakan arsitektur yang efisien dan objektif loss yang sesuai, Yolov8 dapat menghasilkan deteksi objek yang akurat dalam waktu nyata. Model ini sangat cocok untuk aplikasi seperti deteksi kendaraan di jalan raya atau deteksi orang dalam video.


## D. Evaluasi Model Object Detection

#### Mean Average Precision

Penggunaan parameter kinerja mAP sangat populer pada sistem object detection. mAP merupakan rata-rata dari semua nilai Average Precision (AP) setiap kelas objek. Sedangkan AP adalah nilai integral atau area di bawah kurva Precision (P) × Recall (R) dari suatu kelas objek seperti pada gambar dibawah ini.

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/194284895-a1e6518c-ab49-4cd6-8924-db88a5c5fdb9.png" width="480" style="vertical-align:middle">
</p>

Secara matematis, mAP dapat dihitung menggunakan persamaan

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/194285198-08f5e17a-1775-4ae4-a1db-a47e186c4dcd.png" width="360" style="vertical-align:middle">
</p>

dengan,

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/194285258-14e49f8c-7c8f-4d0b-bec2-aa6132b6ebd5.png" width="360" style="vertical-align:middle">
</p>

C merupakan jumlah kelas objek dan N merupakan jumlah semua titik interpolasi nilai P terhadap nilai R.

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/194285730-5641dbdb-d410-444a-95bc-9f6d16d4c4ec.png" width="540" style="vertical-align:middle">
</p>

Berdasarkan gambar diatas, hasil interpolasi ini akan dijadikan estimasi luas area di bawah kurva Precision × Recall. Proses interpolasi ini dilakukan dengan mencari nilai P maksimum pada setiap titik Re yang memiliki nilai lebih besar atau sama dengan nilai R pada titik berikutnya (Rn+1).

Sementara itu, dalam menghitung P dan R perlu melihat nilai-nilai dari confusion matrix seperti pada gambar dibawah ini. Setelah mendapatkan confusion matrix, maka P dan R bisa dihitung dengan persamaan

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/194286013-6e63c4c7-50ac-40ea-beec-b165b7c57d9d.png" width="540" style="vertical-align:middle">
</p>

Standar perhitungan mAP@0.5 PASCAL VOC menggunakan Intersection Over Union (IoU) seperti pada gambar dibawah ini yang digunakan sebagai ambang batas penentuan TP dan FP dari confusion matrix. Adapun definisi dari setiap komponen confusion matrix tersebut adalah sebagai berikut.

<p align="center">
    <img src="https://user-images.githubusercontent.com/72246401/194283614-0d13403a-28a3-416e-9deb-e25f772dee92.png" width="480" style="vertical-align:middle">
</p>

1. TP mendefinisikan bounding-box hasil deteksi yang memiliki IoU ≥ 0,5.
1. FP mendefinisikan bounding-box hasil deteksi yang memiliki IoU < 0,5.
1. FN mendefinisikan tidak ada bounding-box hasil deteksi pada bounding-box ground-truth.
1. TN mendefinisikan semua area pada citra yang tidak memiliki bounding-box ground-truth dan hasil deteksi sehingga nilainya tak terhingga dan tidak digunakan dalam perhitungan.

Selain PASCAL VOC terdapat juga COCO. COCO menggunakan IoU 0.5 sampai dengan 0.95 dengan step 0.05.

---

- Code:
- [Pengumpulan Magang 3](https://www.deeplearningbook.org/)

Source
1.	Paper akademik Yolov8: https://arxiv.org/abs/2104.14754
2.	Repository Github Yolov8: https://github.com/WongKinYiu/yolov8
3.	Artikel blog tentang Yolov8: https://blog.roboflow.com/yolov8/
4.	Presentasi Youtube tentang Yolov8: https://www.youtube.com/watch?v=m8-MvGcvkZI
