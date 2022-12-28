# Scalable Systems

## Distributed Computing Challenges

Ideally adding N servers allows the service to support m*N users. 

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

::: define High Availibility
Service must have 100% 24/7 uptime
:::

::: define Consistency
Data stored/produced by the services must be consistent
:::

::: define Performance
Predictable low latency processing with high throughput
:::


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
