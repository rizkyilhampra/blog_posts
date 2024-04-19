---
author: rip
categories:
  - engineering
date: 2024-04-20 06:44:58 +0800
tag:
  - vps
  - linux
title: Memulai dengan VPS
---

VPS atau ***Virtual Private Server*** adalah mesin virtual atau server yang terpisah dari server atau mesin utama. Simplenya ketika mesin utama yang memiliki *resource* yang besar akan dipecah menjadi beberapa server lagi tapi bukan berupa mesin utuh melainkan berupa *virtual*. Penggunaan VPS biasanya digunakan untuk kebutuhan *hosting* web atau aplikasi. Beberapa penyedia hosting umumnya menyediakan layanan VPS ini, yang mana kelebihannya daripada *shared hosting* pada umumnya adalah kita sebagai user mendapatkan akses sebagai root atau bertindak sebagai root, sehingga kebutuhan untuk mengkonfigurasi server tersebut bisa dilakukan dengan lebih leluasa misalkan untuk konfigurasi *firewall*, install aplikasi/program di dalam server, dan lain sebagainya. Sedangkan di *shared hosting* umumnya terbatas, kita hanya mendapatkan akses untuk sekadar *hosting* web/aplikasi saja dengan cara mengupload folder aplikasi ke dalam server. Walaupun di beberapa *shared hosting* tetap menyediakan SSH, tetapi tetap saja penggunaannya terbatas karena tidak mempunyai kapabilitas sebagai root.

Disini saya akan menjelaskan bagaimana umumnya saya melakukan konfigurasi server atau VPS tadi dengan command line. Sampai tulisan ini dibuat, saya menggunakan VPS hanya untuk kebutuhan hosting aplikasi web saja. Tapi seharusnya disini saya akan bahas secara umum bagaimana setup VPS, setidaknya sampai siap untuk digunakan. 

Pertama-tama kita harus membeli VPS itu sendiri, ada banyak penyedia VPS. Saya jabarkan di bawah beberapa yang saya ketahui, mulai yang beroperasi di indonesia dan di luar.
- https://rumahweb.com
- https://biznetgio.com
- https://hostinger.com
- https://niagahoster.com
- https://digitalocean.com

Bagaimana cara membelinya? umumnnya penyedia hosting ini terdapat beberapa layanan. Disini kalian bisa fokus mencari bagian vps nya saja. Sampai disana biasanya kita diminta untuk mengatur resource server yang akan kita gunakan, atau beberapa pihak lainnya menyediakan pricelist berdasarkan resourcenya saja. Disini sesuaikan saja dengan kebutuhan, jika baru pertama kali dan mau coba-coba akan lebih menggunakan yang paling murah saja. Umumnya yang paling murah rentang 50 - 60 ribu rupiah atau bisa lebih murah tergantung penyedia hosting dan promo apa yang sedang berlaku. 

Setelah memutuskan membeli atau memilih resource yang akan digunakan, biasanya kita diminta untuk memilih menggunakan distro linux yang akan digunakan untuk servernya nanti. Umumnya penyedia hosting akan memberikan beberapa opsi distro linux saja, di penyedia hosting yang lebih besar biasanya baru memberikan pilihan sistem operasi seperti windows server, dan linux server. Untuk yang baru memulai akan lebih baik menggunakan distro linux Ubuntu. Mengapa menggunakan Ubuntu dibanding dengan yang lain? Karena salah satu distro dari based debian linux yang paling besar dari sisi komunitas dan penggunanya. Sehingga jika terjadi masalah akan lebih mudah mencari informasinya di internet.

Pastikan juga saat memilih distro Ubuntu untuk tetap menggunakan versi LTS-nya, umumnya diawali dengan angka genap didepannya. Mengapa menggunakan versi LTS? Dari sepengalaman saya ketika menggunakan versi terbaru akan menyebabkan masalah dimana saat  menambahkan repository diluar dari ubuntu, akan menyebabkan repository tersebut tidak ditemukan, tapi saya tidak tau apakah ini *common issue* atau tidak.

Setelah memilih distro linux, seharusnya akan terdapat pilihan untuk setup ssh key. 

> SSH adalah cara berkomunikasi antar komputer, simplenya ketika kita ingin mengakses komputer orang lain melalui komputer kita sekarang.

Untuk setup ssh biasanya provider hosting berbeda beda, ada yang meminta kita mengisi public key saja, atau mereka memberikan opsi untuk membuat private/public key. Disini biasanya saya akan membuat ssh key di komputer pribadi dan menambahkan public key ke dalam form isian yang provider hosting minta.
## Setup SSH
> Disini saya menggunakan linux, jika anda yang membaca ini menggunakan windows akan ada beberapa tahapan yang harus dilalui sebelum bisa menjalankan perintah di bawah, silahkan googling untuk sisanya.

