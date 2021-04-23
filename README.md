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
  <summary>library("openxlsx")
library("C50")

dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 

drop_columns <- c("kpr_aktif", "pendapatan_setahun_juta", "risk_rating", "rata_rata_overdue")
datafeed <- dataCreditRating[ , !(names(dataCreditRating) %in% drop_columns)]
modelKu <- C5.0(datafeed, as.factor(dataCreditRating$risk_rating))
summary(modelKu)
</summary>
  <table border="0">
 Call:
C5.0.default(x = datafeed, y = as.factor(dataCreditRating$risk_rating))


C5.0 [Release 2.07 GPL Edition]  	Fri Apr 23 04:26:14 2021
-------------------------------

Class specified by attribute `outcome'

Read 900 cases (4 attributes) from undefined.data

Decision tree:

jumlah_tanggungan > 4:
:...durasi_pinjaman_bulan <= 24: 4 (112/30)
:   durasi_pinjaman_bulan > 24: 5 (140/55)
jumlah_tanggungan <= 4:
:...jumlah_tanggungan > 2: 3 (246/22)
    jumlah_tanggungan <= 2:
    :...durasi_pinjaman_bulan <= 36: 1 (294/86)
        durasi_pinjaman_bulan > 36:
        :...jumlah_tanggungan <= 0: 2 (41/8)
            jumlah_tanggungan > 0: 3 (67/4)


Evaluation on training data (900 cases):

	    Decision Tree   
	  ----------------  
	  Size      Errors  

	     6  205(22.8%)   <<


	   (a)   (b)   (c)   (d)   (e)    <-classified as
	  ----  ----  ----  ----  ----
	   208     2     5     6     6    (a): class 1
	    86    33    21     6    13    (b): class 2
	           4   287                (c): class 3
	           2          82    36    (d): class 4
	                      18    85    (e): class 5


	Attribute usage:

	100.00%	jumlah_tanggungan
	 72.67%	durasi_pinjaman_bulan


Time: 0.0 secs
</table>
</details>

<p align="justify"><b>Data Preparation untuk Class Variable</b>
 <details>
 
  <summary>
  library("openxlsx")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
str(dataCreditRating)
#Melakukan konversi kolom risk_rating menjadi factor
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)
#Melihat struktur setelah konversi
str(dataCreditRating)
  </summary> 
  
  <table border="0">
  'data.frame':	900 obs. of  7 variables:
 $ kode_kontrak           : chr  "AGR-000001" "AGR-000011" "AGR-000030" "AGR-000043" ...
 $ pendapatan_setahun_juta: num  295 271 159 210 165 220 70 88 163 100 ...
 $ kpr_aktif              : chr  "YA" "YA" "TIDAK" "YA" ...
 $ durasi_pinjaman_bulan  : num  48 36 12 12 36 24 36 48 48 36 ...
 $ jumlah_tanggungan      : num  5 5 0 3 0 5 3 3 5 6 ...
 $ rata_rata_overdue      : chr  "61 - 90 days" "61 - 90 days" "0 - 30 days" "46 - 60 days" ...
 $ risk_rating            : Factor w/ 5 levels "1","2","3","4",..: 4 4 1 3 2 1 2 2 2 2 ...
  
  </table>
</details>
</p>
<p>Data Preparation untuk Input Variables</p><details>
  <summary>
  library("openxlsx")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
#Merubah tipe data class variable sebagai factor
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)
str(dataCreditRating)
#Menghilangkan beberapa variable input dari dataset
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
str(datafeed)
  </summary>
  
  <table border="0">
  'data.frame':	900 obs. of  2 variables:
 $ durasi_pinjaman_bulan: num  48 36 12 12 36 24 36 48 48 36 ...
 $ jumlah_tanggungan    : num  5 5 0 3 0 5 3 3 5 6 ...
  </table>
</details>
<p align="justify"><b>Training Set dan Testing Set</b>
<ol>Untuk proses pembentukan model machine learning dan melihat akurasinya, biasanya dataset kita perlu dibagi menjadi dua, yaitu:
<li>Training set: adalah porsi dataset yang digunakan oleh algoritma untuk dianalisa dan menjadi input untuk pembentukan model. </li>
<li>Testing set: adalah porsi dataset yang tidak digunakan untuk membentuk model, tapi untuk menguji model yang telah dibentuk.</li></ol>
Pembentukan ini biasanya menggunakan metode pemilihan acak. Untuk praktek selanjutnya, kita akan membagi dataset kita menjadi 800 baris data untuk training set dan 100 baris data untuk testing set.<p>

 <details>
  <summary><b>Mempersiapkan Training dan Testing Set</b></br>
  library("openxlsx")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)
#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
str(input_training_set)
str(class_training_set)
str(input_testing_set)
  
  </summary>
  <table border="0">
  
  > str(input_training_set)
'data.frame':	800 obs. of  2 variables:
 $ durasi_pinjaman_bulan: num  36 24 12 36 48 24 24 36 24 12 ...
 $ jumlah_tanggungan    : num  2 0 3 4 6 4 3 2 1 4 ...
> str(class_training_set)
 Factor w/ 5 levels "1","2","3","4",..: 2 1 3 2 5 3 3 2 2 3 ...
> str(input_testing_set)
'data.frame':	100 obs. of  2 variables:
 $ durasi_pinjaman_bulan: num  12 48 24 12 36 24 24 48 36 12 ...
 $ jumlah_tanggungan    : num  0 3 5 0 5 5 5 0 2 4 ...
 
  </table>
  
</details>
<details>
 <summary><b>Menghasilkan Model dengan Fungsi C5.0</b></br>
 library("openxlsx")
library("C50")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)
#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
#menghasilkan dan menampilkan summary model
risk_rating_model <- C5.0(input_training_set, class_training_set)
summary(risk_rating_model)
 </summary>
 
  <table border="0">
  Call:
C5.0.default(x = input_training_set, y = class_training_set)


C5.0 [Release 2.07 GPL Edition]  	Fri Apr 23 04:36:40 2021
-------------------------------

Class specified by attribute `outcome'

