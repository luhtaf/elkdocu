# Operasional Elasticsearch

## Konfigurasi Xpack
Selanjutnya, akan di enable bagian security dari elasticsearch agar diperlukan otentikasi setiap kali mengakses elasticsearch, dengan menambahkan pada file `/etc/elasticsearch/elasticsearch.yml`

    xpack.security.enabled: true
    xpack.security.authc.api_key.enabled: true

Setelah melakukan enable pada security elasticsearch, kemudian akan dibuat user untuk bisa login ke elasticsearch. Ada 2 cara untuk pembuatan user

1. Menggunakan elasticsearch-setup-passwords

    Cara pertama dapat dilakukan dengan mengetikkan command dibawah, kemudian berikan password untuk masing-masing user.

        /usr/share/elasticsearch/bin/elasticsearch-setup-passwords interactive
    
    Cara ini akan menggenerate semua user untuk masing-masing role yang tersedia

2. Menggunakan elasticsearch-users

    Cara kedua dilakukan dengan menambah manual user yang akan ditambahkan, misalnya dengan command dibawah kita akan menambahkan user dengan username `admin`. Setelahnya sistem akan meminta password yang diinginkan.

        /usr/share/elasticsearch/bin/elasticsearch-users useradd admin

    Setelah itu kita akan memberikan roles `superuser` pada user `admin`, dengan mengetikkan command

        /usr/share/elasticsearch/bin/elasticsearch-users roles admin -a superuser

    Untuk membuat lebih dari 1 user dengan role berbeda-beda, kita dapat membuat banyak user dengan command `useradd` seperti sebelumnya, kemudian assign role pada command `role` seperti pada command diatas. Role yang tersedia pada elasticsearch sesuai dengan daftar dibawah

    | Role Yang Tersedia | | |
    | --- | --- | --- |
    | kibana_dashboard_only_user | apm_system | watcher_admin |
    | viewer | logstash_system | rollup_user |
    | kibana_user | beats_admin | remote_monitoring_agent |
    | rollup_admin | data_frame_transforms_admin | snapshot_user |
    | monitoring_user | enrich_user | kibana_admin |
    | logstash_admin | editor | machine_learning_user |
    | data_frame_transforms_user | machine_learning_admin | watcher_user |
    | apm_user | beats_system | reporting_user |
    | transform_user | kibana_system | transform_admin |
    | transport_client | remote_monitoring_collector | superuser |
    | ingest_admin | | |