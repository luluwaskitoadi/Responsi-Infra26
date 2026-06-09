# Analisis Perbaikan

Nama: Lulu Waskito Adi
NIM: H1H024001

---

## Permasalahan 1: Indentasi dan Variabel pada docker-compose.yml

### Gejala
Saat awal menjalankan perintah `docker compose up -d`, muncul *error parsing* YAML atau *database* tidak mau terhubung.

### Penyebab
Terdapat kesalahan penulisan spasi (indentation) pada blok `services:`. Selain itu, terdapat kesalahan penulisan (typo) pada nama *environment variable* untuk database (seperti `DB_HOST` dan `DB_PASS`).

### Solusi
Memperbaiki indentasi dengan menambahkan spasi yang tepat agar struktur YAML menjadi valid, serta menyesuaikan nama variabel lingkungan agar cocok dengan kredensial database.

---

## Permasalahan 2: Typo Base Image pada Dockerfile

### Gejala
Proses *build image* gagal dan berhenti di tengah jalan dengan pesan *error* bahwa *image* tidak ditemukan.

### Penyebab
Terdapat *typo* penulisan *base image* pada file `Dockerfile` milik `web1` dan `web3`, yaitu tertulis `php:8.2-apach` (kurang huruf 'e').

### Solusi
Mengubah teks `php:8.2-apach` menjadi `php:8.2-apache` di kedua file `Dockerfile` agar sesuai dengan nama *image* resmi di Docker Hub.

---

## Permasalahan 3: Syntax Error pada Konfigurasi Nginx

### Gejala
Container `nginx-lb` langsung berstatus *Exited* (crash) sesaat setelah dijalankan. Pengecekan `docker logs nginx-lb` menunjukkan error `unknown directive "```nginx"`.

### Penyebab
Terdapat sisa simbol blok *markdown* (```) yang tidak sengaja terbawa pada baris paling atas dan paling bawah di dalam file `nginx/nginx.conf`.

### Solusi
Menghapus simbol *markdown* (```) dari file konfigurasi `nginx.conf`.

---

## Permasalahan 4: Kesalahan Upstream Load Balancer

### Gejala
Setelah *syntax error* Nginx diperbaiki, `nginx-lb` masih gagal menyala dengan pesan error `host not found in upstream "web11:80"`. Selain itu, Nginx juga mengarah ke port yang salah untuk web 3.

### Penyebab
Terdapat *typo* nama container `web11` (seharusnya `web1`) dan kesalahan port internal untuk web3 yang tertulis `8080` (seharusnya `80` karena secara *default* web server Apache di dalam container berjalan di port 80).

### Solusi
Mengubah `server web11:80;` menjadi `server web1:80;` dan `server web3:8080;` menjadi `server web3:80;` pada blok `upstream backend` di dalam `nginx/nginx.conf`.

---

## Permasalahan 5: Web 3 Tidak Terhubung ke Load Balancer

### Gejala
Saat dilakukan pengujian *load balancing* menggunakan `curl localhost:8080`, output yang merespons hanya bergantian dari WEB-1 dan WEB-2. WEB-3 tidak pernah muncul.

### Penyebab
Service `web3` pada `docker-compose.yml` hanya dihubungkan ke jaringan `backend`, sehingga tidak berada dalam satu jaringan dengan Nginx yang beroperasi di `frontend`. Nginx tidak dapat menjangkau `web3`.

### Solusi
Menambahkan network `- frontend` pada bagian `networks:` di konfigurasi `web3` dalam file `docker-compose.yml`, lalu melakukan *restart container*.