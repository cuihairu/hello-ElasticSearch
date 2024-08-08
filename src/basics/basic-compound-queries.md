### 基本查询和复合查询

在 Elasticsearch 中，查询 DSL 提供了强大的工具来执行基本和复杂的查询操作。理解这些查询的使用方法是高效检索数据的关键。

#### 1. 基本查询

基本查询用于简单的搜索任务，如查找包含特定词或短语的文档。它们通常用于直接匹配字段的内容。

- **`match` 查询**：用于全文检索，可以对文本进行分词和分析。适合于模糊匹配和对全文的搜索。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match": {
        "content": "Elasticsearch search engine"
      }
    }
  }
  ```

  这个查询将返回 `content` 字段中包含“Elasticsearch search engine”的文档。`match` 查询会对查询词进行分词处理，从而匹配包含这些词的文档。

- **`term` 查询**：用于精确匹配，不会对查询词进行分词处理。适合于关键字、ID 或其他不需要分析的字段。

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

  这个查询将返回 `status` 字段值为“active”的文档。`term` 查询适用于对字段值进行精确匹配的场景。

- **`range` 查询**：用于范围查询，适合于日期、数字等范围数据的匹配。

  ```json
  GET /my-index/_search
  {
    "query": {
      "range": {
        "date": {
          "gte": "2023-01-01",
          "lte": "2023-12-31"
        }
      }
    }
  }
  ```

  这个查询将返回 `date` 字段在 2023 年内的所有文档。

#### 2. 复合查询

复合查询允许将多个查询条件组合在一起，支持更复杂的搜索需求。复合查询通常用于需要多个条件匹配的场景。

- **`bool` 查询**：组合多个查询子句，如 `must`、`should`、`must_not` 等，以实现复杂的查询逻辑。

  - **`must` 子句**：查询中的所有 `must` 条件都必须匹配。类似于逻辑“与”操作。

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

    这个查询将返回 `title` 字段包含“Elasticsearch”，并且 `date` 字段值在 2023 年 1 月 1 日之后的文档。

  - **`should` 子句**：查询中的至少一个 `should` 条件需要匹配。类似于逻辑“或”操作。

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

    这个查询将返回 `title` 字段包含“Elasticsearch”或“Search Engine”的文档。

  - **`must_not` 子句**：查询中的所有 `must_not` 条件都不应匹配。类似于逻辑“非”操作。

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

  - **`filter` 子句**：在 `bool` 查询中应用的过滤条件，不会影响相关性评分，只用于精确匹配。

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

    这个查询将返回 `status` 字段为“active”，并且 `date` 字段值在 2023 年 1 月 1 日之后的所有文档。

通过理解和应用这些基本查询和复合查询，您可以灵活地对 Elasticsearch 中的数据进行检索和筛选，以满足不同的查询需求。