Read 800 cases (3 attributes) from undefined.data

Decision tree:

jumlah_tanggungan > 4:
:...durasi_pinjaman_bulan <= 24: 4 (98/25)
:   durasi_pinjaman_bulan > 24: 5 (129/49)
jumlah_tanggungan <= 4:
:...jumlah_tanggungan > 2: 3 (219/17)
    jumlah_tanggungan <= 2:
    :...durasi_pinjaman_bulan <= 36: 1 (259/80)
        durasi_pinjaman_bulan > 36:
        :...jumlah_tanggungan <= 0: 2 (37/7)
            jumlah_tanggungan > 0: 3 (58/2)


Evaluation on training data (800 cases):

	    Decision Tree   
	  ----------------  
	  Size      Errors  

	     6  180(22.5%)   <<


	   (a)   (b)   (c)   (d)   (e)    <-classified as
	  ----  ----  ----  ----  ----
	   179     1     5     5     6    (a): class 1
	    80    30    14     3    12    (b): class 2
	           4   258                (c): class 3
	           2          73    31    (d): class 4
	                      17    80    (e): class 5


	Attribute usage:

	100.00%	jumlah_tanggungan
	 72.62%	durasi_pinjaman_bulan


Time: 0.0 secs
  </table>
</details>


### Menghasilkan Plot dari Model C5.0
Selain model teks dari praktek sebelumnya, kita juga bisa menghasilkan decision tree dalam bentuk grafik. Dan cuma butuh satu perintah untuk melakukan hal ini, yaitu:
plot(model)
Dengan code praktek yang telah kita lakukan - dimana model kita namakan risk_rating_model - maka perintah tersebut kita sesuaikan menjadi.
plot(risk_rating_model)


