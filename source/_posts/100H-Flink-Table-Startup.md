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

## Managing metadata

This Catalog interface is responsible for reading and writing metadata such as database/table/views/UDFs from a registered catalog. It connects a registered catalog and Flink's Table API. This interface only processes permanent metadata objects. In order to process temporary objects, a catalog can also implement the TemporaryOperationListener interface.

## How to operate data?

The Table object is the core abstraction of the Table API, the Table object describes a pipeline of data transformations, a Table object can actually be considered as a view in SQL terms, the Table object defined method for operating data, such as select field, filter data, join other table and so on.

Things you must know:
+ The Table object does not contain the data itself in any way
+ You can get table schema by getSchema method
+ The Table object describes how to read data from a DynamicTableSource and how to eventually write data to a DynamicTableSink.
+ The declared pipeline can be printed, optimized, and eventually executed in a cluster.
+ The pipeline can work with bounded or unbounded streams which enables both streaming and batch scenarios.

Table method for operating data:
+ using ```Table select(Expression... fields);``` to perform a selection operation.
+ using ```Table filter(Expression predicate);``` or ```Table where(Expression predicate);``` to filter table data, similar to a SQL WHERE clause
+ using ```GroupedTable groupBy(Expression... fields);``` to group the elements on some grouping keys. Use this before a selection with aggregations to perform the aggregation on a per-group basis. Similar to a SQL GROUP BY statement.
+ using ```Table join(Table right);``` to join another table, similar to a SQL join statement
+ more at [Table API](https://nightlies.apache.org/flink/flink-docs-master/docs/dev/table/tableapi/)

## Call method

### Built-In method

First of all, let's introduce Module interface, modules define a set of metadata, including functions, user defined types, operators, rules, etc. Metadata from modules are regarded as built-in or system metadata that users can take advantages of, default implementation is CoreModule, it contains all of Flink’s system (built-in) functions and is loaded and enabled by default.

Modules allow users to extend Flink’s built-in objects, such as defining functions that behave like Flink built-in functions. They are pluggable, and while Flink provides a few pre-built modules, users can write their own.
For example, users can define their own geo functions and plug them into Flink as built-in functions to be used in Flink SQL and Table APIs. Another example is users can load an out-of-shelf Hive module to use Hive built-in functions as Flink built-in functions.
Furthermore, a module can provide built-in table source and sink factories which disable Flink’s default discovery mechanism based on Java’s Service Provider Interfaces (SPI), or influence how connectors of temporary tables should be created without a corresponding catalog.

### DB functions

Using listFunctions method in Catalog interface to list the names of all functions in the given database. An empty list is returned if none is registered.
