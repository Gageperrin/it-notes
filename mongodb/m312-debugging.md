# M312 Diagnostics and Debugging

These are my notes from the official MongoDB course M312 Diagnostics and Debugging which can be found for free [here](https://university.mongodb.com/courses/M312/about).

## Chapter 1: Introduction

The course will cover various diagnostic and debugging tools while providing some common real-world troubleshooting scenarios as a heuristic for understnading the debugging process.

## Chapter 2: Tooling Overview

### MongoDB Tools

* `mongostat` - shows incoming operations.
* `mongotop` - shows collections being read from and written to.
* `mongoreplay` - replay and record network traffic.
* `mongo` - the shell.
* `profiler` - logs queries.
* `compass` - GUI for MongoDB.
* `mtools` - a suite of scripts for testing environments and parsing log files.

### Server Logs

Server logs are the first step for troubleshooting server issues. By default it is stored at `/var/log/mongodb/mongodb.log`.

Server log components:
* ACCESS
* COMMAND
* CONTROL
* GEO
* INDEX
* NETWORK
* QUERY
* REPL
* SHARDING
* STORAGE
* JOURNAL
* WRITE

Logs also feature levels of severity:
* `F` - fatal
* `E` - error
* `W` - warning
* `I` - informational (for verbosity 0)
* `D` - debug (for verbosity greater than 0)

### `currentOp()` and `killOp()`

`db.currentOp()` describes what the server is currently doing such as the duration of the current operation and how long the thread is. It is genearlly bad at finding short-lived processes.

`db.killOp` can be used to kill read operations running on more than one shard in a cluster using the `opid` value.

### `serverStatus()`

`db.serverStatus()` includes information about the instance, asserts, connections, locking, operation stats, security, replication stats, storage engine stats, and metrics.

Asserts is a document that reports the number of assertion errors raised since the server process started.

### Profiler

The database profiler provides granular data about read and write operations as well as cursors. It stores one document per operation into the `system.profile` capped collection. It does this by turning a read operation into a read plus insert, and a write operation into a write plus insert. It should not be left on because it of its intensive processing and should only be used rarely in production.

With `db.getProfilingLevel()` it is possible to get the profiling level. At profile level 1, it captures any queries that take longer than 100ms. At level 2, it can capture every query (however Mongo replay can do this too).

### Server Diagnostic Tools

Server diagnostic tools include `mongoldap`, `mongoperf`, `mongoreplay`, `mongostat` and `mongotop`. Mongo LDAP validates LDAP configuration settings.

`mongoreplay` can monitor, record, and replay MongoDB operations and traffic for debugging purposes.

`mongoperf` is a performance testing tools for the server's disk I/O performance.

`mongostat` is a real time statistics tool for mongod or mongos by sampling once per second to get insert, query, update, delete, and other kinds of data.

`mongotop` measures time per second consumed on different namespaces with reading, writing, and their total aggregation per sampling unit.

### Mtools

Mtools is a set of tools designed for debugging. It is open source but not officially supported by MongoDB.

* `mlogfilter` slices log files by time, combines them, filters them for slow queries, finds table scans, shortens log lines, filters by attribtues, and converts the log entries into a JSON format.
* `mloginfo` returns information about log files.
* `mplotqueries` can visualize log files.
* `mlogvis` creates a HTML file for interactive visualization in a web browser.
* `mlaunch` spins up test deployments on the local host.
* `mgenerate` generates random data for testing use.

### Performance Statistics

Performance statistics can be viewed in real time in MongoDB Compass. It includes memory usage, number of operations, number of operational reads and writes, and network activity. Disk space usage is not currently supported.

### `$indexStats`

`$indexStats` shows what indexes are being and can be useful to know what queries are doing. It increments by one, starting at zero when the server starts up or when indexes are created. It is not allowed in transactions and must be the first stage in an aggregation pipeline. It can be used to identify and remove unused indexes.

## Chapter 3: Slow Queries

### Response Time Degradation

Some culprits can lead to response time degradation such as working set exceeding RAM, queries taking longer as the data set grows, growing pool of clients, unbounded array growth, and excessive number of indexes.

Proper indexing can help reduce data query size from O(N) time to O(log(N)) time. Other improvements include providing enough RAM for the working set and optimizing our query results. `mlogvis` can be used to see the execution time for an operation.

### Sudden Drop in Throughput

* Long-running queries (collection scans, poorly anchored regex, or inefficient index usage)
* Index builds
* Write contention

The server logs, `db.currentOp()`, and the profiler will show any long running queries and can help troubleshoot.

Index building is a blocking operation and can take time that can cause slowdown on throughput for queries.

Write contention can be complicated to resolve. When a documetn is updated in the WiredTiger storage engine, it uses a copy-on-write approach. When a new version is being prepared, only the original document is visible to applciations. The update is committed by switching the pointer to the new version in a single CPU operation. This is known as multiversion concurrency control.

However there is write contention when multiple writers are trying to update the same document at the same time. This will lock the document.

### Impact of Application Changes

Detect application changes by keeping historical monitoring data, watching out for spikes in connections, watching out for spikes in ops/s, and spotting long lasting, non-indexed queries. This can easily be done in MongoDB Atlas.

Connection spikes can be caused by exhausting connection pools, incorrect connections to a production environment, analytical workload and reporting tools. Stale operations or incorrect credentials can be responsible for this. 

Query targeting helps identify long-lasting non-indexed queries (very undesirable scenario to have these).

### Using Mtools to Find Slow Queries

Three tools to identify and troubleshoot slow queries:
* `mloginfo` for a summary of query shapes.
* `mplotqueries` to plot queries over time.
* `mlogvis` to plot queries on a browser.

### Fixing Missing Indexes

Options:
* Build the index on the primary from the shell
* Build the index in the background on the primary from the shell
* Build the index with Compass
* Build the index on each member of the replica set (rolling upgrade)
* Use Ops Manager or Cloud Manager to build the index through a rolling upgrade.


## Chapter 4: Connectivity