library("openxlsx")
library("C50")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
#Mempersiapkan class dan input variables
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)
#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
#menghasilkan dan menampilkan summary model
risk_rating_model <- C5.0(input_training_set, class_training_set)
#[...1...]


download.png

### Label dari Class
Mari kita perhatikan baris pertama potongan output yang kita warnai biru sebagai berikut, output lainnya kita hilangkan.

Class specified by attribute `outcome'

Read 800 ....
Ini artinya class variable kita dilabelkan atau dinamakan sebagai outcome. Jika kita kita ingin merubah label yang lebih mewakili, yaitu "Risk Rating", maka bisa kita tambahkan parameter control dengan input berupa fungsi C5.0Control dan parameter label sebagai berikut.

control = C5.0Control(label="Risk Rating")
Tambahkan code ini ke bagian [...1...] dari code editor, dimana sudah dimasukkan seluruh code dari bab sebelumnya - dan jalankan. Jika berjalan dengan baik, maka akan muncul bagian output class label sebagai berikut.

Class specified by attribute `Risk Rating'

Read 800 ....

source coding
library("openxlsx")
library("C50")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
#Mempersiapkan class dan input variables
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating)
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)
#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
#menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))
summary(risk_rating_model)


output :
Call:
C5.0.default(x = input_training_set, y = class_training_set, control
 = C5.0Control(label = "Risk Rating"))


C5.0 [Release 2.07 GPL Edition]  	Fri Apr 23 04:43:07 2021
-------------------------------

Class specified by attribute `Risk Rating'

Read 800 cases (3 attributes) from undefined.data

Decision tree:

jumlah_tanggungan > 4:
:...durasi_pinjaman_bulan <= 24: 4 (98/25)
:   durasi_pinjaman_bulan > 24: 5 (129/49)
jumlah_tanggungan <= 4:
:...jumlah_tanggungan > 2: 3 (219/17)
    jumlah_tanggungan <= 2:
    :...durasi_pinjaman_bulan <= 36: 1 (259/80)
        durasi_pinjaman_bulan > 36:
        :...jumlah_tanggungan <= 0: 2 (37/7)
            jumlah_tanggungan > 0: 3 (58/2)


Evaluation on training data (800 cases):

	    Decision Tree   
	  ----------------  
	  Size      Errors  

	     6  180(22.5%)   <<


	   (a)   (b)   (c)   (d)   (e)    <-classified as
	  ----  ----  ----  ----  ----
	   179     1     5     5     6    (a): class 1
	    80    30    14     3    12    (b): class 2
	           4   258                (c): class 3
	           2          73    31    (d): class 4
	                      17    80    (e): class 5


	Attribute usage:

	100.00%	jumlah_tanggungan
	 72.62%	durasi_pinjaman_bulan


Time: 0.0 secs

### Jumlah Data dan Variable yang Digunakan
Kita fokus ke bagian berikutnya dari output - yang diwarnai biru sebagai berikut.

Class specified by attribute `Risk Rating'

Read 800 cases (3 attributes) from undefined.data 
Hasil ini artinya kita membaca 800 baris data. Ini karena kita mengambil 800 dari 900 data kita menggunakan function sample berikut.

sample(900, 800)

Kemudian untuk bagian 3 attributes, ini artinya kita memiliki tiga variable, yaitu:

input variables: durasi_pinjaman dan jumlah_tanggungan. Dua variable ini didapatkan dari hasil perintah berikut.

input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
class variable: risk_rating

source :
library("openxlsx")
library("C50")
#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")
#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan", "kpr_aktif")
datafeed <- dataCreditRating[ , input_columns ]
#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 780)
#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
#menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))
summary(risk_rating_model)


out:

