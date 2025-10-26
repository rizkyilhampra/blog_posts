---
id: 1760877811-masalah-xampp-di-eclaim-inacbg-bpjs-dan-cara-mengatasinya
alias: Masalah XAMPP di ECLAIM (INACBG) BPJS dan Cara Mengatasinya
tags: []
created: 2025-09-19
---
# Masalah XAMPP di ECLAIM (INACBG) BPJS dan Cara Mengatasinya

Di sini adalah langkah-langkah yang saya lakukan untuk mengatasi masalah yang berkaitan dengan MySQL di ECLAIM yang mana basisnya menggunakan XAMPP yang *self hosted* pada komputer atau server sendiri.

> Yap, jika ada yang berencana untuk *targeting attack* ke suatu rumah sakit, ada cara yang bisa dilakukan, salah satunya cari di mana ECLAIM berada. Tidak sedikit dari rumah sakit yang juga melakukan *tunneling* untuk *expose* aplikasi ini ke internet (umumnya menggunakan *service* dari [ngrok](https://ngrok.io)). 
> 
> Ini adalah sebuah kritikan juga untuk pihak penyedia sistem atau aplikasi ini yang masih menggunakan XAMPP untuk *self hosted provider*. *You are need to learn docker shit head!*

Cerita sedikit mengenai latar belakang mengapa tulisan ini di buat. Ada suatu rumah sakit militer yang tidak mempunyai pegawai IT didalamnya, kemudian terkendala masalah ECLAIM-nya yang tidak bisa terbuka. Sebuah *chat* pertama kali masuk ke saya dari pihak JKN pada rumah sakit tersebut dengan sebuah gambar yang berisi peringatan atau *error* di log XAMPP yang mengatakan terjadi *port issue*. Kemudian pihak JKN di rumah sakit tersebut bercerita bahwa sebelumnya telah mencoba untuk meng-*update* ECLAIM-nya namun setelah *update* tersebut telah dilakukan, ia tidak bisa membuka *web* tersebut kembali. Setelah meminta yang bersangkutan untuk kemudian *restart* kembali komputernya lalu buka kembali. Ternyata sebelumnya juga telah meminta IT (dengan posisi di sini adalah mitra) untuk melakukan perbaikan atau *troubleshooting* terkait masalah tersebut, namun tampaknya tetap tidak berhasil. Sehingga masalah yang seharusnya kecil ini menjadi lebih besar dan susah untuk diketahui apa penyebabnya dikarenakan telah terjadi modifikasi untuk konfigurasi XAMPP-nya sendiri. 
## Blocking port

Yap, ini adalah *common issue*. Hal yang paling gampang adalah lakukan *restart*. Tidak perlu hubungi seorang IT, anda dapat melakukannya sendiri, yang dilakukan hanya butuh ***restart* komputer!**. *I know* cara lainnya adalah lakukan *mapping* untuk *port* baru atau *kill* *process* yang menggunakan *port* tersebut. Tapi ini butuh *expertise* dan anda tau apa yang anda lakukan.

> Di sinilah masalah kecil yang menjadi besar hanya karena modifikasi MySQL dan Apache *port* ke *value* lain dari yang semestinya, tapi anda tidak tau apa yang sebenarnya dilakukan, terlebih jika hanya berdasarkan informasi dari ChatGPT, *what a shame dawg*.

### Cek port

> Lakukan ini untuk memastikan Apache dan MySQL berada di *port* yang seharusnya.

Walaupun *port* dapat di ubah dengan menuju pada menu Config pada bagian kanan di XAMPP Control Panel lalu Service and Port Settings. Namun dari pengalaman saya, ini tidak sepenuhnya *valid*. Maka periksa langsung terhadap *file* yang berpengaruh.

#### Apache

> Mengetahui terjadi kesalahan konfigurasi *port* pada Apache di XAMPP, *browser* akan mengirimkan kembalian 5xx jika memaksa buka melalui `http://localhost/E-Klaim`.

> `http://localhost/E-Klaim`  adalah umumnya *endpoint* atau *url* untuk mengakses ECLAIM.

Umumnya saat instalasi ECLAIM dengan mengikuti "juknis"-nya, maka *path* untuk konfigurasi port Apache akan berada di:

```bash
C:/xampp/apache/conf/extra/apache-xampp.conf
```

Kemudian, fokuskan ke baris dengan keyword "Listen" atau seperti di bawah

```conf
#
# Listen: Allows you to bind Apache to specific IP addresses and/or
# ports, instead of the default. See also the <VirtualHost>
# directive.
#
# Change this to Listen on specific IP addresses as shown below to 
# prevent Apache from glomming onto all bound IP addresses.
#
#Listen 12.34.56.78:80
Listen 80
```

> Tanda `#` adalah sebuah *comment* sebagai deskripsi singkat terkait baris  tersebut, yang mana di sini adalah `Listen`

Jika baris tersebut masih tetap `Listen 80` maka tidak ada yang salah dengan konfigurasi Apache.

#### MySQL

*Default* konfigurasi MySQL terlebih untuk *port* akan seperti di bawah.

```conf
# C:/xampp/mysql/bin/my.ini

[client] 
port            = 3306 
socket          = "/xampp/mysql/mysql.sock"

[mysqld]
port= 3306
socket = "/xampp/mysql/mysql.sock"
basedir = "/xampp/mysql" 
```

Namun, coba lakukan pemeriksaan di bagian berikut. Ini berguna untuk mengetaui apakah telah terjadi perubahan lain.

```conf
# C:/xampp/mysql/bin/my.ini

[mysqld]
# datadir = "/xampp/mysql/data"
datadir = "c:/E-Klaim/data"

# innodb_data_home_dir = "/xampp/mysql/data"
innodb_data_home_dir = "/E-Klaim/data"
innodb_data_file_path = ibdata1:10M:autoextend
# innodb_log_group_home_dir = "/xampp/mysql/data"
innodb_log_group_home_dir = "/E-Klaim/data"
#innodb_log_arch_dir = "/xampp/mysql/data"
```

## Cek alias

```conf
# C:/xampp/apache/conf/extra/apache-xampp.conf

    Alias /E-Klaim "C:/E-Klaim"
    <Directory "C:/E-Klaim">
        AllowOverride AuthConfig
        Order allow,deny
        Allow from all
        Require all granted
    </Directory>
    
    AliasMatch (?i)^/e-klaim(.*) /E-Klaim$1
    AliasMatch (?i)^/eklaim(.*) /E-Klaim$1
    AliasMatch (?i)^/e-claim(.*) /E-Klaim$1
    AliasMatch (?i)^/eclaim(.*) /E-Klaim$1
```

## Cek Service XAMPP

Penggunaan *service* di XAMPP mungkin sah sah aja, akan tetapi ini akan terkendala untuk orang awam. Service akan membuat Apache dan MySQL akan dapat berjalan di *background* dan juga otomatis menyala setelah *restart* komputer tanpa perlu adanya interaksi kembali dengan XAMPP Control Panel. 

Di kasus ini, *user* melaporkan adanya pesan "Failed empty result" saat mencoba *grouping* INACBG. Setelah mencari informasi mulai dari 'juknis' sampai ke komunitas. Saya menemukan bahwa salah satu masalahnya ada di '*service*' ini. Jadi jika mengalami hal demikian, coba pastikan untuk tidak ada tanda centang hijau pada bagian kiri atau semuanya harus dalam kondisi tanda silang merah. Jika masih sama saja, baru lakukan cek terkait *timezone* dan *region* di Windows *settings*, yang mana seharusnya adalah "Indonesia".

> *Timezone* dan *region* *setting* ini ada di 'juknis', terkecuali masalah *service* tadi. Sebagai catatan juga untuk pihak yang bertanggung jawab dengan ini, untuk menulis dokumentasi selengkap-lengkapnya. 