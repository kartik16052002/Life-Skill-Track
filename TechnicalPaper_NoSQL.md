# Technical Paper - NoSqL

## SQL VS NoSQL

- SQL - Strong ACID transactions, complex joins/analytics, strict schema.
- NoSQL - lexible schemas, massive horizontal scale, very low-latency key/value or document access

## What to choose ? 

As our company is facing performance and scaling issues, we can try opting for NoSQL databases as it would provide us massive horizontal scaling options and a low-latemc key/value access.

## Which NoSQL database to choose and why ? 

Here are the five NoSQL databases from which we can choose.

## 1. MongoDB — Document DB

    A general-purpose document database that stores JSON-like documents (BSON), designed for developer productivity, flexible schemas, and scale-out via sharding.

    ### Strengths

       - Flexible schema - store evolving product/user documents with no expensive migrations.
  
       - Rich query language & indexes: complex filters, secondary indexes, text search, geospatial queries.

       - Transactions available: multi-document ACID transactions since v4.x, so you can support transactional patterns when needed.

       - Mature managed offering: Atlas provides automated backups, autoscaling, global clusters, and monitoring to lower the ops burden.

    ### Weaknesses
    
       - Sharding & scaling complexity: Horizontal scale is powerful but heavily depends on shard-key design. Poor shard keys cause hotspots and degrade performance. 

       - MongoDB Memory & index planning - working set and index size matter; underprovisioning results in high I/O and high latencies. 

       -  Complex multi-table joins or lots of ad-hoc analytics can become inefficient; denormalization is often required. 

       -  MongoDB License considerations - server is under SSPL for newer releases; some orgs factor that into procurement.
  

## 2. Amazon DynamoDB — Key-value / document-style managed

    A general-purpose document database that stores JSON-like documents (BSON), designed for developer productivity, flexible schemas, and scale-out via sharding.

    ### Strengths

       - Flexible schema: Store evolving product/user documents with no costly migrations.

       - Rich query language & indexes - complex filters, secondary indexes, text search, geospatial queries.

       - Transactions available: Multi-document ACID transactions since v4.x, allowing you to support transactional patterns when needed.

       - The mature-managed offering (Atlas) includes automated backups, autoscaling, global clusters, and monitoring, reducing ops burdens.

       - Large ecosystem & drivers, wide language support, tooling for BI/ETL, and connectors to many platforms.


    ### Weaknesses/ risks

       - Sharding & scaling complexity: horizontal scale is powerful, but heavily dependent on shard-key design. Poor shard keys create hotspots that degrade performance.

       - Planning for memory & index - working set & index size matters; underprovisioning leads to high I/O & high latencies.

       - Not a drop-in for heavy relational workloads - complex multi-table joins or lots of ad-hoc analytics can become inefficient; denormalization is often required.

       - License considerations - server is under SSPL for newer releases; some orgs take that into account for procurement.

## 3. Cassandra / Scylla — Wide-column NoSQL

    Cassandra (and its high-performance sibling Scylla) are distributed wide-column stores designed for very high write throughput, linear scale across data centers, and high availability.


    ### What makes them special?

    Data is modeled around query patterns, partition key + clustering columns, and stored in wide rows optimized for append-heavy workloads. For this reason, they are very well-suited for massive write/ingest pipelines and geo-distributed data.


    ### Strengths

       - Massive write scalability & availability: Linear horizontal scaling across nodes and DCs, designed to remain available under node failures.


       - Tunable consistency: you can choose per-operation consistency (ONE, QUORUM, ALL.) to balance latency vs correctness. That gives operational flexibility.


       - Geo/peer replication - Designed for multi-datacenter replication and low-latency local reads.


       - Scylla — performance & lower TCO — Scylla (C++ + Seastar) has much higher throughput and lower latencies than Cassandra with less resource usage in many workloads. If you care about raw throughput and cost per op, it's definitely worth evaluating.


    ### Weaknesses/risks

       - Query-driven modeling: you need to create tables for specific access patterns. Ad-hoc queries, joins, and analytics are painful. Analytics usually require offloading to an OLAP pipeline.


       - Hot partitions & data modeling pitfalls: poorly chosen partition keys or time-series patterns without bucketing create hotspots and throttling. Planning is critical to avoid this.


       - Operational complexity-unless managed, running a large Cassandra cluster requires tuning of compaction, repair, hinted handoff, and anti-entropy; some managed Scylla/Cassandra offerings may alleviate some of this burden.


       - Read/analytics limitations - secondary indexes and full table scans are limited/inefficient; heavy read-analytics should be served from a separate system.

