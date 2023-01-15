# Distributed Data

Why?

- Scalability
- High Availability / Fault Tolerance
- Low Latency




::: question Can you combine replication and partitioning
Yes!
:::

## Replication

::: define Replication
Keeping a copy of the same data on multiple machines that are connected via network
:::

::: question When should you use replication?
High numbers of read only operations
:::

Why Replicate?
- Keep data geographically closer to user --> reduce access latency
- Query different copy if closest is down (availability)
- Each machine can handle many reads

Challenges of replication
- Should there be a lead replica
  - If so, how many?
- Synchronous or asynchronous propagation of updates
- How to handle failed replica
- How to succeed a failed replica



### Leaders and Followers

Each write must be processed by every replica, otherwise nodes will not hold same data. Reads can be performed on both leaders and followers.

::: define Leader
Leaders are the replicas, which can be written to
:::
::: define Follower
Followers are the nodes, who apply the replication log, they receive from leaders
:::

::: question How to Enroll Followers
1. Get consistent snapshot (read: back-up snapshot) of leader's database (already saved somewhere)
2. Copy snapshot to follower
3. Since leader likely has update in this time, get the leader's replication log
4. Apply log. Once processed, follower should be caught-up
:::


#### Synchronous vs Asynchronous Replication

::: define Synchronous Replication
Leader waits until all replicas confirm changes, before reporting success to user. Followers are guaranteed to have up to date versions of data. If follower does not answer, no writes can be done.
:::
::: Asynchronous Replication
Leader sends update message to all replicas, however responds success when it has made changes
:::

#### Outage Handling

Follower Failure: Catch Up Recovery
Leader Failure: Failover

::: define Catchup Recovery
- Keep local log of changes already applied
- After reboot, ask leader for outstanding changes
:::

::: define Failover
- Detect leader has failed
- Promote new follower to leader
- Adjust system accordingly
:::

#### Replication Logs

- Statement based replication
  - Leader logs every write request it executed
- Write ahead log (physical log)
  - All modifications written to log before execution
  - Includes Redo and Undo information
- Change data capture (logical log)
  - Sequence of records, that describe what is written to the database tables (content of table fields changed)
  - Replicas can thus run on different storage engines

#### Eventual Consistency

- Only really realistic for asynchronous applications
  - Due to Availability problems
  - This results in potentially outdated information

::: define Eventual Consistency
When running the same query on leader and follower results in different results. There is no limit to how far a replica can fall behind
:::

Avoidance:
- Read from leader, after known write about a topic
- Make user always read from same replica (prevents comment from disappearing, if its on one replica but not on other)


## Partitioning


::: define Partitioning
- Split dataset into subsets (partitions)
- Partitions placed on separate nodes
- Reduces latency for analytical jobs
:::

---

## Additional Info

::: define Replica
A copy of the dataset
:::

::: question How to handle Follower Failure
Catch Up Recovery
:::
::: question How to handle Leader Failure
Failover
:::