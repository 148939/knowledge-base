# Elasticsearch 搜索核心

## 1. 核心概述（官方定义）

Elasticsearch 是一个基于 Lucene 的开源分布式搜索和分析引擎。它提供了一个分布式、多租户能力的全文搜索引擎，具有 RESTful web 接口和无架构 JSON 文档。Elasticsearch 是用 Java 开发的。

### 核心概念
- **Index**：索引，文档的集合
- **Document**：文档，可被索引的基本信息单元
- **Field**：字段，文档的属性
- **Type**：类型（ES 7.x 已废弃）
- **Mapping**：映射，定义文档结构和字段类型
- **Shard**：分片，索引的分区
- **Replica**：副本，分片的备份

---

## 2. 核心知识点（结构化层级）

### 2.1 Elasticsearch 架构

#### 节点类型
- **Master Node**：主节点，管理集群状态
- **Data Node**：数据节点，存储数据和执行操作
- **Client Node**：协调节点，路由请求
- **Ingest Node**：摄入节点，预处理文档

#### 集群协调
- 集群发现
- 主节点选举
- 分片分配
- 集群健康检查

### 2.2 索引与文档

#### 索引管理
- 创建索引
- 删除索引
- 索引别名
- 索引模板

#### 文档操作
- 创建文档
- 更新文档
- 删除文档
- 批量操作

#### 文档结构
- JSON 格式
- 字段类型
- 嵌套文档
- 父子文档

### 2.3 映射与分析

#### 字段类型
- 文本类型（text、keyword）
- 数字类型（long、integer、short、byte、double、float）
- 日期类型（date）
- 布尔类型（boolean）
- 二进制类型（binary）
- 范围类型（range）
- 复杂类型（object、nested）

#### 分析器
- 字符过滤器（Character Filters）
- 分词器（Tokenizer）
- 词元过滤器（Token Filters）
- 内置分析器
- 自定义分析器

### 2.4 搜索 API

#### 基本搜索
- URI 搜索
- Request Body 搜索
- 分页查询
- 排序

#### 查询 DSL
- 叶子查询（match、term、range）
- 复合查询（bool、dis_max、constant_score）
- 全文搜索
- 精确匹配
- 范围查询

#### 聚合
- 指标聚合（sum、avg、min、max、stats）
- 桶聚合（terms、histogram、date_histogram、range）
- 管道聚合（avg_bucket、sum_bucket、max_bucket）
- 嵌套聚合

### 2.5 高亮与建议

#### 高亮
- 字段高亮
- 高亮标签
- 片段配置
- 多字段高亮

#### 搜索建议
- 词项建议（Term Suggester）
- 短语建议（Phrase Suggester）
- 完成建议（Completion Suggester）
- 上下文建议

### 2.6 性能优化

#### 索引优化
- 分片数规划
- 副本配置
- 索引刷新间隔
- 合并策略

#### 查询优化
- 避免深度分页
- 使用过滤器缓存
- 合理使用聚合
- 查询重写

#### 硬件优化
- 内存配置
- 存储选择
- 网络优化
- JVM 调优

---

## 3. 官方标准语法 / API

### 3.1 REST API

```bash
# 创建索引
PUT /my-index
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1
  },
  "mappings": {
    "properties": {
      "title": { "type": "text" },
      "content": { "type": "text" },
      "author": { "type": "keyword" },
      "publish_date": { "type": "date" },
      "views": { "type": "integer" }
    }
  }
}

# 删除索引
DELETE /my-index

# 添加文档
POST /my-index/_doc/1
{
  "title": "Elasticsearch 入门",
  "content": "Elasticsearch 是一个强大的搜索和分析引擎...",
  "author": "张三",
  "publish_date": "2024-01-15",
  "views": 1000
}

# 获取文档
GET /my-index/_doc/1

# 更新文档
POST /my-index/_update/1
{
  "doc": {
    "views": 1500
  }
}

# 删除文档
DELETE /my-index/_doc/1

# 批量操作
POST /_bulk
{ "index": { "_index": "my-index", "_id": "2" } }
{ "title": "第二篇文章", "content": "内容...", "author": "李四", "publish_date": "2024-01-16", "views": 500 }
{ "index": { "_index": "my-index", "_id": "3" } }
{ "title": "第三篇文章", "content": "内容...", "author": "王五", "publish_date": "2024-01-17", "views": 2000 }
```

### 3.2 查询 DSL

