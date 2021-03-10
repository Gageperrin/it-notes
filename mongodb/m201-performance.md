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

### Single Field Indexes

Single field indexes are the simplest indexes in MongoDB with just a single field for keys. It can find a single value for the indexed field, a range of values, use dot notation to index fields in subdocuments, can be used to find several distinct values in a single query.

An index can be created with `db.[collection].createIndex()`.

### Understanding `Explain()`

`explain()` provides more information about queries by helping to identify the index used in the query, if it provides a sort or projection, how selective the index is, and which part of the plan is the most expensive.

In sharded clusters, `explain()` can help with troubleshooting slow queries by providing information such as execution time, numbers of keys read, documents returned, plans selected, and plans rejected.

`Explain()` can also be used through MongoDB Compass.

### Sorting with Indexes

Documents can be sorted in memory or with an index. In-memory sorts have to read the document from disk into RAM and sort it there which is expensive on memory and can be aborted if it exceeds memory limits. Indexes allow documents to be fetched in a pre-sorted order rather than sorting in memory.

### Querying on Compound Indexes

A compound index is an index on two or more fields. In MongoDB, indexes are b-trees with a flat ordering. Even with two fields, the index is still one-dimensional. Compound indexes can be queried through MongoDB Compass or the Mongo shell.

Index prefixes is a continuous subset of a compound index that starts on the left and moves to the right continuously. The query planner will ignore parts of the index not specified in the prefix.

### When you can sort with Indexes

The simplest way to use compound indexes for sorting is to use the index key pattern as the sort predicate. If the query includes equality conditions on all prefix keys preceding teh sort keys, then an index can be used to both filter and sort a document. Just like a single key pattern, compound indexes can be walked backward by inverting each key. If one key is walked forward and one backward, the query executes a scan first.

### Multikey Indexes

A multi-key index is an index placed on an array field. For each index document, there can at most be one index whose field is an array. Multikey indexes do not support covered queries.

### Partial Indexes

Indexing on a subset of documents can reduce performance and storage costs. This can be done by indexing with a pre-specified filter. This is especially helpful for multi-key indexes where each array entry is given a key. In order to use a partial index, the query must be guaranteed to match a subset of the doucments specified by the filter expression.

Sparse indexes are a special case of partial indexes. They only index documents where the field exists that the query is indexed on. Partial indexes are more expressive than sparse indexes.

Partial indexes have several restrictions. It is not possible to specify both the `partialFilterExpression` and the sparse options. `_id` indexes cannot be partial indexes. Shard key indexes cannot be partial indexes.

### Text Indexes

MongoDB's default query engine support string searches, but this has limitations even if one uses regex. It is possible instead to pass a special text keyword to `createIndex()`. The server scans the text field and creates an index field for every unique use of the specified keyword. It considers both hyphenes and spaces as text delimiters. Each delimited word is treated as a logical OR. the special `textScore` value can be projected onto returned results to assign a score to each result based on its relevance.

Having too many keys will increase the index size, increase the time to build the index, and decrease write performance. To alleviate this compound indexes can be used to reduce indexing strain.

### Collations

Collations allow users to specify language specific ruels for string comparison with rules for letter, case, and accent marks. Collations can be defined at several different levels. Most commonly, it can be defined for a collection which means that all queries and indexes in that collection will use that collation. Creating an index with a different collation from the base collection does not imply overriding the base collection collation.

### Wildcard Indexes

Wildcard indexes are an index type that allows the user to dynamically create indexes on all fields or a selected subset of fields for each document in a collection. Workloads with unpredictable access patterns such as IoT can use wildcard indexes to avoid manually creating indexes. To only index subparts within a field, the wildcard projection option can be used when creating the index. This can include and exclude a set of fields when set to 1 and 0 respectively.

Wildcard indexes are useful for unpredictable workloads, can index all fields in a collection, can use dot notation and wildcard projections to index a subset of fields in each document. It is not a replacement for traditional indexes. Collections with arbitrary query shapes or if there is a need for attribute patterns are good use cases for wildcard indexing.

## Chapter 3: Index Operations

### Building Indexes

A foreground index build is problematic because it locks the database. Background index builds are slower because of their incremental approach but do not lock the database in the same way. The index structure built using this method is also less efficient.

After 4.2, these problems have been resolved with a hybrid index build with the performance of a foreground index build with the non-locking capability of a background index build.

### Query Plans

Query plans are formed when a query comes into the database. It usually follows an `IXSCAN`, `FETCH`, and `SCAN` sequence, but it depends on the structure of the index and the data. If the server is restarted, the amount of work performed by the first portion of a query is excessive, or an index is created or dropped, then the plan will be evicted. Query plans are cached.

