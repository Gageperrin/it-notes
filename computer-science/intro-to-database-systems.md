# Introduction to Database Systems

These are my notes for Cornell University's CS 4320 course Introduction to Database Systems as provided here on [YouTube](https://www.youtube.com/watch?v=4cWkVbC2bNE).

In the past many applications managed their own data using files but this can often cause problems with data overlap and inconsistencies across applications. Database Management Systems were developed to respond to these problems particularly through Structured Query Language (SQL).

## SQL Introduction

The SQL language is used to issue commands to the DBMS. The SQL standard has been around since the 70s and it has many features.

There are several command types:
* DDL (Data Definition Language) to define admissible database content (schema)
* DML (Data Manipulation Language) to change and retrieve database content
* TCL (Transaction Control Language) groups SQL commands (transactions)
* DCL (Data Control Language) assigns data access rights.

A database schema defines relations with their schemata through columns and their types. It defines constraints to restrict admissible content. These constraints can be placed on single relations or link multiple relations.

A schema definition in SQL takes the syntax `CREATE TABLE <table> (<table-def>)`. The table definition consists of comma separated column definitions with column name and data type. DBMS enforces integrity constraints. These constraints can be added to tables via `ALTER TABLE` or can be defined when creating the table.

A primary key constraint refers to a single table and identifies a subset of columns as key columns Fixing values for key columns must identify the row. No two rows have the same values in key columns. For example, `ALTER TABLE <table> ADD CONSTRAINT Primary Key (<key-cols>)`. To include foreign syntax, add the foreign key as a constraint instead and append `REFERENCES <table-2> (<pkey-columns>)`.

One can insert data into a table, delete data from a table, update data in a table, and analyze data in a table. [PAUSE at 00:43:15].
