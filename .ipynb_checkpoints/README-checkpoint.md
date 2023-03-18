# **Default Payment**
## Problem Statement
Pada tahun 2001, 2002, dan 2005, Taiwan merupakan salah satu negara yang mengalami debt crisis (Chang, 
2022). Salah satu faktor yang menyebabkan terjadinya krisis tersebut seperti tidak adanya pengawasan dari 
pihak bank terkait status pengajuan kartu kredit nasabah dan reporting system yang tidak teratur. Pengecekan 
histori transaksi nasabah harus dilakukan dalam menentukan apakah pengajuan kartu kredit nasabah bisa 
diterima atau tidak, sehingga potensi untuk mendapatkan nasabah yang bisa mengakibatkan gagal bayar bisa 
menjadi berkurang

## Goals
Menurunkan jumlah nasabah yang berpotensi mengalami gagal bayar

## Exploratory Data Analysis (EDA)
Berdasarkan hasil dari code ```df.info()```, tidak ada feature yang memiliki *Missing Value*. Total data yang dimiliki adalah **21.000** dengan keterangan sebagai berikut:
<table>
    <tr>
        <td>ID</td>
        <td>ID of each client</td>
    </tr>
    <tr>
        <td>LIMIT_BAL</td>
        <td>Amount of given credit in NT dollars (includes individual and family/supplementary credit)</td>
    </tr>
    <tr>
        <td>SEX</td>
        <td>Gender (1=male, 2=female)</td>
    </tr>
    <tr>
        <td>EDUCATION</td>
        <td>(1=graduate school, 2=university, 3=high school, 4=others, 5=unknown, 6=unknown)</td>
    </tr>
    <tr>
        <td>MARRIAGE</td>
        <td>Marital status (1=married, 2=single, 3=others)</td>
    </tr>
    <tr>
        <td>AGE</td>
        <td>Age in years</td>
    </tr>
    <tr>
        <td>PAY_0</td>
        <td>Repayment status in September, 2005 (-2=no consumption, -1=pay duly, 0=the use of revolving credit, 1=payment delay for one month, 2=payment delay for two months, … 8=payment delay for eight months, 9=payment delay for nine months and above)</td>
    </tr>
    <tr>
        <td>PAY_2</td>
        <td>Repayment status in August, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_3</td>
        <td>Repayment status in July, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_4</td>
        <td>Repayment status in June, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_5</td>
        <td>Repayment status in May, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>PAY_6</td>
        <td>Repayment status in April, 2005 (scale same as above)</td>
    </tr>
    <tr>
        <td>BILL_AMT1</td>
        <td>Amount of bill statement in September, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT2</td>
        <td>Amount of bill statement in August, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT3</td>
        <td>Amount of bill statement in July, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT4</td>
        <td>Amount of bill statement in June, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT5</td>
        <td>Amount of bill statement in May, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>BILL_AMT6</td>
        <td>Amount of bill statement in April, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT1</td>
        <td>Amount of previous payment in September, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT2</td>
        <td>Amount of previous payment in August, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT3</td>
        <td>Amount of previous payment in July, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT4</td>
        <td>Amount of previous payment in June, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT5</td>
        <td>Amount of previous payment in May, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>PAY_AMT6</td>
        <td>Amount of previous payment in April, 2005 (NT dollar)</td>
    </tr>
    <tr>
        <td>default.payment.next.month</td>
        <td>Default payment (1=yes, 0=no)</td>
    </tr>
</table>

Selain itu, dari proses EDA ini juga ditemukan bahwa semua data kategorik sudah di **encode**. Dengan menjalankan fungsi ```df['Column'].value_counts()``` didapatkan hasil sebagai berikut:<br>
<img width="173" alt="value_counts" src="https://user-images.githubusercontent.com/40890491/225890346-305e5066-b01a-468e-97e4-3764d837b474.png">
Dapat dilihat bahwa data pada fitur **SEX** sudah sesuai dengan kategori yang diberikan, namun untuk fitur **EDUCATION** dan **MARRIAGE** masih terdapat data yang tidak sesuai dengan kategori yang ditentukan (0). Sehingga, kesalahan data tersebut akan ditangani pada proses selanjutnta yaitu **Preprocessing Data**

