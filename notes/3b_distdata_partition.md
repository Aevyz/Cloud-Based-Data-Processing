# Partitioning

- [Partitioning](#partitioning)
  - [Types of Partitioning](#types-of-partitioning)
    - [Horizontal Partitioning (Sharding)](#horizontal-partitioning-sharding)
      - [Partition Strategies](#partition-strategies)
      - [Rebalancing Partitions](#rebalancing-partitions)
      - [Request Routing](#request-routing)
        - [Zookeeper](#zookeeper)
    - [Vertical Partitioning](#vertical-partitioning)
    - [Functional Partitioning](#functional-partitioning)
  - [Approach for Partitioning](#approach-for-partitioning)
    - [Partitioning for Scalability](#partitioning-for-scalability)
    - [Partitioning for Improved Query Performance](#partitioning-for-improved-query-performance)
    - [Partitioning for Improved Availability](#partitioning-for-improved-availability)
  - [Additional Content](#additional-content)


:::define Partitioning
For large datasets, or very high throughput, we need to break the data up into partitions. A piece of data, belongs to one exact partition
:::

Alternative Terms: 
- Region
- Tablet
- vnode
- Shard
- vBucket

Benefits of Partitioning...
- Scalability
  - Different nodes
- Improve performance
  - Data operations done on smaller data volumes
  - Parallel
- Improve Availability
  - Partition can fail
  - Without replication, part will be unavailable
- Improve Security
  - Sensitive and non-sensitive data can be put on different partitions
  - Different security / access policies

## Types of Partitioning
### Horizontal Partitioning (Sharding)

::: define Horizontal Partitioning
Splitting a table into multiple segments, whereby each segment has a different set of data.
:::

- Spread data and query load evenly
- Query operations done on both in parallel
- If partitioning is unfair (more data or queries), this is called skewed 
- Disproportionally high load partitions are known as hot spots

#### Partition Strategies

- Key Range (Range Partitioning)
  - e.g. a-g and h-z shards
  - Not necessarily evenly spaced
    - Leads to hot spots
  - Kept in sorted order
- Key Hash (Hash Partitioning)
  - Hot spots should not occur
  - No longer easy to do range queries (only get all names starting with the letters b,c,d)

#### Rebalancing Partitions

- What not to do
  - `hash(key) % number_of_nodes` because if we add a node, most items need to be moved
- Define a `P` so that `P >> N`
  - Assign multiple P's to each node
  - If you add a N, only move the partitions needed to the new node
  - Less movement
- Dynamic Partitions
  - Applicable with range and hash partitioning
  - When partition grows too large, split it in two
- Partition proportional to nodes
  - Fixed number of partitions per node


#### Request Routing

How do you know where to send the request? Three options...

1. Ask the node --> C: Where is Foo? N0: on N1
2. Add a routing layer / third party --> C: Where is foo? R: Found on N1
3. Client knows where to find --> Saved internally

##### Zookeeper

Zookeeper is an example of a routing tier option. It holds the knowledge of what data is stored on which node

### Vertical Partitioning

::: define Vertical Partitioning
Split a table into multiple tables, whereby some columns are in the one table, and some in the other. Ideally, the table holds data that is linked together.
:::

- Reduce IO and performance cost
  - Data found in separate tables
- Slow moving data can be cached
- Separated from dynamic data
- Sensitive data can be separated into a secured partition
- Keyword: Suited for Column Orientated Data Stores

e.g. One table holds description and the other the current stock. Advantage is that the current stock is likely to change more frequently, whilst the description is read only.

### Functional Partitioning

::: define Functional Partitioning
When it is possible to functionally partition data, e.g. a read only table and read-write table, one should partition these.
:::

- Improves isolation
- Improves performance

## Approach for Partitioning

### Partitioning for Scalability
- Analyze application and data access patterns
  - What is returned
  - How frequently
  - What is the latency
  - How taxing is the request
- Determine current and future scalability targets (e.g. data size, workload)
  - Distribute data across partitions to meet targets
  - Make sure each node has the needed resources
- Monitor that the data is distributed well and partitions are handling the load
- Rebalance if needed

### Partitioning for Improved Query Performance
Query performance boostable using smaller data sets and running parallel queries
- Examine Application for requirements and performance
  - Identify critical queries that must be performed quickly
  - Monitor the systems to detect slow queries
  - Identify queries frequently performed
- Partition data causing slow performance
- Run queries in parallel across partitions if needed
### Partitioning for Improved Availability
Avoid single points of failure
- Identify critical data
  - Store data in high available partitions
  - Don't forget to replicate :)
- Define recovery procedure if partition fails
- Partition data by geographical area allows for partition maintenance

---
## Additional Content

:::question Benefits of Partitioning...
- Scalability
- Less contention
- Improve performance
- Optimize storage costs
- Improve Security

:::



::: question What is a skewed shard?
If a partition is unfair, that is, one part has more data or queries.
:::
::: question What is a hot spot shard?
Disproportionally high load
:::

::: question Three Main Types of Partitioning?
- horizontal
- vertical 
- functional
:::
::: question Two Main Approaches of Horizontal Partitioning?
- key range
- hash-based
:::