```bash
# 匹配查询
GET /my-index/_search
{
  "query": {
    "match": {
      "title": "Elasticsearch"
    }
  }
}

# 多字段匹配
GET /my-index/_search
{
  "query": {
    "multi_match": {
      "query": "搜索",
      "fields": ["title", "content"]
    }
  }
}

# 精确匹配
GET /my-index/_search
{
  "query": {
    "term": {
      "author": "张三"
    }
  }
}

# 范围查询
GET /my-index/_search
{
  "query": {
    "range": {
      "views": {
        "gte": 1000,
        "lte": 2000
      }
    }
  }
}

# 复合查询
GET /my-index/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "title": "Elasticsearch" } }
      ],
      "filter": [
        { "range": { "publish_date": { "gte": "2024-01-01" } } }
      ],
      "must_not": [
        { "term": { "author": "李四" } }
      ]
    }
  }
}

# 分页和排序
GET /my-index/_search
{
  "from": 0,
  "size": 10,
  "sort": [
    { "publish_date": "desc" },
    { "views": "desc" }
  ]
}

# 高亮
GET /my-index/_search
{
  "query": {
    "match": { "content": "搜索" }
  },
  "highlight": {
    "fields": {
      "content": {}
    }
  }
}
```

### 3.3 聚合查询

```bash
# 指标聚合
GET /my-index/_search
{
  "size": 0,
  "aggs": {
    "total_views": { "sum": { "field": "views" } },
    "avg_views": { "avg": { "field": "views" } },
    "min_views": { "min": { "field": "views" } },
    "max_views": { "max": { "field": "views" } },
    "stats_views": { "stats": { "field": "views" } }
  }
}

# 桶聚合
GET /my-index/_search
{
  "size": 0,
  "aggs": {
    "by_author": {
      "terms": { "field": "author" },
      "aggs": {
        "total_views": { "sum": { "field": "views" } }
      }
    }
  }
}

# 日期直方图
GET /my-index/_search
{
  "size": 0,
  "aggs": {
    "by_month": {
      "date_histogram": {
        "field": "publish_date",
        "calendar_interval": "month"
      },
      "aggs": {
        "avg_views": { "avg": { "field": "views" } }
      }
    }
  }
}

# 嵌套聚合
GET /my-index/_search
{
  "size": 0,
  "aggs": {
    "by_author": {
      "terms": { "field": "author" },
      "aggs": {
        "by_year": {
          "date_histogram": {
            "field": "publish_date",
            "calendar_interval": "year"
          },
          "aggs": {
            "total_views": { "sum": { "field": "views" } }
          }
        }
      }
    }
  }
}
```

### 3.4 Java 客户端

```java
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.client.RequestOptions;
import org.elasticsearch.client.RestHighLevelClient;
import org.elasticsearch.common.xcontent.XContentType;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import java.io.IOException;

@Service
public class ElasticsearchService {
    
    @Autowired
    private RestHighLevelClient client;
    
    // 索引文档
    public void indexDocument(String index, String id, String json) throws IOException {
        IndexRequest request = new IndexRequest(index)
            .id(id)
            .source(json, XContentType.JSON);
        
        IndexResponse response = client.index(request, RequestOptions.DEFAULT);
        System.out.println("Indexed: " + response.getId());
    }
    
    // 搜索文档
    public SearchResponse searchDocuments(String index, String keyword) throws IOException {
        SearchRequest searchRequest = new SearchRequest(index);
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        
        sourceBuilder.query(QueryBuilders.matchQuery("title", keyword));
        sourceBuilder.from(0);
        sourceBuilder.size(10);
        
        searchRequest.source(sourceBuilder);
        
        return client.search(searchRequest, RequestOptions.DEFAULT);
    }
    
    // 聚合查询
    public SearchResponse aggregateByAuthor(String index) throws IOException {
        SearchRequest searchRequest = new SearchRequest(index);
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        
        sourceBuilder.size(0);
        sourceBuilder.aggregation(
            AggregationBuilders.terms("by_author").field("author")
                .subAggregation(AggregationBuilders.sum("total_views").field("views"))
        );
        
        searchRequest.source(sourceBuilder);
        
        return client.search(searchRequest, RequestOptions.DEFAULT);
    }
}

// 配置类
@Configuration
public class ElasticsearchConfig {
    
    @Bean
    public RestHighLevelClient client() {
        final ClientConfiguration clientConfiguration = 
            ClientConfiguration.builder()
                .connectedTo("localhost:9200")
                .build();
        
        return RestClients.create(clientConfiguration).rest();
    }
}
```

