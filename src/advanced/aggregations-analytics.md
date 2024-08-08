### 聚合和分析

在 Elasticsearch 中，聚合和分析功能允许用户从大量数据中提取有用的信息和见解。聚合用于计算统计信息、生成报告和进行复杂的数据分析，而分析则侧重于对数据进行深入的检查和解释。

#### 1. **聚合基础**

聚合功能可以对数据进行分组、计算和汇总，以提取有价值的信息。Elasticsearch 提供了多种类型的聚合，如统计聚合、分组聚合和自定义聚合。

##### **常见的聚合类型**

- **计数聚合（Count Aggregation）**：计算文档的数量。
- **平均值聚合（Avg Aggregation）**：计算指定字段的平均值。
- **最大值和最小值聚合（Max/Min Aggregation）**：计算指定字段的最大值和最小值。
- **直方图聚合（Histogram Aggregation）**：将数据分组到指定的区间中，并计算每个区间的文档数。
- **范围聚合（Range Aggregation）**：将数据分组到定义的范围内，并计算每个范围的文档数。

##### **示例：计算平均价格**

以下是一个示例查询，计算 `price` 字段的平均值：

```json
POST /products/_search
{
  "size": 0,
  "aggs": {
    "average_price": {
      "avg": {
        "field": "price"
      }
    }
  }
}
```

在这个示例中，`avg` 聚合用于计算 `price` 字段的平均值。

#### 2. **案例分析：统计聚合、分组聚合**

##### **统计聚合**

统计聚合用于计算数据的统计信息，如总数、平均值、最大值和最小值。

###### **示例：统计聚合**

```json
POST /products/_search
{
  "size": 0,
  "aggs": {
    "stats_price": {
      "stats": {
        "field": "price"
      }
    }
  }
}
```

在这个示例中，`stats` 聚合提供了 `price` 字段的总数、平均值、最大值、最小值和标准差。

##### **分组聚合**

分组聚合用于将数据按某个字段进行分组，并对每个组进行进一步的聚合操作。

###### **示例：按类别分组并计算每个类别的平均价格**

```json
POST /products/_search
{
  "size": 0,
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category.keyword"
      },
      "aggs": {
        "average_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

在这个示例中，`terms` 聚合用于按 `category` 字段进行分组，并对每个类别计算 `price` 字段的平均值。

#### 3. **自定义聚合**

Elasticsearch 允许用户创建自定义聚合，以满足特定的分析需求。

##### **示例：自定义聚合**

```json
POST /products/_search
{
  "size": 0,
  "aggs": {
    "custom_aggregation": {
      "bucket_script": {
        "buckets_path": {
          "avgPrice": "average_price"
        },
        "script": "params.avgPrice * 2"
      }
    }
  }
}
```

在这个示例中，`bucket_script` 聚合用于对 `average_price` 聚合结果进行自定义计算（将平均价格乘以 2）。

### 总结

- **聚合基础**：用于计算统计信息、生成报告和进行数据汇总，支持多种聚合类型。
- **统计聚合和分组聚合**：提供了对数据进行汇总和分析的功能，包括计算统计值和按字段分组。
- **自定义聚合**：允许用户根据需要创建特定的聚合操作，以满足特定的分析需求。

聚合和分析功能使 Elasticsearch 能够处理复杂的数据查询和报告生成，是大数据分析的重要工具。