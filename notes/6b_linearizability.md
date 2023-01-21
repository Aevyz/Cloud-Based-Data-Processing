
# Linearizability

::: define Linearizability
- Every operation takes effect atomically
- All operations executed as if on a single copy of data
  - Despite being multiple replicas
- Every op returns an up to date value (strong consistency)
:::


::: define Serializability
The outcome of a series of parallel executed transactions is the same if it were executed serially (without overlapping time)
:::

## When is Linearizability Helpful?

- Distributed Locks
- Leader Elections
- Constraints and Uniqueness Guarantees
  - Only one person with `123` as their ID
- Cross channel timing dependencies
  - Message delivered at right time, despite being multiple channels


## Read After Write Consistency

- After writes, clients all read same data, regardless of replicated node targeted
- Essentially when we write, we send to all
  - In this case, A fails
- When we Read, we send a request to everyone
  - C fails
  - A returns (t_0, outdated_content)
  - B returns (t_1, updated_content)
- Because of the timestamp, we can identify that B has the latest content
- Note that this only works if we have a quorum
![](res/rawc.PNG)

### From the Clients PoV

- It is possible for overlaps to occur
- If e.g. set and get happen parallel
  - It does not matter if v0 or v1
- Real time overlap
![](res/rawcc.PNG)

#### Not Linearizable Example

- Transaction sent to A first, to B and C delayed
- Not linearizable
  - Client 2 gets v1
  - Client 3 gets v0
- Because Client 2's operation finishes before client 3's operation starts
  - Require client 3 to receive answer no more stale than client 2's
  - Violated
![](res/notlin.PNG)

#### Ensuring Quorum R/W are Linearizable (ABD Algorithm)

- Essentially if inconsistent values detected, send updated value to all nodes
- Formally:
  - Send Read Request
  - Wait for Quorum Response
  - If more recent valid option sent, send a set with the value to the stale replicas
  - Only after a quorum write is the read finished
![](res/qrwlin.PNG)

## Linearizability for Different Ops

- Get (Quorum Read)
- Set (Blind Write to Quorum)
  - Multiple concurrent writes may overwrite each other
  - Write whatever to field
- Compare and Swap
  - CAS(field, oldValue, newValue)
  - Only Write if oldValue there
  - NOT LINEARIZABLE WITH ABD
    - Operations may be seen in different order
    - Possible concurrency issues
  - Total Order Broadcast will allow linearizability
    - Broadcast every operation
    - Execute when operation is delivered
    - TOB ensures every replica has the same state post execution