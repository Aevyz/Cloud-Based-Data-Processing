# Cloud Computing

- [Cloud Computing](#cloud-computing)
  - [X as a Service](#x-as-a-service)
  - [Cloud Pros and Cons](#cloud-pros-and-cons)


Datacenters offer hardware and software, that allows customers to make use of their (virtualized) computing resources and services.

- Datacenters are vendors that rent servers and computing resources
  - Anyone can rent
- Fine grained pricing model
  - Pay as you go
- Can scale on the go
  - If you need more, just buy more
  - Only pay for what you use

Cloud Provider (e.g. AWS) --> Cloud User / SaaS Provider (e.g. Netflix) --> SaaS User (You)

Cloud User examples
- Software / Websites serving real users
  - Netflix, Pinterest, Spotify
- Data Analytics / ML companies
- Mobile and IoT backends
- DC operator's own software 
  - Onedrive

::: define Public Cloud
Resources owned and operated by one org, usable by any paying customer
:::
:::define Private Cloud
Resources owned, operated and used by a single org. 

Note: Can be on-prem and/or hosted
:::
:::define Hybrid Cloud
Combine Public and Private cloud. Better control over sensitive data, whilst taking advantage of scale of public cloud provider.

e.g. patient database in private cloud, but API in public cloud
:::
::: define Multi Cloud
Use multiple clouds for an application / service. Redundant, but introduces complexity (API differences, migration)
:::

:::define On-Premise
Resources located locally (or in a datacenter operated by company)
:::
::: define Hosted
Resources hosted and managed by third-party
:::

## X as a Service

![](https://cdn.shortpixel.ai/spai/q_glossy+w_742+to_auto+ret_img/https://www.webhostingsecretrevealed.net/wp-content/uploads/paas.jpg)

::: define Infrastructure as a Service
Rent IT infrastructure (servers and VMs), as well as corresponding networking and security measures.
:::

::: define Platform as a Service
Get a on demand environment for development/deployment
:::

::: define Function as a Service / Severless
Build app, without managing servers, as cloud vendors provide that. Using Lambdas, so better scaling?
:::

::: define Software as a Service
Cloud hosted software, cloud provider handles all software and infrastructure.

:::
## Cloud Pros and Cons

Pros:
- Elimination of up front commitment
- Buy more resources if you need (short term)
- Speed
- Scale
- Security taken care of
- Flexible

Cons
- Dependent on network / internet
- Privacy concerns
- Security relies on 3rd party
- Cost of mirgration
- Vendor Lock-in