Hal serupa juga dilakukan untuk fitur kategori **PAY_N (0-6)**, dimana ditemukan **TIDAK ADA** data yang hilang ataupun tidak sesuai dengan kategori yang ditentukan (-2 s/d 8). 

## Univariate Analysis
Univariate analysis dilakukan untuk memahami setiap fitur dari dataset lebih dalam lagi. Untuk pemahaman fitur **AGE, LIMIT_BAL, BILL_AMT(N), dan PAY_AMT(N)** dilakukan dengan melakukan visualisasi boxplot dengan menggunakan code ```sns.boxplot(y=df[account[i]], color='blue', orient='v')```

![box_plot](https://user-images.githubusercontent.com/40890491/225930017-7601be80-887f-4501-9830-606b3445679b.png)

Ada beberapa fitur diatas yang memiliki **indikasi outliers** seperti fitur **BILL_AMT(N) dan PAY_AMT(N)**. Perhitungan IQR atau Z-Score juga akan dilakukan di tahapan selanjutnya untuk menentukan Outliers dari fitur tersebut. Selain itu, dilihat juga distribusi dari fitur-fitur tersebut dengan menggunakan perintah ```sns.kdeplot(x=df[account[i]], color='green')```

![kde_plot](https://user-images.githubusercontent.com/40890491/225931736-fa2aafc7-4e5d-46e1-8e32-d04255ccd1c2.png)

Dapat dilihat bahwa diantara fitur-fitur numerik tersebut, **tidak ada fitur yang memiliki distribusi normal**. Untuk analisis fitur *categorical* digunakan fungsi countplot berikut untuk memahami fitur tersebut ```sns.countplot(x=df[categoricals_demography[i]], data=df, color='green')```

![count_plot](https://user-images.githubusercontent.com/40890491/225932589-a1d6ac27-1907-4292-bece-32176c22b080.png)
![count_plot_2](https://user-images.githubusercontent.com/40890491/225932759-286bbff4-645b-469d-b935-9000506eb87b.png)
![count_plot_3](https://user-images.githubusercontent.com/40890491/225932920-13ebccc3-afcb-4d66-b5fc-9d68b608a264.png)
![count_plot_4](https://user-images.githubusercontent.com/40890491/225932927-386b5201-681a-460c-9f11-c59074f2242e.png)

Berdasarkan dari visualisasi tersebut, didapatkan beberapa *insight* seperti berikut:
<li>Nasabah paling banyak berjenis kelamin Perempuan</li>
<li>Sebagian besar dari nasabah masih berusia < 30 tahun</li>
<li>Kebanyakan nasabah dari bank di Taiwan memiliki latar belakang pendidikan Sarjana (S1)</li>
<li>Status dari sebagian besar nasabah bank adalah SINGLE</li>
<li>Bedasarkan dari historical data yang ada, sebagian besar nasabah yang terdaftar sudah membayar tagihan kredit sesuai dengan jatuh tempo</li>

Hal yang sama juga dilakukan untuk fitur *categorical* **PAY_N** dengan menggunakan code berikut ```sns.countplot(x=df[categoricals_payment[i]], color='green', orient='h')```

![count_plot_pay](https://user-images.githubusercontent.com/40890491/225933844-fbf86d5a-3c64-4d57-998d-dbb185c12ec9.png)

Untuk kategori PAY_N memiliki bentuk distribusi yang Skewed, dan sebagian besar nasabah menggunakan fasilitas kartu kredit mereka untuk melakukan penarikan tunai kredit (Kategori 0)

## Multivariate Analysis

Penggunaan visualisasi heatmap dilakukan untuk melihat hubungan korelasi antara **variable fitur dengan fitur** dan **variable fitur dengan target**

![heat_map](https://user-images.githubusercontent.com/40890491/225934997-aa414673-2e93-4a84-b6f0-1f279e8c8f54.png)

Berdasarkan dari heatmap di atas, korelasi **Fitur > Fitur** paling tinggi adalah hubungan antara **PAY_N ke sesama PAY_N** dan **BILL_AMT(N) ke sesama BILL_AMT(N)**, dimana berarti data pada fitur tersebut saling berhubungan erat satu dengan lainnya. Hal ini juga dapat dihubungkan dengan *business logic* yang terjadi pada dataset dimana data pada fitur **PAY_(N)** merupakan keterangan dari data pada fitur **BILL_AMT(N)**

Sedangkan hubungan antara data **Target > Fitur** paling tinggi adalah **default_payment_next_month ke PAY_N**.

Fitur <b>default_payment_next_month</b> memiliki korelasi positif paling tinggi dengan fitur <b>Pay_0 - PAY_6</b>, dimana pola korelasinya adalah <b>SEMAKIN SEDIKIT PERIODE PELUNASAN KREDIT SUATU NASABAH, SEMAKIN BESAR JUGA KEMUNGKINAN NASABAH TERSEBUT UNTUK GAGAL BAYAR</b><br>

Sedangkan <b>default_payment_next_month</b> memiliki korelasi negatif paling tinggi dengan fitur <b>LIMIT_BAL</b>, dimana <b>SEMAKIN TINGGI LIMIT DARI SUATU NASABAH, MAKA KEMUNGKINAN NASABAH UNTUK GAGAL BAYAR AKAN SEMAKIN KECIL</b><br>

Sehingga, kami menyimpulkan bahwa Fitur <b>PAY_0 - PAY_6</b> dan <b>LIMIT_BAL</b> adalah fitur yang cukup menarik untuk ditelusuri lebih lanjut terhadap data target

## Business Insight
![insight_1](https://user-images.githubusercontent.com/40890491/226077448-5a3a1a2d-ceb4-4532-9ce3-74741bb24562.png)

Dari korelasi terlihat adanya hubungan yang negatif antara limit balance dan kemungkinan default nasabah. Dari distribusi di atas terlihat pula bahwa dengan limit ballace yang tinggi lebih banyak yang tidak default. Dari hal ini didapatkan bahwa membatasi limit ballance nasabah bukan cara yang efektif untuk mencegah nasabah melakukan default. Perlu dilihat dari kriteria lain terkait strategi yang dapat diterapkan dalam mencegah nasabah melakukan default

![insight_2](https://user-images.githubusercontent.com/40890491/226077503-e720988a-beee-4ea3-ad74-0ecc88ff7875.png)

Diantara fitur berjenis kategori, selain status pembayaran (fitur PAY_0 - PAY_6) korelasi antara default status dengan sex memiliki nilai yang lebih tinggi. Laki-laki cenderung lebih banyak melakukan default dibandingkan dengan perempuan. Dengan mengombinasikan kriteria jenis kelamin dan kriteria lainnya bisa didapatkan kriteria yang akan cenderung melakukan default sehingga ketika nasabah dengan kriteria tersebut akan mengajukan kartu kredit dapat dipertimbangkan kembali terkait pengajuannya

![insight_3](https://user-images.githubusercontent.com/40890491/226077536-cc5eb265-1cae-45d2-86e2-226cdbee21b6.png)

Dari grafik di atas terlihat bahwa nasabah yang telat melakukan pembayaran lebih cenderung untuk melakukan default dibandingkan dengan nasabah yang membayar tepat waktu dan bahkan yang melakukan tarik tunai dengan kartu kreditnya. Nasabah yang telat melakukan pembayaran 3 bulan lebih banyak yang akhirnya melakukan default. Dari hal ini, bank perlu memastikan nasabah untuk membayar tagihan tepat waktu agar menurunkan kemungkinan nasabah untuk default