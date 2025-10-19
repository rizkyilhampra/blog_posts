---
id: 1760877811-masalah-di-mysql-pada-eclaim-inacbg-bpjs-dan-cara-mengatasinya
alias: Masalah di MySQL pada ECLAIM (INACBG) BPJS dan Cara Mengatasinya
tags: []
created: 2025-09-19
---
# Masalah di MySQL pada ECLAIM (INACBG) BPJS dan Cara Mengatasinya

Di sini adalah langkah-langkah yang saya lakukan untuk mengatasi masalah yang berkaitan dengan MySQL di ECLAIM yang mana basisnya menggunakan XAMPP yang *self hosted* pada komputer atau server sendiri.

> Yap, jika ada yang berencana untuk *targeting attack* ke suatu rumah sakit, ada cara yang bisa dilakukan, salah satunya cari di mana ECLAIM berada. Tidak sedikit dari rumah sakit yang juga melakukan *tunneling* untuk *expose* aplikasi ini ke internet (umumnya menggunakan *service* dari [ngrok](https://ngrok.io)). 
> 
> Ini adalah sebuah kritikan juga untuk pihak penyedia sistem atau aplikasi ini yang masih menggunakan XAMPP untuk *self hosted provider*. You are need to learn docker shit head! Fuck that about called "juknis" it's needs to be long and too techinical

Cerita sedikit mengenai latar belakang mengapa tulisan ini di buat. Ada suatu rumah sakit militer yang tidak mempunyai pegawai IT didalamnya, kemudian terkendala masalah ECLAIM-nya yang tidak bisa terbuka. Sebuah *chat* pertama kali masuk ke saya dari pihak JKN pada rumah sakit tersebut dengan sebuah gambar yang berisi peringatan atau *error* di log XAMPP yang mengatakan terjadi *port issue*. Kemudian pihak JKN di rumah sakit tersebut bercerita bahwa sebelumnya telah mencoba untuk meng-*update* ECLAIM-nya namun setelah *update* tersebut telah dilakukan, ia tidak bisa membuka *web* tersebut kembali. Setelah meminta yang bersangkutan untuk kemudian *restart* kembali komputernya lalu buka kembali. Ternyata sebelumnya juga telah meminta IT (dengan posisi di sini adalah mitra) untuk melakukan perbaikan atau *troubleshooting* terkait masalah tersebut, namun tampaknya tetap tidak berhasil. Sehingga masalah yang seharusnya kecil ini menjadi lebih besar dan susah untuk diketahui apa penyebabnya dikarenakan telah terjadi modifikasi untuk konfigurasi XAMPP-nya sendiri. 
## Blocking port

Yap, ini adalah *common issue*.
