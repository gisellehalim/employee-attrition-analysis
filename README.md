# Proyek Akhir: Menyelesaikan Permasalahan Perusahaan Edutech

## Business Understanding
Jaya Jaya Maju merupakan perusahaan multinasional yang didirikan pada tahun 2000. Perusahaan ini memiliki lebih dari 1000 karyawan yang tersebar di seluruh penjuru negeri. Perusahaan besar seperti Jaya Jaya Maju membutuhkan data untuk menyelesaikan berbagai permasalahan dalam perusahaan. Data membantu perusahaan untuk mengambil keputusan yang terarah sesuai data.

### Permasalahan Bisnis
Jaya Jaya Maju sedang mengalami kesulitan dalam mengelola karyawan. Hal ini berimbas pada tingginya attrition rate (rasio jumlah karyawan yang keluar dengan total karyawan keseluruhan) hingga lebih dari 10%. Jumlah ini terbilang cukup tinggi dan akan berbahaya jika dibiarkan begitu saja. Hal inilah yang menyebabkan perusahaan Jaya Jaya Maju membutuhkan analisis data untuk melihat pengaruh apa saja yang menyebabkan tingginya attrition rate. 

### Cakupan Proyek
Pada proyek ini akan dilakukan 2 tugas, yaitu:
- Analisis data secara menyeluruh untuk melihat faktor-faktor yang menentukan keluarnya karyawan.
- Pembuatan model machine learning dengan algoritma XGBoost untuk membantu perusahaan memprediksi potensi keluarnya seorang karyawan. Model akan dibuat di file yang sama dengan exploratory data analysis.

### Persiapan

Sumber data: Data berikan dari Dicoding untuk mengerjakan proyek akhir (Dengan nama perusahaan Jaya Jaya Maju). Tetapi, data asli berasal dari IBM HR Analytics.

Setup environment: Tidak dilakukan setup environment karena pengerjaan dilakukan di Google Colab dan dashboard dibuat dengan Microsoft PowerBI.

## Business Dashboard

Business dashboard yang dibuat memperlihatkan faktor-faktor keluarnya karyawan dari perusahaan. Dashboard membahas beberapa faktor, seperti demografis, pengalaman bekerja, pengalaman di kantor, gaji, jarak dari rumah, dan lainnya. Isi dari dashboard ini hanya memperlihatkan perbandingan secara umum dan tidak terlalu mendetil seperti pada kode Python. Dashboard ini dibuat dengan Microsoft PowerBI yang merupakan tools sejenis Tableau dan Looker Studio. Data yang digunakan adalah 'employee_data_clean' (terlampir pada folder data) yang telah dibersihkan datanya sehingga tidak ada null value dan angka rating dibuat agar lebih rapi dilihat.

Link Dashboard:
https://app.powerbi.com/links/jTjsmmyjnf?ctid=0127957a-1dfd-4219-ba67-9031b9d3857e&pbi_source=linkShare (link dashboard ini hanya dapat diakses jika memiliki akun Microsoft PowerBI Premium)

## Modeling
Pada proyek ini dibangun model untuk memprediksi potensi keluarnya karyawan dari perusahaan. Ini dilakukan dengan melatih model menggunakan data karyawan dengan status attrition mereka menjadi variabel target. Model yang digunakan adalah XGBoost tanpa ada pengaturan hyperparameter. Dipilih algoritma XGBoost karena efisien dalam menangani data yang besar, meningkatkan akurasi, dan mengoptimalkan waktu komputasi.

**Tahapan membagi data training dan testing**
Data memiliki 1 kolom identitas (EmployeeID) dan 2 kolom yang tidak memiliki kegunaan (karena isinya sama dan tidak ada perbedaan), sehingga ketiga kolom tersebut dihapus saat memproses data untuk training. Kolom identitas harus dihapus untuk mencegah terjadinya overfitting pada model yang dibuat, karena jika ada identitas pembeda, model akan belajar untuk menghafal identitas tersebut bukan melihat pola. Sedangkan untuk dua kolom lainnya yang dihapus, ini harus dilakukan karena valuenya sama dan tidak akan memberikan kontribusi pada pelatihan model.

