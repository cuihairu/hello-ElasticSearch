### 案例分析：统计聚合与分组聚合

在 Elasticsearch 中，统计聚合和分组聚合是两种常用的聚合方式，用于从数据中提取有用的信息和洞察。以下是两个实际案例，展示如何使用统计聚合和分组聚合来分析数据。

#### 案例 1：统计聚合

**目标**：计算产品的价格统计信息，包括平均价格、总和、最小值、最大值和标准差。

**数据集**：假设我们有一个名为 `products` 的索引，其中包含多个产品的价格信息。

**示例查询**：

```json
POST /products/_search
{
  "size": 0,
  "aggs": {
    "price_stats": {
      "stats": {
        "field": "price"
      }
    }
  }
}
```

**解释**：

- `size: 0`：不返回任何文档，仅返回聚合结果。
- `price_stats`：聚合的名称。
- `stats`：统计聚合类型，计算字段的基本统计信息。
- `field: "price"`：指定要计算统计信息的字段。

**结果**：

```json
{
  "aggregations": {
    "price_stats": {
      "count": 100,
      "min": 10.0,
      "max": 1000.0,
      "avg": 150.5,
      "sum": 15050.0,
      "std_deviation": 200.25
    }
  }
}
```

**分析**：

- `count`：产品总数。
- `min`：最低价格。
- `max`：最高价格。
- `avg`：平均价格。
- `sum`：总价格。
- `std_deviation`：价格的标准差。

#### 案例 2：分组聚合

**目标**：按产品类别对价格进行分组，并计算每个类别的平均价格。

**数据集**：同样使用 `products` 索引，其中包含产品的类别信息和价格信息。

**示例查询**：

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

**解释**：

- `size: 0`：不返回任何文档，仅返回聚合结果。
- `by_category`：桶聚合的名称，用于按类别分组。
- `terms`：按 `category.keyword` 字段的唯一值对文档进行分组。
- `average_price`：度量聚合的名称，用于计算每个类别的平均价格。
- `avg`：计算字段的平均值。
- `field: "price"`：指定要计算平均值的字段。

**结果**：

```json
{
  "aggregations": {
    "by_category": {
      "buckets": [
        {
          "key": "Electronics",
          "doc_count": 50,
          "average_price": {
            "value": 200.0
          }
        },
        {
          "key": "Books",
          "doc_count": 30,
          "average_price": {
            "value": 15.0
          }
        },
        {
          "key": "Clothing",
          "doc_count": 20,
          "average_price": {
            "value": 50.0
          }
        }
      ]
    }
  }
}
```

**分析**：

- `buckets`：每个桶代表一个类别。
- `key`：类别名称。
- `doc_count`：该类别下的产品数量。
- `average_price`：该类别下的平均价格。

#### 总结

- **统计聚合**：用于计算单个字段的统计信息，如平均值、总和、最小值、最大值和标准差。
- **分组聚合**：用于将数据分组到不同的桶中，根据字段的唯一值进行分组，并对每个组计算统计信息，如平均价格。

这两个聚合案例展示了如何利用 Elasticsearch 的聚合功能从数据中提取有用的信息和洞察，帮助用户进行数据分析和决策。