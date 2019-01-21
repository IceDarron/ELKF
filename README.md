# ELKF
ELKF简介
===
ELKF是 Elasticsearch + Logstash + Kibana + FileBeat 四个组件的组合。本文版本为6.5.4，简单介绍一些概念，架构等。

首先介绍一下，elastic。官网（https://www.elastic.co/）。elastic公司最早最出名的产品应该是elasticsearch（简称es）。

es是基于lucene的一个分布式的基于RESTful接口的搜索和分析引擎。也是一个json格式的文档型存储引擎。接口方便，可以迅速的搜索非常大的数据量（PB级）。

es知识elastic公司的一个产品，logstash、kibana、beats都是起产品。这些都已开源为主。

对于每个组件的具体功能，概念术语，适用场景等，建议直接阅读官网，非常详细。国内可以参考elastic中文社区。

#### 官方文档：
elasticsearch：

+ https://elasticsearch.cn/
+ https://www.elastic.co/guide/en/elasticsearch/reference/current/index.html

Filebeat：

+ https://www.elastic.co/cn/products/beats/filebeat
+ https://www.elastic.co/guide/en/beats/filebeat/5.6/index.html

Logstash：

+ https://www.elastic.co/cn/products/logstash
+ https://www.elastic.co/guide/en/logstash/5.6/index.html

Kibana：

+ https://www.elastic.co/cn/products/kibana
+ https://www.elastic.co/guide/en/kibana/5.5/index.html

#### 概述这四个组件在ELKF中的作用：

+ Elasticsearch：分布式搜索引擎
+ Logstash：日志的搜集、分析、过滤日志的工具，支持大量的数据获取方式。老的版本组合一般工作方式为C/S架构，即logstash的client部署在目标机器收集日志，server负责获取数据后的过滤修改等，并发送到es。
+ Kibana：Web界面，可以帮助汇总、分析和搜索重要数据日志。
+ Filebeat：隶属于Beats。用于搜集文件数据，go语言编写。现在代替了logstash的client端，节省了资源。

#### ELKF可以满足一个完整的集中式日志系统的各项需求：

+ 收集－能够采集多种来源的日志数据
+ 传输－能够稳定的把日志数据传输到中央系统
+ 存储－如何存储日志数据
+ 分析－可以支持 UI 分析
+ 警告－能够提供错误报告，监控机制
