# Query Elasticsearch
Selanjutnya masih akan digunakan Dev Tools untuk melakukan query lanjutan

## Basic Query
Basic Query di elasticsearch menggunakan command `_search` seperti yang sudah dibahas pada [Query 1](../query). Query ini adalah query dasar dari elastticsearch.

    GET data-restaurant/_search?q=Mission

selain itu, kita bisa menspesifikkan field yang akan di query dengan menggunaka command dibawah.

    GET data-restaurant/_search
    {
    "from" : 0, "size" : 87,
    "query": {
        "query_string" : {
            "query": "Our Mission",
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
            "value": "Big Gri",
            "fuzziness": "AUTO",
            "max_expansions": 50,
            "prefix_length": 0,
            "transpositions": true,
            "rewrite": "constant_score"
        }
        }
    }
    }

## Aggregate
