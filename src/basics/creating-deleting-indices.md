## 创建和删除索引

在 Elasticsearch 中，索引是存储数据的基本单位。有效地创建和删除索引可以帮助管理数据的存储和提高搜索性能。本节将介绍如何创建和删除 Elasticsearch 索引，包括基本的操作和常见的配置选项。

### 1. 创建索引

#### 1.1. 基本创建索引

要在 Elasticsearch 中创建一个新索引，可以使用 HTTP PUT 请求。例如，创建一个名为 `my-index` 的索引：

```sh
curl -X PUT "localhost:9200/my-index?pretty"
```

该命令将创建一个默认设置的索引。如果没有特别指定设置和映射，Elasticsearch 会使用默认的配置。

#### 1.2. 创建带有自定义设置和映射的索引

创建索引时，可以指定自定义设置和映射，以满足特定的数据需求。例如，以下命令创建一个名为 `my-index` 的索引，并设置了分片数、副本数和字段映射：

```sh
curl -X PUT "localhost:9200/my-index?pretty" -H 'Content-Type: application/json' -d'
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 2
  },
  "mappings": {
    "properties": {
      "title": {
        "type": "text"
      },
      "price": {
        "type": "float"
      }
    }
  }
}'
```

在这个示例中：
- `number_of_shards`：指定索引的主分片数为 3。
- `number_of_replicas`：指定每个主分片的副本数为 2。
- `mappings`：定义了索引中文档的字段及其类型，例如 `title` 字段的类型是 `text`，`price` 字段的类型是 `float`。

### 2. 删除索引

#### 2.1. 基本删除索引

删除索引将永久删除索引及其所有的数据，因此请在执行此操作前谨慎确认。要删除一个名为 `my-index` 的索引，可以使用 HTTP DELETE 请求：

```sh
curl -X DELETE "localhost:9200/my-index?pretty"
```

该命令将删除 `my-index` 索引及其所有的数据。请注意，删除操作是不可逆的，一旦执行将无法恢复。

#### 2.2. 批量删除索引

如果需要删除多个索引，可以使用通配符。例如，删除所有以 `log-` 开头的索引：

```sh
curl -X DELETE "localhost:9200/log-*?pretty"
```

该命令将删除所有符合 `log-*` 模式的索引。请确保通配符模式正确，以避免意外删除重要的索引。

### 3. 索引的验证

在创建和删除索引后，可以通过以下命令验证索引是否已成功创建或删除：

- **查看现有索引**：

  ```sh
  curl -X GET "localhost:9200/_cat/indices?v"
  ```

  该命令将列出所有现有的索引及其状态。

- **查看特定索引的详细信息**：

  ```sh
  curl -X GET "localhost:9200/my-index?pretty"
  ```

  该命令将返回 `my-index` 索引的详细信息，包括设置和映射。

- **确认索引是否被删除**：

  ```sh
  curl -X GET "localhost:9200/my-index?pretty"
  ```

  如果索引已被删除，您将收到 404 错误响应。

通过正确创建和删除索引，您可以有效地管理 Elasticsearch 中的数据，并优化系统的性能和存储。了解索引的基本操作对于确保数据的有效管理至关重要。

