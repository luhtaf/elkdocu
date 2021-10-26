#Operasional Kibana

## Otentikasi pada kibana
Setelah kita enable otentikasi pada elasticsearch, kita harus menambahkan username password untuk elasticsearch pada konfigurasi kibana pada `/etc/kibana/kibana.yml`.

    elasticsearch.username: "username"
    elasticsearch.password: "password"
