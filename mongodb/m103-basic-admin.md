# M103: Basic Cluster Administration

These are my notes from the offical MongoDB course M103 which focuses on MongoDB administration. [The course can be found here for free](https://university.mongodb.com/mercury/M103/).

## Chapter 1: The Mongod

Mongod is the main daemon process for MongoDB, the core server of the database to handle requests and persist data.

Default values:
* port: 27017
* dbpath: `/data/db`
* bind_ip: `localhost`
* auth: `disabled`

Common commands and options:
* `mongod --help`
* `mongod --dbpath <directory path>`
* `mongod --port <port number>`
* `mongod --config /etc/mongod.conf`
* `mongod --auth`
* `mongod --bind_ip`
* `mongod --replSet`
* `mongod --keyFile`
* `mongod --sslPEMKey`
* `mongod --sslCAKey`
* `mongod --sslMode`
* `mongod --fork`

### The MongoDB Configuration File

Each command line option has a configuration file option:

* `storage.dbPath`
* `systemLog.path` and `systemLog.destination`
* `net.bind_ip`
* `replication.replSetName`
* `security.keyFile`
* `net.ssl.sslPEMKey`
* `net.ssl.sslCAKey`
* `net.sslMode`
* `processManagement.fork`

### File Structure

MongoDB uses WiredTiger, a NoSQL open source extensible plugin for data management. WiredTiger syncs the journal to the disk every 100ms. `diagnostic.data` and log files can be used for diagnostic troubleshooting.

### Basic Commands for Cluster Administration

* `createUser()`
* `dropUser()`
* `[collection].renameCollection()`
* `[collection].createIndex()
* `[collection].drop()`
* `dropDatabase()`
* `createCollection()`
* `serverStatus()`
* `runCommand()`
* `createIndex()`

### Logging Basics

View logs with `tail -f [log-path]`. Look for instructions in the log file with `grep -i 'update' /data/db/mongod.log`.

Run `db.getLogComponents()` to see the configured verbosity for each aspect of log reporting. Change the verbosity level with `db.setLogLevel ( [int], [log-type])`

Verbosity levels:
-1 - Inherit from parent
0 - Default verbosity to include informational messages
1-5 - increase the verbosity level to include debug messages

### Profiling the Database

The database profiler captures CRUD operations, administrative operations, and configuration operations.

`db.runCommand({listCollections: 1})` to list all collection names. Then run for example `mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '` with `db.getProfilingLevel()` to get the profiling level. To set the profiling level, run the same first command with `db.SetProfilingLevel([int])`.

Profiling levels:
0 - The profiler is off and does not collect any data. Default
1 - The profiler collects data for operations that take longer than the value of slowms.
2 - The profiler collects data for all operations.

### Basic Security and RBAC

MongoDB uses SCRAM and X.509 for authentication. At the Enterprise level, LDAP and KERBEROS can be used.

For authorization, MongoDB uses RBAC with roles, privileges, actions, and resources. At the minimum, SCRAM-SHA-1 should be configured with a single administrative user protected by a strong password.

MongoDB includes several kinds of built-in roles:
* Database user: `read` and `readWrite`
* Database administration: `dbAdmin`, `userAdmin`, `dbOwner`
* Cluster administration: `clusterAdmin`, `clusterManager` `clusterMonitor`, and `hostManager`.
* Backup/restore: `backup` and `restore
* Super user: `root`

Grant a role to user with `db.grantRolesToUser()`. These can be defined for one or any database.

`userAdmin` privileges can include:
* `changeCustomData`
* `changePassword`
* `createRole`
* `createUser`
* `dropRole`
* `dropUser`
* `grantRole`
* `revokeRole`
* `setAuthenticationRestriction`
* `viewRole`
* `viewUser`

`dbAdmin` privileges can include:
* `collStats`
* `dbHash`
* `dbStats`
* `killCursors`
* `listIndexes`
* `listCollections`
* `bypassDocumentValidation`
* `collMod`
* `collStats`
* `compact`
* `convertToCapped`

`dbOwner` combines `readWrite`, `dbAdmin`, and `dbUser` permissions into one meta role.

### Server Tools Overview

Run `find /usr/bin/ -name "mongo*"` to see what server tools have been included in the local installation.

Common tools:
* `mongostat` gives quick statistics on running Mongod processes.
* `mongodump` gets a BSON dump of a collection.
* `mongorestore` restores a collection from a BSON dump.
* `mongoexport` to export a collection to JSON or CSV or `stdout`.
* `tail [file].json` to tail the exported file.
* `mongoimport [file].json` to create a collection from a JSON file.

## Chapter 2: Replication

Replication is essential for maintaining high availability and fault tolerance for data. In MongoDB, a group of nodes that each have a copy of the same data is called a replica set. The data is sent to the primary node and is then replicated to seocnadry nodes. The secondary nodes determines the primary through an election.

Data replication has two possible forms: binary replication and statement-based replication.

Binary replication examines the exact bytes that were changed in data files and records those changes in a binary log. The secondary nodes then receive a copy of the binary log and write the specified dta that changed to the exact byte locations that are specified on the binary log. This is not very strenuous for the secondary nodes. However binary replication assumes homogeneous OS configuration across systems. Forgetting to update one database server on a node can corrupt data across the whole interface.

Statement-based replication means that when a write is completed on the primary node, the write statement is stored in the `oplog` and the secondary nodes sync their `oplogs` with the primary and replay statements on their own data. This works regardless of the OS or instruction set. MongoDB uses statement-based replication. MongoDB's particular version of replication uses idempotency to ensure that the stored statements can be applied multiple times and still result in the same final state.

### MongoDB Replica Set

Replica set members have the same data between them. A replica set member can be either primary or secondary. Primary nodes serve all read and write requests. Secondary nodes replicate the primary node and serve as a HA failover backup. Secondary nodes receive data through an asynchronous replication mechanism.

MongoDB uses an asynchronous replication protocol with two different versions: `PV1` and `PV0`. Protocol Version 1 is currently the default and based on the RAFT protocol. 

The `oplog` is a segment based lock that tracks all write operations acknowledged by the replica sets. When a write is successfully applied to the primary node, it will be recorded to the `oplog` in idempotent form.

A replica set member can also be an arbiter that does not hold data but serves as a tiebrekaer between secondary nodes in an election. There should always be an odd number of nodes in a replica set, and the arbiter can be injected to make a node configuration odd-numbered. Any replica set topology change (adding members, failing members, or changing configuration aspects) triggers an election. MongoDB replica sets can contain up to 50 members, but only up to 7 members can vote. Arbiters do cause consistency issues in distributed systems and should only be used as a last resort.

Hidden nodes can also be used to allow resilience to application level corruption by setting a delay timer in their replication process so they hold   data as a potential hot backup in case of a database drop.

### Setting Up a Replica Set

Replica sets can be created by building a configuration file for a node and duplicating it with different names for the other nodes. The `dbpath`, `port`, and `logpath` needs to be configured separately for each node iteration.

Creating a keyfile and setting permissions on it:
```
sudo mkdir -p /var/mongodb/pki/
sudo chown vagrant:vagrant /var/mongodb/pki/
openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
chmod 400 /var/mongodb/pki/m103-keyfile
```

After applying the configuration, connect to the first node and run `rs.initiate()`. `rs.status()` can be run to determine the status of the replica set. Status heartbeat checks are run every 2 seconds by default. `rs.add()` adds new members to the replica set. `rs.isMaster()` identifies the primary node. `rs.stepDown()` demotes the current primary node.

When connecting to a replcia set, the mongo shell will redirect the connection to the primary node. Enabling internal authentication in a replica set implicitly enables client authentication.

### Replication Configuration

A replication configuration document can be used to configure replica set topology and replication opitons. Edits can also be made through shell helpers. Whenever mongod is started it can be associated with a `--replSet` name. Versioning can also be set for replication configurations. It is automatically incremented when the topology changes. The in-line field members marks each sub-document member of the replica set as an array. Each member has a set of flags to determine their role such as `arbiterOnly`.

### Replication Commands

* `rs.status()`
* `rs.isMaster()`
* `db.serverStatus()['repl']`
* `rs.printReplicationInfo()` only returns timestamps for `oplog` statements.

### Local DB

On a local implementation, `oplog.rs` is the central point of the replication mechanism. This is a capped collection which is limited to a specific size. The `.capped` flag will indicate if the `oplog` is capped. By default, the `oplog.rs` will take up to 5% of the free disk. MongoDb will also indicate how much time it will take to start overwriting entries in the `oplog`. The replication window is proportional to the load of the system. Each node will only see its own `oplog`. The `local` database is not replicated.

### Reconfiguring a Running Replica Set

Reconfiguring the replica with `rs.reconfig()` means that the node configuration files do not need to be reconfigured.

### Reads and Writes on a Replica Set

Read commands to a secondary node must be confirmed first with `rs.slaveOk()`. Writes can never be enabled on a secondary node.

### Failover and Elections

When there are an even number of voters in the set, a tiebreaker is needed by re-running the election. If elections are repeated, all queries are paused which can cause inconvenience. Nodes can be assigned higher priority to designate them as the preferred primary node candidate or they can be blocked from becoming the primary node by assigning them the value of `0`.

### Write Concerns

Developers can set higher levels of acknowledgement to guarantee stronger durability, (i.e. the write has propagated to more member nodes). The more replica set members that acknowledge the success of a write, the more likely the write is durable in the event of a failure. This comes at the expense of time.

A write concern level of 0 means that the application does not wait for any acknolwedgements. It can be incremented by integers from there. The default write concern is one. It can also be set to a simple majority so that write concerns do not need to be adjusted with replica scaling.

There are two additional write concern options: `wtimeout` and `j` (journal). The former sets a maximum amount of time allotted for waiting before marking the write concern operation as failed. It does not mark the write operation itself as failed, only the durability guarantee. The latter requires that each replica set member receive the write and commit an acknowledgement to the journal filed to the write. A write concern of majority assumes journal is set to true. This increases the durability guarantee by ensuring it is written to an on-disk journal.

Write concern commands include:
* `insert`
* `update`
* `delete`
* `findAndModify`

Write concerns are supported on all MongoDB deployment types including standalone, replica sets, and sharded clusters.

### Read Concern

Read concern is like the write concern mechanism. It strengthens the guarantee of consistency across nodes for a read operation. But a document that does not meet the specified read concern is not a document that is guaranteed to be lost, only that the data had not propagated enough to be guaranteed.

There are four read concern levels:
* Local returns the most recent data in the cluster.
* Available read concerns the same as local read concern for replica set deployments. (default)
* Majority
* Linearizable - provides (Read your own Write functionality)

Linearizable waits until all prior writes (including subsequent ones after the read operation) have been replicated to a majority of nodes before it returns a response.
Best practice to pick two of three: latest, safe, and fast.

### Read Preference

There are five read preference nodes:
* primary (default)
* primaryPreferred
* secondary
* secondaryPreferred
* nearest

The trade off is between operation load/time and the possibility of reading stale data. Every read preference other than primary can return stale reads.

## Chapter 3: Sharding

### What is Sharding?

Instead of vertical scaling, MongoDB implements horizontal scaling through sharding. Sharding divides a dataset into pieces and divides it across a number of shards that are collectively known as the sharded cluster. Each shard contains a replica set to provide a level of fault tolerance.

A router process known as Mongos receives requests and processes which shard should receive the query. It uses shard metadata stored on replicated config servers to make its decisions. Config servers are deployed as replica sets.

### When to Shard

Before sharding, determine if it isn't more economically viable to vertically scale instead. This is a good first step for any throughput performance or volume bottleneck resolution.

Another aspect is the operational impact. Vertically scaling can mean significantly more back up data and sharding will allow horizontal performance through parallelization of the backup, restore, and sync processes.

Also, some workloads are better suited for distributed deployments such as single threaded aggregation framework commands. But not all stages of an aggregation pipeline are parallelizable, so this needs to be considered on a case-by-case basis.

Geodistributed data is much simpler to manage using zone sharding to easily distribute data that needs to be co-located.

### Sharding Architecture

Any number of shards can be added to a sharded cluster. Mongos is the router than mediates between shards and the client. Mongos can be replicated for high availability or higher performance. Note that it does not have a `dbpath` since it does not store any data. Metadata is stored on the config servers. Config servers can shift data across shards to promote equal distribution. 

Each sharded cluster also has a primary shard that includes all non-sharded collections and is responsible for merge operations for aggregation commands. Mongos also executes a shard merge to sort results if specified by the query. Mongos processes are invisible to the client, it just makes the queries and receives the response.

The bare minimum for a sharded cluster is Mongos, a config server replica set, and one shard.

### Config DB

Data should not be written to Config DB, unless MongoDB support engineers provide guidance when doing so. It is used to provide information about the sharded cluster including shard keys and chunks.

### Shard Keys

Shard keys are the indexed fields used to partition data in a sharded collection and distribute them. The shard key is used to divide documents into logical groupings known as chunks. The shard key defines which chunk a document belongs to.

Shards handle distributed write and read operations. For the former, Mongos checks which shard contains the chunk for a document's key-value. For the latter, Mongos redirects queries only to relevant chunks.

A field cannot be selected for a shard key if there is no existing index for the field.

To enable sharding, enable `sh.enablesharding` with the name of the database and then create an index to back the shard key with `db.[collection].createindex`. Then use `sh.shardCollection` with the full path to the collection alongside the shard key to shard the specified collection. Shard status can be checked with `sh.status`.

The shard key is immutable but the shard key value has recently been made mutable as of MongoDB 4.2 .

### Picking a Good Shard Key

Three properties of the shard key can ensure proper distribution: cardinality, frequence, and monotonicality. 

Cardinality is the measure of the number of elements in a set of values. It is the number of unique possible shard key values. A low cardinality means fewer shards can be present in a cluster. High cardinality allows more chunks and more room for sharding.

Frequency represents how often the unique values occurs in the data. High frequency can lead to hot spotting and bad performance. Low frequency means the values are more evenly distributed.

Monotonically changing means that the possible shard key values for a new document changes at a constant rate. It is bad for performance because documents with a higher shard key value than the previous one will end up with most data in the upper boundary chunk. This is why timestamps and ID numbers are a bad shard key.

Read isolation can also be used to help with distribution by targeting frequent queries to its right shard. Shard keys should support the most frequent kinds of queries. If the queried fields make bad shard keys then specify a compound index as the undelrying index for the shard key.

A collection cannot be unsharded once sharded. A shard key cannot be updated once a collection has been sharded (only its key-value). It is best to test shard keys in staging first before sharding in production. 

### Hashed Shard Keys

Hashed shard keys are shard keys with the underlying index hashed. In other words, Mongo hashes a value to sort the query into the right shard. This promotes a more even distribution of data and can alleviate problems with monotonically changing shard keys.

However, queries on ranges of shard key values are more likely to be scatter-gather. Hashed shard keys cannot support geographically isolaetd read operations using zoned sharding. Hashed index must be on a single non-array field. Hashed Indexes do not support fast sorting.

To enable sharding on a database, run `sh.enableSharding("<database>")` then `db.[collection].createIndex( { "<field>" : "hashed"} )` to create the index. Then use `sh.shardCollection( "<database>.<collection>", { <shard key field > : "hashed" })` to shard the collection.

### Chunks

Each chunk has an inclusive lower bound and an exclusive bound. In between these two points is the key space. The chunk will be split up over time to evenly distribute data. The number of chunks that a shard key allows defines the max number of shards for a system. This is why cardinality is important. 

`ChunkSize` is by default 64MB but can be defined between 1MB and 1024MB. If there is a high frequency of shard key values, then it creates jumbo chunks that are larger than the defined chunk size. They cannot be moved and the balancer will skip over it. Sometimes these cannot even be split.

### Balancing

The MongoDB balancer identifies which shard has too many chunks and re-arrnages chunks accordingly. The primary node of the Config Server Replica Set is responsible for balancing processes. Migrating chunks can cause performance issues but there is built-in functionality to minimize disruption.

Commands:
`sh.startBalancer(timeout, interval)`
`sh.stopBalancer(timeout, interval)`
`sh.setBalancerState(boolean)`

### Queries in a Sharded Cluster

Queries are directed to the mongos. It determines the list of shards that receive the query. If the query predicate includes a shard key then it will only target the relevant shard. If there is a vague shard key, then mongos will target every shard which can lead to slowdown depending on the size of the implementation.

Mongos opens a cursor against each targeted shard and executes the query predicate and returns any data to mongos. It merges the results together to form the whole set of document and returns them to the client.

Mongos can handle query modifiers as well. `sort()` pushes the sort to each shard and merge-sorts the results. `limit()` the mongos passes the limit to each targeted shard and then re-applies the limit to the merged set of results. With `skip()` the mongos performs the skip against the merged set of results.

In short, mongos handles all queries in a cluster, builds a list of shards to target a query, and handles query modifiers.

### Targeted Queries vs Scatter Gather

The mongos holds a cached version of Config server metadata as a map for queries. With targeted queries, mongos only opens a cursor against shards that meets the predicated shard keys. This is faster than scatter gather that opens a cursor against all shards.

If using a compound index, it is possible to specify every prefix up to the entire shard key and still have a targeted query. It cannot use any arbitrary field, only pathed ones.

Targeted queries require the shard key in the query otherwise it performs a scatter-gather query. Ranged queries on the shard key may still require targeting every shard in the cluster.
