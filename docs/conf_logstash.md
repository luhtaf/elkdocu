# Operasional Logstash

## Mengatur Filter Untuk Beats
Pertama-tama dalam konfigurasi logstash cek file `/etc/logstash/pipelines.yml`

    - pipeline.id: main
      path.config: "/etc/logstash/conf.d/*.conf"

Konfigurasi di atas menunjukkan bahwa logstash akan membaca semua file yang berada di folder `/etc/logstash/conf.d/` dengan ekstensi `.conf` yang dikenal oleh logstash dengan id pipeline `main`. kita bisa menspesifikkan file konfigurasi yang akan dibaca oleh logstash dengan mengubah `*.conf` menjadi file spesifik.

kemudian, kita akan melakukan konfigurasi `input` `filter` dan `output` untuk filebeats di logstash.

pada `input` akan diberikan konfigurasi untuk membuka port 5044, yang menandakan pada konfigurasi ini hanya berlaku pada data yang melalui port 5044, dengan command:

    input
    {
        beats
        {
            port =>5044
        }
    }

pada `filter` kita bisa melakukan berbagai macam hal, pastikan semua konfigurasi yang berkaitan dengan `filter` dituliskan seperti konfigurasi berikut:

    filter
    {
        #konfigurasi filter disini
    }


seperti menspesifikkan data yang akan diambil sebagai sumber utama:

        json
        {
            source => "message"
        }

_menggenerate_ hash berdasarkan field tertentu dari sumber log:

        fingerprint {
            target => "generated_id"
            method => "SHA1"
            source => "message"
            concatenate_sources => "true"	
        }

mengatur timestamp yang akan digunakan:

        date {
            match => ["timestamp", "yyyy-MM-dd HH:mm:ss"]
        }

kita juga bisa memetakan asal negara, kota, bahkan instansi dengan menggunakan database yang sudah disediakan oleh elasticsearch:

        geoip {
            source => "[src_ip]"
            target => "SourceIP_Org"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-ASN.mmdb"
        }
        geoip {
            source => "[src_ip]"
            target => "SourceIP_Country"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-Country.mmdb"
            }
        geoip {
            source => "[src_ip]"
            target => "SourceIP_City"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-City.mmdb"
            }
        geoip {
            source => "[dest_ip]"
            target => "DestIP_Org"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-ASN.mmdb"
        }
        geoip {
            source => "[dest_ip]"
            target => "DestIP_Country"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-Country.mmdb"
            }
        geoip {
            source => "[dest_ip]"
            target => "DestIP_City"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-City.mmdb"
            }

Pada bagian `output`, kita bisa memetakan output index sesuai dari source log yang ada:

        output 
        {
            if[fields][source] == "apache"
            {
                elasticsearch 
                {
                    hosts => ["localhost:9200"]
                    document_id => "%{[generated_id]}"
                    index => "apache-%{+yyyy.MM.dd}"
                }
            }

            if[fields][site] == "suricata"
            {
                elasticsearch 
                {
                    hosts => ["localhost:9200"]
                    document_id => "%{[generated_id]}"
                    index => "suricata-%{+yyyy.MM.dd}"
                }
            }

        }

Bagian Konfigurasi full, dapat dilihat sebagai berikut:

    input {
    beats {
    port => 5044
    }
    }

    filter {
        json
        {
            source => "message"
        }
        fingerprint {
            target => "generated_id"
            method => "SHA1"
            source => "message"
            concatenate_sources => "true"	
        }
        date {
            match => ["timestamp", "yyyy-MM-dd HH:mm:ss"]
        }
        geoip {
            source => "[src_ip]"
            target => "SourceIP_Org"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-ASN.mmdb"
        }
        geoip {
            source => "[src_ip]"
            target => "SourceIP_Country"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-Country.mmdb"
            }
        geoip {
            source => "[src_ip]"
            target => "SourceIP_City"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-City.mmdb"
            }
        geoip {
            source => "[dest_ip]"
            target => "DestIP_Org"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-ASN.mmdb"
        }
        geoip {
            source => "[dest_ip]"
            target => "DestIP_Country"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-Country.mmdb"
            }
        geoip {
            source => "[dest_ip]"
            target => "DestIP_City"
            database => "/usr/share/elasticsearch/modules/ingest-geoip/GeoLite2-City.mmdb"
            }
        }

    output {

    if[fields][site] == "1"
    {
    elasticsearch {
        hosts => ["localhost:9200"]
        document_id => "%{[generated_id]}"
        index => "sensor-site1-%{+yyyy.MM.dd}"
        
    }
    }

    if[fields][site] == "2"
    {
    elasticsearch {
        hosts => ["localhost:9200"]
        document_id => "%{[generated_id]}"
        index => "sensor-site2-%{+yyyy.MM.dd}"
        
    }
    }

    }

