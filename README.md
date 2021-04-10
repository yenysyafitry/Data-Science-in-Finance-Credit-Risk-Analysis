<p align="justify"><b>Credit risk </b>adalah resiko yang harus ditanggung oleh seorang individu atau lembaga ketika memberikan pinjaman - biasanya dalam bentuk uang - ke individu atau pihak lain.
<ol>Resiko ini berupa tidak bisa dibayarkannya pokok dan bunga pinjaman, sehingga mengakibatkan kerugian berikut:
 <li>gangguan aliran kas (cash flow) sehingga modal kerja terganggu.</li>
 <li>meningkatkan biaya operasional untuk mengejar pembayaran tersebut (collection).</li></ol>
Untuk memperkecil resiko kredit ini, biasanya dilakukan proses yang disebut dengan credit scoring dan credit rating terhadap pihak peminjam. Output proses ini akan menjadi basis untuk menentukan apakah aplikasi pengajuan pinjaman baru diterima atau ditolak.</p>
<p align="justify"><b>Credit score</b> adalah nilai resiko yang diberikan kepada seorang individu atau organisasi yang mengajukan pinjaman berdasarkan rekam jejak pinjaman dan pembayaran yang dilakukan. Proses pemberian credit score ini biasanya disebut sebagai credit scoring.</p>
<p align="justify">Perhitungan credit score biasanya dibuat berdasarkan data historis lamanya keterlambatan pembayaran dan yang tidak bayar sama sekali (bad debt). Bad debt biasanya mengakibatkan lembaga pemberian kredit harus menyita aset atau melakukan write off .</p>
<p align="justify">Nilai credit score biasanya bervariasi antar lembaga. Namun banyak yang kemudian mengadopsi model FICO Score yang memiliki rentang nilai 300 - 850. Semakin tinggi nilai yang didapatkan, maka semakin baik tingkat kemampuan seseorang atau sebuah lembaga untuk membayar pinjaman.</p>
<p align="justify"><b>Analisa dan Model Pengambilan Keputusan</b>
<p align="justify">Masih terkait dengan contoh data sebelumnya, namun dengan contoh data utuh - DQLab akan memberikan ilustrasi aktivitas tindak lanjut terhadap data dengan contoh skenario berikut.
<ol>Seorang analis akan melakukan penelusuran terhadap data kita untuk mencari pola. Berikut adalah temuannya: 
<li>Jika jumlah tanggungan berjumlah lebih dari 4, kecenderungan resikonya sangat tinggi (rating 4 dan 5).</li>
<li>Jika durasi pinjaman semakin lama yaitu lebih dari 24 bulan, maka kecenderungan resiko juga meningkat (rating 4 dan 5).</li></ol>
<ol align="justify">Dari kedua temuan ini, analis akan membentuk aturan-aturan untuk menuntun pengambilan keputusan (decision making model) terhadap pengajuan pinjaman baru untuk sebagai berikut:
<li>Jika jumlah tanggungan berjumlah kurang dari 5 orang , dan durasi pinjaman kurang dari 24 bulan maka rating diberikan nilai 2 dan pengajuan pinjaman diterima. 
<li>Jika jumlah tanggungan berjumlah lebih dari 4 orang dan durasi pinjaman lebih dari 24 bulan maka maka rating diberikan nilai 5 dan pengajuan pinjaman ditolak.
<li>Jika jumlah tanggungan berjumlah kurang dari 5, dan durasi pinjaman kurang dari 36 bulan maka maka rating diberikan nilai 3 dan diberikan pinjaman. </li></ol>
Nah, tiga aturan itu kita sebut sebagai model untuk memprediksi nilai risk rating dan menjadi basis pengambilan keputusan terhadap aplikasi pinjaman baru.
Dengan model ini, lembaga pinjaman akan semakin cepat mengambil keputusan dan dengan tingkat kesalahan pengambilan keputusan yang lebih minim.</p>

<p align="justify"><b>Contoh Pemodelan Decision Tree dengan Machine Learning</b></p>
<details>
  <summary>library("openxlsx")</br>
library("C50")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) </br>
#Menggunakan C5.0</br>
drop_columns <- c("kpr_aktif", "pendapatan_setahun_juta", "risk_rating", "rata_rata_overdue")</br>
datafeed <- dataCreditRating[ , !(names(dataCreditRating) %in% drop_columns)]</br>
modelKu <- C5.0(datafeed, as.factor(dataCreditRating$risk_rating))</br>
summary(modelKu)
</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> library("C50")</br>
> #Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
</td></tr></table>
</details>

