### Lucene 查询语法

Lucene 查询语法是 Elasticsearch 的基础查询语言，提供了灵活且强大的搜索功能。它允许用户构造复杂的查询来匹配和检索文档。以下是 Lucene 查询语法的一些常用功能和示例。

#### 1. **基本查询**

- **单词匹配**

  查询包含特定单词的文档。

  ```json
  GET /index/_search
  {
    "query": {
      "query_string": {
        "query": "search_term"
      }
    }
  }
  ```

- **短语匹配**

  查询包含特定短语的文档。

  ```json
  GET /index/_search
  {
    "query": {
      "query_string": {
        "query": "\"exact phrase\""
      }
    }
  }
  ```

#### 2. **布尔查询**

布尔查询允许组合多个查询条件，通过 `must`、`should` 和 `must_not` 来定义文档的匹配规则。

- **`must` 查询**（匹配所有条件）

  ```json
  GET /index/_search
  {
    "query": {
      "bool": {
        "must": [
          { "match": { "field1": "value1" } },
          { "match": { "field2": "value2" } }
        ]
      }
    }
  }
  ```

- **`should` 查询**（匹配任一条件）

  ```json
  GET /index/_search
  {
    "query": {
      "bool": {
        "should": [
          { "match": { "field1": "value1" } },
          { "match": { "field2": "value2" } }
        ]
      }
    }
  }
  ```

- **`must_not` 查询**（排除特定条件）

  ```json
  GET /index/_search
  {
    "query": {
      "bool": {
        "must_not": [
          { "match": { "field": "excluded_value" } }
        ]
      }
    }
  }
  ```

#### 3. **通配符查询**

通配符查询允许使用 `*`（匹配零个或多个字符）和 `?`（匹配一个字符）来进行模糊匹配。

- **通配符匹配**

  ```json
  GET /index/_search
  {
    "query": {
      "wildcard": {
        "field": "term*"
      }
    }
  }
  ```

- **模糊查询**

  ```json
  GET /index/_search
  {
    "query": {
      "fuzzy": {
        "field": "term",
        "fuzziness": "AUTO"
      }
    }
  }
  ```

#### 4. **范围查询**

范围查询用于查找字段值在特定范围内的文档。

- **数值范围查询**

  ```json
  GET /index/_search
  {
    "query": {
      "range": {
        "field": {
          "gte": 10,
          "lte": 100
        }
      }
    }
  }
  ```

- **日期范围查询**

  ```json
  GET /index/_search
  {
    "query": {
      "range": {
        "date_field": {
          "gte": "2024-01-01",
          "lte": "2024-12-31"
        }
      }
    }
  }
  ```

#### 5. **模糊查询**

模糊查询允许查找与指定词相似的文档，处理拼写错误和变体。`fuzziness` 参数定义允许的编辑距离。

- **模糊匹配**

  ```json
  GET /index/_search
  {
    "query": {
      "fuzzy": {
        "field": {
          "value": "term",
          "fuzziness": "AUTO"
        }
      }
    }
  }
  ```

- **自定义编辑距离**

  ```json
  GET /index/_search
  {
    "query": {
      "fuzzy": {
        "field": {
          "value": "term",
          "fuzziness": 2
        }
      }
    }
  }
  ```

#### 6. **短语查询**

短语查询用于查找包含指定短语的文档，并且短语中的单词按顺序出现。

- **短语匹配**

  ```json
  GET /index/_search
  {
    "query": {
      "match_phrase": {
        "field": "exact phrase"
      }
    }
  }
  ```

#### 7. **分组查询**

分组查询（`parent/child` 关系）用于处理具有层次结构的文档。

- **父子文档关系**

  ```json
  GET /index/_search
  {
    "query": {
      "has_parent": {
        "parent_type": "parent",
        "query": {
          "match": {
            "parent_field": "value"
          }
        }
      }
    }
  }
  ```

#### 8. **其他查询**

- **`exists` 查询**（检查字段是否存在）

  ```json
  GET /index/_search
  {
    "query": {
      "exists": {
        "field": "field_name"
      }
    }
  }
  ```

- **`prefix` 查询**（匹配以指定前缀开头的词）

  ```json
  GET /index/_search
  {
    "query": {
      "prefix": {
        "field": "prefix*"
      }
    }
  }
  ```

#### 总结

Lucene 查询语法为 Elasticsearch 提供了强大的查询能力，支持从基本匹配到复杂的布尔查询和模糊匹配。通过灵活使用这些查询语法，可以实现高效的全文搜索和信息检索。