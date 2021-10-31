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

* `windows`: C:/Program Files/Filebeat
* `ubuntu`: /etc/filebeat/

Konfigurasi dasar yang akan kita lakukan adalah menentukan output dari filebeat ini, yaitu pada logstash dan kibana seperti berikut:

        output.logstash:
            hosts: ["192.168.38.3:5044"]

        setup.kibana:

            # Kibana Host
            # Scheme and port can be left out and will be set to the default (http and 5601)
            # In case you specify and additional path, the scheme is required: http://localhost:5601/path
            # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
            host: "192.168.38.3:5601"