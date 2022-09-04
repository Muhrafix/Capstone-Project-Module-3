# Capstone Project Module 3 - Bike Sharing
### **Bike Sharing (Capital Bikeshare)**

![CAPITAL BIKESHARE LOGO](https://user-images.githubusercontent.com/107845860/188307123-87e7eee9-0198-4ae1-87b6-58bb958680aa.png)

### **Contents**

1. Business Problem Understanding
2. Data Understanding
3. Data Preprocessing
4. Modeling
5. Conclusion
6. Recommendation

****
### **Business Problem Understanding**
**Context**<br>

**Capital Bikeshare** adalah sebuah sistem peminjaman sepeda yang terdapat di Amerika Serikat. Sistem peminjaman sepeda ini adalah generasi baru dari rental sepeda tradisional dimana seluruh proses keanggotaan, penyewaan, dan pengembalian telah diotomasi. Melalui sistem ini, pengguna mampu secara mudah menyewa sepeda dari satu lokasi tertentu dan mengembalikkannya di lokasi yang lain. Saat ini, terdapat sekitar lebih dari 500 program *bike-sharing* di seluruh dunia dengan lebih dari 500.000 sepeda. **Capital Bikeshare** sendiri memiliki lebih dari 600 stasiun dan sekitar 5.000 sepeda [Capital Bikeshare](https://capitalbikeshare.com/) yang tersebar di wilayah Washington. Dewasa ini, minat terhadap sistem ini cukup besar dikarenakan peran penting terhadap isu lalu lintas, lingkungan, dan kesehatan.

Terlepas dari menariknya pengaplikasian sistem peminjaman sepeda di dunia nyata, karakteristik data yang dihasilkan oleh sistem ini membuat hal tersebut menarik untuk diteliti. Berbanding terbalik dengan sarana transport lain seperti bus atau *subway*, durasi perjalanan, posisi keberangkatan dan kedatangan terekam secara jelas di sistem ini. Fitur ini menjadikan sistem peminjaman sepeda menjadi sebuah *virtual sensor network* yang bisa digunakan untuk merasakan mobilitas di dalam kota. Oleh sebab itu, diharapkan banyak kejadian penting di dalam kota dapat dideteksi melalui pemantauan data ini.
<br>

**Problem Statement**<br>

Salah satu dari banyak tantangan dalam sistem peminjaman ini ialah **untuk menyediakan jumlah unit sepeda yang cukup di setiap kondisi dan situasi. Jika tidak, maka kita bisa gagal dalam memenuhi tuntutan dari pengguna sistem peminjaman sepeda tersebut dan bahkan bisa berujung kepada hilangnya kepercayaan pelanggan**. Namun apabila jumlah ketersediaan sepeda terlalu banyak, hal itu bisa menyebabkan banyaknya unit sepeda yang tidak terpakai, dalam kata lain, tidak efisien. Dimana hal tersebut bisa berimbas kepada membengkaknya biaya manajemen, logistik, serta perawatan untuk tiap unit sepeda. Seperti yang dilansir [Wikipedia](https://en.wikipedia.org/wiki/Capital_Bikeshare), setiap satu unit sepeda berharga $1000 dan biaya operasi tahunan setiap sepedanya adalah $1860.
<br>

**Goals**<br>

Berdasarkan permasalahan tersebut, Capital Bikeshare tentu perlu memiliki 'tool' yang baik guna memprediksi serta membantu tiap *stakeholder* yang terkait (baik klien maupun Capital Bikeshare itu sendiri) untuk dapat **menentukan jumlah unit sepeda yang tersedia dengan tepat di setiap kondisi dan situasi**. Adanya perbedaan situasi dan kondisi seperti cuaca, musim, kelembaban, suhu, dapat menambah keakuratan prediksi jumlah unit sepeda yang perlu disediakan. Dimana hal tersebut tidak hanya mendatangkan profit namun juga menjaga efisiensi *operational cost* dari sisi Capital Bikeshare dan jika dari sisi pengguna (klien) tentunya hal tersebut sangat membantu dalam memenuhi tuntutan kebutuhan pelanggan.
<br>

**Analytic Approach**<br>

kita akan melakukan analisa terhadap data untuk dapat menemukan pola dari fitur-fitur yang ada, yang membedakan satu kondisi dengan yang lainnya, serta bagaimana tiap fitur tersebut mempengaruhi jumlah unit sepeda yang perlu tersedia. Selanjutnya, kita akan membangun suatu model regresi yang akan membantu dalam menentukan jumlah unit sepeda yang perlu disediakan oleh Capital Bikeshare, yang mana akan berguna untuk menjaga efisiensi *operational cost*.
<br>

**Metric Evaluation**<br>

Evaluasi metrik yang akan digunakan adalah MAE, MAPE, dan R-squared. Dimana MAE adalah rataan nilai absolut dari error, sedangkan MAPE adalah rataan persentase error yang dihasilkan oleh model regresi. Semakin kecil nilai MAE dan MAPE yang dihasilkan, berarti model semakin akurat dalam memprediksi jumlah unit sepeda dengan limitasi fitur yang digunakan.  Selain itu, kita juga akan menggunakan nilai R-squared jika model yang nanti terpilih sebagai final model adalah model linear. Nilai R-squared digunakan untuk mengetahui seberapa baik model dapat merepresentasikan varians keseluruhan data, seberapa besar pengaruh variabel independen terhadap variabel dependen. Semakin mendekati 1, maka semakin fit pula modelnya terhadap data observasi. Namun, metrik ini tidak valid untuk model non-linear.

### **Data Understanding**

- Dataset merupakan data peminjaman sepeda di sistem ***Capital Bikeshare*** dalam rentang tahun 2011-2012.
- Setiap baris data merepresentasikan informasi terkait waktu peminjaman, cuaca, dan musim yang sesuai.

**Attributes Information**

| **Attribute** | **Data Type** | **Description** |
| --- | --- | --- |
| dteday | Object | Date |
| hum | Float | Normalized humidity (the values are divided to 100)|
| weathersit | Integer | 1: Clear, Few clouds, Partly cloudy, Partly cloudy<br> 2: Mist + Cloudy, Mist + Broken clouds, Mist + Few clouds, Mist<br> 3: Light Snow, Light Rain + Thunderstorm + Scattered clouds, Light Rain + Scattered clouds<br> 4: Heavy Rain + Ice Pallets + Thunderstorm + Mist, Snow + Fog |
| holiday | Integer | 0: Not holiday<br> 1: Holiday |
| season | Integer | 1: Winter<br> 2: Spring<br> 3: Summer<br> 4: Fall |
| atemp | Float | "Feels like" temperature in Celsius |
| temp | Float | Normalized temperature in Celsius |
| hr | Integer | Hour (0 to 23) |
| casual | Integer | Count of casual users |
| registered | Integer | Count of registered users |
| cnt | Integer | Count of total rental bikes including both casual and registered users|

### **Data Preprocessing**

Pada tahap ini, kita akan melakukan cleaning pada data yang nantinya data yang sudah dibersihkan akan kita gunakan untuk proses analisis selanjutnya. Beberapa hal yang perlu dilakukan adalah:
- Menyesuaikan beberapa nama kolom dan value untuk memudahkan keterbacaan data.
- Mengubah tipe data feature agar sesuai dengan nilai yang dimiliki.
- Mengecek missing value dan data duplikat.
- Drop feature yang tidak memiliki relevansi terhadap permasalahan yang sedang dihadapi.
- Mengecek korelasi antar data.
- Mengecek outlier pada data numerik.

#### **Clean Dataset**

![CLEAN DATASET](https://user-images.githubusercontent.com/107845860/188306707-79126c9d-d69d-43b9-a680-2bd11672373e.png)

### **Modelling (Using Train Set)**

![BENCHMARK MODEL](https://user-images.githubusercontent.com/107845860/188306746-31fd3c84-e749-45d3-9b3b-0e3266dd305a.png)

### **Predict to Test Set with Best Paramaters (XGBoost)**

![PREDICT TO TEST SET](https://user-images.githubusercontent.com/107845860/188306876-1713bb09-0ef1-41c6-a8d2-cb52013fb7b0.png)

### **Feature Importances**

![FEATURE IMPORTANCES](https://user-images.githubusercontent.com/107845860/188306975-005aa105-fc56-4612-bc8e-9b00bbabfac8.png)

### **Residual Plot**

![RESIDUAL PLOT](https://user-images.githubusercontent.com/107845860/188307008-b823cec6-7a5c-4449-b7db-8fa4b14b633d.png)

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

# Muhammad Rafi Amiruddin JC Data Science and Machine Learning
