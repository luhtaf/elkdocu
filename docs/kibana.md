# Kibana
Pada pelatihan kali ini, akan digunakan kibana versi 7.15.1 pada sistem operasi Ubuntu 18.04. Goalsnya adalah dapat mengakses dashboard kibana melalui website host.

## Download dan Install
Untuk mendownload kibana, ketikkan command:
    
    wget https://artifacts.elastic.co/downloads/kibana/kibana-7.15.1-amd64.deb

Setelah mendownload elasticsearch, dapat dilakukan command berikut untuk menginstall elasticsearch:

    sudo dpkg -i kibana-7.15.1-amd64.deb

Kemudian, untuk membuat kibana otomatis dijalankan meskipun server setelah direstart, gunakan command:

    sudo systemctl enable kibana

Perintah berikut ini untuk mengecek status apakah kibana berjalan atau tidak.
    
    sudo systemctl status kibana

Dan jalankan perintah berikut untuk menjalankan service kibana.

    sudo systemctl start kibana

(Optional) Perintah berikut digunakan untuk menghentikan service kibana.

    sudo systemctl stop kibana


## Konfigurasi
Setelah mendownload dan menginstall kibana, perlu dilakukan beberapa konfigurasi tambahan Untuk bisa mengakses kibana melalui browser host. Konfigurasi dilakukan dengan mengubah command pada file `/etc/kibana/kibana.yml` dari

    #server.host: "localhost"

diubah menjadi

    server.host: "0.0.0.0"