Buka terminal, lalu ketikkan perintah di bawah.

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

> 1. `ssh-keygen`: command untuk menghasilkan ssh key pairing, public key dan private key.
> 2. `-t ed25519`: opsi untuk menggunakan algoritma `ed25519`. Algoritma ini direkomendasikan oleh [Github](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) untuk pairing dengan ssh. Algoritma lain ada `rsa`, `dsa`. Defaultnya akan menggunakan `rsa`. Perbedaan algoritma ini hanyalah terkait keamanan.
> 3. `-C "your_email@example.com`: menambahkan label atau penanda pada ssh key.

Setelah itu akan terdapat pesan untuk menaruh file ssh key dimana, defaultnya seharusnya di `~/.ssh/`, silahkan enter saja jika tidak tau mau taruh dimana.

Setelah menerima prompt tadi, selanjutnya kita akan diminta untuk mengisi *passphrase*. *passphrase* ini kurang lebih bisa diartikan sebagai password. Silahkan untuk mengisi atau biarkan kosong.

Maka akan ada dua file yang akan dibuat otomatis ke dalam folder/directory `~/.ssh`. private key dan public key. Pergi directory tersebut untuk melihat.

```bash
cd ~/.ssh
```

Jika baru pertama kali membuat ssh key maka disana akan terdapat dua file. Silahkan lihat dengan perintah berikut.

```bash
ls -a
```

Public key atau file yang dibelakang namanya terdapat `.pub`. Public key ini yang akan dimasukkan dimasukkan ke form isian yang diminta oleh provider hosting tadi. Lalu private key sebagai penghubung dengan public key tadi. Jika orang lain hanya melihat public key tapi tidak mengetahui private key, maka koneksi ssh tersebut akan gagal, begitupun sebaliknya. 

Untuk melihat isi file dari public key, jalankan perintah berikut.

```bash
cat id_ed25519.pub
```

Setelah itu arahkan mouse untuk menyeleksi isi dari file tersebut untuk di copy, lalu paste di form isian yang provider hosting minta. 
## Masuk ke Komputer Server / VPS dengan SSH
Setelah setup ssh tadi, saatnya kita akan masuk ke komputer server dengan ssh.

```bash
ssh -i ~/.ssh/nama_file_private_key username_server@ip_address_server
```

> 1. `ssh`: program untuk menjalankan remote akses ke server yang kita tuju.
> 2. `-i ~/.ssh/nama_file_private_key`: Mengidentifikasi private key yang akan kita gunakan.

Perhatikan `username_server` dan `ip_address_server`, sesuaikan dengan informasi dari VPS yang anda telah beli. Biasanya akan ada informasi IP address static dan username. Jika tidak ada keterangan username, maka seharusnya root adalah default, jadi bisa ganti dengan `root@ip_address_server`.

Setelah itu akan diminta mengisi passphrase, isikan *passphrase* sesuai *passhprase* yang dibuat sebelumnya. Jika sebelumnya mengosongkan *passphrase* maka kosongkan juga bagian ini.

> Jika port ssh tidak `22`. Tambahkan ini di akhir perintah di atas `-p nomor_port`

## Bertindak dengan Root dengan User Baru
Jika user yang diberikan oleh penyedia hosting adalah root, maka paling baik untuk membuat user baru. Jika user dengan username tersebut bukan root maka silahkan skip langkah ini. 
### Buat User Baru Sebagai Pengganti Root
Mengapa tidak menggunakan user dengan username root saja? Disamping adalah common practice, penggunaan root user adalah bahaya terkait keamanan. Meskipun sudah setup ssh key dan mengisi *passphrase* untuk menjaga keamanan. Penggunaan user dengan username root sebaiknya dihindari, mengapa? Mungkin nantinya kita lupa menginstall beberapa aplikasi di dalam komputer server kita, lalu menjalankan aplikasinya dalam mode root atau simplenya memberikan akses penuh aplikasi tersebut berjalan, kemungkinannya aplikasi tersebut akan mengeksploitasi sistem kita. Walaupun kasusnya jarang, tetap saja hal ini sama sekali tidak direkomendasikan.

Jalankan perintah di bawah di terminal anda untuk membuat user baru.

```bash
adduser user
```

> Ubah `user` dengan nama user sesuai preferensi anda.

Selanjutnya memberikan akses user yang baru kita buat dengan akses root. Mengapa ini dibutuhkan? karena 
Untuk menggantikan root maka kita perlu membuat user yang juga bertindak atau memiliki akses sebagai root. Ini lebih baik dibanding secara eksplisit menggunakan root.

```bash
usermod -aG sudo user
```

> 1. `usermod`: command untuk memodifikasi akun user.
> 2. `-aG sudo`: Opsi untuk menambahkan akun user sekarang ke dalam group sudo.

