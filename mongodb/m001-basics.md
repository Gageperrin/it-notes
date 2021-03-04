# M001: MongoDB Basics Course

## Chapter 1: What Is MongoDB?

MongoDB is a NoSQL database that stores data as documents which are in turn stored as collections.

A document is a way to organize and store data as a set of field-value pairs. Documents are grouped together into collections, and a database often contains multiple document collections.

MongoDB Atlas is the cloud solution for MongoDB storage. It is a database as a service.

Atlas users can deploy clusters where each server is configured as a replica set. A replica set consists of a few connected machines that stroe the same data to ensure that if something happens to one of the machines then the data will remian intact. An instance is a single machine on-premise on in the cloud running the MongoDB database software.

Atlas can manage cluster creation, run and maintain database deployment, use a cloud service prvoider of choice, and be used for experimenting with new tools and features. Atlas has a free tier that includes one free cluster with a 3-server replica set and 512 MB storage. It never expires. The free tier also includes charts, Realm, and is suitable for small-scale apps.

## Chapter 2: Importing, Exporting, and Querying Data

MongoDB stores data in BSON, but Users work with MongoDB documents in JSON. BSON is a binary encoded format for machines that offers a larger range of data support than JSON does.

To import and export in JSON run `mongoimport` and `mongoexport` and for BSON run `mongorestore` and `mongodump`. These take the `--uri` argument for the Uniform Resource Identifier. BSON takes a serv for its URI string to establish a secure connection.

Data explorer can provide an accessible view of collections and documents in a database in the UI. A namespace is the concatenation of the database name and collection. Queries must be valid JSON.

To query through the command line, first connect to the database (`connect(url, user,password)` and verify the connection to the right cluster with `show dbs`. To run a query, use `db.[collection-name].find {"field":"value"}`. Use `it` iterate through the cursor which is a pointer to a result set of a query. A pointer is a direct address of the memory location.

## Chapter 3: Creating and Manipulating Documents

When creating a new document object, it will be provided with a unique value for the id `ObjectId()`. Additional fields can be added as necessary when inserting into the collection.

When many documents are inserted, they are by default in the order they are listed unless specified otherwise with `{"ordered": false}`.

Update operators:
* `{"$inc": {"field": value, "field2" : <increment value> }}` will increment a field value by a specified amount.
* `{"$set": {"field": value, "field2" : <new value> }}` will set the field value to a new specified value.
* `{$push: { "field": value }}` adds an element to an array field.

Common commands:
* `use [db]` to switch to another database
* `db.[collection].findOne();` to get a random document
* `db.[collection].insert({JSON})` to insert a document
* `db.[collection].updateOne({conditional match JSON}, {update JSON});` to update a single document in the collection. `.updateMany` will update all documents in the collection with the matching condition.
* `db.[collection].deleteOne({JSON})` can be used to delete a document. `.deleteMany` is better for deleting all that match the criteria.
* `db.[collection].drop` to drop a collection.
* `show collections` to see what collections are present in the current collection directory.

## Chapter 4: Advanced CRUD Operations

MQL operators include update operators such as `$inc`, `$set`, and `$unset` which modify data in the database. There are also query operators that provide additional ways to locate data within the database.

Comparison operators:
* `$eq` is equal to
* `$gt` is greater than
* `$gte` is greater than or equal to
* `$neq` is not equal to
* `$lt` is less than
* `$lte` is less than or equal to

Syntax: `{ <field>: { <operator>: <value> } }`

Logic operators:
* `$and`
* `$or`
* `$nor`
* `$not`

Syntax: ` { $operator : [{ clause1 }, { clause2 } ] } `
For `$not`: ` {$not: {clause}`.

Expressive operator `$expr` allows the use of aggregation expressions with syntax `{ $expr: { expression } }`. It iterates through a document to find matching field values that have `$`.

Array operators focus on arrays:
* `$push` adds an element to an array or converts a field to an array.
* `$size` will return documents whose array are equal to the given length.
* `$all` returns a cursor with all documents that contain the given elements.
* `$elemMatch` returns only the first element in an object that matches the given condition.
*  `$regex` can be used for regex matches.

Syntax: ` {<array field> : { $size : <number> }}`

Projection syntax can be used to include or exclude specific fields: `db.<collection>.find({<query>},{<field1>: 0, <field2>: 1 })`

`1` includes the field, `0` excludes the field.

Query an array returns only exact array matches. Querying a single element will return all documents that contain the specified element.

To use an array operator on a sub document, append the subfield with a `.` so for example ` db.[collection].find( "field.subfield" : "value"`. If it is an array, specify the position in the array.

## Chapter 5: Indexing and Aggregation Pipeline

The aggregation framework is an alternate way to query data in MongoDB.
```
db.[collection].aggregate([
   { $match: { "field" : "value" }},
   { $project: { "f2" : "v2", "f3" : "v3" }}  ]}
```

This translates to find all documents that have "value" as a "field" and only output "v2" and "v3" in the resulting cursor. `$match` is a filter for what to pull and `$project` filters what is printed.

Aggregation is preferable for creating multi dimensional data pipelines. `$group` can filter data into several distinct reservoirs. `$match` does not filter the queried data only what is displayed in the cursor.

`.sort()` can be used to sort results in a particular order while `.limit()` determines how many entries are displayed in the cursor. Set the sort to `1` for increasing order and `-1` for decreasing order. Order is by default alphabetical or numerical.

An index is a special data structure that stores a small portion of a collection's data into an easily traversible form. Indexes should be used often for frequent kinds of queries.

Syntax: `db.[collections].createIndex({"field": 1})`

Upserting is simultaneously updating and inserting to a document. It will update a match or insert if there is no matching document. Upsert is set to true within an `updateOne()` command.