Class specified by attribute `Risk Rating'

Read 780 cases (4 attributes) from undefined.data

Decision tree:

kpr_aktif = TIDAK:
:...durasi_pinjaman_bulan <= 24: 1 (189/45)
:   durasi_pinjaman_bulan > 24: 2 (136/47)
kpr_aktif = YA:
:...jumlah_tanggungan <= 4: 3 (259/4)
    jumlah_tanggungan > 4:
    :...durasi_pinjaman_bulan <= 24: 4 (87/15)
        durasi_pinjaman_bulan > 24: 5 (109/31)


Evaluation on training data (780 cases):

	    Decision Tree   
	  ----------------  
	  Size      Errors  

	     5  142(18.2%)   <<


	   (a)   (b)   (c)   (d)   (e)    <-classified as
	  ----  ----  ----  ----  ----
	   144    44     2                (a): class 1
	    44    89                 1    (b): class 2
	           2   255                (c): class 3
	                 2    72    30    (d): class 4
	     1     1          15    78    (e): class 5


	Attribute usage:

	100.00%	kpr_aktif
	 66.79%	durasi_pinjaman_bulan
	 58.33%	jumlah_tanggungan


Time: 0.0 secs

### Merubah Label Class Variable
Label risk rating pada confusion matrix kita saat ini adalah 1 sampai dengan 5. Ini karena kebetulan contoh class variable kita adalah angka terurut seperti itu.

Agar tidak bingung mari kita ubah isi dari variable risk_rating dari 1, 2, 3, 4, dan 5 menjadi "satu", "dua", "tiga", "empat" dan "lima" dengan perintah-perintah berikut.

dataCreditRating$risk_rating[dataCreditRating$risk_rating == "1"]  <-  "satu"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "2"]  <-  "dua"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "3"]  <-  "tiga"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "4"]  <-  "empat"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "5"]  <-  "lima"
Tiap perintah ini memiliki tiga bagian, kita akan ambil contoh untuk perintah pertama:

dataCreditRating$risk_rating[...], artinya kita mengakses kolom risk_rating  pada indeks ke ...
dataCreditRating$risk_rating == "1", artinya kita mengambil indeks dimana nilai risk_rating bernilai "1"
<- "satu", teks "satu" dimasukkan ke ...
Dengan demikian, arti dari perintah pertama adalah memasukkan teks "satu" ke variable risk_rating dengan indeks dimana nilai risk_rating bernilai "1".

Masukkan kelimat perintah tersebut untuk menggantikan #[...1...] pada code editor kita dan jalankan. Jika berhasil, perhatikan output confusion matrix dimana labelnya - ditandai dengan warna biru - sudah berubah menjadi teks "dua", "empat", "lima", "satu", dan "tiga" (diurutkan berdasarkan alfabet).

source :
library("openxlsx")
library("C50")

#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")

#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "1"] <- "satu"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "2"] <- "dua"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "3"] <- "tiga"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "4"] <- "empat"
dataCreditRating$risk_rating[dataCreditRating$risk_rating == "5"] <- "lima"
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]

#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)

#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]

#menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))
summary(risk_rating_model)

out :
Class specified by attribute `Risk Rating'

Read 800 cases (3 attributes) from undefined.data

Decision tree:

jumlah_tanggungan > 4:
:...durasi_pinjaman_bulan <= 24: empat (98/25)
:   durasi_pinjaman_bulan > 24: lima (129/49)
jumlah_tanggungan <= 4:
:...jumlah_tanggungan > 2: tiga (219/17)
    jumlah_tanggungan <= 2:
    :...durasi_pinjaman_bulan <= 36: satu (259/80)
        durasi_pinjaman_bulan > 36:
        :...jumlah_tanggungan <= 0: dua (37/7)
            jumlah_tanggungan > 0: tiga (58/2)


Evaluation on training data (800 cases):

	    Decision Tree   
	  ----------------  
	  Size      Errors  

	     6  180(22.5%)   <<


	   (a)   (b)   (c)   (d)   (e)    <-classified as
	  ----  ----  ----  ----  ----
	    30     3    12    80    14    (a): class dua
	     2    73    31                (b): class empat
	          17    80                (c): class lima
	     1     5     6   179     5    (d): class satu
	     4                     258    (e): class tiga


	Attribute usage:

	100.00%	jumlah_tanggungan
	 72.62%	durasi_pinjaman_bulan


