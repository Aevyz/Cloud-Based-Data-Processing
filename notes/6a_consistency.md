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

- Client sends begin transaction
- Transaction executed as normally
- Command to commit is sent to coordinator
  - Coordinator asks nodes to prepare for commit
  - Only when all have given okay is the transaction commited
- Typically linearizable

What happens in the event of a crash?
- Coordinator decides when to write to disk
- Upon recovery, send decision to replicas
- Problem if crash occurs between prepare and commit (or revert)
- Algorithm is blocked until coordinator recovers (single point of failure)

## Fault Tolerant 2PC --> Take a look in more depth

- Every node that participates in the transaction uses a total order broadcast to vote (commit or abort).
- If a node A suspects a Node B has crashed, it will vote abort for Node B 
- Potential race condition if two conflicting votes are received from same Node
  - Count first vote to arrive

![](res/6/ft2pc1.PNG)
![](res/6/ft2pc2.PNG)

--- 

::: question Difference between Atomic Commit and Consensus
In consensus 1 or more nodes propose values, only one of which is accepted. Crashes are tolerated in this system. 

As for Atomic Commits, every node votes whether or not it wishes to commit or abort. Commits require all nodes, whilst aborts require 1+ nodes to vote abort. Crashes cause a direct abort.
:::

::: define ACID
Atomicity, Consistency, Isolation, Durability
:::

::: question Linearizability vs Serializability
Linearizability only handles a single transaction, whilst Serializability groups transactions together, that can be executed in parallel.
:::

::: question Can you combine Linearizability and Serializability
Yes, known as Strict Serializability
:::

::: define Strict Serializability / Strong One Copy Serializability
Combine Serializability and Linearizability
:::
