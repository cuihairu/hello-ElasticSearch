### 分词器和分析器

在 Elasticsearch 中，**分词器**和**分析器**是处理和优化文本数据的关键工具。它们分别负责将文本拆分成词项，并对这些词项应用进一步的处理，以提高搜索的相关性和性能。

#### 分词器（Tokenizer）

**分词器**的主要作用是将输入的文本拆分成一系列词项。每个词项代表一个单独的词或符号，这些词项将用于索引和查询。Elasticsearch 提供了多种内置的分词器，也支持自定义分词器。

- **标准分词器（Standard Tokenizer）**：这是默认的分词器，根据 Unicode 字符分类进行分词，适用于大多数情况。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "tokenizer": {
          "standard_tokenizer": {
            "type": "standard"
          }
        }
      }
    }
  }
  ```

- **空白分词器（Whitespace Tokenizer）**：基于空白字符（如空格、制表符）进行分词，将文本按空白字符拆分成词项。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "tokenizer": {
          "whitespace_tokenizer": {
            "type": "whitespace"
          }
        }
      }
    }
  }
  ```

- **关键词分词器（Keyword Tokenizer）**：将整个输入文本作为单一词项，不进行拆分。适用于不需要分词的场景。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "tokenizer": {
          "keyword_tokenizer": {
            "type": "keyword"
          }
        }
      }
    }
  }
  ```

- **正则表达式分词器（Pattern Tokenizer）**：根据正则表达式拆分文本。可以自定义正则表达式以满足特定需求。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "tokenizer": {
          "pattern_tokenizer": {
            "type": "pattern",
            "pattern": "\\W+"
          }
        }
      }
    }
  }
  ```

#### 分析器（Analyzer）

**分析器**是对文本进行进一步处理的组件。它结合了分词器和过滤器，允许用户对词项进行额外的处理，例如小写化、去除停用词等。

- **标准分析器（Standard Analyzer）**：这是默认的分析器，使用标准分词器和一系列默认的过滤器（如小写化和停用词过滤器）处理文本。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "analyzer": {
          "standard_analyzer": {
            "type": "standard"
          }
        }
      }
    }
  }
  ```

- **自定义分析器（Custom Analyzer）**：允许用户自定义分词器和过滤器，以创建符合特定需求的分析器。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "analyzer": {
          "custom_analyzer": {
            "type": "custom",
            "tokenizer": "standard",
            "filter": ["lowercase", "stop"]
          }
        }
      }
    }
  }
  ```

  在这个示例中，自定义分析器使用了标准分词器和两个过滤器：小写化过滤器（将所有词项转换为小写）和停用词过滤器（移除常见的无意义词）。

- **同义词分析器（Synonym Analyzer）**：通过同义词过滤器处理文本，将不同的词汇映射到同一词项，以提高搜索的相关性。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "analyzer": {
          "synonym_analyzer": {
            "type": "custom",
            "tokenizer": "standard",
            "filter": ["synonym_filter", "lowercase"]
          }
        },
        "filter": {
          "synonym_filter": {
            "type": "synonym",
            "synonyms": ["quick, fast", "jumps, leaps"]
          }
        }
      }
    }
  }
  ```

- **词干分析器（Stemmer Analyzer）**：使用词干过滤器将词汇还原为其基本形式，例如将“running”还原为“run”。

  ```json
  PUT /my-index
  {
    "settings": {
      "analysis": {
        "analyzer": {
          "stemmer_analyzer": {
            "type": "custom",
            "tokenizer": "standard",
            "filter": ["lowercase", "porter_stem"]
          }
        }
      }
    }
  }
  ```

  在这个示例中，自定义分析器使用了词干过滤器（Porter Stemming Filter）来处理词汇。

通过选择和配置合适的分词器和分析器，您可以显著提高 Elasticsearch 对文本数据的处理能力，使搜索结果更加准确和相关。