Time: 0.0 secs

### Menggunakan Fungsi Predict
library("openxlsx")
library("C50")

#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")

#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]

#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)

#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]

#menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))

#menggunakan model untuk prediksi testing set
predict(risk_rating_model, input_testing_set)

out :
predict(risk_rating_model, input_testing_set)
  [1] 1 3 4 1 5 4 4 2 1 3 3 1 3 3 3 4 3 1 1 3 5 3 5 1 4 4 1 3 3 1 2 3 3 1 3 5 3
 [38] 3 3 1 3 3 5 1 5 1 5 1 3 3 4 1 3 1 1 3 1 1 1 5 1 1 3 3 3 1 3 4 1 3 3 3 1 3
 [75] 4 3 2 1 3 1 5 5 1 1 4 4 3 2 4 1 1 4 1 3 1 1 1 5 4 3
Levels: 1 2 3 4 5

### Menggabungkan Hasil Prediksi
library("openxlsx")
library("C50")

#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")

#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]

#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)

#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]

#menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))

#menyimpan risk_rating dari data awal dan hasil prediksi testing set ke dalam kolom hasil_prediksi
input_testing_set$risk_rating <- dataCreditRating[-indeks_training_set,]$risk_rating
input_testing_set$hasil_prediksi <- predict(risk_rating_model, input_testing_set)

#menampilkan variable input_testing_set
print(input_testing_set)

out :
 durasi_pinjaman_bulan jumlah_tanggungan risk_rating hasil_prediksi
3                      12                 0           1              1
8                      48                 3           2              3
21                     24                 5           2              4
26                     12                 0           1              1
28                     36                 5           2              5
37                     24                 5           2              4
39                     24                 5           1              4
45                     48                 0           1              2
49                     36                 2           1              1
53                     12                 4           2              3
54                     48                 1           2              3
58                     12                 0           1              1
60                     48                 3           2              3
61                     48                 2           2              3
62                     24                 4           2              3
65                     24                 5           2              4
75                     24                 3           2              3
97                     12                 0           1              1
112                    12                 0           1              1
117                    24                 3           3              3
123                    36                 6           4              5
125                    36                 4           3              3
128                    36                 6           4              5
162                    12                 0           1              1
166                    24                 6           4              4
187                    12                 6           5              4
192                    36                 1           1              1
198                    12                 3           3              3
219                    48                 2           3              3
221                    36                 1           1              1
239                    48                 0           2              2
250                    24                 4           3              3
255                    48                 1           3              3
276                    36                 1           1              1
284                    24                 4           3              3
287                    36                 6           5              5
291                    24                 4           3              3
323                    48                 4           3              3
329                    48                 1           3              3
335                    24                 0           1              1
336                    48                 2           3              3
338                    36                 3           3              3
345                    36                 5           4              5
348                    24                 0           1              1
350                    48                 6           5              5
356                    36                 2           2              1
386                    48                 5           4              5
398                    12                 1           1              1
406                    36                 4           3              3
416                    24                 3           3              3
434                    12                 6           4              4
460                    12                 1           1              1
466                    48                 1           3              3
514                    36                 2           1              1
520                    24                 1           2              1
526                    24                 3           3              3
532                    36                 1           1              1
539                    36                 2           2              1
541                    24                 1           1              1
548                    48                 6           5              5
560                    24                 1           1              1
564                    24                 1           1              1
568                    48                 3           3              3
570                    48                 4           3              3
572                    24                 4           3              3
584                    36                 2           1              1
585                    36                 4           3              3
587                    12                 6           4              4
588                    12                 2           1              1
646                    48                 1           3              3
660                    36                 4           3              3
665                    12                 4           3              3
666                    36                 0           2              1
673                    48                 3           3              3
677                    24                 5           4              4
678                    48                 4           3              3
679                    48                 0           2              2
681                    36                 1           1              1
684                    48                 2           3              3
687                    12                 2           1              1
701                    48                 5           4              5
704                    48                 6           5              5
707                    12                 2           1              1
708                    12                 2           1              1
716                    12                 5           4              4
732                    24                 6           4              4
734                    24                 3           3              3
767                    48                 0           2              2
776                    12                 6           4              4
781                    24                 2           1              1
782                    24                 2           1              1
790                    12                 6           4              4
813                    24                 2           1              1
839                    24                 4           3              3
842                    36                 0           2              1
845                    24                 2           2              1
853                    24                 2           1              1
861                    48                 5           5              5
862                    12                 6           4              4
870                    48                 3           3              3


