# Kafka


These are my notes from Stephane Maarek's Apache Kafka Series.

## Kafka for Beginners

### Introduction

Kafka integrates different source and target systems through a high-throughput distributed messaging system. Kafka was created by LinkedIn but is now open source and maintained by Confluent.

Kafka is a distributed resilient architecture with fault tolerance, horizontal scalability, and high performance of sub 10ms latency. Kafka can be used as a messaging system, for activity tracking, gathering metrics from many different locations, application logs gathering, stream processing, decoupling of system dependencies, and big data integrations.

### Kafka Fundamentals

### Kafka Theory

#### Topics, Partitions, and Offsets ####

Topics are a particular stream of data. It is similar to a table in a database. You can have as many topics as you want. A topic is identified by its name.

A topic is divided into ordered partitions. Each message within a partition gets an incremental id, called offsets. The number of messages inside a partition is independent of the other partitions.

An offset can only have a meaning for a specific partition. Order is guaranteed only within a partition. Data is only kept for a limited time (default of one week). Offsets continue incrementing regardless. Once data is written to a partition, it is immutable and cannot be changed. Data is assigned randomly to a partition unless a key is provided.

### Brokers and Topics

A Kafka cluster consists of multiple brokers (servers/nodes). Each broker is identified with its ID. Each broker contains certain topic partitions. After connecting to any broker, the client is connected to the entire cluster. It is normal to start with 3 brokers but some larger clusters have over a hundred brokers.

Example. Broker 101 has topic A with partition 0 and topic B with partition 1. Broker 102 has topic A with partition 2 and topic B with partition 0, and broker 103 has topic A with partition 1.

Topics should have a replication factor > 1. Ideally, it should be 3. If a broker is down, then another broker can serve the data. At any time only one broker can be a leader for a given partition. Onl yhtat leader can receive and serve data for a partition. The other brokers will merely synchronize the data on that partition. Therefore each partition has one leader and multiple ISRs (in-sync replicas).

Producers write data to topics. Producers automatically know to which broker and partition to write to. If a broker fails, producers will automatically recover. Producers load balance automatically. Producers can choose to receive acknowledgment of data wrwites in three ways:
* `acks=0` Producer will not wait for acknowledgment
* `acks=1` Producer will wait for the leader to acknowledge. Default
* `acks=all` Producer will wait for acknowledgment from both leaders and replicas. 

Producers can choose to send a key with the message (string, number, etc.). If the key is null, data is sent round robin. If a key is sent, then all messages for that key will always go to the same partition. A key is basically sent if you need message ordering for a specific field.

Consumers read data from a topic identified by name. Consumers know which broker to read from. Consumers automatically do failover for reads. Data is read in order within each partition. Consumers read data in consumer groups. Each consumer within a group reads from exclusive partitions. If there are more consumers than partitions, some consumers will be inactive. There need to be enough partitions if all consumers need to be kept active.

Kafka stores the offsets at which a consumer group has been reading. The offsets are committed live in a Kafka topic named `__consumer_offsets`. When a consumer in a group has processed data received from Kafka, it should be committing the offsets. If a consumer dies, it will be able to read back from where it left off.

Consumers choose when to commit offsets. There are three deliery semantics:
1. At most once. Offsets are committed as soon as the message is received.
2. At least once (usually preferred). The message will be read again if lost. This can result in duplicate processing, so the the messaging system must be idempotent.
3. Exactly once. Can be achieved using Kafka workflows using Kafka Streams API. For Kafka to External Systems workflows, use an idempotent consumer.

Every Kafka broker is also called a bootstrap server. You only need to connect to one broker, and you are connected to the entire cluster. Each broker knows about all brokers, topics and partitions using metadata.

Zookeeper manages brokers and helps in performing leader election for partitions. Zookeeper sends notifications to Kafka in case of changes. Kafka cannot work without Zookeeper (outdated with 3.0). Zookeeper by design operates with an odd number of server to maintain quorum. Zookeeper has a leader that handles writes and the rest of servers are followers that handle reads. Zookeeper does not store consumer offsets. Zookeeper consists of separate servers connected to Kafka clusters.

Kafka guarantees:
* Messages are appended to a topic partition in the order they are sent.
* Consumers read messages in the order stored in a topic-partition.
* With a replication factor of N, producers and consumers can tolerate up to (N-1) brokers being down. A replication factor of 3 allows for one broker to be down while another one is down for maintenance. As long as the number of partitions remain constant for a topic, the same key will always go to the same partition.

### Kafka CLI

