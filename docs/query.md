# Query Elasticsearch
Elasticsearch sebagai database sudah memiliki API untuk mengakses datanya. Selain itu, Elasticsearch juga memiliki berbagai query yang disediakan. Pada bagian ini akan di simulasikan dan diujicoba beberapa query yang ada di elasticsearch, silahkan gunakan `Dev Tool` seperti pada gambar
![Dev tools](https://drive.bssn.go.id/core/preview?fileId=2213112&x=1536&y=864&a=true)

## Istilah
| Nama | Deskripsi | simbol di db |
| --- | --- | --- |
| index | Seperti sebuah Database | `_index`|
document | Seperti sebuah Tabel | `_doc` |
id | Identitas data | `_id` |
timestamp | penunjuk waktu | `@timestamp` |


## Membuat Index
Untuk membuat index pada elasticsearch menggunakan method PUT seperti berikut

    _PUT /index/

## Import Json
Pada sesi ini kita kana memasukkan data secara manual kedalam elasticsearch