### Membuat Table Confusion Matrix
library("openxlsx")
library("C50")
library("reshape2")
# Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://academy.dqlab.id/dataset/credit_scoring_dqlab.xlsx")
# Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
# Mempersiapkan training dan testing set
set.seed(100) # untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)
# Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
# menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))
# menyimpan risk_rating dari data awal dan hasil prediksi testing set ke dalam kolom hasil_prediksi
input_testing_set$risk_rating <- dataCreditRating[-indeks_training_set,]$risk_rating
input_testing_set$hasil_prediksi <- predict(risk_rating_model, input_testing_set)
# membuat confusion matrix
dcast(hasil_prediksi ~ risk_rating, data=input_testing_set)

### Jumlah Data dengan Prediksi Benar
Untuk menghitung persentase error, kita bisa menghitung terlebih dahulu jumlah data dengan prediksi yang benar. Hasil dikatakan benar jika data risk_rating sama dengan hasil_prediksi. Ini kalau kita tuliskan dengan code adalah sebagai berikut.
input_testing_set$risk_rating==input_testing_set$hasil_prediksi

source
library("openxlsx")
library("C50")

#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")

#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]

#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)

#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]

#menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))

#menyimpan risk_rating dari data awal dan hasil prediksi testing set ke dalam kolom hasil_prediksi
input_testing_set$risk_rating <- dataCreditRating[-indeks_training_set,]$risk_rating
input_testing_set$hasil_prediksi <- predict(risk_rating_model, input_testing_set)

#Menghitung jumlah prediksi yang benar
nrow(input_testing_set[input_testing_set$risk_rating==input_testing_set$hasil_prediksi,])

out :
nrow(input_testing_set[input_testing_set$risk_rating==input_testing_set$hasil_prediksi,])
[1] 75

### Jumlah Data dengan Prediksi Salah

library("openxlsx")
library("C50")
# Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://academy.dqlab.id/dataset/credit_scoring_dqlab.xlsx")
# Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]
# Mempersiapkan training dan testing set
set.seed(100) # untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)
# Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]
# menghasilkan model
risk_rating_model <- C5.0(input_training_set, class_training_set, control = C5.0Control(label="Risk Rating"))
# menyimpan risk_rating dari data awal dan hasil prediksi testing set ke dalam kolom hasil_prediksi
input_testing_set$risk_rating <- dataCreditRating[-indeks_training_set,]$risk_rating
input_testing_set$hasil_prediksi <- predict(risk_rating_model, input_testing_set)
# Menghitung jumlah prediksi yang benar
nrow(input_testing_set[input_testing_set$risk_rating==input_testing_set$hasil_prediksi,])

out : 
nrow(input_testing_set[input_testing_set$risk_rating==input_testing_set$hasil_prediksi,])
[1] 75

### empersiapkan Data Pengajuan Baru
Data pengajuan baru perlu dibentuk sebagai satu data frame dengan input dimana nama-nama variable yang digunakan harus sama persis. Dari awal pemodelan, kita menggunakan dua variable yakni:

