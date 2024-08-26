# Important Elastic Search Commands

I hve followed this for setting elastic search and kibana in local - https://www.elastic.co/guide/en/elasticsearch/reference/8.15/run-elasticsearch-locally.html

This was followed to understand hitting elastic search queries on kibana - https://www.elastic.co/guide/en/elasticsearch/reference/8.15/getting-started.html

Following steps can be used to gte a holistic understanding of elastic search. These are essentially simple REST Queries, but they show how basic operations can be performed in Elastic Search.

###  Create Index
We can create index. This is equivalent to table in SQL
#### Console Request
```
PUT /user
```
#### Curl Request
```bash
curl -X PUT "localhost:9200/users" \
-u elastic:$ELASTIC_PASSWORD
```
#### Response
```json
{
  "acknowledged": true,
  "shards_acknowledged": true,
  "index": "users"
}
```

###  Add Document
We can add document in a given index
#### Console Request
```
POST /users/_doc/1
{
  "name": "John Doe",
  "age": 30,
  "occupation": "Software Engineer"
}
```
#### Curl Request
```bash
curl -X POST "localhost:9200/users/_doc/1" \
-u elastic:$ELASTIC_PASSWORD \
-H 'Content-Type: application/json' \
--data-raw '
{
  "name": "John Doe",
  "age": 30,
  "occupation": "Software Engineer"
}'
```
#### Response
```json
{
  "_index": "users",
  "_id": "1",
  "_version": 1,
  "result": "created",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 0,
  "_primary_term": 1
}
```

###  Get Document
We can get document by
#### Console Request
```
GET /users/_doc/1
```
#### Curl Request
```bash
curl -X GET "localhost:9200/users/_doc/1" \
-u elastic:$ELASTIC_PASSWORD
```
#### Response
```json
{
  "_index": "users",
  "_id": "1",
  "_version": 1,
  "_seq_no": 2,
  "_primary_term": 1,
  "found": true,
  "_source": {
    "name": "John Doe",
    "age": 30,
    "occupation": "Software Engineer"
  }
}
```

###  Update Document
Update a document with a certain id
#### Console Request
```
POST /users/_update/1
{
  "doc": {
    "occupation": "Senior Software Engineer"
  }
}
```
#### Curl Request
```bash
curl -X POST "localhost:9200/users/_update/1" \
-u elastic:$ELASTIC_PASSWORD \
-H 'Content-Type: application/json' \
--data-raw '
{
  "doc": {
    "occupation": "Senior Software Engineer"
  }
}'
```
#### Response
```json
{
  "_index": "users",
  "_id": "1",
  "_version": 4,
  "result": "updated",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 5,
  "_primary_term": 1
}
```

###  Search with Match Query
Search in an index with a match query
#### Console Request
```
GET /users/_search
{
  "query": {
    "match": {
      "age": 30
    }
  }
}
```
#### Curl Request
```bash
curl -X GET "localhost:9200/users/_search" \
-u elastic:$ELASTIC_PASSWORD \
-H 'Content-Type: application/json' \
--data-raw '
{
  "query": {
    "match": {
      "age": 30
    }
  }
}'

```
#### Response
```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 1,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "users",
        "_id": "1",
        "_score": 1,
        "_source": {
          "name": "John Doe",
          "age": 30,
          "occupation": "Senior Software Engineer"
        }
      }
    ]
  }
}
```

###  Adding bulk data
Adding bulk data into an index
#### Console Request
```
POST /users/_bulk
{ "index": { "_id": "2" } }
{ "name": "Jane Doe", "age": 28, "occupation": "Data Scientist" }
{ "index": { "_id": "3" } }
{ "name": "Alice", "age": 32, "occupation": "DevOps Engineer" }
```
#### Curl Request
```bash
curl -X POST "localhost:9200/users/_bulk" \
-u elastic:$ELASTIC_PASSWORD \
-H 'Content-Type: application/json' \
--data-raw '
{ "index": { "_id": "2" } }
{ "name": "Jane Doe", "age": 28, "occupation": "Data Scientist" }
{ "index": { "_id": "3" } }
{ "name": "Alice", "age": 32, "occupation": "DevOps Engineer" }
'
```
#### Response
```json
{
  "errors": false,
  "took": 7311841,
  "items": [
    {
      "index": {
        "_index": "users",
        "_id": "2",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 10,
        "_primary_term": 1,
        "status": 200
      }
    },
    {
      "index": {
        "_index": "users",
        "_id": "3",
        "_version": 2,
        "result": "updated",
        "_shards": {
          "total": 2,
          "successful": 1,
          "failed": 0
        },
        "_seq_no": 11,
        "_primary_term": 1,
        "status": 200
      }
    }
  ]
}
```

###  Get All Documents
Get all documents in an index
#### Console Request
```
GET /users/_search
```
#### Curl Request
```bash
curl -X GET "localhost:9200/users/_search" \
-u elastic:$ELASTIC_PASSWORD
```
#### Response
```json
{
  "took": 2,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 3,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "users",
        "_id": "1",
        "_score": 1,
        "_source": {
          "name": "John Doe",
          "age": 30,
          "occupation": "Senior Software Engineer"
        }
      },
      {
        "_index": "users",
        "_id": "2",
        "_score": 1,
        "_source": {
          "name": "Jane Doe",
          "age": 28,
          "occupation": "Data Scientist"
        }
      },
      {
        "_index": "users",
        "_id": "3",
        "_score": 1,
        "_source": {
          "name": "Alice",
          "age": 32,
          "occupation": "DevOps Engineer"
        }
      }
    ]
  }
}
```

###  Make Custom Query
We can make custom queriesin this manner. here we get all users with age greater than 29.
#### Console Request
```
GET /users/_search
{
  "query": {
    "range": {
      "age": {
        "gt": 29
      }
    }
  }
}
```
#### Curl Request
```bash
curl -X GET "localhost:9200/users/_search" \
-u elastic:$ELASTIC_PASSWORD \
-H 'Content-Type: application/json' \
--data-raw '
{
  "query": {
    "range": {
      "age": {
        "gt": 25
      }
    }
  }
}'
```
#### Response
```json
{
  "took": 1,
  "timed_out": false,
  "_shards": {
    "total": 1,
    "successful": 1,
    "skipped": 0,
    "failed": 0
  },
  "hits": {
    "total": {
      "value": 2,
      "relation": "eq"
    },
    "max_score": 1,
    "hits": [
      {
        "_index": "users",
        "_id": "1",
        "_score": 1,
        "_source": {
          "name": "John Doe",
          "age": 30,
          "occupation": "Senior Software Engineer"
        }
      },
      {
        "_index": "users",
        "_id": "3",
        "_score": 1,
        "_source": {
          "name": "Alice",
          "age": 32,
          "occupation": "DevOps Engineer"
        }
      }
    ]
  }
}
```

###  Delete Document
We can delete a document with a specific id
#### Console Request
```
DELETE /users/_doc/1
```
#### Curl Request
```bash
curl -X DELETE "localhost:9200/users/_doc/1" \
-u elastic:$ELASTIC_PASSWORD
```
#### Response
```json
{
  "_index": "users",
  "_id": "1",
  "_version": 7,
  "result": "deleted",
  "_shards": {
    "total": 2,
    "successful": 1,
    "failed": 0
  },
  "_seq_no": 12,
  "_primary_term": 1
}
```

###  Delete Index
We can delete an index 
#### Console Request
```
DELETE /users
```
#### Curl Request
```bash
curl -X DELETE "localhost:9200/users" \
-u elastic:$ELASTIC_PASSWORD
```
#### Response
```json
{
  "acknowledged": true
}
```