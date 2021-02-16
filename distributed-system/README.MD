# Distributed System Overview

A distributed system provides an illusion of a **single system** even though the actual **computation** or **storage of data** is managed on multiple machines.

#### Consumers expect the following from a distributed system...

- **Availability** = uptime / (uptime + downtime)

- **Scalability** = Will adding more hardware increase the performance proportionally?

- **Reliability** = Will the system provide the same result for the same inputs under load?

#### You as a system engineer can provide the following guarantees...
- Build a system that is either optimized for read or write.

- Guarantee that the system is either strongly consistent or eventually consistent.

- Make interactions either synchronous or asynchronous.

- Indicate how the system behaves in case of network failures.

#### How is the performance of a distributed system measured?

- **Processing time**  = The total time taken to process a request.

- **Latency** = The time it takes for the request to reach the server.

- **Response time** = Processing time + Latency.

- **Average response time** is not a good measure of performance. 

- Instead use percentile. If a system says the response time is 250 ms for 90%, it means 90% of the users experienced a response time of 250 ms or less.

- **Median response** time indicates where the middle exists. (50th percentile)

- **Throughput** is the number of transactions per second - For a batch processing system throughput is very important.

#### Distributed Storage Options

- Use a single master for read/write and **scale up** the server by increasing the hardware specification.

- **Use read replicas** - All writes go a master node, the data is then replicated onto replica nodes. Reads can be served from replicas. 

- **Shard the database** - Distribute the traffic across multiple nodes based on a shard key. The disadvantage of this approach is that adding a new node results in "incorrect lookup".

#### Consistent hashing (Sharding)

- Decide the number of virtual nodes - Say 100. Think of this as a circle from [0 - 100]

- Then pass say something like the server IP through the hashing function. This would yield a value such as 30.

- Use the hash value to decide a range of keys that the server would handle. [0-30]

- If this server crashes, then the server that precedes this in the ring, would automatically start handling the reads for the crashed node.

- Use a **peer to peer replication** of data across nodes - No single master. Writes and reads can go to any node.

**CAP theorem :**

- Consistency - Do all the nodes in the system see the same data?

- Availability - Is the system always available for reads & writes ?

- How should the system behave in case of a network partition ?

- Refuse writes to preserve consistency or allow writes with eventual consistency?

**Tunable consistency**

- Make consistency tunable - **R + W > N.**

- Total number of nodes (N)

- How many nodes should agree for a write to succeed ? (W)

- How many nodes should agree for a read to succeed? (R)

# Interface, Implementation, Data Formats & Protocols

## Relational Model
- Model is defined in SQL.

- Declarative by nature.

## Document Model
- Model is defined in JSON.

- Usually imperative code.

## Distributed File System
- Map Reduce runs the computation.

- Mix of declarative & imperative

- Spark RDD is a developer friendly abstraction on top of Map Reduce. 

- Spark is storage agnostic whereas Map Reduce assumes HDFS. 

## Graph
- Model data as vertices and edges. [vertex = person, edge - represents connection between two people.]

**Property graph.**

- Each vertex has a unique identifier, incoming edges , out going edges, properties represented as key values.

- Each edge has a unique identifier, start vertex, end vertex, label to describe the relationship between vertices & properties represented as key values.

- Just like a user would POST JSON document in a document database, in Neo4J the graph is described using Cypher and queried using Cypher.

## Append only log
- Used by BitCask the storage driver for Riak

- Immutable , write only structure.

- The offset of the data in the log, can be stored against a key in Hash Map. [ Called a Hash Map Index] 

- Break the logs into segments, to avoid one big log file.

- Each such segment contains a "HashMap" of its own. The HashMap is stored in RAM.

- Data updates change only offsets.

- After a certain point of time, all the stale values are removed, the segment is compacted and the index is updated.

- Crash of the system, results in a lost HashMap contents. The index can be rebuilt by reading the segments.

- Instead, the HashMap itself can be stored on disk periodically to avoid slow rebuild time.

- Range queries are slow.

## Sorted String Table  SSTable

