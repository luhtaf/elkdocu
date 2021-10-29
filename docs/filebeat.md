# filebeat
Pada pelatihan kali ini, akan digunakan filebeat versi 7.15.1 pada sistem operasi Ubuntu 18.04. Goalsnya adalah dapat mengakses dashboard filebeat melalui website host.

## Linux
Untuk mendownload filebeat, ketikkan command:
    
    wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.15.1-amd64.deb

Setelah mendownload elasticsearch, dapat dilakukan command berikut untuk menginstall elasticsearch:

    sudo dpkg -i filebeat-7.15.1-amd64.deb

Kemudian, untuk membuat filebeat otomatis dijalankan meskipun server setelah direstart, gunakan command:

    sudo systemctl enable filebeat

Perintah berikut ini untuk mengecek status apakah filebeat berjalan atau tidak.
    
    sudo systemctl status filebeat

Dan jalankan perintah berikut untuk menjalankan service filebeat.

    sudo systemctl start filebeat

(Optional) Perintah berikut digunakan untuk menghentikan service filebeat.

    sudo systemctl stop filebeat


## Windows
Untuk Filebeat di windows bisa di download [di sini](https://www.elastic.co/downloads/beats/filebeat), lakukan instalasi seperti biasa


## Konfigurasi

Konfigurasi filebeat bisa ditemukan di folder instalasi filebeat, pada `filebeat.yml`