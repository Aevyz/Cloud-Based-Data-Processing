# Replication

- [Replication](#replication)
  - [Leaders and Followers](#leaders-and-followers)
    - [Synchronous vs Asynchronous Replication](#synchronous-vs-asynchronous-replication)
    - [Outage Handling](#outage-handling)
    - [Replication Logs](#replication-logs)
    - [Eventual Consistency](#eventual-consistency)
  - [Multi Leader Replication](#multi-leader-replication)
  - [Leaderless Replication](#leaderless-replication)
    - [Read and Write Quorums (Quorum Consistency)](#read-and-write-quorums-quorum-consistency)
    - [Sloppy Quorum \& Hinted Handoff](#sloppy-quorum--hinted-handoff)
  - [Additional Info](#additional-info)


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



## Leaders and Followers

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


### Synchronous vs Asynchronous Replication

::: define Synchronous Replication
Leader waits until all replicas confirm changes, before reporting success to user. Followers are guaranteed to have up to date versions of data. If follower does not answer, no writes can be done.
:::
::: define Asynchronous Replication
Leader sends update message to all replicas, however responds success when it has made changes
:::

### Outage Handling

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

### Replication Logs

- Statement based replication
  - Leader logs every write request it executed
- Write ahead log (physical log)
  - All modifications written to log before execution
  - Includes Redo and Undo information
- Change data capture (logical log)
  - Sequence of records, that describe what is written to the database tables (content of table fields changed)
  - Replicas can thus run on different storage engines

### Eventual Consistency

- Only really realistic for asynchronous applications
  - Due to Availability problems
  - This results in potentially outdated information

::: define Eventual Consistency
When running the same query on leader and follower results in different results. There is no limit to how far a replica can fall behind
:::

Avoidance:
- Reading you write
- Monotonic reads
::: define Reading you write
- If a user updates a field, they are guaranteed that their reads will contain the updated information
  - e.g. by reading only from the master
- No guarantee for other users
:::
::: define Monotonic Reads
Avoid time skips (e.g. comment appearing, and then gone, due to not being in other replica yet), by reading same data always from the same replica.
:::

## Multi Leader Replication

Multiple leaders, who each have a master replica. They perform conflict resolution to apply changes.

The problem is write conflicts can occur. These can be handled by...
- Making conflict detection synchronous
- Conflict avoidance
  - All writes for a particular record go through a particular leader
- Converging to a consistent state
  - Last Writer Wins (comes with data loss)
  - Custom conflict resolution (PC > Phone)

## Leaderless Replication

- Leaderless replication has no failover
- Client sends writes and reads to multiple nodes
  - Version numbers are used to determine which value is the newest
- How to catch up?
  - Read Repair
  - Anti-entropy process


::: define Read Repair
Client detects stale response and writes the new response
:::
::: define Anti-entropy process
Background process checks for inconsistencies and fixes them
(unlike replication log, order is not preserved)
:::

### Read and Write Quorums (Quorum Consistency)

```
n = Number of Nodes
w = Confirmations for Write
r = Confirmations for Read
```

- If `w + r > n`, an up to date value when reading is expected
- Reads and Writes that obey these values are called quorum reads/writes

Assuming quorum conditions, we can tolerate some failures...
- As long as `w < n - failed_nodes`, we can still process writes
- As long as `w < r - failed_nodes`, we can still process reads
- With (3,2,2), we can tolerate 1 node failure
- With (5,3,3), we can tolerate 2 node failures

Essentially we send reads/writes to all nodes in parallel. `w` and `r` determine, how many nodes we wait to respond, before we can consider it successful and get an "acceptable" result

Limitations:
- sloppy quorum
- edge cases where stale values are returned
- no order in which writes are applied

### Sloppy Quorum & Hinted Handoff

- Networks can be partitioned due to network disruptions, leading two groups of nodes
- Sloppy Quorum occurs in network partition cases
- Reads and Writes still require `w` and `r` respectively
- Once network interruption is fixed, writes that have been accepted are transferred to the rest
  - This is called a hinted handoff



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

::: question Replication can be...
synchronous or asynchronous
:::

::: question Three main approaches to replication are...
- Single-leader replication
- Multi-leader replication
- Leaderless replication
:::

::: define  read-after-write consistency
the ability to view changes (read data) right after making those changes
:::