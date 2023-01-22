# Broadcast Protocols

- [Broadcast Protocols](#broadcast-protocols)
  - [Reliable Broadcasts](#reliable-broadcasts)
    - [FIFO Broadcast](#fifo-broadcast)
    - [Causal Broadcast](#causal-broadcast)
    - [Total Order Broadcast](#total-order-broadcast)
      - [Algorithms](#algorithms)
  - [Replication using Total Order Broadcast](#replication-using-total-order-broadcast)


## Reliable Broadcasts

Broadcast|Explanation
---|---
FIFO | Messages sent by the same node must be delivered in the order that they were sent. Messages by other nodes do not matter.
Causal | if b(m1) => b(m2), m1 must be delivered before m2 (b from any node) 
Total Order | if m1 is delivered before m2 on one node => all nodes (including self) must have received m1 before m2
FIFO Total Order | Combination of FIFO and Total Order Broadcast 


### FIFO Broadcast
- A sends M1 and M3
- B sends M2
- That means valid orders are (m1,m2,m3), (m2,m1,m3), (m1,m3,m2)
- m3 and m1 are not allowed to change place

### Causal Broadcast
- A sends M1 first
- A and B send M2 (A) and M3 (B) concurrently
- Valid orders are (m1,m2,m3) & (m1,m3,m2)
  - m2 and m3 are allowed to change place, as concurrent
- Essentially a node's broadcast is always sorted

### Total Order Broadcast 
![](res/4/tob.PNG)

- Only one order in this case
- But as you can see, M3 must be adapted as also applies to self

#### Algorithms

::: define Single Leader Approach (Total Order Broadcast)
- One node designated leader
- Broadcast by sending to leader
- Single point of failure -> Hard to change leader
:::
::: define Logical Clocks Approach (Total Order Broadcast)
- Attach a vector timestamp to every message
- Deliver messages in order of timestamps
- Requires FIFO links 
  - Wait until timestamp >=T on every node before continuing
:::

Not fault tolerant!

## Replication using Total Order Broadcast

Assume that every node delivers the same message in the same order. We can then perform State Machine Replication.

::: define State Machine Replication
- FIFO Total Order Broadcast all replica updates
- Replica applies update
- Replica acts as a state machine, whereby all replicas start at the same point and have the same inputs
:::

::: question Limitations of State Machine Replication
- Cannot update state immediately, must wait for delivery
- Need fault tolerant total order broadcast
:::