### Forcing Indexes with `hint()`

Sometimes the query optimizer does not always choose the best index for a given query. This can be overriden with `hint()` by passing in the shape or the name of the index.

### Resource Allocation for Indexes

MongoDB shows not only database size but also collection and index size which is helpful for seeing resource allocation.

The cache will show how many bytes are in the cache, how many bytes are read into the cache, how many pages are requested from or read into the cache, RAM allocation can be surmised from cache details as well as hit/miss ratios. For the fastest processing, ensure that indexes fit entirely in RAM.

To mitigate the effects of BI/BA tools use secondary indexes to reply to requests.

It is important to remember that indexes require resources, they are a part of the database working set, and they need to be taken into consideration in sizing and maintenance strategy.

### Basic Benchmarking

Benchmarking correlates performance indicators and metrics through public test suites (e.g. iBench, Jepsen) or through specific private testing environments. Database server benchmarking can help with identifying metrics such as data set load, writes per second, reads per second, balanced workloads, and read-write ratio.

With self managed benchmarking it is important to set up anti-patterns that avoid data interference like a local laptop, database replace and swap, using mongo shell or `mongoimport` to test, or using default MongoDB parameters. These are all problematic for accurate benchmarking.


## Chapter 4 - CRUD Optimization

### Optimizing CRUD Operations

Optimized queries tend to:
* Put most selective index fields first
* Follow the sequence "Equality, Sort, Range"
* Or be less selective for performance considerations

### Covered Queries

Covered queries are satisfied by index keys and do not touch the documents, very high performance. A query cannot be covered if any of the indexed fields are arrays, any of the indexed fields are embedded documents, or when run against a mongos if the index does not contain the shard key.

### Regex Performance

To reduce the overhead of operators like `$text`, regex can be used with indexing for better perfrmance. At the start of the regex expression include a `^` like `username: /^bob/` to ignore branches of the b-tree that do not begin with "bob". This reduce the number of index keys that need to be examined.

### Aggregation Performance

Realtime processing provides data for applications. Query performance is more important here. Batch processing provides data for analytics so query performance is less important because it is less urgent.

Aggregation queries should use indexes as much as possible. Aggregation queries form a pipeline that transform the data into the target format. Since aggregation piplines move from the first pipeline to the last, once there is a stage that is not able to use an index, then the following stages will not be able to either. The query optimizer will do its best to navigate this. The `explain()` option can be used to analyze the aggregation query.

Limits and sorts should be placed together at the beginning of the query so the server can do a top-k sort which is very performant efficient.

There are some memory constraints. Results are subject to the 16MB document limit. 100MB of RAM per stage. `allowDiskUse` can be set to true to increase query speed but this seriously impacts performance. It is generally only used for batch processing.

## Performance on Clusters

### Performance Considerations in Distributed Systems

A distributed system in MongoDB is a replica or sharded cluster. Distributed systems require taking into account latency, data spread across different nodes, read implications, and write implications.

There are two types of reads in MongoDB: scatter-gather and routed queries. With distributed systems, scatter-gather will have a high performance toll while routed queries have lower latency.

Within a shard cluster, the Mongos will drive the request to designated shards and locally sort then bring the data to the primary data for a merge. After the sort merge, the information will be returned to the client. Skip and limit also follow this procedure.

### Increasing Write Performance with Sharding

Best practices for high write performance with sharding include:
* Vertical and/or horizontal scaling
* Good shard key (high cardinality, low frequency, non-monotonically changing)
* Bulk writes should be unordered (parallel operation)

### Reading from Secondaries

`readPref()` can be used to set read preferences for reading from one of the secondaries instead. This only goes for reads as writes can only be directed to the primary node.

The options are:
* `primary`
* `primaryPreferred`
* `secondary`
* `secondaryPreferred`
* `nearest`

Reading from secondary nodes can return stale data because of its asychronous eventual consistency.

### Replica Sets with Differing Indexes

It is not very common to have an architecture that relies on secondary nodes with specific indexes. It can be used for analytics, reporting, or text searches. To set this up, then secondary nodes should be prevented from becoming primary because the application would begin communicating with a replica set not design for these queries. While indexes can be set on the primary node, in this case it is desirable to avoid operational workload performance impacts.

### Aggregation Pipeline on a Sharded Cluster

In a sharded cluster, since data is partitioned, aggregation pipelines queries can be difficult but MongoDB's query optimizer will navigate routing to avoid scatter-gather queries. It will locate the data and then merge them on a random shard. This does not happen for `$out`, `$facet`, `$lookup`, or `$graphLookup` where the primary shard will do the merging.
