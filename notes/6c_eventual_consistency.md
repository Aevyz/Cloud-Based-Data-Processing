# Eventual Consistency

- Weaker form of linearizability
- Form of Optimistic Replication


::: define Eventual Consistency
If there are no more updates, replicas will eventually go towards the same state (may take a long time)
:::

::: define Strong Eventual Consistency
If two replicas communicate with one another, they will converge towards the same state, even if updates occur. Consists of...
- Eventual Delivery
- Convergence
:::

- Properties of Eventual Consistency
  - Does not require waiting for network communications
  - Causal broadcast can disseminate updates
  - Concurrent updates 
    - Leads to conflict potential

## CAP Theorem Addendum

- A system can be strongly Consistent (linearizability) or available, when a network is partitioned
- Can be both consistent and available, if we discount network partitions
- For instance a Node who is partitioned, must either wait indefinitely, or return stale values 

## Causal Consistency

- Causality is important
  - e.g.
    - Question before Answer
    - First Create than Update
    - etc.
- A system who obeys causal ordering is causally consistent

### Causality vs Linearizability

- Linear
  - Total Order of Operations
  - System behaves as if a single copy of data exists
- Causality
  - Partial Order of Operations
  - We cannot say which concurrent operation happened first
- Linearizability implies causality
  - Causality does not imply linearity
- Causal Consistency is the strongest possible consistency model that... 
  - can handle network delays
  - remains available in the face of network partitions
----

::: define Optimistic Replication
Replicas process operations based off of local state only
:::


::: define Eventual Delivery
- Every update made to a non-faulty replica is eventually passed on to every other non-faulty replica
:::

::: define Convergence
Any two replicas that have processed the same set of updates are in the same state (even if processing order differs)
:::