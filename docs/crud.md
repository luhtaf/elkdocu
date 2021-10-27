# CRUD Elasticsearch
Elasticsearch sebagai database sudah memiliki API untuk mengakses database. API ini bisa digunakan untuk Create, Read, Update dan Delete seperti database pada umumnya, silahkan gunakan `Dev Tool` seperti pada gambar.
![Dev tools](https://drive.bssn.go.id/apps/files_sharing/publicpreview/Pc73kGpLY3C7WEq?fileId=2213238&file=/dev_tools.png&x=1536&y=864&a=true)

## Istilah
| Nama | Deskripsi | simbol di db |
| --- | --- | --- |
| index | Seperti sebuah Database | `_index`|
| type | Seperti sebuah Tabel | `_type` |
| id | Identitas data | `_id` |
| Timestamp | penunjuk waktu | `@timestamp` |

## Melihat database
Perintah untuk melihat semua indeks yang tersedia 

    GET _cat/indices

> jalankan perintah untuk melihat indeks yang ada di elasticsearch anda

## Membuat Index
Untuk membuat index pada elasticsearch menggunakan method PUT seperti berikut

    PUT data

> Buatlah sebuah indeks untuk personil ST16

## Create
Untuk menambahkan data pada elasticsearch digunakan method POST seperti berikut

    POST data/_doc{
        "name":"thul",
        "title":"Mr",
        "active":true,
        "age":18
    }

> Buatlah 5 dokumen dengan random generate id

pada contoh diatas, parameter `_id` di generate otomatis. Kita bisa menspesifikkan `_id` yang aka di generate dengan perintah `POST` maupun `PUT`

    POST data-dummy/_doc/id1{
        "name":"Cornellia",
        "title":"Mrs",
        "active":false,
        "age":29,
        "job":"teacher"
    }

> Buatlah 5 dokumen masing-masing dengan method `POST` dan `PUT` dengan id yang ditentukan. Temukan perbedaannya

untuk memasukkan banyak data sekaligus, digunakan perintah

    POST data/_bulk
    {"index": {"_index": "data-dummy", "_type": "_doc"}}
    {"name":"thul","age":20}
    {"index": {"_index": "data-dummy", "_type": "_doc"}}
    {"name":"san","age":16}

kemudian, untuk mencegah adanya data yang saling ditimpa dengan id yang sama, kita bisa menggunakan perintah

    PUT data-dummy/_create/id1
    {
    "name": "Aqua",
    "gender": "cwk"
    }

## Read
Untuk membaca data gunakan perintah `_search` dengan method GET

    GET _search

Data juga bisa dispesifikkan untuk sebuah indeks tertentu

    GET data/_search

Bila indeks yang di query lebih dari 1, maka digunakan query

    GET data,data-dummy/_search

Jika indeks memiliki pola yang sama seperti `data`, `data-dummy`, `data-user`,`data-restaurant` yang memiliki pola sama sama memiliki prefiks `data`, maka dapat digunakan query berikut untuk mencari di semua indeks yang memiliki pola sama tersebut

    GET data*/_search

Kita juga bisa menspesifikkan dokumen yang akan kita cari dengan id

    GET data-dummy/_doc/id1


## Update
Untuk mengupdate data, digunakan method POST dengan parameter `_update` seperti berikut untuk mengubah suatu field tertentu pada sebuah dokumen

    POST data-dummy/_update/id1{
        "script" : "ctx._source.name = 'Cornellia2'"
        }

Jika ingin mengubah lebih banyak field pada suatu dokumen dapat digunakan perintah

    POST data-dummy/_update/id1{

        "doc": {
            "name": "Connel",
            "degree": "Magister"
        }

    }

## Delete
Kita dapat menghapus data pada suatu indeks dengan perintah

    DELETE data-dummy/_doc/id1

atau kita juga bisa menghapus satu indeks dengan perintah

    DELETE data-dummy


## Import Json
Pada sesi ini kita kana memasukkan data secara manual kedalam elasticsearch melalui kibana, pertama buka tab `Home` di side menu kibana, kemudian buka menu `Add your data`, setelah membuka halaman baru, pilih tab `Upload file`, kemudian upload file yang akan di upload.
![Kibana Upload](https://drive.bssn.go.id/apps/files_sharing/publicpreview/Pc73kGpLY3C7WEq?fileId=2213976&file=/upload_kibana.png&x=1536&y=864&a=true)

Kemudian setelah file diupload, klik tombol Import, masukkan nama indeks, dan klik Import lagi. 

Data yang akan diupload dapat di download melalui [drive](https://drive.bssn.go.id/s/tqjTj2pbgiTcDrd). Kemudian spesifikasi indeks yang akan dibuat:

| Nama File | Nama Indeks |
| --- | --- |
| data-user.json | data-user |
| data-restaurant.json | data-restaurant |
| data-commerce.json | data-commerce |