jumlah_tanggungan
durasi_pinjaman_bulan
Keduanya dalam bentuk numerik (angka). Dan berikut adalah contoh membentuk data frame dengan dua variable tersebut.

aplikasi_baru <- data.frame(jumlah_tanggungan = 6, durasi_pinjaman_bulan = 12)

source : 
#Membuat data frame aplikasi baru
aplikasi_baru <- data.frame(jumlah_tanggungan = 6, durasi_pinjaman_bulan = 12)

out : 
jumlah_tanggungan durasi_pinjaman_bulan
1                 6                    12

### Melakukan Prediksi terhadap Data Pengajuan Baru
Data aplikasi baru yang telah kita buat sebelumnya akan diprediksi nilai risk_rating-nya dengan fungsi predict, dimana cara penggunaannya masih sama dengan cara yang ditunjukkan pada bab evaluasi model, yaitu dengan syntax berikut.

predict(model, dataframe)
Code editor telah terisi code-code yang digunakan untuk membentuk model dan data frame aplikasi baru. Gantilah bagian [...1...] dengan code untuk melakukan prediksi. Sesuaikan perintah di atas dengan nama model dan variable yang digunakan. Variable risk_rating_model sebagai model dan aplikasi_baru sebagai data frame yang akan di prediksi.
print(aplikasi_baru)

source :
library("openxlsx")
library("C50")

# Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://academy.dqlab.id/dataset/credit_scoring_dqlab.xlsx")

# Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]

# Mempersiapkan training dan testing set
set.seed(100) # untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)

# Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]

# menghasilkan dan menampilkan summary model
risk_rating_model <- C5.0(input_training_set, class_training_set)

# Membuat data frame aplikasi baru
aplikasi_baru <- data.frame(jumlah_tanggungan = 6, durasi_pinjaman_bulan = 12)

# melakukan prediksi
predict(risk_rating_model, aplikasi_baru)

out :
[1] 4
Levels: 1 2 3 4 5

### Merubah Durasi Pinjaman
Pada pelajaran sebelumnya kita sudah mempelajari cara memprediksi dari data frame berdasarkan model yang telah dibuat. Sekarang kita mencoba memprediksi dari data yang tidak ada dari data set yang dijadikan model.

library("openxlsx")
library("C50")

#Mempersiapkan data
dataCreditRating <- read.xlsx(xlsxFile = "https://storage.googleapis.com/dqlab-dataset/credit_scoring_dqlab.xlsx")

#Mempersiapkan class dan input variables 
dataCreditRating$risk_rating <- as.factor(dataCreditRating$risk_rating) 
input_columns <- c("durasi_pinjaman_bulan", "jumlah_tanggungan")
datafeed <- dataCreditRating[ , input_columns ]

#Mempersiapkan training dan testing set
set.seed(100) #untuk menyeragamkan hasil random antar tiap komputer
indeks_training_set <- sample(900, 800)

#Membuat dan menampilkan training set dan testing set
input_training_set <- datafeed[indeks_training_set,]
class_training_set <- dataCreditRating[indeks_training_set,]$risk_rating
input_testing_set <- datafeed[-indeks_training_set,]

#menghasilkan dan menampilkan summary model
risk_rating_model <- C5.0(input_training_set, class_training_set)

#Membuat data frame aplikasi baru
aplikasi_baru <- data.frame(jumlah_tanggungan = 6, durasi_pinjaman_bulan = 64)

#melakukan prediksi
predict(risk_rating_model, aplikasi_baru)

out :
[1] 5
Levels: 1 2 3 4 5

### Kesimpulan
Bab ini telah menunjukkan bagaimana kita melakukan prediksi nilai resiko kredit (risk_rating) terhadap data aplikasi baru.

Fungsi yang digunakna sangat sederhana, tetapi kita perlu ketat dengan syarat ketika menyiapkan input:

Input berupa data frame.
Kolom-kolom di dalam data frame harus sama dengan input yang digunakan untuk menghasilkan model.
