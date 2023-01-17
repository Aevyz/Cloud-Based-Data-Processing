# Reliable Cloud Applications

- [Reliable Cloud Applications](#reliable-cloud-applications)
  - [Fault Tolerance](#fault-tolerance)
  - [Unreliable Networks](#unreliable-networks)
    - [Detecting Faults](#detecting-faults)
  - [System Models](#system-models)
    - [Network Behavior](#network-behavior)
    - [Node Behavior](#node-behavior)
    - [Synchronous Timing](#synchronous-timing)
      - [Synchronity Violations](#synchronity-violations)
  - [Additional Content](#additional-content)


## Fault Tolerance

Typically a crash-stop/crash-recovery system works by...
- Sending a message
- Awaiting Response
- Message Timeout
- Label Node as Crashed

This however means, that it is difficult to identify...
- a crashed node
- temporarily unresponsive node
- lost message
- delayed message


## Unreliable Networks

Remember that "even" data center internal networks can be faulty...
- Your request may be lost
- Your request may be in a queue and is delayed
- Remote Node may have failed
- Remote Node may have temporarily stopped response
- Remote Node may have processed request but response is delayed/lost

Typically you send a response (ACK) message, but that too may be lost

### Detecting Faults
- Detection should be automatic
  - Load Balancer should stop sending requests to dead nodes
  - Leader promotion for single leader replication DBs
- How long should the timeout be?
  - How long does the message propagate through the network?
  - Network congestion and queuing must be taken into account

## System Models

Used to model assumptions about our system, specifically

- Network Behavior
- Node Behavior
- Timing Behavior

### Network Behavior
- No network is perfectly reliable
  - Cable could be unplugged
  - Solar radiation
  - DOS
- Networks may be partitioned
- Point to Point communication between two nodes can be...
  - Reliable Links
    - Message received only if sent
    - Ordering may be wrong
  - Fair Loss Links
    - Message may be lost, duplicated or reordered
    - Retries will eventually let message through
  - Arbitrary Links
    - Active Adversary
    - Eavesdrop, Inject, Modify, Repeat, Drop  



### Node Behavior 
Executed algorithm could...
- Crash Stop (stops executing forever)
- Crash Recovery (may resume, sometime later)
- Byzantine (anything: crashing or malicious behavior)

### Synchronous Timing

- Synchronous
- Partially Synchronous
- Asynchronous

#### Synchronity Violations

- Network latency and loss
- Network congestion 
- Network Routing
- OS Scheduling
- Garbage Collection
- Node errors

---

## Additional Content

::: define Synchronous
Message latency no greater than known upper bound
Executed at a known speed
:::


::: define Partially synchronous
System is asynchronous for a finite time, synchronous otherwise
:::

::: define Asynchronous
Messages may be delayed arbitrarily, could be paused, no timing guarantee
:::

::: define Crash-Stop
Crash at any time, stops executing
:::

::: define Crash Recovery
Crash at any time, may resume and some point in time
:::

::: define Byzantine
Deviates from algorithm, could crash, could be malicious
:::

::: define Failure
System as a whole is not working
:::

::: define Fault
 some part of the system is not working
:::

::: define Node fault
crash (crash-stop/crash-recovery), deviating from algorithm (Byzantine)
:::

::: define Network fault
dropping or significantly delaying messages
:::

::: define Fault tolerance
System as a whole continues working, despite faults (some maximum number of faults assumed)
:::

::: define Failure detector
Algorithm that detects whether another node is faulty
:::

::: define Perfect Failure detector
labels a node as faulty if and only if it has crashed
:::

:::define MTTR
Mean Time to Recovery
:::
:::define MTBF
Mean Time Between Failures
:::
:::define SLO
Service Level Objective
Percentage of requests that need to return a correct response within a specific time

e.g. 99.9% of Requests in a a day get a response in 200ms
:::

:::define SLO
Service Level Agreement
Contract specifying a SLO, typically with penalties for violation
:::

:::question Three System Models
- Network Behavior
- Node Behavior
- Timing Behavior
:::