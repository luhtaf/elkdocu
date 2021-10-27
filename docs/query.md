# Query Elasticsearch
Elasticsearch sebagai database sudah memiliki API untuk mengakses datanya. Selain itu, Elasticsearch juga memiliki berbagai query yang disediakan. Pada bagian ini akan di simulasikan dan diujicoba beberapa query yang ada di elasticsearch, silahkan gunakan `Dev Tool` seperti pada gambar.
![Dev tools](https://drive.bssn.go.id/apps/files_sharing/publicpreview/Pc73kGpLY3C7WEq?fileId=2213238&file=/dev_tools.png&x=1536&y=864&a=true)

## Istilah
| Nama | Deskripsi | simbol di db |
| --- | --- | --- |
| index | Seperti sebuah Database | `_index`|
| type | Seperti sebuah Tabel | `_type` |
| id | Identitas data | `_id` |
| Timestamp | penunjuk waktu | `@timestamp` |

## Melihat database
Perintah untuk melihat semua database yang tersedia 

    GET _cat/indices

## Membuat Index
Untuk membuat index pada elasticsearch menggunakan method PUT seperti berikut

    PUT data

## Create
Untuk menambahkan data pada elasticsearch digunakan method POST seperti berikut

    POST data/_doc
    {
        "name":"thul",
        "title":"Mr",
        "active":true,
        "age":18
    }

pada contoh diatas, parameter `_id` di generate otomatis. Kita bisa menspesifikkan `_id` yang aka di generate dengan perintah

    POST data-dummy/_doc/id1
    {
        "name":"Cornellia",
        "title":"Mrs",
        "active":false,
        "age":29,
        "job":"teacher"
    }

untuk memasukkan banyak data sekaligus, digunakan perintah, perintah ini akan dibahas lebih lanjut pada sesi (Bulk)[#Bulk]

    POST data/_bulk
    {"index": {"_index": "data-dummy", "_type": "_doc"}}
    {"name":"thul","age":20}
    {"index": {"_index": "data-dummy", "_type": "_doc"}}
    {"name":"san","age":16}


## Read
Untuk membaca data gunakan perintah `_search` dengan method GET

    GET _search

Data juga bisa dispesifikkan untuk sebuah indeks tertentu

    GET data/_search

Bila indeks yang di query lebih dari 1, maka digunakan query

    GET data,data-dummy/_search

Jika indeks memiliki pola yang sama seperti `data`, `data-dummy`, `data-user`,`data-restaurant` yang memiliki pola sama sama diawali oleh `data`, maka dapat digunakan query berikut untuk mencari di semua indeks yang memiliki pola sama tersebut

    GET data*/_search

Kita juga bisa menspesifikkan dokumen yang akan kita cari melalui id

    GET data-dummy/_doc/id1


## Update
Untuk mengupdate data

## Delete
Kita dapat menghapus data pada suatu indeks dengan perintah

    DELETE data-dummy/_doc/id1

atau kita juga bisa menghapus satu indeks dengan perintah

    DELETE data-dummy

## Import Json
Pada sesi ini kita kana memasukkan data secara manual kedalam elasticsearch