Pada dataset, ada data yang kosong pada kolom attrition, sehingga dilakukan penghapusan data null pada awal load dataset.

Beberapa kolom bersifat kategorikal sehingga harus diubah ke numerik agar dapat digunakan model. Dilakukan labelling value kolom-kolom tersebut dengan mapping value menggunakan LabelEncoder.

Setelah labelling data, ditentukan variabel x berupa kolom-kolom data karyawan dan variabel y berupa status attrition. 

Selanjutnya, dilakukan balancing data dengan metode oversampling SMOTE karena jumlah data tidak seimbang. Balancing data dilakukan agar model dapat mencapai akurasi yang lebih tinggi. Pemilihan oversampling adalah karena dengan data yang sedikit, melakukan undersampling akan semakin mengurangi jumlah data dan berpotensi untuk menurunkan akurasi. Dengan metode oversampling SMOTE, kelas minoritas (pada kasus ini adalah kelas "1" atau keluar dari perusahaan) akan ditambahkan jumlah datanya dengan membuat data sintesis.

Dataset lalu dibagi menjadi train dan test dengan rasio 80:20.
Alasan kenapa ditetapkan jumlah tersebut adalah karena pada umumnya 80/20 dianggap cukup baik (kecuali jika data training sangatlah banyak, maka rasio data split bisa berubah). Pada dataset ini, jumlah kolom tidak terlalu banyak sehingga diperlukan data *training* yang cukup untuk memastikan model terlatih dengan baik.

Dengan pengaturan random_state = 42, dataset akan mengeluarkan data acak yang sama untuk data training dan data testing. Melalui konfigurasi pembagian ini, didapat data training sebanyak 80% dari dataset dan data testing sebanyak 20% dari dataset.

**Tahapan membangun model XGBoost**

1. Import algoritma XGBoost.
2. Proses pelatihan/fitting model menggunakan x_train dan y_train yang telah dibagi sebelumnya.
3. Memprediksi data testing dengan model yang telah dilatih.

Setelah model dilatih dan membuat prediksi dari data testing, model dievaluasi dengan beberapa metrik yang akan dibahas lebih lanjut hasilnya dibagian evaluasi. Hasil evaluasi akan menentukan apakah model yang dibuat sudah baik.

## Evaluation
Model yang telah dibuat akan dievaluasi dengan berbagai metrik, yaitu precision, recall, f1-score, accuracy, dan roc-auc score.

Selain itu digunakan juga confusion matrix untuk melihat nilai true positive, true negative, false positive, dan false negative.

**Metrik yang digunakan pada proyek ini:**

Accuracy: Accuracy adalah metrik yang paling umum digunakan untuk mengukur kinerja model machine learning. Accuracy didefinisikan sebagai proporsi data uji yang diklasifikasikan dengan benar oleh model.

$$Accuracy = (True Positives + True Negatives) / (Total Number of Cases)$$

Recall: Recall adalah metrik yang mengukur kemampuan model untuk mendeteksi kasus positif. Recall didefinisikan sebagai proporsi kasus positif (atau pada kasus ini, karyawan yang keluar dari perusahaan) yang diklasifikasikan dengan benar oleh model. 

$$Recall = True Positives / (True Positives + False Negatives)$$

Precision: Precision adalah metrik yang mengukur kemampuan model untuk mendeteksi kasus positif yang benar-benar positif. Precision didefinisikan sebagai proporsi kasus positif yang diklasifikasikan oleh model sebagai kasus positif yang benar-benar positif.

$$Precision = True Positives / (True Positives + False Positives)$$

AUC-ROC: AUC-ROC (Area Under the Receiver Operating Characteristic Curve) adalah metrik yang mengukur kinerja model machine learning pada tugas klasifikasi biner. AUC-ROC dihitung dengan membuat grafik True Positive Rate (TPR) terhadap False Positive Rate (FPR) pada berbagai ambang batas, dan kemudian menghitung luas di bawah kurva yang dihasilkan.

$$AUC-ROC = ∫_0^1 TPR(t) dt$$

