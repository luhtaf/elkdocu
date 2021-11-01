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

Kita akan mengload dashboard otomatis pada kibana, yang kita lakukan adalah mengatur output dari elasticsearch dan kibana pada `filebeat.yml`

    output.elasticsearch:
        # Array of hosts to connect to.
        hosts: ["192.168.38.3:9200"]

        # Protocol - either `http` (default) or `https`.
        #protocol: "https"

        # Authentication credentials - either API key or username/password.
        #api_key: "id:api_key"
        username: "admin"
        password: "admin123"


    # Starting with Beats version 6.0.0, the dashboards are loaded via the Kibana API.
    # This requires a Kibana endpoint configuration.
    setup.kibana:

        # Kibana Host
        # Scheme and port can be left out and will be set to the default (http and 5601)
        # In case you specify and additional path, the scheme is required: http://localhost:5601/path
        # IPv6 addresses should always be defined as: https://[2001:db8::1]:5601
        host: "192.168.38.3:5601"

        # Kibana Space ID
        # ID of the Kibana Space into which the dashboards should be loaded. By default,
        # the Default Space will be used.
        #space.id:

Kemudian jalankan command berikut untuk load dashboardnya

    filebeat -setup

Setelah kita load dashboardnya, kemudian matikan output untuk kibana dan elasticsearch, kemudian lakukan konfigurasi enable pada:

    output.logstash:
        hosts: ["192.168.38.3:5044"]