## 4. Redis — in-memory data-structure store

    Redis is an in-memory key/value and data-structure server (strings, hashes, lists, sets, sorted-sets, Streams, etc.) with outstanding performance, frequently used as a cache, real-time datastore, message broker, and ephemeral database.


    ### Strengths

       - Blazing speed: everything served from memory, with optional persistence, providing single-digit millisecond or sub-millisecond reads/writes. Great for ultra-low-latency paths.


       - Rich data structures & atomic ops: sorted sets for leaderboards, lists for queues, hashes for compact objects, bitmaps/hyperloglog for analytics, streams for event processing. These primitives enable you to implement functionality with very little glue code.


       - Ecosystem & modules: With Redis Stack, modules add capabilities for RedisJSON, RediSearch, RedisGraph, RedisTimeSeries, RedisBloom, and vector search so that Redis can expand beyond pure key/value uses.


       - Flexible deployments / managed options: run OSS Redis, Redis Enterprise - Redis Inc., or cloud-managed services, such as AWS ElastiCache, Azure Cache, and Redis Cloud, based on ops appetite and features needed.


    ### Disadvantages / risks

       - Memory cost & working set limits: Because performance comes from memory, data size vs. RAM is a major cost/architecture consideration. Eviction policies and item sizing matter.


       - Durability trade-offs: While Redis supports RDB snapshots and AOF for persistence, the durability semantics vary, and active-active persistence has limitations; you need to design for acceptable data-loss windows, or use enterprise features.


       - Scaling & partitioning constraints: Redis Cluster scales quite well, sharding data across hash slots, while multi-key commands spanning hash slots are either limited or require careful key design/resharding. Hot keys/partitions might become bottlenecks if access isn't well spread out.


       - Feature/ license awareness: Some enterprise capabilities, such as active-active, advanced tooling, RDI, and the newest packaging/licensing choices, are material for procurement and multi-cloud strategy. Refer to OSS vs Redis Software/Enterprise differences for your legal/ops teams.
  
## 5. Neo4j — Native graph database

    One-liner: Neo4j is a native graph database built for fast traversal of highly-connected data — it stores nodes and relationships directly so relationship queries are extremely efficient.

    ### Strengths

       - Fast relationship traversal: Neo4j's native graph storage and "index-free adjacency" make multi-hop queries very fast compared with doing many JOINs in an RDBMS or doing many ad-hoc queries in a document store.


       - Purpose-built for relationship problems-great for recommendations, fraud detection, access control, knowledge graphs, social networks, and lineage where the relationships are the data.


       - ACID transactions + Cypher query language: Supports transactional integrity and an expressive graph query language (Cypher) that is easily used by analysts and developers.


       - Managed cloud: Neo4j Aura is a managed offering that removes most operational headaches-provisioning, backups, upgrades-and can simplify adoption.


    ### Weaknesses / risks

       - Not a general-purpose store — if your primary workloads are large ad-hoc analytics, time-series or document search, a graph DB is not the right primary store. Graphs are specialized for connected-data queries.


       - Scaling requires careful design - Neo4j scales but complex write/cluster topologies (causal clustering / fabric/federation) and very large datasets require attention to partitioning, cluster configuration, and query patterns. Some analytic graph DBs, for example, TigerGraph, claim higher parallel analytical throughput in benchmarks.


       - Operational cost for very large graphs. Hosting big graphs across many regions with low latency can be expensive. Carefully evaluate managed Aura vs self-managed nodes.

  ## References

- [MongoDB Documentation](https://www.mongodb.com/docs/)
- [Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/)
- [Apache Cassandra Documentation](https://cassandra.apache.org/doc/latest/)
- [Redis Documentation](https://redis.io/docs/latest/)
- [Neo4j Documentation](https://neo4j.com/docs/)
