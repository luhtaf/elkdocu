# Konfigurasi Log 
Setelah kita memberikan konfigurasi dasar pada filebeat dan logstash, kemudian kita akan memberikan konfigurasi tergantung pada log yang akan kita konfigurasi


## Konfigurasi Suricata
Pertama enable module Suricata pada filebeat dengan command

    filebeat modules enable suricata

pada filebeat di module `suricata.yml` dapat diisi dengan command

    # Module: suricata
    # Docs: https://www.elastic.co/guide/en/beats/filebeat/7.x/filebeat-module-suricata.html

    - module: suricata
    # All logs
    eve:
        enabled: true

        # Set custom paths for the log files. If left empty,
        # Filebeat will choose the paths depending on your OS.
        var.paths: ["/home/ubuntu/eve.json"]
        input:
            fields_under_root: false
            fields.app.type: "Suricata"
            fields.site: 1

kemudian di pipeline logstash pada file `beats.conf` kita tidak perlu mengatur banyak hal untuk suricata karena sudah diberikan banyak konfigurasi dasar pada modul suricata nya. jadi kita hanya perlu mengatur konfigurasi `output`nya seperti berikut:

    output
    {
        if[fields][app][type] == "Suricata"
        {
            elasticsearch
                {
                    hosts => ["http://192.168.38.3:9200"]
                    index => "suricata-%{[fields][site]}-%{+yyyy.MM.dd}"
                    user => "admin"
                    password => "admin123"
                }
            }
    }

Setelah itu restart logstash dan filebeat, kemudian cek data yang masuk



## Konfigurasi Apache
Pertama enable module Apache pada filebeat dengan command

    filebeat modules enable apache

pada filebeat di module `apache.yml` dapat diisi dengan command

    Module: apache
    # Docs: https://www.elastic.co/guide/en/beats/filebeat/7.x/filebeat-module-apache.html

    - module: apache
    # Access logs
    access:
        enabled: true

        # Set custom paths for the log files. If left empty,
        # Filebeat will choose the paths depending on your OS.
        var.paths: ["/home/ubuntu/access.log"]
        input:
            fields_under_root: false
            fields.app.type: "Apache"
            fields.site: 2

    # Error logs
    error:
        enabled: false

        # Set custom paths for the log files. If left empty,
        # Filebeat will choose the paths depending on your OS.
        #var.paths:


kemudian di pipeline logstash pada file `beats.conf` kita tidak perlu mengatur banyak hal untuk apache karena sudah diberikan banyak konfigurasi dasar pada modul apache nya. jadi kita hanya perlu mengatur konfigurasi `output`nya seperti berikut:

    output
    {
        if[fields][app][type] == "Apache"
        {
            elasticsearch
                {
                    hosts => ["http://192.168.38.3:9200"]
                    index => "apache-%{[fields][site]}-%{+yyyy.MM.dd}"
                    user => "admin"
                    password => "admin123"
                }
            }
    }

Setelah itu restart logstash dan filebeat, kemudian cek data yang masuk


## Konfigurasi Syslog dan Auth Log
Pertama enable module System pada filebeat dengan command

    filebeat modules enable system

pada filebeat di module `system.yml` dapat diisi dengan command

    # Module: system
    # Docs: https://www.elastic.co/guide/en/beats/filebeat/7.x/filebeat-module-system.html

    - module: system
    # Syslog
    syslog:
        enabled: true

        # Set custom paths for the log files. If left empty,
        # Filebeat will choose the paths depending on your OS.
        var.paths: ["/home/ubuntu/syslog"]
        input:
            fields_under_root: false
            fields.app.type: "System"

    # Authorization logs
    auth:
        enabled: true

        # Set custom paths for the log files. If left empty,
        # Filebeat will choose the paths depending on your OS.
        var.paths: ["/home/ubuntu/auth.log"]
        input:
        fields_under_root: false
        fields.app.type: "System"



kemudian di pipeline logstash pada file `beats.conf` kita tidak perlu mengatur banyak hal untuk log system karena sudah diberikan banyak konfigurasi dasar pada modul system nya. jadi kita hanya perlu mengatur konfigurasi `output`nya seperti berikut:

    output
    {
        if[fields][app][type] == "System"
        {
            elasticsearch
                {
                    hosts => ["http://192.168.38.3:9200"]
                    index => "syslog-%{+yyyy.MM.dd}"
                    user => "admin"
                    password => "admin123"
                }
            }
    }

Setelah itu restart logstash dan filebeat, kemudian cek data yang masuk




## Konfigurasi untuk log mgnx
Pada log ini, tidak perlu ada module yang di setting,

kemudian, kita harus mengubah beberapa file konfigurasi pada `filebeat.yml` seperti:

    filebeat.inputs:

    # Each - is an input. Most options can be set at the input level, so
    # you can use different inputs for various configurations.
    # Below are the input specific configurations.

    - type: log

    # Change to true to enable this input configuration.
    enabled: true
    

    # Paths that should be crawled and fetched. Glob based paths.
    paths:
        - /home/ubuntu/mgnx.json

    fields:
        app:
            type: "mgnx"
        site: 8




kemudian di pipeline logstash pada file `beats.conf` kita tidak perlu mengatur waktu pada filternya seperti berikut:

    filter
    {
        if[fields][app][type] == "mgnx"
        {
            date
            {
                match => ["Recent Occurrence", "yyyy-MM-dd HH:mm:ss"]
            }
        }
    }




kemudian untuk `output` kita perlu mengatur konfigurasi `output`nya seperti berikut:

    output
    {
        if[fields][app][type] == "mgnx"
        {
            elasticsearch
                {
                    hosts => ["http://192.168.38.3:9200"]
                    index => "mgnx-%{[fields][site]}-%{+yyyy.MM.dd}"
                    user => "admin"
                    password => "admin123"
                }
            }
    }

Setelah itu restart logstash dan filebeat, kemudian cek data yang masuk