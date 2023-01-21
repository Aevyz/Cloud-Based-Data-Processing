# Collaboration and Conflict Resolution 

- Conflict Free Replicated Data Types (CRDT)
  - Operation Based
  - State Based
- Operational Transformations (OT)

## Conflict-free Replicated Data Types
- Operation Based has smaller messages
- State based can tolerate message loss/duplication
### Operation Based CRDT
- Replicated object can be a...
  - Set
  - List
  - Map
  - Tree
- Requires...
  - Commutative operations (ordering does not matter)
  - Reliable broadcast
    - Every broadcast is eventually delivered to all replicas 
- e.g. Calender
  - Local state is key value map
  - On Read: Return Local Value
  - On Write: Create a globally unique timestamp and reliable broadcast (timestamp, key, value)
    - Broadcast every operation
  - Last Writer Wins

Strong Eventual Consistency because...
- Eventual delivery
  - When both nodes are online, share list of changes
- Convergence
  - Conflict Resolution: LWW

### State Based Map CRDT
- Replicated object can be a...
  - Set
  - List
  - Map
  - Tree
- Example again of a map
  - Initialize empty map
  - Read local
  - Write: 
    - Apply operations locally
    - Only send the value when needed
  - Merge together when value received
- Benefit: Can batch together multiple operations and only send result
  - e.g. When you make multiple changes to calender offline

Does not need broadcast.


### Text Editing CRDT

- Rather than identifying text index, each character is given a rational number between 0 and 1
- Insert new character between `i` and `j` by inserting at `(i+j)/2`
- Simpler conflict resolution vs Operational Transformation
- Use Causal Broadcast
  - e.g. insertion of character is delivered before its deletion
- Insertion and Deletion must be commutative

## Operational Transformation

- Each node keeps track of history of operations performed
- When you receive a concurrent operation, transform operation so that `op' = T(op1, op2)`
  - Whereby `op` does both `op1`'s and `op2`'s intended effect 
  - e.g. D gets moved from position 2 to 3, because A inserts at position 0
- Needs network access, and leader


![](res/6/opt.PNG)
---

::: question Which CRDT requires Broadcast
Operation Based CRDT
:::

::: question Merge Operation of Two CRDT States must be...
- Commutative: `s1 U s2 = s2 U s1`
- Associative: `(s1 U s2) U s3 = s1 U (s2 U s3)` 
- Idempotent: `s1 U s1 = s1`
:::
---

::: define LWW
Last Writer Wins
:::

::: define Commutative
Order of delivery does not matter
:::