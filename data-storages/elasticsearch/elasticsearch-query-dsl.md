# Elasticsearch Query DSL Note

**Content**

- [I. Getting Started - Start Searching](#I. Getting Started - Start Searching)
- [II. Search your data](#II. Search your data)
- [III. Query DSL](#III. Query DSL)
- [IV. Aggregations](#IV. Aggregations)
- [V. Scripting](#V. Scripting)
- [VI. REST APIs](#VI. REST APIs)

## I. Getting Started - Start Searching

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

    - `bool.filter`: The criteria in a `must_not` clause is treated as a *filter*. It affects whether or not the document is included in the results, but does not contribute to how documents are scored. You can also explicitly specify arbitrary filters to include or exclude documents based on structured data.

      ```
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



## II. Search your data



### Introduction

A *search query*, or *query*, is a request for information about data in Elasticsearch data streams or indices.

A *search* consists of one or more queries that are combined and sent to Elasticsearch. Documents that match a search’s queries are returned in the *hits*, or *search results*, of the response.

A search may also contain additional information used to better process its queries. For example, a search may be limited to a specific index or only return a specific number of results.

#### Run a search

You can use the [search API](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html) to search and [aggregate](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html) data stored in Elasticsearch data streams or indices. The API’s `query` request body parameter accepts queries written in [Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html).

For example:

```
GET /my-index-000001/_search
{
  "query": {
    "match": {
      "user.id": "kimchy"
    }
  }
}
```

The API response returns the top 10 documents matching the query in the `hits.hits` property.

#### Common search options

You can use the following options to customize your searches.

**Query DSL**

Query DSL supports a variety of query types you can mix and match to get the results you want. Query types include:

- [Boolean](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html) and other [compound queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/compound-queries.html), which let you combine queries and match results based on multiple criteria
- [Term-level queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/term-level-queries.html) for filtering and finding exact matches
- [Full text queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/full-text-queries.html), which are commonly used in search engines
- [Geo](https://www.elastic.co/guide/en/elasticsearch/reference/current/geo-queries.html) and [spatial queries](https://www.elastic.co/guide/en/elasticsearch/reference/current/shape-queries.html)

**Aggregations**

You can use [search aggregations](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html) to get statistics and other analytics for your search results.

**Search multiple data streams and indices**

You can use comma-separated values and grep-like index patterns to search several data streams and indices in the same request. You can even boost search results from specific indices.

```
GET /my-index-000001,my-index-000002/_search
{
  "query": {
    "match": {
      "user.id": "kimchy"
    }
  }
}
```

**Paginate search results**
By default, searches return only the top 10 matching hits. To retrieve more or fewer documents, see [*Paginate search results*](https://www.elastic.co/guide/en/elasticsearch/reference/current/paginate-search-results.html).

**Retrieve selected fields**
The search response’s `hit.hits` property includes the full document [`_source`](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-source-field.html) for each hit. To retrieve only a subset of the `_source` or other fields, see [*Retrieve selected fields*](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-fields.html).

**Sort search results**
By default, search hits are sorted by `_score`, a [relevance score](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-filter-context.html#relevance-scores) that measures how well each document matches the query. To customize the calculation of these scores, use the [`script_score`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-script-score-query.html) query. To sort search hits by other field values, see [*Sort search results*](https://www.elastic.co/guide/en/elasticsearch/reference/current/sort-search-results.html).

**Run an async search**
Elasticsearch searches are designed to run on large volumes of data quickly, often returning results in milliseconds. For this reason, searches are *synchronous* by default. The search request waits for complete results before returning a response.

#### Search timeout

By default, search requests don’t time out. The request waits for complete results before returning a response.

While [async search](https://www.elastic.co/guide/en/elasticsearch/reference/current/async-search-intro.html) is designed for long-running searches, you can also use the `timeout` parameter to specify a duration you’d like to wait for a search to complete. If no response is received before this period ends, the request fails and returns an error.

```
GET /my-index-000001/_search
{
  "timeout": "2s",
  "query": {
    "match": {
      "user.id": "kimchy"
    }
  }
}
```

To set a cluster-wide default timeout for all search requests, configure `search.default_search_timeout` using the [cluster settings API](https://www.elastic.co/guide/en/elasticsearch/reference/current/cluster-update-settings.html). This global timeout duration is used if no `timeout` argument is passed in the request. If the global search timeout expires before the search request finishes, the request is cancelled using [task cancellation](https://www.elastic.co/guide/en/elasticsearch/reference/current/tasks.html#task-cancellation). The `search.default_search_timeout` setting defaults to `-1` (no timeout).

#### Search cancellation

You can cancel a search request using the [task management API](https://www.elastic.co/guide/en/elasticsearch/reference/current/tasks.html#task-cancellation). Elasticsearch also automatically cancels a search request when your client’s HTTP connection closes. We recommend you set up your client to close HTTP connections when a search request is aborted or times out.

#### Track total hits

Generally the total hit count can’t be computed accurately without visiting all matches, which is costly for queries that match lots of documents. The `track_total_hits` parameter allows you to control how the total number of hits should be tracked. 

the `"hits.total.relation"` of query response returned in the `"total"` object in the search response determines how the `"hits.total.value"` should be interpreted. A value of `"gte"` means that the `"hits.total.value"` is a lower bound of the total hits that match the query and a value of `"eq"` indicates that `"hits.total.value"` is the accurate count.

- `"track_total_hits": true`: the search response will always track the number of hits that match the query accurately.

  ```
  GET my-index-000001/_search
  {
    "track_total_hits": true,
      "query": {
        "match" : {
          "user.id" : "elkbee"
        }
      }
  }
  ```

- `track_total_hits:<integer>`: query will accurately track the total hit count that match the query up to specified <integer> documents.

  ```
  GET my-index-000001/_search
  {
    "track_total_hits": 100,
    "query": {
      "match": {
        "user.id": "elkbee"
      }
    }
  }
  ```

- `"track_total_hits": false`: Don’t to track the total number of hits, you can improve query times.

  ```
  GET my-index-000001/_search
  {
    "track_total_hits": false,
    "query": {
      "match": {
        "user.id": "elkbee"
      }
    }
  }
  ```

  ```
  {
    "_shards": ...
    "timed_out": false,
    "took": 10,
    "hits": {
    	// no total.value
      "max_score": 1.0,
      "hits": ...
    }
  }
  ```

  

#### Quickly check for matching docs

If you only want to know if there are any documents matching a specific query, you can set the `size` to `0` to indicate that we are not interested in the search results. You can also set `terminate_after` to `1` to indicate that the query execution can be terminated whenever the first matching document was found (per shard).

```console
// q=user.id:elkbee equals to { "query":{ "match": { "user.id": "elkbee" }}}
GET /_search?q=user.id:elkbee&size=0&terminate_after=1
```

The response will not contain any hits as the `size` was set to `0`. The `hits.total` will be either equal to `0`, indicating that there were no matching documents, or greater than `0` meaning that there were at least as many documents matching the query when it was early terminated. Also if the query was terminated early, the `terminated_early` flag will be set to `true` in the response.

### Collapse search results

TODO

### Filter search results



## III. Query DSL

### Introduction

Elasticsearch provides a full Query DSL (Domain Specific Language) based on JSON to define queries. Think of the Query DSL as an AST (Abstract Syntax Tree) of queries, consisting of two types of clauses:

- **Leaf query clauses**

  Leaf query clauses look for a particular value in a particular field, such as the [`match`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-match-query.html), [`term`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-term-query.html) or [`range`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-range-query.html) queries. These queries can be used by themselves.

- **Compound query clauses**

  Compound query clauses wrap other leaf **or** compound queries and are used to combine multiple queries in a logical fashion (such as the [`bool`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-bool-query.html) or [`dis_max`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-dis-max-query.html) query), or to alter their behaviour (such as the [`constant_score`](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl-constant-score-query.html) query).

**Allow expensive queries**

Certain types of queries will generally execute slowly due to the way they are implemented, which can affect the stability of the cluster.

The execution of such queries can be prevented by setting the value of the `search.allow_expensive_queries` setting to `false` (defaults to `true`).

### Query and filter context

### Compound queries

### Full text queries

### Geo queries

### Shape queries

### Joining queries

### Match all

### Span queries

### Specialized queries

### Term-level queries

### Others

## IV. Aggregations

### Introduction

The aggregations framework helps provide aggregated data based on a search query. It is based on simple building blocks called aggregations, that can be composed in order to build complex summaries of the data.

There are many different types of aggregations, each with its own purpose and output. To better understand these types, it is often easier to break them into four main families:

- [*Bucketing*](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-bucket.html)
- [*Metric*](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-metrics.html)
- [*Matrix*](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-matrix.html)
- [*Pipeline*](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations-pipeline.html)

The interesting part comes next. Since each bucket effectively defines a document set (all documents belonging to the bucket), one can potentially associate aggregations on the bucket level, and those will execute within the context of that bucket. This is where the real power of aggregations kicks in: **aggregations can be nested!**

#### Structure Aggregations

The following snippet captures the basic structure of aggregations:

```js
"aggregations" : {
    "<aggregation_name>" : {
        "<aggregation_type>" : {
            <aggregation_body>
        }
        [,"meta" : {  [<meta_data_body>] } ]?
        [,"aggregations" : { [<sub_aggregation>]+ } ]?
    }
    [,"<aggregation_name_2>" : { ... } ]*
}
```

The `aggregations` object (the key `aggs` can also be used) in the JSON holds the aggregations to be computed.

Each aggregation is associated with a logical name that the user defines (e.g. if the aggregation computes the average price, then it would make sense to name it `avg_price`). These logical names will also be used to uniquely identify the aggregations in the response.

Each aggregation has a specific type (`<aggregation_type>` in the above snippet) and is typically the first key within the named aggregation body. Each type of aggregation defines its own body, depending on the nature of the aggregation (e.g. an `avg` aggregation on a specific field will define the field on which the average will be calculated).

#### Values Source

Some aggregations work on values extracted from the aggregated documents. Typically, the values will be extracted from a specific document field which is set using the `field` key for the aggregations. It is also possible to define a [`script`](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-scripting.html) which will generate the values (per document).



### Metrics Aggregations

#### Introduction

The aggregations in this family compute metrics based on values extracted in one way or another from the documents that are being aggregated. The values are typically extracted from the fields of the document (using the field data), but can also be generated using scripts.

Numeric metrics aggregations are a special type of metrics aggregation which output numeric values. Some aggregations output a single numeric metric (e.g. `avg`) and are called `single-value numeric metrics aggregation`, others generate multiple metrics (e.g. `stats`) and are called `multi-value numeric metrics aggregation`. 

#### Avg Aggregation

A `single-value` metrics aggregation that computes the average of numeric values that are extracted from the aggregated documents. These values can be extracted either from specific numeric fields in the documents, or be generated by a provided script.

```
POST /exams/_search?size=0
{
  "aggs": {
    "avg_grade": { 
    	"avg": { "field": "grade" } 
    }
  }
}
```

**Script**

TODO



#### Weighted Avg Aggregation



### Bucket Aggregations

### Pipeline Aggregations

### Matrix Aggregations

### Caching heavy aggregations

### Returning only aggregation results

### Aggregation Metadata

### Returning the type of the aggregation

### Indexing aggregation results with transforms



## V. Scripting

### Introduction

### How to use scripts

Wherever scripting is supported in the Elasticsearch API, the syntax follows the same pattern:

```js
  "script": {
    "lang":   "...",  
    "source" | "id": "...", 
    "params": { ... } 
  }
```

- `lang`: The language the script is written in, which defaults to `painless`.
- `source`: A specified inline script.
- `id`: A specified stored script.
- `params`: Any named parameters that should be passed into the script.



### Accessing document fields and special variables

### Scripting and security

### Painless scripting language

### Lucene expressions language

### Advanced scripts using script engines



## VI. REST APIs





## References

[1] [Elasticsearch Reference 7.9](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)