---

## 4. 官方示例代码（可运行）

### 4.1 博客搜索系统

```java
// 文章实体
@Document(indexName = "blog")
public class Article {
    
    @Id
    private String id;
    
    @Field(type = FieldType.Text, analyzer = "ik_max_word")
    private String title;
    
    @Field(type = FieldType.Text, analyzer = "ik_max_word")
    private String content;
    
    @Field(type = FieldType.Keyword)
    private String author;
    
    @Field(type = FieldType.Date)
    private Date publishDate;
    
    @Field(type = FieldType.Integer)
    private Integer views;
    
    @Field(type = FieldType.Keyword)
    private List<String> tags;
    
    // getters and setters
}

// 仓库接口
public interface ArticleRepository extends ElasticsearchRepository<Article, String> {
    
    List<Article> findByTitle(String title);
    
    List<Article> findByAuthor(String author);
    
    List<Article> findByTitleContainingOrContentContaining(String title, String content);
    
    @Query("{\"bool\": {\"must\": [{\"match\": {\"title\": \"?0\"}}]}}")
    List<Article> searchByTitle(String title);
}

// 服务类
@Service
public class ArticleService {
    
    @Autowired
    private ArticleRepository repository;
    
    @Autowired
    private ElasticsearchRestTemplate template;
    
    // 保存文章
    public Article save(Article article) {
        return repository.save(article);
    }
    
    // 搜索文章
    public List<Article> search(String keyword) {
        NativeSearchQueryBuilder queryBuilder = new NativeSearchQueryBuilder();
        queryBuilder.withQuery(
            QueryBuilders.multiMatchQuery(keyword, "title", "content")
        );
        queryBuilder.withHighlightFields(
            new HighlightBuilder.Field("title"),
            new HighlightBuilder.Field("content")
        );
        
        SearchHits<Article> searchHits = template.search(
            queryBuilder.build(), Article.class
        );
        
        return searchHits.getSearchHits().stream()
            .map(SearchHit::getContent)
            .collect(Collectors.toList());
    }
    
    // 按作者统计
    public Map<String, Long> statsByAuthor() {
        NativeSearchQueryBuilder queryBuilder = new NativeSearchQueryBuilder();
        queryBuilder.withAggregations(
            AggregationBuilders.terms("by_author").field("author")
        );
        
        SearchHits<Article> searchHits = template.search(
            queryBuilder.build(), Article.class
        );
        
        Terms terms = searchHits.getAggregations().get("by_author");
        return terms.getBuckets().stream()
            .collect(Collectors.toMap(
                Terms.Bucket::getKeyAsString,
                Terms.Bucket::getDocCount
            ));
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 索引设计

1. **合理分片**
   - 分片数不宜过多或过少
   - 每个分片大小控制在 20-40GB
   - 根据数据量增长规划

2. **副本配置**
   - 生产环境至少 1 个副本
   - 副本数不超过数据节点数
   - 平衡性能和可靠性

3. **映射设计**
   - 明确字段类型
   - 合理使用 text 和 keyword
   - 避免动态映射

### 5.2 查询优化

1. **避免深度分页**
   - 使用 search_after
   - 使用 scroll
   - 考虑限制结果数

2. **合理使用过滤器**
   - 过滤器不计算评分
   - 过滤器结果被缓存
   - 优先使用 filter

3. **聚合优化**
   - 减少聚合层级
   - 合理设置 size
   - 使用 composite 聚合

### 5.3 性能调优

1. **索引优化**
   - 调整 refresh_interval
   - 批量索引文档
   - 合理设置副本数

2. **内存配置**
   - 堆内存不超过 32GB
   - 保留足够的系统内存
   - 开启内存锁定

3. **存储优化**
   - 使用 SSD
   - 合理配置段合并
   - 使用索引生命周期管理

---

## 6. 官方文档参考链接

- [Elasticsearch 官方文档](https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html)
- [Elasticsearch 入门指南](https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html)
- [Elasticsearch Java 客户端](https://www.elastic.co/guide/en/elasticsearch/client/java-rest/current/index.html)
- [Spring Data Elasticsearch](https://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/)
- [Elasticsearch 查询 DSL](https://www.elastic.co/guide/en/elasticsearch/reference/current/query-dsl.html)
- [Elasticsearch 聚合](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-aggregations.html)
