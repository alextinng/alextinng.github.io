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
