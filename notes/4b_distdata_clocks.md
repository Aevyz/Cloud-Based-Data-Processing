# Unreliable Clocks

- Distributed systems need to agree upon time, for example
  - Schedulers
  - Metrics
  - Logs
  - Failure Detrection
  - Retry
  - etc.

Time basics:
- Unix Time begins on the 1st January 1970
- ISO 8601
  - Year:Month:Day:Hour:Min:Sec:TimezoneOffset



## Types of Clocks
### Physical Clocks
::: define Physical Clocks
Count number of seconds elapsed
:::

- Quartz clocks are cheap but inaccurate (drift)
  - Drift is measured in parts per million (ppm)
  - 1 ppm = 1microsecond/second
  - 32s/year
  - Most computer clocks write within 50ppm
- Atomic clocks used for accuracy
  - Need to handle Leap Seconds
- Most times, uncertainty is not exposed
- Non synced clocks can lead to ordering problems

#### Clock Synchronization
::: define Clock Skew
Difference between two clocks
:::

- Get time from Atomic Clock or GPS Receiver
- Protocols
  - Network Time Protocol
  - Precision Time Protocol
- Works by sending multiple requests to same server
- Use metrics to reduce network latency variance

#### Time of Day Clocks

::: define Time of Day Clock
Time since a fixed date (e.g. Unix Epoch)
:::

- May suddenly shift due to clock sync (theoretically possible to have negative if unfortunate sync)
- Comparable across nodes, assuming synced

e.g. `System.currentTimeMillis()`

#### Monotonic Clock

::: define Monotonic Clock
Time since arbitrary point (e.g. boot)
:::

- Moves forward at constant speed
- Good on a single node

e.g. `System.nanoTimer()`

### Logical Clocks
::: define Logical Clocks
Count Events (messages sent)
:::
- Lamport Clocks not handled
#### Vector Clocks

- Detection of Concurrent Events
- n nodes in a System
  - `N = <N_1, N_2, ..., N_n>`
- Vector timestamp of an event a is `V(a) = <t_1, t_2, ..., t_n>`
  - whereby `t_i` is the timestamp number of events observed by the node `N_i`
- Each vector timestamp is attached to each message

![](https://miro.medium.com/max/1064/1*xvQm1wP0v0eSmp3pnuIbEA.png)

Basically when a message is sent, the vector is updated to the node's current time
---
::: define ppm
Parts per Million
Used to measure Quartz drift
:::
::: question Two types of Logical Clocks
1. Vector
2. Lamport
:::