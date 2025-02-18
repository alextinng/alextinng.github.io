---
title: '[100H] Flink Table Startup'
date: 2025-01-13 20:29:20
tags: Flink
---

## Entrypoint of flink table programs
A table environment is the base class, entry point, and central context for creating Table and SQL API programs.

responsibilites:
+ Connecting to external systems.
+ Registering and retrieving Tables and other meta objects from a catalog.
+ Executing SQL statements.
+ Offering further configuration options.

configuration of table environment
| config | comment |
|--|--|
|RUNTIME_MODE | Runtime execution mode of DataStream programs(default in streaming mode, can be set in batch mode). Among other things, this controls task scheduling, network shuffle behavior, and time semantics. |
|builtInCatalogName| Specifies the name of the initial catalog to be created when instantiating a TableEnvironment. This catalog is an in-memory catalog that will be used to store all temporary objects (e.g. from TableEnvironment.createTemporaryView(String, Table) or TableEnvironment.createTemporarySystemFunction(String, UserDefinedFunction)) that cannot be persisted because they have no serializable representation.|
|builtInDatabaseName | Specifies the name of the default database in the initial catalog to be created when instantiating a TableEnvironment. This database is an in-memory database that will be used to store all temporary objects (e.g. from TableEnvironment.createTemporaryView(String, Table) or TableEnvironment.createTemporarySystemFunction(String, UserDefinedFunction)) that cannot be persisted because they have no serializable representation.|
|classLoader| Specifies the classloader to use in the planner for operations related to code generation, UDF loading, operations requiring reflections on user classes, discovery of factories. By default, this is configured using Thread.currentThread().getContextClassLoader(). Modify the ClassLoader only if you know what you're doing.|

## How to operate data?

The Table object is the core abstraction of the Table API, the Table object describes a pipeline of data transformations, a Table object can actually be considered as a view in SQL terms, you can operate data upon a Table object.

four tips you must know:
+ It does not contain the data itself in any way
+ It describes how to read data from a DynamicTableSource and how to eventually write data to a DynamicTableSink.
+ The declared pipeline can be printed, optimized, and eventually executed in a cluster.
+ The pipeline can work with bounded or unbounded streams which enables both streaming and batch scenarios.

Java example(how to read table data): 
```java
TableEnvironment tableEnv = TableEnvironment.create(...);
Table table = tableEnv.from("MyTable")
  .select($("colA").trim(), $("colB").plus(12));
table.execute().print();
```
