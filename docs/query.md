# Query Elasticsearch
Selanjutnya masih akan digunakan Dev Tools untuk melakukan query lanjutan

## Basic Query
Basic Query di elasticsearch menggunakan command `_search` seperti yang sudah dibahas pada [CRUD](../crud). Query ini adalah query dasar dari elastticsearch.

    GET data-*/_search?q=Mission


> ada berapa jumlah data yang dihasilkan dari query di atas?


selain itu, kita bisa menspesifikkan field yang akan di query dengan menggunaka command dibawah.

    GET data-restaurant/_search
    {
    "from" : 0, "size" : 87,
    "query": {
        "query_string" : {
            "query": "Some Query",
            "fields": ["description","address"],
        }
    }
    }

atau

    GET data-restaurant/_search
    {
    "query": {
        "query_string" : {
            "query": "Culvers",
            "default_field": "description"
        }
    }
    }

Bisa juga dilakukan query terhadap suatu range pada field tertentu

    GET data-user/_search
    {
    "query": {
        "range": {
        "address.coordinates.lng": {
            "gte": 10
        }
        }  
        }
    }


> lakukan query pada `data-user`, `data-restaurant`, `data-commerce`
> pada data-restaurant:
> - berapa jumlah data dengan query `Culvers` pada field description? - 85
> - berapa restotan yang memasak makanan itali? - 35
> - berapa restoran yang tutup di hari senin? - 527
> pada data-commerce
> - berapa jumlah barang yang dijual dengan harga lebih dari 60? - 371
> - berapa jumlah barang yang dijual dengan harga kurang dari 10? - 112
> - berapa jumlah barang yang dijual dengan rentang harga 10-70? - 618

> hint: lihat auto complete dan [referensi](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html)

## Term Query
Term Query di elasticsearch melakukan query hanya pada nilai yang tepat

    GET data-restaurant/_search
    {
    "query": {
        "term": {
        "description": "Culvers"
        }
    }
    }



    GET data-restaurant/_search
    {
    "query": {
        "term": {
        "type": "Juice & Smoothies"
        }
    }
    }

> Berapa jumlah data dari masing-masing contoh di atas? - 0 dan 26
> ada berapa restoran dengan tipe `Bakery`? - 29

## Fuzzy Query
Fuzzy Query melakukan query tidak hanya pada nilai yang di query tapi juga mengquery nilai yang mirip

    GET data-restaurant/_search
    {
    "query": {
        "fuzzy": {
        "name": {
            "value": "Big Grill"
        }
        }
    }
    }

jika kita menggunakan parameter lanjutan dari fuzzy ini yaitu

    GET /_search
    {
    "query": {
        "fuzzy": {
        "name": {
            "value": "Big Gril",
            "fuzziness": "AUTO",
            "max_expansions": 50,
            "prefix_length": 0,
            "transpositions": true,
            "rewrite": "constant_score"
        }
        }
    }
    }

> Apa hasil fuzzy dari command diatas? - BC Grill

## Aggregations
Aggregations di elasticsearch ini memiliki fungsi merangkum beberapa kondisi index pada elasticsearch seperti metric, statictic, dan analytic lainnya. Jika Elasticsearch dimanfaatkan sebagai database dalam suatu instansi pendidikan, fungsi agregasinya kita bisa mencari tahu :

* siapa user yang paling berprestasi ? 
* berapa jumlah peserta yang memiliki suatu nilai tertentu ?
* berapa nilai rata-rata peserta ?


Aggregations di elasticsearch sendiri dibagi menjadi 3 kategori:

* `metric` aggregations yang menghitung ukuran dari dokumen, seperti jumlah dan rata-rata nilai dari suatu fields.
* `bucket` aggregations yang mengkelompokkan dokumen menjadi sebuah bucket / kelompok.
* `pipeline` aggregations yang menerima inputan dari aggregations lainnya.

Disini akan dijelaskan hanya 2 teknik aggregations dasar, yaitu terms bucket aggregation dan metric aggregations (avg,min,max,sum). Untuk mempelajaari aggregations lebih lanjut dapat dilihat dari [link berikut](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)

`Pertama` adalah terms bucket aggregations yang akan mengelompokkan data berdasarkan keyword dari suatu field, yang dapat dilihat dengan menggunakan command

    GET data-commerce/_search
    {
    "aggs": {
        "my-agg-name": {
        "terms": {
            "field": "material"  }
        }
    }
    }

kedua adalah metric aggregations yang akan menghitung nilai rata-rata. Aggregations ini hanya menerima field berjenis angka.

    GET data-commerce/_search
    {
    "size":10,
    "aggs": {
        "my-agg-name": {
        "stats": {
            "field": "price"  }
        }
    }
    }



Selain itu, kita bisa mengatur aggregations berdasarkan range tertentu. Misal pada contoh dibawah, aggregations dilakukan pada field "material", namun hanya dilakukan pada dokumen yang field `price` ada di rentang 3-7.

    GET data-commerce/_search
    {
    "size":0,
    "query": {
        "range": {
        "price": {
            "gte": 3,
            "lt": 7
        }}},
    "aggs": {
        "my-agg-name": {
        "terms": {
            "field": "material"}
        }
    }
    }


> Jelaskan hasil aggregations dari contoh pertama?
> apa saja hasil statistik yang termasuk pada contoh ke-2?