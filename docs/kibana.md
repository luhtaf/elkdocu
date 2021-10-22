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
Setelah mendownload dan menginstall elasticsearch, perlu dilakukan beberapa konfigurasi tambahan Untuk bisa mengakses elasticsearch, perlu dilakukan konfigurasi IP pada elasticsearch. Konfigurasi dilakukan dengan menambahkan command pada file `/etc/elasticsearch/elasticsearch.yml`:

    network.host: 0.0.0.0
    transport.host: 127.0.0.1
    transport.port: 9300
    http.port: 9200

    #note: `0.0.0.0` dapat diganti dengan ip address dari sistem operasi ubuntu anda untuk menspesifikkan ip dari servis elasticsearch anda.

(Optional) Apabila anda tidak dapat mengakses elasticsearch melalui hostnya setelah melakukan konfigurasi, mungkin ada rules dari firewall ubuntu yang menghalangi host untuk mengakses elasticsearch, kita dapat mengosongkan rules iptables dengan command dibawah.

    iptables -F

tapi INGAT, command ini hanya dilakukan di sini, terutama jika anda belum mendalami iptables. Dan jangan jalankan command diatas ketika anda menginstall di server yang sebenarnya karena command diatas akan mengosongkan `RULES` dari iptables pada OS anda