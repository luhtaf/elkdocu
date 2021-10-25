# Logstash
Pada pelatihan kali ini, akan digunakan logstash versi 7.15.1 pada sistem operasi Ubuntu 18.04. Goalsnya adalah dapat mengecek service logstash berjalan.

## Download dan Install
Untuk mendownload logstash, ketikkan command:
    
    wget https://artifacts.elastic.co/downloads/logstash/logstash-7.15.1-amd64.deb

Setelah mendownload logstash, dapat dilakukan command berikut untuk menginstall logstash:

    sudo dpkg -i logstash-7.15.1-amd64.deb

Kemudian, untuk membuat logstash otomatis dijalankan meskipun server setelah direstart, gunakan command:

    sudo systemctl enable logstash


Dan jalankan perintah berikut untuk menjalankan service logstash.

    sudo systemctl start logstash

Perintah berikut ini untuk mengecek status apakah logstash berjalan atau tidak.
    
    sudo systemctl status logstash

(Optional) Perintah berikut digunakan untuk menghentikan service logstash.

    sudo systemctl stop logstash


## Konfigurasi
Konfigurasi logstash akan dijelaskan pada bagian [operasi](./conf_logstash)