- Similar to the append only log except for two changes.

- The keys in the log are stored in sorted order and appear only once.

- "Some of the keys" are stored in memory (called memtable) serving as index.

- Since the keys are sorted ... any key is either found in memory or is "in-between" other keys.

- In such a case the offset of the key in memory is picked and then the segment is scanned until the required key is found.

- The segments can also be compressed saving disk space and reducing IO bandwidth.

- An SSTable on disk is a serialized representation of sorted in-memory tree (such as Red Black tree)

- When a certain threshold is crossed, the memtable is persisted as a segment.

- To avoid loss of data if the system crashes and the memtable is not yet written to the disk, a transaction log is used to ensure that the memtable can be recreated.

## LSM Tree

TODO

## BTree

TODO

## In-Memory databases

- Completely in-memory relational databases such as VoltDB

- Data structure servers such as Redis

## OLTP databases

- Pattern is to insert/update a few records at a time. The queries also are generally user specific or of the type give me the latest N records.

## OLAP databases

- The pattern of the data access typically spans a huge range of data. What are the total sales in India? What was the most frequently bought item? etc.

- The data is made amenable to analytics by storing data in fact table and dimension tables.

- Fact table usually has a lot of columns, with each column being foreign key reference to dimension table.

## Columnar storage

- Traditional databases store row wise in the file system. Columnar databases store the data column wise. As a result sum, average etc (OLAP) operations become easy.

- TODO - Bit Map indexes , SIMD and vectorized processing.

- A **materialized view** is different from a regular database view. The results of the view are cached and the database invalidates the cache when the data has changed.


## Encoding/Serialization formats

- JSON, XML, CSV

- Binary JSON formats such as Message Pack, BSON

- Apache Thrift, Avro and Protocol Buffer

- Avro has the option of writing schema along with the payload.

## Protocols for communication

- REST, SOAP, Message Broker

- Actor model TODO

## Replication

- Master slave replication. Writes handled by Master/Leader. Reads can be handled by slaves/read replicas/followers

- Replication can be synchronous or asynchronous

- Semi synchronous replication - To avoid the system grinding to a halt, because a follower has not responded. Seems more like a quorum based approach?

- When to start replication when a new node is added? - Start with a well known snapshot of the master, then request for changes since that point of time. Eventually the follower will catch up with the master.

## Common problems in replication

- How to elect a new leader, when the current leader has crashed?

- What to do when the original leader comes back?

- Split brain - What if both nodes believe to be the leaders?

- What constitutes as a leader failure? What should be the time duration?

- A user submits a comment, comment goes to leader, the data is not synced with the follower. The user reads from the follower and becomes unhappy- [We need read your writes consistency]

- It might be necessary to decide that some information is always read from the leader.

- Some data might disappear if a read replica with a greater lag is queried. Monotonic reads solves this problem by routing a particular users request always to the same replica.

- Reordered writes which result in confusing comment thread. The answer to a question might appear before the question.

- Conflicts - Preserve conflicts and let the application code decide.

- Automatic conflict resolution - CRDT - TODO

- Sloppy quorum and hinted handoff - TODO

## Replication techniques :

- **Hardware level replication**- SAN - Happens at block level

- **Operating system level replication** - DRBD - Happens at block level

- **Database physical replication** - WAL file - Information about which byte has changed on the disk is conveyed to the replica

	- In PostgreSQL , this can happen by either sending WAL files across or through streams 

	- Streaming replication is at cluster level (everything gets replicated by default)

	- Read replicas cannot be modified

	- Unidirectional replication

	- Replication can be synchronous. Mix of synchronous & async.

	- Also quorum based.  N servers, first N servers in a sequence. 

- **Database logical replication** - Information about which row has changed in which table is conveyed to the replica.

	- Needed when partial replication is desired.

	- Postgres 10 has built in logical replication through publish/subscribe mechanism

	- Schema is not replicated, only data
	
	- pg_logical is an extension, that provides more such capabilities. Replicate only certain columns, replicate rows that match a criteria and so on.
	
- Trigger based - Bucardo

- Application level replication
