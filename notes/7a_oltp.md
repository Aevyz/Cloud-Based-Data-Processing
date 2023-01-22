# Online Transaction Processing - OLTP

Expectations:
- Durability
- Support large databases (100 TB)
- Scalability
  - Workload
  - Dataset size
- Fault Tolerance
- High Availability
- High Performance
  - Low Tail Latency
  - High throughput
- Elastic with workload demand
- Small blast radius
  - small impact of a failure
- Good performance/cost ratio
- Data protection

## Traditional Monolithic Stack
Section | What is there
---|---
Application | Your application
SQL | Compiler, Optimizer, Query Processing
Transaction | Transaction manager, locking, undo manager
Caching | Buffer Cache
Logging | Redo logging, crash recovery, backup
Storage | Durable Storage

- Works well for on premise deployments
  - In memory database
  - Fast storage solutions
  - Expensive, but reliable hardware

## Scaling Options

- Sharding
  - Shared Application layer
  - Multiple Databases
  - Application knows which database to query
- Shared Nothing
  - Application knows both databases
  - Application queries a database
  - Database's SQL layer ensures transaction is executed correctly
- Shared Disk
  - Application knows both database
  - Link at caching and storage layer
  - Ensures data is not saved duplicated

## State of the Art DBaaS Architectures

1. Non Partitioned shared nothing log replicated state machine (Microsoft HADR SQL Server)
2. Non Partitioned shared data (Aurora, Socrates, Taurus)
3. Partitioned shared nothing log replicated state machine (Spanner, CockroachDB)
   1. Requires distributed transactions
4. Tightly coupled nodes over fast interconnect (Oracle RAC, Exadata)