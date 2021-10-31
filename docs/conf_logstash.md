# Operasional Logstash

## Mengatur Filter Untuk Beats
Pertama-tama dalam konfigurasi logstash cek file `/etc/logstash/pipelines.yml`

    - pipeline.id: main
      path.config: "/etc/logstash/conf.d/*.conf"

Konfigurasi di atas menunjukkan bahwa logstash akan membaca semua file yang berada di folder `/etc/logstash/conf.d/` dengan ekstensi `.conf` yang dikenal oleh logstash dengan id pipeline `main`. kita bisa menspesifikkan file konfigurasi yang akan dibaca oleh logstash dengan mengubah `*.conf` menjadi file spesifik.

ada 3 bagian pada konfigurasi pipeline logstash yaitu `input` `filter` dan `output`. Kemudian untuk membuat pipeline di logstash buatlah sebuah file bernama `beats.conf`, kemudian buatlah konfigurasi untuk membuka port 5044, yang menandakan pada konfigurasi ini hanya berlaku pada data yang melalui port 5044, dengan command:

    input
    {
        beats
        {
            port =>5044
        }
    }

Kemudian pada konfigurasi `filter` dan `output` akan dibahas di sesi selanjutnya