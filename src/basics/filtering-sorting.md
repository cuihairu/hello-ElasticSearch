### 过滤和排序

在 Elasticsearch 中，过滤和排序是优化搜索结果的重要操作。通过使用这些功能，您可以精确控制检索到的数据并确保结果按照特定的顺序返回。

#### 1. 过滤

过滤用于排除或包括特定的数据子集，不会影响文档的相关性评分。通常用于精确匹配和范围查询。

- **`term` 过滤器**：用于精确匹配单个值，适用于不需要分析的字段。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "filter": [
          { "term": { "status": "active" } }
        ]
      }
    }
  }
  ```

  这个查询将返回 `status` 字段值为“active”的所有文档。

- **`range` 过滤器**：用于范围匹配，适合于日期、数字等范围数据的过滤。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "filter": [
          { "range": { "date": { "gte": "2023-01-01", "lte": "2023-12-31" } } }
        ]
      }
    }
  }
  ```

  这个查询将返回 `date` 字段在 2023 年 1 月 1 日到 2023 年 12 月 31 日之间的文档。

- **`exists` 过滤器**：用于过滤出包含指定字段的文档。

  ```json
  GET /my-index/_search
  {
    "query": {
      "bool": {
        "filter": [
          { "exists": { "field": "author" } }
        ]
      }
    }
  }
  ```

  这个查询将返回包含 `author` 字段的所有文档。

- **`bool` 过滤器**：用于组合多个过滤条件，以实现复杂的过滤逻辑。

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

#### 2. 排序

排序用于控制检索结果的显示顺序。可以基于字段值的升序或降序进行排序，以满足不同的需求。

- **按字段排序**：可以基于单个字段的值对结果进行排序。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match_all": {}
    },
    "sort": [
      { "date": { "order": "desc" } }
    ]
  }
  ```

  这个查询将返回所有文档，并按 `date` 字段的降序排列。

- **多字段排序**：可以根据多个字段进行排序，以实现更复杂的排序逻辑。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match_all": {}
    },
    "sort": [
      { "date": { "order": "desc" } },
      { "popularity": { "order": "asc" } }
    ]
  }
  ```

  这个查询将首先根据 `date` 字段的降序排序，如果 `date` 字段相同，则根据 `popularity` 字段的升序排序。

- **自定义排序**：对于某些特殊排序需求，可以使用脚本排序。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match_all": {}
    },
    "sort": [
      {
        "_script": {
          "type": "number",
          "script": {
            "source": "doc['popularity'].value * 2"
          },
          "order": "desc"
        }
      }
    ]
  }
  ```

  这个查询使用脚本对文档进行排序，脚本根据 `popularity` 字段的值计算排序分数。

通过有效使用过滤和排序功能，您可以优化搜索查询的性能，并确保检索到的结果符合业务需求和用户期望。