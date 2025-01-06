# Elasticsearch Python Integration

This project demonstrates how to use Elasticsearch with the Python Elasticsearch client (elasticsearch-py) to build a powerful search and analytics application.

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Getting Started](#getting-started)
  - [Connecting to Elasticsearch](#connecting-to-elasticsearch)
  - [Creating an Index](#creating-an-index)
  - [Indexing Documents](#indexing-documents)
  - [Searching Documents](#searching-documents)
  - [Updating Documents](#updating-documents)
  - [Deleting Documents](#deleting-documents)
- [Advanced Features](#advanced-features)
  - [Bulk Operations](#bulk-operations)
  - [Aggregations](#aggregations)
  - [Asynchronous Client](#asynchronous-client)
- [Use Cases](#use-cases)
- [Conclusion](#conclusion)

## Introduction

Elasticsearch is a distributed, open-source search and analytics engine built on top of Apache Lucene. It is designed for real-time search, full-text search, and analytics. This project demonstrates how to integrate Elasticsearch with Python using the official `elasticsearch-py` library.

## Prerequisites

Before you begin, ensure you have the following installed:

### Elasticsearch:
- Download and install Elasticsearch from [here](https://www.elastic.co/downloads/elasticsearch).
- Start Elasticsearch by running:

```bash
./bin/elasticsearch
```

By default, Elasticsearch runs on [http://localhost:9200](http://localhost:9200).

### Python:
- Ensure Python 3.6+ is installed. Download it from [here](https://www.python.org/downloads/).

### Elasticsearch Python Library:
- Install the library using pip:

```bash
pip install elasticsearch
```

## Installation

1. Clone this repository:

```bash
git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name
```

2. Install the required Python packages:

```bash
pip install -r requirements.txt
```

## Getting Started

### Connecting to Elasticsearch

```python
from elasticsearch import Elasticsearch

# Connect to a local Elasticsearch instance
es = Elasticsearch("http://localhost:9200")

# Check if the cluster is running
if es.ping():
    print("Connected to Elasticsearch!")
else:
    print("Could not connect to Elasticsearch.")
```

### Creating an Index

```python
index_name = "my_index"

# Create an index with custom mapping
mapping = {
    "mappings": {
        "properties": {
            "title": {"type": "text"},
            "description": {"type": "text"},
            "timestamp": {"type": "date"}
        }
    }
}

es.indices.create(index=index_name, body=mapping)
```

### Indexing Documents

```python
document = {
    "title": "Introduction to Elasticsearch",
    "description": "Elasticsearch is a distributed search engine.",
    "timestamp": "2023-10-01"
}

# Index the document
es.index(index=index_name, id=1, body=document)
```

### Searching Documents

```python
query = {
    "query": {
        "match": {
            "title": "Elasticsearch"
        }
    }
}

response = es.search(index=index_name, body=query)

# Print search results
for hit in response["hits"]["hits"]:
    print(hit["_source"])
```

### Updating Documents

```python
update_body = {
    "doc": {
        "description": "Elasticsearch is a powerful distributed search and analytics engine."
    }
}

es.update(index=index_name, id=1, body=update_body)
```

### Deleting Documents

```python
es.delete(index=index_name, id=1)
```

## Advanced Features

### Bulk Operations

Perform multiple indexing, updating, or deleting operations in a single request:

```python
from elasticsearch.helpers import bulk

actions = [
    {"_index": index_name, "_id": 2, "_source": {"title": "Bulk Insert 1", "description": "First bulk document"}},
    {"_index": index_name, "_id": 3, "_source": {"title": "Bulk Insert 2", "description": "Second bulk document"}}
]

bulk(es, actions)
```

### Aggregations

Perform analytics on your data:

```python
aggregation_query = {
    "size": 0,
    "aggs": {
        "avg_timestamp": {
            "avg": {"field": "timestamp"}
        }
    }
}

response = es.search(index=index_name, body=aggregation_query)
print(response["aggregations"])
```

### Asynchronous Client

Use `AsyncElasticsearch` for non-blocking operations:

```python
from elasticsearch import AsyncElasticsearch

async def main():
    es = AsyncElasticsearch("http://localhost:9200")
    response = await es.search(index=index_name, body=query)
    print(response)
```

## Use Cases

- **Search Applications**: Build full-text search for websites or applications.
- **Log Analysis**: Use with Logstash and Kibana for log aggregation and visualization.
- **E-commerce**: Implement product search with filters and sorting.
- **Data Analytics**: Perform real-time analytics on large datasets.

## Conclusion

This project demonstrates how to integrate Elasticsearch with Python using the `elasticsearch-py` library. Elasticsearch is a powerful tool for search and analytics, and this library makes it easy to use in Python applications.

For more information, refer to the official documentation:
- [Elasticsearch Documentation](https://www.elastic.co/guide/index.html)
- [Elasticsearch Python Client Documentation](https://elasticsearch-py.readthedocs.io/)
