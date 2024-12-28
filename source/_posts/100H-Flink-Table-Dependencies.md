---
title: 100H - Flink Table Dependencies
date: 2024-12-28 09:42:38
tags:
---

# Flink API
Flink提供了两大 API：Datastream API 和 Table API & SQL，它们可以单独使用，也可以混合使用，具体取决于你的使用场景：

| 你要使用的 API	| 你需要添加的依赖项|
|---|--|
|DataStream|	flink-streaming-java|
|DataStream Scala 版|	flink-streaming-scala_2.12|
|Table API	|flink-table-api-java|
|Table API Scala 版	|flink-table-api-scala_2.12|
|Table API + DataStream|	flink-table-api-java-bridge|
|Table API + DataStream Scala 版	|flink-table-api-scala-bridge_2.12|

maven scope should be provided

# Flink Connectors

 connector用于连接不同的数据源，在创建表时可以通过connector配置将flink table与数据源连接起来，常见的connector比如：
```xml
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-connector-files</artifactId>
  <version>${flink.version}</version>
</dependency>
```

# Flink Clients

maven scope should be provided

# Flink format

Flink 提供了一套与表连接器（table connector）一起使用的表格式（table format）。表格式是一种存储格式，定义了如何把二进制数据映射到表的列上，常见的依赖比如：
```xml
<dependency>
  <groupId>org.apache.flink</groupId>
  <artifactId>flink-csv</artifactId>
  <version>${flink.version}</version>
</dependency>
```

Flink 支持以下格式：

|Formats	|Supported Connectors|
|-- | --|
|CSV	|Apache Kafka, Upsert Kafka, Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose, Filesystem|
|JSON	|Apache Kafka, Upsert Kafka, Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose, Filesystem, Elasticsearch|
|Apache Avro|	Apache Kafka, Upsert Kafka, Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose, Filesystem|
|Confluent Avro|	Apache Kafka, Upsert Kafka|
|Debezium CDC	|Apache Kafka, Filesystem|
|Canal CDC|	Apache Kafka, Filesystem|
|Maxwell CDC|	Apache Kafka, Filesystem|
|OGG CDC	|Apache Kafka, Filesystem|
|Apache Parquet|	Filesystem|
|Apache ORC|	Filesystem|
|Raw|	Apache Kafka, Upsert Kafka, Amazon Kinesis Data Streams, Amazon Kinesis Data Firehose, Filesystem|

# 运行和打包
如果你想通过简单地执行主类来运行你的作业，你需要 classpath 里有 flink-runtime。对于 Table API 程序，你还需要 flink-table-runtime 和 flink-table-planner-loader，这也意味着这些包不是打包时必须的
