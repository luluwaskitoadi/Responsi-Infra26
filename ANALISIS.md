# Analisis Responsi Infrastruktur
Nama: Lulu Waskito Adi
NIM: H1H024001

Rangkuman Troubleshooting:
1. Menambahkan spasi (indentation) yang hilang pada baris `services:` di file `docker-compose.yml`.
2. Memperbaiki typo *environment variables* (DB_HOST, DB_PASS) dan nama `volumes` di `docker-compose.yml`.
3. Memperbaiki typo base image menjadi `php:8.2-apache` di file Dockerfile web1 dan web3.
4. Menghapus sisa tag markdown (```) yang menyebabkan error di `nginx/nginx.conf`.
5. Memperbaiki typo nama server dari `web11` menjadi `web1` dan menyesuaikan port internal menjadi port 80 di `nginx/nginx.conf`.
6. Menambahkan network `frontend` pada service `web3` di `docker-compose.yml` agar dapat dijangkau oleh load balancer Nginx.
