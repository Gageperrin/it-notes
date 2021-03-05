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

## Chapter 3: Sharding
