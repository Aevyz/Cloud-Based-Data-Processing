# Consistency Summary Table

Problem | Must wait for communication | Synchrony
---|---|---
Atomic Commit | All participating nodes | Partially Synchronous
Consensus, Total Order Broadcast, Linearizable CAS | Quorum | Partially Synchronous
Linearizable Get and Set | Quorum | Asynchronous
Eventual Consistency, Casual Broadcast, FIFO broadcast | Local Replica Only | Asynchronous 