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
str(dataCreditRating)</summary> </br>
  <table border="0"><tr><td>> library("openxlsx") </br>
> #Mempersiapkan data </br>
</td></tr></table>
</details>
</p>
