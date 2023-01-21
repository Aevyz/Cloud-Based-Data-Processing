# Consistency

- Consistency is important when replicating data
- Inconsistencies can occur, regardless of method
  - No perfect method

Note that consistency may mean different things to different systems

System | Definition
---|---
ACID | Satisfying the database's / application's constraints
Read After Write Consistency |  The ability to view recent changes (read data) directly afterward (write data)
Replication | Read operations return same result / is in same state

## Consistency Models vs Isolation Levels

### Transaction Isolation
- Avoid race conditions due to concurrently executing transactions
  - Serizlization
  - Repeatable read
  - Read commits
  - Read uncommits

### Distributed Consistency
- Coordination of the replica's state, when delays or faults occur
  - Strict
  - Linearizability
  - Causal
  - Monotonic reads/writes
  - Read-your-writes

## Two Phase Commits

![](res/6/2pc.PNG)


--- 

::: question Difference between Atomic Commit and Consensus
In consensus 1 or more nodes propose values, only one of which is accepted. Crashes are tolerated in this system. 

As for Atomic Commits, every node votes whether or not it wishes to commit or abort. Commits require all nodes, whilst aborts require 1+ nodes to vote abort. Crashes cause a direct abort.
:::

::: define ACID
Atomicity, Consistency, Isolation, Durability
:::