# Broadcast Protocols

- Broadcast / Multicast is a group communication
  - One node sends message
  - Rest of nodes in group delivers
  - If one node if faulty rest carries on
- Best Effort: Messages may be dropped
- Reliable: Non-faulty deliver every message --> retransmit dropped


## Reliable Broadcasts

Broadcast|Explanation
---|---
FIFO | Messages sent by the same node must be delivered in the order that they were sent. Messages by other nodes do not matter. 
Casual | if b(m1) => b(m2), m1 must be delivered before m2 (b from any node) 
Total Order | if m1 is delivered before m2 on one node => all nodes (including self) must have received m1 before m2
FIFO Total Order | Combination of FIFO and Total Order Broadcast 

### Total Order Broadcast Algorithms

::: define Single Leader Approach (Total Order Broadcast)
- One node designated leader
- Broadcast by sending to leader
- Single point of failure -> Hard to change leader
:::
::: define Logical Clocks Approach (Total Order Broadcast)
- Attach a vector timestamp to every message
- Deliver messages in order of timestamps
- Requires FIFI links
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
