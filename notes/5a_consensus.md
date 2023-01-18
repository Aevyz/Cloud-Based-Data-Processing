# Consensus

::: define Consensus
When several nodes come to an agreement about an single value
:::

A consensus algorithm must satisfy the following properties
- Uniform Agreement
- Integrity
- Validity
- Termination

Most algorithms are partially synchronous and crash recovery

Asynchronous is not possible because...
- FLP Result: No deterministic consensus algorithm that is guaranteed to terminate in an asynchronous crash-stop system model


## Consensus Leader

- Leader must be elected
- Leader determines sequence of messages
  - Failure Detector (timeout) used to determine crash / unavailability of a leader
  - If crashed, elect a new one
  - Need to prevent both leaders seen as valid
- One leader per term and only one vote
---
::: define Uniform agreement
no two nodes decide differently
:::
::: define Integrity
no node decides twice
:::
::: define Validity
if a node decides value v, then v was proposed by some node.
:::
::: define Termination
every node that does not crash, eventually decides some value.
:::