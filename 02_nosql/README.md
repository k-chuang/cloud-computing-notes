# NoSQL Overview
- Not Just SQL is the newer definition
- Primary goal is to achieve horizontal scalability
- SQL Properties
  - Schema-less
  - Easy to use with load balanced clusters
  - Are ACID within a node of the cluster, and eventually consistent across cluster
  - Persistent data
  - Scale to available memory
- Consistency Models: Instead of ACID, NoSQL follows BASE
  - BASE = Basic Availablility Soft-state Eventual Consistency
    - Basic Available = database is available most of the time
    - Soft-state = Stores don't have to be write-consistent, nor do replicas have to be mutually consistent all the time
    - Eventual Consistency = Stores exhibit consistency at some later point

## Reason for NoSQL
- Improve programmer productivity by using database that better matches application needs
  - Using NoSQL databases allow developers to develop without having to convert in-memory data structures to relational structures
- Improve data access performance via some combination of handling larger data volumes, reducing latency, and improving throughput

## NoSQL Data Models
- Key-Value (Riak, Redis, DynamoDB)
  - Good for storing session information, user profiles, preferences, shopping cart data.
  - Bad for when we need to query by data, have relationships between the data being stored or we need to operate on multiple keys at the same time
- Document (MongoDB)
  - Good for content management systems, blogging platforms, web analytics, e-commerce, real time analytics
  - Bad for when need complex transactions spanning multiple operations or queries against varying aggregate structures
- Column family (Cassandra, Hbase, BigTable)
  - Good for content management systems, blogging platforms, maintaining counters, expiring usage, heavy write volume such as log aggregation.
  - Bad for systems that are early in development, changing query patterns
- Graph (Neo4J, OrientDB, Infinite Graph)
  - Good for connected data such as social networks, spatial data, routing information fo goods and money, recommendation engines

## NoSQL Distribution Models
- **Sharding**: Sharding distributes different data across multiple servers, so each server acts as the single source for a subset of data.
- **Replication**: Replication copies data across multiple servers, so each bit of data can be found in multiple places. Replication comes in two forms,
  - **Master-slave replication** makes one node the authoritative copy that handles writes while slaves synchronize with the master and may handle reads.
    - Reduces update conflict, so maximizes consistency
  - **Peer-to-peer replication** allows writes to any node; the nodes coordinate to synchronize their copies of the data.
    - Avoids overloading a single server to maximize availability
- A system may use either or both techniques. Like Riak database shards the data and also replicates it based on the replication factor.

## SQL vs NoSQL
- Relational SQL databases are designed to run on a single machine, so to scale you need to scale up a single machine, which has a hard limit.
  - It is cheaper and more effective to scale out by buying lots of cheaper machines

## Applied CAP Theorem
- CA systems = Traditional RDBMS (MySQL, Postgres)
- CP systems = MongoDB, BigTable, HBase, Redis
- AP systems = Dynamo, Riak

## Modern CAP
- Maximize combinations of consistency and availability that make sense for the specific application, while always having partition tolerance
  - Partition Mode
  - Partition Recovery

## Polygot Persistence
- The idea that best to use multiple data storage technologies, chosen based upon the way data is being used by individual applications or components of a single application. **Different kinds of data are best dealt with different data stores.**

## Optimistic and Pessimistic Offline Locking
- Pessimistic Offline Lock
  - Prevents conflicts between concurrent business transactions by allowing only one business transaction at a time to access data
	 - Acquire lock on piece of data before it starts to use it, so no concurrency
- Optimistic Offline Lock
  - Prevents conflicts between concurrent business transactions by detecting a conflict and rolling back the transaction
  	- Validates changes about to be committed by one session doesn't conflict with another session, obtaining a lock indicating ok to go ahead with change, and updates occur within a single system transaction

## Version Stamps
- A field that changes each time data is updated
- Examples:
	- Counter - requires single master to generate unique values
	- GUID - anyone can generate, but values are large and hard to compare
	- Hash - hash of content with a big enough hash key size for global uniqueness
	- Timestamp - nodes have to synchronize clocks
	- Vector stamps - A vector of version stamps from each node, [Node A: 2, Node B: 5, Node C:10]

## Rule of Thumb
- CP Systems are generally Master Slave replication (ex. BigTable)
- AP Systems are generally Peer-to-Peer replication (ex. DynamoDB)

## Examples
- Ring or Peer-to-Peer systems
  - Riak, Voldmort, Dynamo, Cassandra
- Master Slave Systems
  - MongoDB, HBase, Big Table, Membase,
- Replication based
  - Redis and CouchDB

## References
- https://www.thoughtworks.com/insights/blog/nosql-databases-overview
- https://bigdata-ir.com/wp-content/uploads/2017/04/NoSQL-Distilled.pdf
