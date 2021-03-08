# M201 - MongoDB Performance

These are my notes from the official MongoDB course M201 MongoDB Performance which can be found for free [here](https://university.mongodb.com/mercury/M201/).

## Chapter 1: Introduction

## Hardware Considerations and Configurations

A computer can be represented by the Von Neumann Architecture which includes memory, I/O, and a CPU which contains ALU (arithmetic logic unit), registers, and control logic.

The expansion of memory and RAM technologies shifted databases to be oriented around memory rather than I/O. Aggregation, index traversing, write operations, query engine, and connections occur in memory.

CPU is used for storage engine and concurrency modeling. MongoDB will try to use all available CPU cores to mediate requests.

If one document portion is constantly being written to, multiple CPUs will not help performance as threads cannot work in parallel. 

MongoDB persists information on server disks. The IOPS measures how quickly data is read or written and how quickly the persistence layer responds to queries.

RAID architectures can be used as well for redundancy of read and write operations. RAID 10 is recommended for MongoDB with higher redundancy. RAID 10 consists of a RAID 0 branched between two RAID 1 structures. RAID 5 and RAID 6 are discouraged as they do not provide sufficient performance. RAID 0 is discouraged because of its low availability and fault tolerance.

Networking and its configured security can also affect database performance.

## Chapter 2: MongoDB Indexes

Without indexes, MongoDB searches would have an order of N operation, i.e. O(N), with a linear running time. Indexes can speed this up but also decrease, write, update, and delete performance. MongoDB uses a B tree to store indexes. 

How data is stored on MongoDB depends on the storage engine used, whether it is MQL, MongoDB Document Data Model, Wired Tiger, or others.
