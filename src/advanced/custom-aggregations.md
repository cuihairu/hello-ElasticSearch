### 自定义聚合

在 Elasticsearch 中，自定义聚合允许用户根据特定需求创建复杂的聚合操作。通过自定义聚合，用户可以实现特定的业务逻辑，或者组合多个标准聚合来获得更复杂的结果。以下是如何使用 Elasticsearch 的自定义聚合进行数据分析的示例。

#### 示例 1：自定义聚合的基本结构

自定义聚合通常是通过嵌套多个标准聚合来实现的。以下示例展示了如何使用自定义聚合计算每个类别的销售总额，并按销售额降序排列：

**目标**：按产品类别分组，并计算每个类别的销售总额，然后按总额排序。

**数据集**：假设我们有一个名为 `sales` 的索引，其中包含产品类别和销售金额字段。

**示例查询**：

```json
POST /sales/_search
{
  "size": 0,
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category.keyword",
        "size": 10
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "amount"
          }
        }
      }
    }
  }
}
```

**解释**：

- `size: 0`：不返回文档，仅返回聚合结果。
- `by_category`：按 `category.keyword` 字段对文档进行分组。
- `total_sales`：计算每个类别的销售总额。
- `sum`：求和聚合，计算字段 `amount` 的总和。

**结果**：

```json
{
  "aggregations": {
    "by_category": {
      "buckets": [
        {
          "key": "Electronics",
          "doc_count": 50,
          "total_sales": {
            "value": 50000.0
          }
        },
        {
          "key": "Books",
          "doc_count": 30,
          "total_sales": {
            "value": 15000.0
          }
        },
        {
          "key": "Clothing",
          "doc_count": 20,
          "total_sales": {
            "value": 10000.0
          }
        }
      ]
    }
  }
}
```

#### 示例 2：自定义聚合与嵌套聚合

有时候需要进行更复杂的自定义聚合操作，例如在每个分组中进行进一步的分析。以下示例展示了如何在每个类别中按时间进行销售趋势分析：

**目标**：按产品类别分组，并在每个类别中计算每月的销售总额。

**数据集**：假设 `sales` 索引包含日期字段 `date` 和销售金额字段 `amount`。

**示例查询**：

```json
POST /sales/_search
{
  "size": 0,
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category.keyword"
      },
      "aggs": {
        "monthly_sales": {
          "date_histogram": {
            "field": "date",
            "calendar_interval": "month"
          },
          "aggs": {
            "total_sales": {
              "sum": {
                "field": "amount"
              }
            }
          }
        }
      }
    }
  }
}
```

**解释**：

- `by_category`：按 `category.keyword` 字段分组。
- `monthly_sales`：在每个类别中，按月份对销售数据进行分组。
- `date_histogram`：以月份为时间间隔对 `date` 字段进行直方图聚合。
- `total_sales`：计算每个月的销售总额。

**结果**：

```json
{
  "aggregations": {
    "by_category": {
      "buckets": [
        {
          "key": "Electronics",
          "doc_count": 50,
          "monthly_sales": {
            "buckets": [
              {
                "key_as_string": "2024-01-01T00:00:00.000Z",
                "key": 1672531200000,
                "doc_count": 20,
                "total_sales": {
                  "value": 20000.0
                }
              },
              {
                "key_as_string": "2024-02-01T00:00:00.000Z",
                "key": 1675209600000,
                "doc_count": 30,
                "total_sales": {
                  "value": 30000.0
                }
              }
            ]
          }
        }
      ]
    }
  }
}
```

#### 示例 3：复杂的自定义聚合

如果需要实现更复杂的自定义聚合，可以结合多种聚合类型。以下示例展示了如何计算每个类别的销售总额，并为每个类别生成销售金额的百分位数统计信息：

**目标**：按产品类别分组，计算销售总额，并计算销售金额的 50%、90% 和 99% 百分位数。

**数据集**：使用 `sales` 索引，包含 `category` 和 `amount` 字段。

**示例查询**：

```json
POST /sales/_search
{
  "size": 0,
  "aggs": {
    "by_category": {
      "terms": {
        "field": "category.keyword"
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "amount"
          }
        },
        "sales_percentiles": {
          "percentiles": {
            "field": "amount",
            "percents": [50, 90, 99]
          }
        }
      }
    }
  }
}
```

**解释**：

- `by_category`：按 `category.keyword` 字段进行分组。
- `total_sales`：计算每个类别的销售总额。
- `sales_percentiles`：计算 `amount` 字段的 50%、90% 和 99% 百分位数。

**结果**：

```json
{
  "aggregations": {
    "by_category": {
      "buckets": [
        {
          "key": "Electronics",
          "doc_count": 50,
          "total_sales": {
            "value": 50000.0
          },
          "sales_percentiles": {
            "values": {
              "50.0": 150.0,
              "90.0": 300.0,
              "99.0": 500.0
            }
          }
        }
      ]
    }
  }
}
```

#### 总结

- **自定义聚合**：通过结合多个标准聚合实现特定的业务需求或分析逻辑。
- **嵌套聚合**：可以在一个聚合的基础上进行进一步的聚合，例如按时间进行分组。
- **复杂自定义聚合**：结合多种聚合类型来获取详细的统计信息和洞察。

这些示例展示了如何在 Elasticsearch 中使用自定义聚合来满足各种分析需求。