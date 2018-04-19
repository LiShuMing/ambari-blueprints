### 包含组件

* Zookeeper;
* HDFS;
* YARN;
* MapReduce;
* HBase;
* Kafka;
* Spark2;
* Hive;
* Ambari Metrics;

### 注意
* 傻瓜式安装可能安装Hive失败，亲测第一次安装失败，是由于Hive设定的Mysql metastore数据库默认是localhost导致的，该问题可以通过安装后修改指定元数据库地址重启修复；