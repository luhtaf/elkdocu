# Elasticsearch sebagai Secondary Database
Pada dasarnya elasticsearch didesain untuk menjadi sebuah searchengine. Elasticsearch ini memiliki karakteristik **cepat dalam melakukan query (Read Data)**, namun **lambat dalam perubahan data (Create, Update, dan Delete Data)**. Itulah yang menjadi alasan utama kenapa elasticsearch ini sebaiknya tidak digunakan pada database utama aplikasi-aplikasi yang membutuhkan perubahan data yang dinamis walaupun tools ini dapat menyimpan data layaknya database pada umumnya.

Oleh karena itu, biasanya elasticsearch ini dijadikan sebagai **secondary database**, maksudnya adalah semisal kita memiliki aplikasi yang menggunakan database mysql sebagai data utama. Ingat karakteristik elasticsearch? **cepat dalam melakukan query**, jadi kita dapat memanfaatkan karakteristik tersebut dengan menggunakan database elasticsearch untuk menampilkan data pada halaman-halaman yang memerlukan pencarian yang cepat, dan menggunakan mysql untuk menambah atau memodifikasi data. Pemanfaatan lainnya adalah ketika seorang data analis hendak melakukan analisis pada suatu data yang memerlukan query yang rumit pada suatu aplikasi yang menggunakan elasticsearch, skema yang sama juga dapat diterapkan.

Cara kerjanya adalah dengan ketika kita menyimpan data ke mysql, secara bersamaan data tersebut akan di sinkronkan (data pada mysql disalin) ke elasticsearch juga. Kekurangan dari pemanfaatan secondary database ini adalah kita memerlukan penyimpanan yang lebih besar (2x lipat) karena data disimpan di mysql dan elasticsearch secara bersamaan. 

Salah satu cara yang dapat digunakan untuk melakukan sinkronisasi tersebut adalah dengan menggunakan tools jdbc yang akan kita bahas.

## Instalasi JDBC
Untuk mendownload JDBC, kita bisa mendownloadnya dengan menggunakan command:
    
    wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-7.15.1-amd64.deb

Kemudian lakukan instalasi dengan menggunakan command:

    dpkg -i ...........


## JDBC untuk melakukan sinkronisasi mysql dengan elasticsearch
Bentuk database mysql yang merupakan sebuah Relational Database Management System (RDBMS) yang menyimpan data dengan relasi, atau simpelnya kita bisa sebut database yang menggunakan tabel. Sebaliknya, elasticsearch adalah sebuah NoSQL atau Non-RDBMS. Karena bentuk data yang berbeda, umumnya dilakukan sinkronisasi per tabel per 1 JDBC config, karena JDBC ini menggunakan query syntax mysql yang akan dieksekusi (Namun, kita dapat memanfaatkan metode-metode seperti join tabel untuk menggabungkan data terlebih dahulu sebelum disimpan di elasticsearch).

Pertama, sebelum melakukan konfigurasi jdbc kita cek dulu apakah kita bisa mengakses database mysql yang akan di sinkronkan dengan mengetik command:

    mysql -u {username_mysql} -h {ip_mysql} -p

* flag **-u** menunjukkan username mysql yang akan mengakses database
* flag **-h** menunjukkan hostname/ip mysql yang akan diakses
* flag **-p** menunjukkan bahwa kita akan mengakses mysql dengan menggunakan password. kemudian ketikkan password
* kemudian jika port mysql di custom (bukan 3306), kita bisa menambahkan flag **-P {port_mysql}**
<em>Apabila gagal login, kemungkinan username/password yang diberikan tidak valid, ip/port yang dimasukkan salah, atau ip pengakses belum di whitelist di database</em>

Setelah berhasil login, kita cek apakah database yang akan disinkronkan tersedia atau bisa diakses dengan mengetikkan command:

    show databases;

<em>apabila database tidak ada, maka kemungkinan database memang belum tersedia atau akun yang kita akses belum memiliki akses ke database tersebut</em>

Kemudian apabila database ditemukan, kita cek apakah tabel yang akan di sinkronkan tersedia atau tidak dengan command:

    show tables;

<em>apabila tabel tidak ada, maka kemungkinan tabel memang belum tersedia atau akun yang kita gunakan belum memiliki akses ke tabel pada database tersebut</em>

Setelah ditemukan tabel, kita dapat menggunakan command:

    select * from {tabel};

Nah, setelah itu kita perlu mengetahui struktur tabel dengan mengetik command:

    describe {table};

Struktur tabel ini diperlukan untuk mengetahui track column, track column ini yang menjadi kunci yang memberitahukan apabila ada perubahan data pada suatu row (jika track column lebih baru, maka row tersebut akan diperbarui), biasanya track column itu adalah timestamp.

Ketika semua tahap persiapan sebelumnya sudah dilakukan, kita dapat melakukan konfigurasi input jdbc mysql, berikut adalah commandnya:

    input {
        jdbc {
            jdbc_driver_library => "/usr/share/java/mysql-connector-java.jar"
            jdbc_driver_class => "com.mysql.jdbc.Driver"
            jdbc_connection_string => "jdbc:mysql://{ip_database}:{port_database}/{database}?useUnicode=true&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=Asia/Jakarta"
            jdbc_user => {username_mysql}
            jdbc_password => {password}
            clean_run => true
            tracking_column => "unix_ts_in_secs"
            use_column_value => true
            tracking_column_type => "numeric"
            schedule => "*/5 * * * * *"
            statement => "SELECT *, UNIX_TIMESTAMP({track_column}) AS unix_ts_in_secs FROM audit_trail WHERE (UNIX_TIMESTAMP({track_column}) > :sql_last_value AND {track_column} < NOW()) ORDER BY {track_column} ASC"
            record_last_run => true
            last_run_metadata_path => "/tmp/.logstash_jdbc_last_run"
            tags=> ["jdbc",{tag_tambahan}]
        }
    }

* {ip_database} adalah alamat ip mysql yang akan diakses
* {port_database} adalah port mysql yang akan diakses
* {database} adalah nama database yang akan disinkronkan
* {username_mysql} adalah username yang akan digunakan untuk mengakses database
* {password_mysql} adalah password yang akan digunakan untuk mengakses database
* {track_column} adalah kolom yang digunakan untuk memberitahukan apabila ada perubahan pada data yang biasanya adalah timestamp

Konfigurasi diatas dapat disimpan menjadi 1 file sehingga ada beberapa konfigurasi 1 jdbc dalam 1 konfigurasi input ataupun 1 input menjadi 1 file sendiri tergantung preferensi, dan konfigurasi diatas dijalankan setiap 5 menit (apabila ingin mengatur menjalankan command selain setiap 5 menit, line <b>"\*/5 \* \* \* \* \*"</b> dapat diganti dengan menggunakan konfigurasi cronjob)

kemudian setelah selesai melakukan konfigurasi input, dapat ditambhkan konfigurasi berikut didalam 1 file yang sama maupun file terpisah.

    filter{
        # Konfigurasi Filter tertentu dengan memberi if pada tag jdbc
        if "jdbc" in [tags]{
            #konfigurasi filter tertentu ada di sini
        }
    }

    output {
        if "jdbc" in [tags]{
            elasticsearch {
                hosts => [{alamat_elasticsearch}]
                ssl_certificate_verification => false
                user => {username_elastic}
                password => {password_elastic}
                index => "log-db_audit_trail-%{+yyyy.MM.dd}"
                action => "index"
            }
        }
    }

Pastikan semua konfigurasi logstash disimpan pada `/etc/logstash/conf.d/` dan semua konfigurasi disimpan dengan ekstensi `.conf`