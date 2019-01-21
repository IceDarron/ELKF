# ELKF
#### ELKF简介
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

#### 架构图

ELKF架构可以根据实际情况自行搭配，可以只是用ELK，或者增加消息队列在L和E端。EL可以建成集群等。
本次，搭建的架构如下（网上找的一张比较好的图，本人懒不爱画）：

![Image text](https://github.com/IceDarron/ELKF/blob/master/Image/ELKF_Architecture.png)

elasticsearch：

![Image text](https://github.com/IceDarron/ELKF/blob/master/Image/elasticsearch_theory.png)

logstash：

![Image text](https://github.com/IceDarron/ELKF/blob/master/Image/logstash_theory.png)

filebeat：

![Image text](https://github.com/IceDarron/ELKF/blob/master/Image/filebeat_theory.png)

#### 参考文献

+ logstash：https://blog.csdn.net/wfs1994/article/details/80862225
+ filebeat：https://www.cnblogs.com/aresxin/p/8035137.html
+ 部署：https://blog.csdn.net/fenglailea/article/details/52486471
+ 部署：https://segmentfault.com/a/1190000012858853

#### 部署遇到的一些问题及解决方案
+ windows下，ELKF属于开箱即食的，基本只需要解压启动就好。
+ filebeat可以直接输出到es，这样可以在kibana的log下直接查看，是一个时序页面，更为清晰。
+ es在linux下部署，需要解决两个问题，一个是创建用户，另一个是调整linux各项参数，参考：https://blog.csdn.net/qq_33363618/article/details/78882827
+ ELKF参数配置最好参照官方文档配置，均使用yaml格式。
+ logstash配置参数时注意，logstash.yml是setting。logstash-sample.conf是config。测试情况下conf不需要配置，可以直接在控制台输入输出。如果要设置input源，则根据自己选择的源创建单独的config进行配置。例如，如果修改logstah的ip port需要在logstash.yml（setting）配置。如果需要配置filebeat则，创建filebeat-logstash.conf，编写input filter output。
+ 注意启动书序，es --》 logstash --》 filebeat，kibana在es启动之后即可。建议如此顺序，减少不必要日志报错。顺序错，不会影响数据等。

#### 启动和停止命令