### Sync Authorized Keys dengan User Baru
 Melakukan sync authorized key berfungsi agar hubungan ssh dari/ke komputer lokal kita yang sebelumnya menggunakan root juga bisa diakses melalui user yang baru.

```bash
rsync --archive --chown=user:user ~/.ssh /home/user
```

> 1. `rsync`: command untuk sinkronisasi file/directory antara dua lokasi.
> 2. `--archive`: opsi untuk menjalankan `rsync` dalam mode *archive*, mode ini akan memastikan metadata file/directory juga tersalin.
> 3. `--chown=user:user`: Opsi untuk memastikan ownership dari folder/file yang akan kita saling adalah user yang didefinisikan (disini adalah `user`).  

### Login Kembali dengan User Baru
Jalankan perintah di bawah untuk keluar.

```bash
exit
```

Kemudian disini kita menggunakan file private key ssh yang sama seperti di awal. Perbedannya adalah nama user-nya saja. Jalankan perintah di bawah untuk login kembali dengan user yang definisikan di [sini](#buat-user-baru-sebagai-pengganti-root)

```bash
ssh -i ~/.ssh/nama_file_private_key user@ip_address_server
```
## Buat alias SSH (Optional)
Jika merasa menulis baris kode 
`ssh -i ~/.ssh/nama_file_private_key user@ip_address_server` terlalu panjang. Kita bisa membuat alias untuk menyingkatnya.

> Perhatian: Langkah-langkah di bawah dijalankan di komputer lokal/bukan di komputer server.

Untuk membuat alias, kita buat dulu file `config` di dalam directory `.ssh` 
```bash
cd ~/.ssh
touch config
```

Kemudian buka text editor, lalu isi sesuai informasi server yang digunakan.

```bash
vim config
```

Kemudian ketik `i` untuk memulai memasukkan data, lalu isi seperti berikut. Jika telah selesai, ketik `Esc` lalu `:wq` untuk menyimpan perubahan.

```
Host nama_alias 
    HostName ip_address_server/domain
    User user
    Port 22
    IdentityFile ~/.ssh/nama_file_private_key
```

Setelah itu kita dapat login hanya dengan perintah berikut

```bash
ssh nama_alias
```
## Setup Firewall (Optional)
Setup firewall akan membuat kita kerja dua kali ketika *service* yang nanti akan kita gunakan meminta terhubung dengan koneksi luar. Tapi dengan kelebihan lebih aman dan kita tau apa saja *service* yang boleh dan yang tidak. Tapi saya biasanya skip bagian ini.

Untuk setup firewall ada beberapa *tool/service/program* yang bisa digunakan salah satunya adalah UFW. Pastikan tidak menggunakan *firewall service* lebih dari 1 jika tidak berpengalaman terkait ini.

Untuk melihat *profile* atau *service* apa saja yang tersedia untuk kita batasi dengan *firewall*. Jalankan perintah berikut.

```bash
sudo ufw app list
```

Untuk memastikan saat mengaktifkan *firewall*, koneksi SSH kita sekarang tidak terputus. Jalankan perintah berikut untuk mengijinkan SSH terlebih dahulu.

```bash
sudo ufw allow origin
```

Selanjutnya untuk mengaktifkan *firewall*, jalankan perintah berikut

```bash
sudo ufw enable
```

Untuk melihat list *service/profile* yang dibatasi, jalankan perintah berikut

```bash
sudo ufw status
```

Silahkan *googling* untuk penggunaannya lebih lanjut.
## ***Update Packages/Dependenies***

```bash
sudo apt update
```

Disini kita bisa menggunakan `sudo` untuk memberikan suatu *command* berjalan dalam mode root, karena disini akan mengupdate *packages/dependencies* di linux kita, maka mode root dibutuhkan. *Command* ini akan meminta sistem memeriksa repositori dari paket yang terdaftar di sistem dan mengambil informasi terbaru tentang *package-package* yang tersedia. 
## ***Upgrade Packages/Dependenies*** (Optional)
Bisa menjadi bahasan diskusi, apakah harus *upgrade packages/dependencies* di VPS? Jika mengikut dari pengalaman saya, maka saya akan melakukannya. Alasannya karena perintah *upgrade* ini akan meng-*upgrade* kernel linux Ubuntu itu sendiri, maka menurut saya ini penting dilakukan, disamping untuk memastikan *package-package* dalam kondisi *up to date*, ini juga membuat dari sisi keamanan dan kestabilan sistem yang digunakan juga terjaga. 

Untuk meng-*upgrade* jalankan perintah berikut.

```bash
sudo apt upgrade
```

Atau sekaligus dengan `sudo apt update` dan *flag* `-y` untuk memaksa *upgrade* tanpa peringatan atau *prompt*.

```bash
sudo apt update && sudo apt upgrade -y
```

