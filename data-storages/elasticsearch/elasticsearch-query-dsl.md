# Elasticsearch Query DSL Note

**Content**

- Getting Started - Start Searching
- Search your data
- Query DSL
- Aggregations

## Getting Started - Start Searching

Once you have ingested some data into an Elasticsearch index, you can search it by sending requests to the `_search` endpoint. To access the full suite of search capabilities, you use the Elasticsearch Query DSL to specify the search criteria in the request body. You specify the name of the index you want to search in the request URI.

For example, the following request retrieves all documents in the `bank` index sorted by account number:

```console
GET /bank/_search
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

By default, the `hits` section of the response includes the first 10 documents that match the search criteria:

```json
{
  "took" : 63,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
        "value": 1000,
        "relation": "eq"
    },
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    }, ...
    ]
  }
}
```

Search Response Fields:

- `took` – how long it took Elasticsearch to run the query, in milliseconds
- `timed_out` – whether or not the search request timed out
- `_shards` – how many shards were searched and a breakdown of how many shards succeeded, failed, or were skipped.
- `max_score` – the score of the most relevant document found
- `hits.total.value` - how many matching documents were found
- `hits.sort` - the document’s sort position (when not sorting by relevance score)
- `hits._score` - the document’s relevance score (not applicable when using `match_all`)
- `hits.hits` - search return data area.
- `hits.aggregations.<aggregate_name>.buckets` - aggregation operations return result.

Request Fields:

- `query`

  - `match_all`

    ```
    GET /bank/_search
    {
      "query": { "match_all": {} }
    }
    ```

  - `match`: To search for specific terms within a field, you can use a `match` query.

    ```
    GET /bank/_search
    {
      "query": { "match": { "address": "mill lane" } }
    }
    ```

  - `match_phrase`: To perform a phrase search rather than matching individual terms

    ```
    GET /bank/_search
    {
      "query": { "match_phrase": { "address": "mill lane" } }
    }
    ```

  - `bool`: you can use a `bool` query to combine multiple query criteria. You can designate criteria as required (must match), desirable (should match), or undesirable (must not match). Each `must`, `should`, and `must_not` element in a Boolean query is referred to as a query clause.

    ```
    {
      "query": {
        "bool": {
          "must": [
            { "match": { "age": "40" } }
          ],
          "must_not": [
            { "match": { "state": "ID" } }
          ]
        }
      }
    }
    ```

  - `filter`: The criteria in a `must_not` clause is treated as a *filter*. It affects whether or not the document is included in the results, but does not contribute to how documents are scored. You can also explicitly specify arbitrary filters to include or exclude documents based on structured data.

    ```json
    {
      "query": {
        "bool": {
          "must": { "match_all": {} },
          "filter": {
            "range": {
              "balance": {
                "gte": 20000,
                "lte": 30000
              }
            }
          }
        }
      }
    }
    ```

- `sort`

  ```
  GET /bank/_search
  {
    "query": { "match_all": {} },
    "sort": [
      { "account_number": "asc" }
    ]
  }
  ```

- `from`, `size`

  Each search request is self-contained: Elasticsearch does not maintain any state information across requests. To page through the search hits, specify the `from` and `size` parameters in your request.

  For example, the following request gets hits 10 through 19:

  ```
  GET /bank/_search
  {
    "query": { "match_all": {} },
    "sort": [
      { "account_number": "asc" }
    ],
    "from": 10,
    "size": 10
  }
  ```

- `aggs.<aggregate_name>.terms`: aggregation operations. Request set `size=0`, the response only contains the aggregation results. To group all of the records by a field, and returns the ten records with the most count values in descending order.

  ```
  {
    "size": 0,
    "aggs": {
      "group_by_state": {
        "terms": {
          "field": "state.keyword"
        }
      }
    }
  }
  ```

  - `order`

    ```
    {
      "size": 0,
      "aggs": {
        "group_by_state": {
          "terms": {
            "field": "state.keyword",
            "order": {
              "average_balance": "desc"
            }
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
    ```

  - `aggs.<aggregate_name>.avg`: To calculate the average values.

    ```
    {
      "size": 0,
      "aggs": {
        "group_by_state": {
          "terms": {
            "field": "state.keyword"
          },
          "aggs": {
            "average_balance": {
              "avg": {
                "field": "balance"
              }
            }
          }
        }
      }
    }
    ```

In addition to basic bucketing and metrics aggregations like these, Elasticsearch provides specialized aggregations for operating on multiple fields and analyzing particular types of data such as dates, IP addresses, and geo data. You can also feed the results of individual aggregations into pipeline aggregations for further analysis.

The core analysis capabilities provided by aggregations enable advanced features such as using machine learning to detect anomalies.

## Search your data



## Query DSL

## Aggregations



## References

[1] [Elasticsearch Reference 7.9](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)