# Scalable Systems

## Distributed Computing Challenges

Ideally adding N servers allows the service to support N users. 

Linear scalability is hard to achieve because...

* Overheads and synchronization is needed
* Load imbalances create hot spots
  * Popular content
  * Poor hash functions
* Amdahl's law
  * A straggler slows everything down

### Challenges

#### Scalability

::: define Scalability
Independent parallel processing of tasks / sub-requests ==> Can add additional servers for further concurrency
:::

![](https://www.researchgate.net/profile/Mark-Burgess-9/publication/293654281/figure/fig28/AS:613921840979968@1523381781092/Sublinear-linear-and-superlinear-scaling-of-a-value-with-respect-to-a-control.png)


#### Fault Tolerance

::: define Fault Tolerance
Must be able to handle and recover from both software and hardware failures. Services and data must be replicated for redundancy. 
:::

* Full redundancy is too expensive
* Thus failure recovery is used

Failure Recovery Methods

* Replication
  * Replicate data and service
  * Consistency may be harmed
    * New data is pushed
* Re-Consumption
  * Remember data lineage for job
  * Re-run task
  * Easy for stateless services

#### High Availability

::: define High Availibility
Service must have 100% 24/7 uptime
:::

* Downtime is bad
  * IT downtime costs on average 5600$ per minute!
    * Gartner study
* Cloud service providers provide contractual agreemente for quality of service

| Downtime Guarantee | Max Downtime per Year | Estimated Costs |
| ------------------ | --------------------- | --------------- |
| 99.9%              | 8.77h                 | ~3,000,000$     |
| 99.99%             | 52.6min               | ~300,000$       |

#### Consistency
::: define Consistency
Data stored/produced by the services must be consistent
:::

::: define CAP Theorum
Consistency
Availability
Partition Tolerance
:::

::: define Strongly Consistent
Cost of additional latency
:::

::: define Inconsistent Operations
Better performance, applications are harder to write
:::

Gmail's email sending is strongly consistent, but marking a message is inconsistent.

#### Performance
::: define Performance
Predictable low latency processing with high throughput
:::

::: define Tail Latency
The last 0.X% of the request latency distribution time
:::

Remember that overall latency >= latency of slowest component

## Design Principles of Cloud Applications

::: define Design for Self Healing
In a distributed system, failures happen all the time. Design the application to be self-healing
:::

::: define Make all things redundant
Build redundancy into your application to avoid having single points of failure.
:::

::: define Minimize Coordination
Minimize coordination between application services to achieve better scalability
:::

::: define Design to Scale Out
Design your application so that it can scale horizontally, adding or removing new instances on demand.
:::

::: define Partition Around Limits
Use partitioning to work around database, network and compute limits.
:::

::: define Use of stateless services
Scaling without having a state is trivial
:::

::: define Caching
Latency is king. Caching helps to significantly reduce the job’s latency
:::

::: define Use the best data store for the job
Pick the storage technology that is the best fit for your data and how it will be used.
:::

::: define Distribute Computation
Partition/Aggregate compute pattern is one that scales pretty well
:::

::: define Design for Evolution
An evolutionary design is key for continuous innovation
:::

## Typical Design of Scalable Service
1. Find the requirements and goals of the system (e.g., functional, non-functional)
2. Figure out the workloads the system should be optimized for (e.g., is it a read-heavy workload, etc.)
3. Do a back-of-the-envelope calculations for estimated storage capacity needs
4. High-level system design
5. Do the database schema based on the functional requirements
6. Do the large-scale system design based on the non-functional requirements
  1. How do you scale the system?
  2. How can you make it reliable and redundant?
  3. How would you do data sharding?
  4. Cache and load balancing?
7. How can you implement the functional compute requirements in the scaled system

## Additional Content

### Amdahl's Law
In general terms, Amdahl’s Law states that in parallelization, if P is the proportion of a system or program that can be made parallel, and 1-P is the proportion that remains serial, then the maximum speedup S(N) that can be achieved using N processors is:
                `S(N)=1/((1-P)+(P/N))`
As N grows the speedup tends to 1/(1-P).
 
Speedup is limited by the total time needed for the sequential (serial) part of the program. For 10 hours of computing, if we can parallelize 9 hours of computing and 1 hour cannot be parallelized, then our maximum speedup is limited to 10 times as fast. If computers get faster the speedup itself stays the same.

> http://www.umsl.edu/~siegelj/CS4740_5740/Overview/Amdahl%27s.html

::: question What is the consequence of Amdahl's law on parallel systems
 a straggler slows everything down
:::

::: question Why is linear scalability hard to achieve (4)?
1. Overhead for parallelization
2. Synchronization is required
3. Load imbalances
4. Amdahl's Law
:::

::: question How can load imbalanced be created? (2)
1. Popular Content
2. Poor hash functions
:::

::: question What is super linear scalability
Better than linear scalability (per server >1 user)
:::

::: question What is linear scalability?
Per resource added, exactly one extra user can additionally use the service
:::

::: question What is Sub Linear Scalability
Per resource added, less than one user can additionally use the service
:::

::: question Why full redundancy is not used?
Too expensive to make everything fully redundant
:::

::: question What are the two Failure Recovery Methods?
1. Replication
2. Reconsumption
:::

::: question Failure Recovery: Replication
Replicate the data and service and have the job run again.
:::

::: question Failure Recovery: Re-Consumption
Remember the data lineage for a job and re-run task. Easier for stateless.
:::

::: question Failure Recovery: Replication Problem
May introduce data consistency problems (e.g. data consumed, new data pushed)
:::

::: define SLA
Service Level Agreement
::: 