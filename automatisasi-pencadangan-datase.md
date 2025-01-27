# Otomatisasi Pencadangan Database dengan Rsync, Cron dan Systemd
## Pendahuluan
Ada banyak *service* maupun aplikasi pihak ketiga mulai dari berbayar sampai gratis dan *open source*, untuk melakukan pencadangan *database* secara otomatis. Berkembangannya AI dengan sangat pesat disertai Language Model yang bervariasi juga dapat digunakan untuk membantu dalam menjelaskan atau memberikan tahapan-tahapan untuk melakukan hal tersebut. Namun disini saya akan membahas bagaimana melakukannya **versi saya sendiri**, yang mana menggunakan *command* atau *package* bawaan distribusi populer Linux, yaitu Rsync, Cron(Crontab), dan Systemd.

## Persyaratan
1. VPS/Remote Computer/Server yang bersifat *self manage* atau dapat melakukan SSH kedalamnya 
2. *Local computer* dengan OS Linux (distro Arch direkomendasikan)

## 
