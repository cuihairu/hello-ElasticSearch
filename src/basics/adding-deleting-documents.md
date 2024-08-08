
## 添加和删除文档

在 Elasticsearch 中，添加和删除文档是数据操作的基本任务。这些操作使您能够将数据插入索引中或从中移除不再需要的数据。本小节将详细介绍如何在 Elasticsearch 中执行这些操作。

### 1. 添加文档

#### 1.1. 使用 `index` API 添加文档

`index` API 可以用于添加新文档或更新已有文档。您可以通过指定文档 ID 来添加文档，Elasticsearch 将自动处理文档的存储和索引。

- **添加文档示例**：

  ```sh
  curl -X POST "localhost:9200/my-index/_doc/1" -H 'Content-Type: application/json' -d'
  {
    "title": "My First Document",
    "content": "This is the content of the document."
  }'
  ```

  在此命令中：
  - `localhost:9200` 是 Elasticsearch 服务器的地址。
  - `my-index` 是要将文档添加到的索引名称。
  - `_doc` 是文档类型（在新版本中，通常可以忽略）。
  - `1` 是文档的 ID。Elasticsearch 会用这个 ID 存储文档。
  - 请求体中的 JSON 数据是要存储的文档内容。

#### 1.2. 自动生成文档 ID

如果不指定文档 ID，Elasticsearch 会自动生成一个唯一的 ID。

- **自动生成 ID 示例**：

  ```sh
  curl -X POST "localhost:9200/my-index/_doc" -H 'Content-Type: application/json' -d'
  {
    "title": "Document with Auto-ID",
    "content": "This document ID is auto-generated."
  }'
  ```

  在这个示例中，没有指定 ID，Elasticsearch 会为该文档生成一个唯一的 ID，并返回该 ID。

### 2. 删除文档

#### 2.1. 使用 `delete` API 删除文档

要删除文档，可以使用 `delete` API，您需要提供文档的 ID 以及要删除文档的索引名称。

- **删除文档示例**：

  ```sh
  curl -X DELETE "localhost:9200/my-index/_doc/1"
  ```

  在此命令中：
  - `localhost:9200` 是 Elasticsearch 服务器的地址。
  - `my-index` 是包含要删除文档的索引名称。
  - `_doc` 是文档类型（在新版本中，通常可以忽略）。
  - `1` 是要删除的文档 ID。

#### 2.2. 批量删除文档

您可以使用 `bulk` API 执行批量删除操作，这在删除大量文档时特别有用。

- **批量删除示例**：

  ```sh
  curl -X POST "localhost:9200/_bulk" -H 'Content-Type: application/json' -d'
  { "delete": { "_index": "my-index", "_id": "1" } }
  { "delete": { "_index": "my-index", "_id": "2" } }
  '
  ```

  在这个示例中，`_bulk` API 被用来同时删除 ID 为 `1` 和 `2` 的文档。每个删除操作由两个 JSON 对象组成，第一个对象指定操作类型（`delete`），第二个对象包含目标文档的索引和 ID。

### 3. 添加和删除文档的最佳实践

- **确保文档唯一性**：在添加文档时，选择合适的 ID 或让 Elasticsearch 自动生成唯一 ID，以避免覆盖现有数据。
- **处理文档版本**：如果您使用相同的 ID 添加新文档，请注意旧文档将被覆盖。可以考虑使用版本控制来管理文档的不同版本。
- **优化删除操作**：定期清理不再需要的文档，避免索引变得庞大和低效。

通过掌握这些添加和删除文档的基本操作，您可以有效地管理 Elasticsearch 中的数据，确保系统的高效和准确性。

---

如果您有任何问题或需要更多的信息，可以参考 [Elasticsearch 官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html)。