F1-score: F1-score adalah kombinasi dari precision dan sensitivity/recall.

$$F1-score = 2 * (Precision * Recall) / (Precision + Recall)$$

**Hasil Pengukuran Metrik**

![XGBoost](https://i.ibb.co/m62MmKK/XGBoost-CM.png)
Gambar 1

Berdasarkan hasil pengukuran metrik pada algoritma XGBoost, algoritma telah menunjukkan performa yang cukup baik dalam mengidentifikasi kedua kelas dengan akurasi 0,93 dan nilai precision, recall, serta f1-score yang juga mencapai 0,93. Skor AUC juga menunjukkan kinerja model yang baik dengan skor 0,932. Ini menunjukkan bahwa model mampu memprediksi potensi attrition seorang karyawan dengan cukup akurat. Selain itu dapat dilihat pada Gambar 1 diatas, hasil confusion matrix menunjukkan model mampu mencari pola yang mempengaruhi keluarnya karyawan dari perusahaan.

## Prediction
Model dapat diuji dengan file 'Prediction' yang akan memprediksi status attrtition karyawan dengan model XGBoost yang telah disimpan dalam file .pkl. Untuk memprediksi, cukup menjalankan kode dan mengubah baris data yang ingin diprediksi dengan mengganti nilai variabel 'row_index'. File ini akan memperlihatkan hasil prediksi dengan label sebenarnya.

## Conclusion
Dari exploration data analysis yang dilakukan (Baik dari kode Python dan Dashboard PowerBI), terdapat insight sebagai berikut:

- Faktor yang paling berpengaruh terhadap attrition adalah usia muda, berada pada departmen atau posisi tertentu, jarak dari rumah, banyaknya perjalanan bisnis, status pernikahan, kepuasan karyawan dengan pekerjaan (hubungan dengan rekan kerja, keterlibatan, lingkungan, dll), work-life balance, adanya lembur, lama bersama manajer, dan jumlah training. Penjelasan akan dijabarkan pada poin-poin selanjutnya.

- Jumlah attrition (keluarnya karyawan dari perusahaan) yang dialami perusahaan mencapai 179 karyawan atau 16.9% dari jumlah karyawan. Jumlah ini cukup tinggi karena dapat mempengaruhi kinerja perusahaan jika tidak diatasi (20% merupakan batas tinggi).

- Jumlah karyawan laki-laki dan perempuan cukup seimbang dengan persentase 59% dan 41% untuk masing-masingnya. Mengenai jumlah attrition, karyawan laki-laki cenderung lebih banyak keluar dari perusahaan. Tetapi jumlah ini tidak berbeda jauh dengan karyawan perempuan sehingga gender tidak mempengaruhi keluarnya karyawan secara keseluruhan.

- Jumlah attrition paling banyak berasal dari karyawan dengan range usia 20 - 30 tahun. Hal ini dapat disebabkan karena pada usia tersebut, karyawan cenderung fokus untuk mengembangkan karir daripada stabilitas. Ini dapat terlihat dari tren attrition yang menurun semakin tua usianya.

- Attrition paling banyak terjadi pada department sales mencapai 20.7% dengan perbedaan sekitar 5% jika dibandingkan dengan department lainnya. Ini dapat disebabkan karena beratnya beban kerja sales.

- Jika dibandingkan per gender, karyawan perempuan di department human resources dan sales lebih banyak keluar dari perusahaan. Sedangkan karyawan laki-laki di department RnD paling banyak keluar dari perusahaan.

- Semakin jauh jarak kantor dari rumah, semakin besar kemungkinan karyawan untuk keluar dari perusahaan. Hal ini dapat disebabkan oleh besarnya biaya transportasi dan faktor lainnya.

- Jika dilihat berdasarkan gender, baik laik-laki atau perempuan akan lebih berpotensi keluar jika jarak kantor jauh dari tempat tinggal, sehingga tidak ada hubungan dengan gender.

- Semakin banyak perjalanan bisnis yang dilakukan, karyawan berpotensi lebih besar untuk keluar dari perusahaan. Ini dapat disebabkan oleh kelelahan perjalanan.

- Status pernikahan menjadi salah satu faktor yang mempengaruhi keluarnya karyawan dari perusahaan, karyawan lajang paling banyak keluar dari perusahaan sedangkan karyawan yang telah bercerai lebih jarang keluar. Ini dapat disebabkan oleh kecenderungan tidak adanya tanggungan pada karyawan yang lajang sehingga mereka daapt lebih bebas mencari kesempatan lain sedangkan untuk karyawan yang telah menikah atau bercerai, mereka telah biasanya telah memiliki tanggungan (misal, pasangan atau anak). Hal ini tidak dipengaruhi oleh gender karena pada laki-laki dan perempuan, tren yang sama dapat terlihat.

- Karyawan yang telah memiliki lebih banyak pengalaman bekerja lebih cenderung untuk meninggalkan perusahaan, tetapi perbedaannya tidak jauh dengan karyawan dengan pengalaman lebih sedikit.

- Level pendidikan karyawan tidak terlalu mempengaruhi keluarnya mereka dari perusahaan. Karyawan dengan level edukasi kategori 5 memang memiliki tingkat attrition yang lebih rendah, tetapi ini bukanlah sebuah tren jika dilihat dari kategori lainnya.

- Kepuasan karyawan dengan lingkungan kerja menjadi salah satu faktor mereka keluar dari perusahaan. Semakin kecil kepuasan mereka terhadap lingkungan kerja, semakin besar potensi mereka keluar dari perusahaan.

- Perasaan terlibat dengan pekerjaan menjadi salah satu faktor mereka bertahan dalam perusahaan. Semakin banyak kontribusi mereka, semakin kecil peluang mereka keluar dari perusahaan. Ini dapat disebabkan oleh keinginan karyawan untuk berkembang dan berkontribusi pada job desk mereka masing-masing.

- Walau tren naik turun, ada kecenderungan dimana karyawan dengan jabatan lebih tinggi akan bertahan di perusahaan.

- Posisi sales representative, sales executive, human resources, laboratory technician, dan research scientist mengalami attrition yang cukup besar jika dibandingkan posisi lainnya. Attrition paling tinggi terjadi pada karyawan di posisi sales representative yang kemungkinan disebabkan oleh tingginya tekanan.

- Kepuasan karyawan terhadap pekerjaan mereka memiliki pengaruh dalam peluang mereka keluar dari perusahaan. Semakin puas mereka terhadap pekerjaan mereka, karyawan akan cenderung bertahan.

- Work-life balance cukup mempengaruhi keputusan karyawan untuk mengundurkan diri. Semakin baik work-life balance, semakin kecil kesempatan karyawan untuk keluar dari perusahaan walaupun attrition meningkat pada karyawan yang memberi nilai 4.

- Gaji karyawan (per jam atau bulan) tidak terlalu mempengaruhi keputusan mereka untuk keluar dari perusahaan.

- Kenaikan gaji karyawan tidak terlalu mempengaruhi keputusan mereka untuk keluar dari perusahaan.

- Rating kinerja karyawan tidak terlalu mempengaruhi keputusan mereka untuk keluar dari perusahaan.

- Karyawan yang menjalani lembur jauh lebih berpotensi untuk keluar dari perusahaan. Hal ini dapat disebabkan oleh kelelahan bekerja.

- Karyawan yang memiliki waktu kerja lebih sedikit bersama manager mereka lebih berpotensi untuk meninggalkan perusahaan. Ini dapat disebabkan oleh ketidakcocokan atau tidak terbentuknya chemistry yang tepat diantara karyawan dan manager.

- Lama karyawan di perusahaan tidak memiliki banyak pengaruh terhadap peluang mereka keluar dari perusahaan.

- Lama promosi tidak memiliki banyak pengaruh terhadap peluang karyawan keluar dari perusahaan.

- Banyaknya training karyawan dalam setahun tidak terlalu mempengaruhi peluang keluarnya karyawan. Perbedaan hanya terlihat pada karyawan yang tidak diberikan training sama sekali. Jika karyawan diberikan training, peluang mereka pergi akan lebih kecil.

- Hubungan baik dengan rekan kerja memiliki pengaruh yang tidak terlalu besar terhadap peluang karyawan keluar dari perusahaan. Tetapi, semakin baik hubungan karyawan dengan rekan kerjanya, akan semakin kecil peluang mereka untuk keluar (walaupun tren meningkat sedikit pada karyawan yang memberi nilai 4).

### Rekomendasi Action Items 
Dari insight yang telah didapatkan dari data, terlihat bahwa tingkat attrition pada perusahaan cukup tinggi sehingga diperlukan tindakan untuk mengatasi masalah tersebut. Beberapa saran dibawah dapat dipertimbangkan untuk meningkatkan performa perusahaan:

**Menangani Tingkat Attrition Secara Keseluruhan:**
- Lakukan survei berkala untuk mengidentifikasi aspek-aspek yang perlu diperbaiki dalam lingkungan kerja. Ciptakan budaya kerja yang positif dan suportif.

- Meningkatkan program pengembangan dan pelatihan karyawan untuk membantu mereka berkembang dan merasa tertantang dalam pekerjaan mereka. Sediakan program training dan pengembangan yang berkualitas dan relevan.
- Memperjelas visi, misi, dan nilai-nilai perusahaan untuk meningkatkan rasa keterlibatan karyawan. Buat karyawan merasa bahwa kontribusi mereka menghasilkan sesuatu yang baik bagi perusahaan dan diri mereka. Manfaatkan program penghargaan dan pengakuan untuk menghargai kontribusi karyawan. 
- Menerapkan kebijakan jam kerja yang fleksibel.
- Mendukung cuti dan program keseimbangan kehidupan kerja lainnya.
- Membatasi lembur sebisa mungkin. Salah satu caranya adalah menerapkan alur kerja yang lebih efektif dan efisien sehingga memperkecil kebutuhan lembur.
- Melakukan riset pasar untuk memastikan gaji dan benefit yang ditawarkan kompetitif.
- Meningkatkan komunikasi antara manajer dan karyawan, serta mendorong kerjasama dan kolaborasi antar karyawan. Salah satu caranya adalah dengan melakukan kegiatan team building bagi karyawan atau memberikan pelatihan kepemimpinan dan pengembangan tim bagi pimpinan.
- Melakukan proses promosi yang transparan dan adil. Pertimbangkan kinerja, pengalaman, dan skill karyawan dalam proses promosi. Berikan kesempatan bagi karyawan untuk berkembang dan mengambil peran yang lebih besar dalam perusahaan.

**Menangani Masalah Karyawan di Department Sales:**
- Mengurangi beban kerja sales dengan meningkatkan efisiensi proses penjualan.

- Meningkatkan komisi dan insentif untuk sales yang berprestasi.

**Membantu Karyawan yang Bekerja Jauh:**
- Memberikan tunjangan transportasi atau kompensasi lain untuk membantu karyawan yang bekerja jauh.

- Meningkatkan teknologi komunikasi dan kolaborasi untuk membantu karyawan yang bekerja jauh tetap terhubung dengan tim mereka.
- Membatasi perjalanan bisnis untuk hal yang penting untuk meminimalisir kelelahan karyawan serta penggunaan resources perusahaan.

**Mempertahankan Karyawan yang Memiliki Banyak Pengalaman Kerja:**
- Menawarkan program pengembangan karir yang menantang dan berdampak bagi karyawan dengan pengalaman kerja yang banyak.
- Memberikan kesempatan bagi karyawan dengan pengalaman kerja yang tinggi untuk mengambil peran kepemimpinan dalam perusahaan.

**Memperbaiki Tingkat Attrition Karyawan di Posisi Tertentu:**
- Melakukan analisis lebih lanjut untuk mengidentifikasi alasan spesifik attrition pada posisi tertentu, seperti sales representative (yang paling urgent), sales executive, human resources, laboratory technician, dan research scientist.
- Mengembangkan program intervensi yang ditargetkan untuk mengatasi alasan-alasan spesifik tersebut.