<p align="justify"><b>Data Preparation untuk Class Variable</b>
 <details>
  <summary>library("openxlsx")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
str(dataCreditRating)</br>
#Melakukan konversi kolom risk_rating menjadi factor</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) </br>
#Melihat struktur setelah konversi</br>
str(dataCreditRating)</summary> 
  <table border="0"><tr><td>> library("openxlsx") </br>
> #Mempersiapkan data </br>> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")
</td></tr></table>
</details>
</p>
<p>Data Preparation untuk Input Variables</p><details>
  <summary>library("openxlsx")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Merubah tipe data class variable sebagai factor </br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) </br>
str(dataCreditRating)</br>
#Menghilangkan beberapa variable input dari dataset </br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")</br>
datafeed <- dataCreditRating[ , input_columns ]</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> #Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
</td></tr></table>
</details>
<p align="justify"><b>Training Set dan Testing Set</b>
<ol>Untuk proses pembentukan model machine learning dan melihat akurasinya, biasanya dataset kita perlu dibagi menjadi dua, yaitu:
<li>Training set: adalah porsi dataset yang digunakan oleh algoritma untuk dianalisa dan menjadi input untuk pembentukan model. </li>
<li>Testing set: adalah porsi dataset yang tidak digunakan untuk membentuk model, tapi untuk menguji model yang telah dibentuk.</li></ol>
Pembentukan ini biasanya menggunakan metode pemilihan acak. Untuk praktek selanjutnya, kita akan membagi dataset kita menjadi 800 baris data untuk training set dan 100 baris data untuk testing set.<p>
 <details>
  <summary><b>Mempersiapkan Training dan Testing Set</b></br>library("openxlsx")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Mempersiapkan class dan input variables</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)</br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")</br>
datafeed <- dataCreditRating[ , input_columns ]</br>
#Mempersiapkan training dan testing set</br>
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer</br>
indeks_training_set <- sample(900, 800)</br>
#Membuat dan menampilkan training set dan testing set</br>
input_training_set <- datafeed[indeks_training_set,]</br>
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating</br>
input_testing_set <- datafeed[-indeks_training_set,]</br>
str(input_training_set)</br>
str(class_training_set)</br>
str(input_testing_set)</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> # Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")
</td></tr></table>
</details>
<details>
 <summary><b>Menghasilkan Model dengan Fungsi C5.0</b></br>library("openxlsx")</br>
library("C50")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Mempersiapkan class dan input variables</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)</br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")</br>
datafeed <- dataCreditRating[ , input_columns ]</br>
#Mempersiapkan training dan testing set</br>
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer</br>
indeks_training_set <- sample(900, 800)</br>
#Membuat dan menampilkan training set dan testing set</br>
input_training_set <- datafeed[indeks_training_set,]</br>
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating</br>
input_testing_set <- datafeed[-indeks_training_set,]</br>
#menghasilkan dan menampilkan summary model</br>
risk_rating_model <- C5.0(input_training_set, class_training_set)</br>
summary(risk_rating_model)</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> library("C50")</br>
> #Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")
</td></tr></table>
</details>
<p align="justify"><b>Algoritma C5.0 </b>adalah algoritma machine learning yang digunakan untuk membentuk model pohon keputusan (decision tree) secara otomatis, dan cocok untuk memodelkan dan sebagi alat prediksi credit rating.
<ol>Karena algoritma ini masuk ke dalam kategori classification, variable yang diperlukan oleh algoritma ini ada dua macam:
<li>Class variable, yaitu variable yang berisi nilai yang ingin kita klasifikasikan. Variable ini harus berisi tipe factor.</li>
<li>Input variables, yaitu variable-variable yang berisi nilai input.</li></ol>
<ol>Dan untuk mengukur akurasi dari model yang kita hasilkan, kita sebaiknya membagi dataset yang ada menjadi dua porsi secara random:
<li>Training set, yang digunakan untuk memberikan input ke algoritma untuk membentuk model.</li>
<li>Testing set, yang akan digunakan untuk data pembanding untuk mengukur akurasi algoritma.</li></ol>
Setelah semua persiapan dataset ini selesai, pada penutup bab dataset ini kita menggunakan fungsi C5.0 untuk membentuk model risk rating. Dan untuk mencetak hasil yang bisa dibaca atas model ini, kita gunakan fungsi  summary. Sedangkan untuk menghasilkan diagram decision tree kita gunakan perintah plot. Semua function ini dari package C50.</p>
<details>
 <summary><b>Label dari Class</b></br>library("openxlsx")
