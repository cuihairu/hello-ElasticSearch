### 使用查询 DSL

Elasticsearch 的查询 DSL（Domain Specific Language）是构建和执行搜索查询的核心工具。它基于 JSON 的格式，允许用户编写复杂且灵活的查询语句。以下是如何使用查询 DSL 进行基本和复杂查询的示例。

#### 1. 基本查询

基本查询是查询 DSL 的基础，用于匹配文档中的内容。最常见的基本查询是 `match` 查询，它用于全文搜索。

- **`match` 查询**：用于全文搜索，在指定字段中查找包含查询词的文档。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match": {
        "title": "Elasticsearch"
      }
    }
  }
  ```

  这个查询将返回 `title` 字段中包含“Elasticsearch”词的所有文档。

- **`term` 查询**：用于精确匹配，不进行分词处理。

  ```json
  GET /my-index/_search
  {
    "query": {
      "term": {
        "status": "active"
      }
    }
  }
  ```

  这个查询将返回 `status` 字段为“active”的所有文档。

#### 2. 复合查询

复合查询允许您组合多个查询条件，以便进行更复杂的搜索。

- **`bool` 查询**：允许结合多个查询条件，如 `must`（必须满足）、`should`（应该满足）、`must_not`（不应满足）等。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "must": [
          { "match": { "title": "Elasticsearch" } },
          { "range": { "date": { "gte": "2023-01-01" } } }
        ]
      }
    }
  }
  ```

  这个查询返回 `title` 字段包含“Elasticsearch”的文档，并且 `date` 字段的值在 2023 年 1 月 1 日之后。

- **`must` 子句**：查询中所有的 `must` 条件都必须匹配。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "must": [
          { "match": { "title": "Elasticsearch" } },
          { "match": { "content": "search engine" } }
        ]
      }
    }
  }
  ```

  这个查询将返回 `title` 字段中包含“Elasticsearch”，且 `content` 字段中包含“search engine”的文档。

- **`should` 子句**：查询中至少一个 `should` 条件需要匹配。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "should": [
          { "match": { "title": "Elasticsearch" } },
          { "match": { "title": "Search Engine" } }
        ]
      }
    }
  }
  ```

  这个查询将返回 `title` 字段中包含“Elasticsearch”或“Search Engine”的文档。

- **`must_not` 子句**：查询中所有的 `must_not` 条件都不应匹配。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "must_not": [
          { "term": { "status": "inactive" } }
        ]
      }
    }
  }
  ```

  这个查询将返回 `status` 字段不是“inactive”的所有文档。

#### 3. 过滤器

过滤器用于限制查询结果集，通常用于精确匹配，且不会影响相关性评分。

- **`filter` 子句**：用于在 `bool` 查询中应用过滤条件。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "filter": [
          { "term": { "status": "active" } },
          { "range": { "date": { "gte": "2023-01-01" } } }
        ]
      }
    }
  }
  ```

  这个查询将返回 `status` 字段为“active”的文档，并且 `date` 字段的值在 2023 年 1 月 1 日之后。

#### 4. 排序和分页

- **排序**：您可以根据字段对查询结果进行排序。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match": {
        "title": "Elasticsearch"
      }
    },
    "sort": [
      { "date": { "order": "desc" } }
    ]
  }
  ```

  这个查询将返回 `title` 字段中包含“Elasticsearch”的文档，并按 `date` 字段的降序排列结果。

- **分页**：通过 `from` 和 `size` 参数控制结果的分页。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match": {
        "title": "Elasticsearch"
      }
    },
    "from": 0,
    "size": 10
  }
  ```

  这个查询将返回前 10 条 `title` 字段中包含“Elasticsearch”的文档。

通过以上示例，您可以灵活地构建和执行各种查询，以满足不同的搜索需求。更多关于查询 DSL 的详细信息，请参考 [Elasticsearch 查询 DSL 文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)。