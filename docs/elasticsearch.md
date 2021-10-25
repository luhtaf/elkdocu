# Elasticsearch
Pada pelatihan kali ini, akan digunakan elasticsearch versi 7.15.1 pada sistem operasi Ubuntu 18.04. Goalsnya adalah dapat mengakses elasticsearch melalui website host.

## Download dan Install
Untuk mendownload elasticsearch, ketikkan command:
    
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.15.1-amd64.deb

Setelah mendownload elasticsearch, dapat dilakukan command berikut untuk menginstall elasticsearch:

    sudo dpkg -i elasticsearch-7.15.1-amd64.deb

Kemudian, untuk membuat elasticsearch otomatis dijalankan meskipun server setelah direstart, gunakan command:

    sudo systemctl enable elasticsearch

Perintah berikut ini untuk mengecek status apakah elasticsearch berjalan atau tidak.
    
    sudo systemctl status elasticsearch

Dan jalankan perintah berikut untuk menjalankan service elasticsearch.

    sudo systemctl start elasticsearch

(Optional) Perintah berikut digunakan untuk menghentikan service elasticsearch.

    sudo systemctl stop elasticsearch

Setelah menjalankan service elasticsearch, kita akan memastikan kembali apakah elasticsearchnya berjalan dengan semestinya, maka ketikkan command berikut

    curl localhost:9200

Apabila command berhasil, maka akan memberikan output

    {
    "name" : "ubuntu",
    "cluster_name" : "elasticsearch",
    "cluster_uuid" : "z9mqP82QSuCrhQmhoyDcTw",
    "version" : {
        "number" : "7.15.1",
        "build_flavor" : "default",
        "build_type" : "deb",
        "build_hash" : "83c34f456ae29d60e94d886e455e6a3409bba9ed",
        "build_date" : "2021-10-07T21:56:19.031608185Z",
        "build_snapshot" : false,
        "lucene_version" : "8.9.0",
        "minimum_wire_compatibility_version" : "6.8.0",
        "minimum_index_compatibility_version" : "6.0.0-beta1"
    },
    "tagline" : "You Know, for Search"
    }



## Konfigurasi
Setelah mendownload dan menginstall elasticsearch, perlu dilakukan beberapa konfigurasi tambahan Untuk bisa mengakses elasticsearch, perlu dilakukan konfigurasi IP pada elasticsearch. Konfigurasi dilakukan dengan menambahkan command pada file `/etc/elasticsearch/elasticsearch.yml`:

    network.host: 0.0.0.0
    transport.host: 127.0.0.1
    transport.port: 9300
    http.port: 9200

(Optional) Apabila anda tidak dapat mengakses elasticsearch melalui hostnya setelah melakukan konfigurasi, mungkin ada rules dari firewall ubuntu yang menghalangi host untuk mengakses elasticsearch, kita dapat mengosongkan rules iptables dengan command dibawah.

    iptables -F

tapi INGAT, command ini hanya dilakukan di sini, terutama jika anda belum mendalami iptables. Dan jangan jalankan command diatas ketika anda menginstall di server yang sebenarnya karena command diatas akan mengosongkan `RULES` dari iptables pada OS anda