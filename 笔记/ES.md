Elasticsearch

查询

_search

全文检索

- match模糊查询，但又不太一样 例如“小华”，能查到华为 或 小米
- match_phrase完全匹配

- 分页

  - form    size

- 指定查询数据某些字段

  - _source

- 排序

  - sort  排序关键字
    - "order":  "desc" 降序排序

- 过滤

  - ![img](https://api2.mubu.com/v3/document_image/de765113-428b-427e-b048-fa464efa715f-13454343.jpg)

  gte 大于等于 lte 小于等于

- 高亮显示

  - highlight
    - fields: {}

- 聚合操作

  - aggs
    - price_group:{ "terms" : { "field": "price"}}  按price分组
    - price_avg:{ "avg" : { "field": "price"}} price平均值

- 文档数据
  - _doc
- 映射关系
  - mapping
    - {"type":  "keyword",  "index": true}
    - keyword必须完全匹配， text 是类似模糊查询， index:false 则不能作为条件查询