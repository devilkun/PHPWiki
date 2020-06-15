## 概念区别

|    Mysql    |               MongoDB                |
| :---------: | :----------------------------------: |
|    库DB     |                 库DB                 |
|   表Table   |           集合collections            |
|    行Row    |            文档documents             |
|  列fields   |              字段fields              |
|   表 Join   |               内嵌文档               |
| 主键id 自增 | 主键（由 MongoDB 提供的默认 key_id） |





|            Mysql             |  ElasticSearch   |
| :--------------------------: | :--------------: |
|             库DB             |    index索引     |
|           表Table            | 文档类型（type） |
|            行Row             | 文档（document） |
|           列fields           |    字段fields    |
|           表 Join            |                  |
| 数据库的组织和结构（Schema） | 映射（mapping）  |



## 使用场景

### Mysql

> 关系型数据  支持事务   不适合频繁更新

### MongoDB

> 非关系型数 不支持事务  适合频繁更新数据的业务场景 比如直播

### ElasticSearch

> 关系非关系均可   不支持事务   中文搜索场景  分词场景    不适合频繁更新
>
> 全文搜索、适用于电商商品搜索、APP搜索、企业内部信息搜索
>
> 日志分析   ELK日志系统
>
> 已经不支持在一个索引下创建多个类型，并且类型概念已经在后续版本中删除

## 读写性能

MongoDB > ElasticSearch > Mysql

MongoDB   读写性能 每秒20万QPS

ElasticSearch  每秒1万QPS

Mysql 每秒3000左右QPS  不适合频繁更新