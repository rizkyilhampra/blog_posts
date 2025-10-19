---
id: 1760877811-masalah-di-mysql-pada-eclaim-inacbg-bpjs-dan-cara-mengatasinya
alias: Masalah di MySQL pada ECLAIM (INACBG) BPJS dan Cara Mengatasinya
tags: []
created: 2025-09-19
---
# Masalah di MySQL pada ECLAIM (INACBG) BPJS dan Cara Mengatasinya

Di sini adalah langkah-langkah yang saya lakukan untuk mengatasi masalah yang berkaitan dengan MySQL di ECLAIM yang mana basisnya menggunakan XAMPP yang di *self hosted* pada komputer atau server sendiri.

Cerita sedikit mengenai latar belakang mengapa tulisan ini di buat. Ada suatu rumah sakit militer yang tidak mempunyai pegawai IT didalamnya, kemudian terkendala masalah ECLAIM-nya yang tidak bisa terbuka. Sebuah *chat* pertama kali masuk adalah sebuah gambar yang berisi peringatan atau *error* di log XAMPP yang mengatakan terjadi *port issue*, kemudian pihak JKN di rumah sakit tersebut bercerita bahwa telah mencoba untuk meng-*update* Setelah meminta yang bersangkutan untuk kemudian *restart* kembali komputernya lalu buka kembali. Ternyata yang dilakukan adalah meminta IT (mitra) untuk melakukannya, sehingga masalah yang seharusnya kecil menjadi lebih besar dan susah untuk diketahui apa penyebabnya. 
## Blocking port

