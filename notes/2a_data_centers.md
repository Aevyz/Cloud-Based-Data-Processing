# Data Centers

- [Data Centers](#data-centers)
  - [Datacenter Architecture](#datacenter-architecture)
    - [Power Usage Effectiveness](#power-usage-effectiveness)
  - [Challenges of Data Centers](#challenges-of-data-centers)
  - [Improving Resource Utilization](#improving-resource-utilization)
  - [Types of Datacenter Architectures](#types-of-datacenter-architectures)
  - [Additional Content](#additional-content)


Physical facility used by enterprises to house computing and storage. Provides...
- Power
- Cooling
- Shelter
- Security

## Datacenter Architecture

::: define Scale Up
High cost powerful CPUs, with more core and more memory
:::
:::define Scale Out
Add more lower cost servers
:::

Servers with CPU, DRAM and Disks are placed in Racks (typically 40-80 servers). Racks are placed in rows and connected to a cluster switch.

### Power Usage Effectiveness

::: define PUE
Power Usage Effectiveness is the ratio of total energy used at DC vs that which is delivered to computing equipment
:::

::: define Total Facility Power
All IT Systems (servers, networking etc.) + Other Equipment (cooling, UPS, generators, lights, fans)
:::

::: question You can improve the PUE by...
- choosing the location of the DC
  - e.g. Iceland
- raise temperature of aisles
- reduce conversion of energy (e.g. directly work at 12V)
- reuse dissapated heat
:::

::: question Trivia: 2018 PUE levels
1.15-1.18
:::

## Challenges of Data Centers

- How to Cool?
- Energy Proportional Computing
  - The measure of the relationship between power consumed in a computer system, and the rate at which useful work is done [Wikipedia]
- Idle Servers
  - Virtualize servers into resource pools
  - Allow other virtualized services access to resources if needed
- Efficient Monitoring
  - Better understand what is going on
- Managing Scale and Growth
  - How to scale data center operations
- Networking at Scale
  - How to abstract networks to work at these sizes

## Improving Resource Utilization

- Hyper Scale System Management Software
  - Treat datacenter as a huge computer
  - Software defined "datacenters"
  - Compose a software defined system of pooled resources to assign resources based off of workload requirements 
- Dynamic Resource Allocation
  - Virtualization is not enough to improve efficiency
  - Ability to dynamically allocate CPU resources --> Shift demand

## Types of Datacenter Architectures

- Traditional Rack Architecture
  - Server which has all components
    - GPU server: CPU, RAM, GPU
    - General server: CPU, RAM
    - SCUTI-O Server: CPU, RAM , SCUTI-O
- Disaggregated Rack Architecture
  - Racks with all CPU, rack with DRAM, etc.
  - Software selects what is needed and creates virtualized systems 

---


## Additional Content

:::question What do DC provide (4)
- Power
- Cooling
- Shelter
- Security

:::

::: question PUE is the...
Inverse of data center efficiency
:::