library("C50")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Mempersiapkan class dan input variables </br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) </br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")</br>
datafeed <- dataCreditRating[ , input_columns ]</br>
#Mempersiapkan training dan testing set</br>
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer</br>
indeks_training_set <- sample(900, 800)</br>
#Membuat dan menampilkan training set dan testing set</br>
input_training_set <- datafeed[indeks_training_set,]</br>
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating</br>
input_testing_set <- datafeed[-indeks_training_set,]</br>
#menghasilkan model</br>
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))</br>
summary(risk_rating_model)
</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> library("C50")</br>
> #Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
</td></tr></table>
</details>
<details>
 <summary><b>Jumlah Data dan Variable yang Digunakan</b></br>library("openxlsx")</br>
library("C50")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Mempersiapkan class dan input variables</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)</br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan", "kpr_aktif")</br>
datafeed <- dataCreditRating[ , input_columns ]</br>
#Mempersiapkan training dan testing set</br>
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer</br>
indeks_training_set <- sample(900, 780)</br>
#Membuat dan menampilkan training set dan testing set</br>
input_training_set <- datafeed[indeks_training_set,]</br>
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating</br>
input_testing_set <- datafeed[-indeks_training_set,]</br>
#menghasilkan model</br>
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))</br>
summary(risk_rating_model)</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> library("C50")</br>
> #Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")
</td></tr></table>
</details>
<details>
 <summary><b>Merubah Label Class Variable</b></br>
library("openxlsx")</br>
library("C50")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Mempersiapkan class dan input variables</br>
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "1"] <- "satu"</br>
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "2"] <- "dua"</br>
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "3"] <- "tiga"</br>
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "4"] <- "empat"</br>
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "5"] <- "lima"</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)</br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")</br>
datafeed <- dataCreditRating[ , input_columns ]</br>
#Mempersiapkan training dan testing set</br>
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer</br>
indeks_training_set <- sample(900, 800)</br>
#Membuat dan menampilkan training set dan testing set</br>
input_training_set <- datafeed[indeks_training_set,]</br>
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating</br>
input_testing_set <- datafeed[-indeks_training_set,]</br>
#menghasilkan model</br>
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))</br>
summary(risk_rating_model)
</summary>
  <table border="0"><tr><td>> library("openxlsx")</br>
> library("C50")</br>
> #Mempersiapkan data</br>
> dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")
</td></tr></table>
</details>
<details>
 <summary><b>Menggunakan Fungsi Predict</b></br>library("openxlsx")</br>
library("C50")</br>
#Mempersiapkan data</br>
dataCreditRating <- read.xlsx(xlsxFile = "https://github.com/yenysyafitry/Data-Science-in-Finance-Credit-Risk-Analysis")</br>
#Mempersiapkan class dan input variables</br>
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)</br>
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")</br>
datafeed <- dataCreditRating[ , input_columns ]</br>
#Mempersiapkan training dan testing set</br>
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer</br>
indeks_training_set <- sample(900, 800)</br>
#Membuat dan menampilkan training set dan testing set</br>
input_training_set <- datafeed[indeks_training_set,]</br>
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating</br>
input_testing_set <- datafeed[-indeks_training_set,]</br>
#menghasilkan model</br>
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))</br>
#menggunakan model untuk prediksi testing set</br>
predict(risk_rating_model, input_testing_set)</summary>
  <table border="0"><tr><td>> predict(risk_rating_model, input_testing_set)</br>
  [1] 1 3 4 1 5 4 4 2 1 3 3 1 3 3 3 4 3 1 1 3 5 3 5 1 4 4 1 3 3 1 2 3 3 1 3 5 3</br>
 [38] 3 3 1 3 3 5 1 5 1 5 1 3 3 4 1 3 1 1 3 1 1 1 5 1 1 3 3 3 1 3 4 1 3 3 3 1 3</br>
 [75] 4 3 2 1 3 1 5 5 1 1 4 4 3 2 4 1 1 4 1 3 1 1 1 5 4 3
Levels: 1 2 3 4 5</td></tr></table>
</details>
