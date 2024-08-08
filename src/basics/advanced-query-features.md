### 高级查询功能

在 Elasticsearch 中，高级查询功能可以帮助您实现更复杂和灵活的搜索需求。这些功能包括但不限于多字段查询、模糊查询、邻近查询和高亮显示等。

#### 1. 多字段查询

- **`multi_match` 查询**：用于在多个字段中执行搜索。适合于需要在多个字段中查找匹配项的情况。

  ```json
  GET /my-index/_search
  {
    "query": {
      "multi_match": {
        "query": "Elasticsearch",
        "fields": ["title", "description", "content"]
      }
    }
  }
  ```

  这个查询将在 `title`、`description` 和 `content` 字段中搜索包含“Elasticsearch”的文档。

- **`function_score` 查询**：用于修改查询结果的评分，通过函数来调整每个文档的分数。

  ```json
  GET /my-index/_search
  {
    "query": {
      "function_score": {
        "query": {
          "match": { "title": "Elasticsearch" }
        },
        "functions": [
          {
            "filter": { "term": { "category": "search" } },
            "weight": 2
          }
        ]
      }
    }
  }
  ```

  这个查询将对 `title` 字段中匹配“Elasticsearch”的文档应用额外的权重，如果文档的 `category` 字段为“search”。

#### 2. 模糊查询

- **`fuzzy` 查询**：用于处理拼写错误或类似的变体。通过允许一定程度的字符差异来匹配文档。

  ```json
  GET /my-index/_search
  {
    "query": {
      "fuzzy": {
        "title": {
          "value": "Elasticserch",
          "fuzziness": "AUTO"
        }
      }
    }
  }
  ```

  这个查询将匹配 `title` 字段中与“Elasticserch”相似的文档。

#### 3. 邻近查询

- **`match_phrase` 查询**：用于查找在特定字段中按指定顺序出现的短语。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match_phrase": {
        "content": "real-time search"
      }
    }
  }
  ```

  这个查询将匹配 `content` 字段中包含“real-time search”短语的文档。

- **`match_phrase_prefix` 查询**：用于查找以指定短语开头的文档，适用于自动补全等场景。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match_phrase_prefix": {
        "content": "real-time"
      }
    }
  }
  ```

  这个查询将匹配 `content` 字段中以“real-time”开头的短语的文档。

#### 4. 高亮显示

- **高亮显示**：用于突出显示查询结果中匹配的部分，便于用户查看相关信息。

  ```json
  GET /my-index/_search
  {
    "query": {
      "match": { "content": "Elasticsearch" }
    },
    "highlight": {
      "fields": {
        "content": {}
      }
    }
  }
  ```

  这个查询将返回 `content` 字段中包含“Elasticsearch”的文档，并高亮显示匹配的部分。

#### 5. 自定义评分

- **`custom_score` 查询**：通过自定义评分逻辑来调整文档的相关性得分。

  ```json
  GET /my-index/_search
  {
    "query": {
      "custom_score": {
        "query": {
          "match": { "title": "Elasticsearch" }
        },
        "script_score": {
          "script": {
            "source": "Math.log(2 + doc['popularity'].value)"
          }
        }
      }
    }
  }
  ```

  这个查询将使用脚本来调整 `title` 字段中匹配“Elasticsearch”的文档的得分，得分基于 `popularity` 字段的对数值。

#### 6. 矢量搜索（向量空间模型）

- **`dense_vector` 查询**：用于基于向量进行相似度搜索，适用于机器学习和深度学习应用。

  ```json
  GET /my-index/_search
  {
    "query": {
      "knn": {
        "field": "embedding_vector",
        "query_vector": [0.5, 0.1, 0.4],
        "k": 10
      }
    }
  }
  ```

  这个查询将基于 `embedding_vector` 字段中存储的向量进行 K 最近邻搜索，找到与查询向量最相似的 10 个文档。

通过掌握这些高级查询功能，您可以更精准地检索和处理 Elasticsearch 中的数据，以满足各种复杂的搜索需求。