# CHAPTER 12 &emsp Colocate Pattern

<div align="justify">

This basic pattern focuses on avoiding unnecessary network latency.

Communication between nodes is faster when the nodes are close together. Distance adds network latency. In the cloud, “close together” means in the same data center (sometimes even closer, such as on the same rack).

There are good reasons for nodes to be in different data centers, but this chapter focuses on ensuring that nodes that should be in the same data center actually are. Accidentally deploying across multiple data centers can result in terrible application performance and unnecessarily inflated costs due to data transfer charges.

This applies to nodes running application code, such as compute nodes, and nodes implementing cloud storage and database services. It also encompasses related decisions, such as where log files should be stored.


## Context

The Colocation Pattern effectively deals with the following challenges:

- One node makes frequent use of another node, such as a compute node accessing a database
- Application deployment is basic, with no need for more than a single data center
- Application deployment is complex, involving multiple data centers, but nodes within each data center make frequent use of other nodes, which can be colocated in the same data center

In general, resources that are heavily reliant on each other should be colocated.

A multitier application generally has a web or application server tier that accesses a database tier. It is often desirable to minimize network latency across these tiers by colocating them in the same data center. This helps maximize performance between these tiers and can avoid the costs of cloud provider data transmission.

This pattern is typically used in combination with the Valet Key and CDN Patterns. Reasons to deviate from this pattern, such as proximity to consumers and overall reliability, are discussed in *Multisite Deployment Pattern* (Chapter 15).


### Cloud Significance

Public cloud platforms typically offer many worldwide data center locations to which applications can be easily deployed. These data centers span continents, and sometimes multiple data centers are available within the same continent. An application can be easily deployed to and use the cloud platform services in any of these data centers. This multi-data center flexibility brings great advantages, but also introduces the risk that application code and services will be unnecessarily distributed across multiple data centers, resulting in extra costs and a degraded user experience.


## Impact

Cost Optimization, Scalability, User Experience


## Mechanics

When you think about it, this may seem an obvious pattern, and in many respects it is. Depending on the structure of your company’s hardware infrastructure (whether a private data center or rented space), it may have been very difficult to do anything *other than* colocate databases and the servers that accessed them.

With public cloud providers, multiple data centers are typically offered across multiple continents, sometimes with more than one data center per continent or region. If you plan to deploy to a single data center, there may be more than one reasonable choice. This is good news and bad news, since it is possible (and easy) to choose any data center as a deployment target. This makes it possible to deploy a database to one data center, while the servers that access the database are deployed to a different data center.

The performance penalty of a split deployment can be severe for a data-intensive application.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="60">
        </td>
        <td>
            Hadoop-based “big data” applications tend to be data-intensive in the extreme and require that data and compute be colocated. See <i>MapReduce Pattern</i> (Chapter 6).
        </td>
    </tr>
</table>


### Automation Helps

Enforcing colocation is really not a technological problem, but rather a process issue. However, it can be mitigated with automation.

You will want to take proactive steps to avoid accidentally splitting a deployment across data centers. Automating deployments into the cloud is a good practice, as it limits human error from repetitive tasks. If your application spans multiple data centers but each site operates essentially independently, add checks to ensure that data access is not accidentally spanning data.

Outside of automation, your cloud platform may have specific features that make colocation mistakes less likely.


### Cost Considerations

Operations will generally be less expensive if your databases and the compute resources that access them are in the same data center. There are cost implications when splitting them.

As of this writing, the Amazon Web Services and Windows Azure platforms do not charge for network traffic to enter a data center, but do charge for network traffic leaving a data center (even if it is to another of their data centers). There are no traffic charges when data stays within a single data center.


### Non-Technical Considerations

There may be non-technical influences on application architecture that result in databases being stored at a different location than the compute resources that access them. This topic is considered further in *Multisite Deployment Pattern* (Chapter 15).


## Example: Building PoP on Windows Azure

When the Page of Photos (PoP) application (which was described in the Preface) was first developed, it made sense to deploy it and use cloud services inside of a single data center.

Windows Azure allows you to specify the target data center for any resource that is deployable to a specific data center. As examples, the target data center can be specified for code deployment (such as Web Roles, Worker Roles, or Virtual Machines) and cloud services (such as Windows Azure Storage, SQL Database, and more). When such resources will be used together, they should be colocated in the same data center.

 
### Affinity Groups

Windows Azure goes a step further for some resources, supporting affinity groups. An *affinity group* is a logical grouping of resources tied to a data center. You can provide a custom name for an affinity group, using a name that makes sense for your business and is not simply a generic data center name. This is an example of a cloud platform feature that can help avoid colocation mistakes.

In the case of PoP, if we deploy to a single North American data center, we can have an affinity group called *PoP-North America-Production* for our production data center. Upon creation, the decision is made as to which North American data center will be chosen, which removes this as a decision point for any downstream deployments that use the *PoP-North America-Production* affinity group.

As of this writing, not all Windows Azure resources support affinity groups; currently only Windows Azure Compute and Windows Azure Storage are supported. Most notably, SQL Database is not supported, although it can still be placed in the same data center as the other resources.

While affinity groups are tied to a specific data center, they also provide a hint to Windows Azure that allows for further local optimization for supported resource types. Not only are they in the same data center, but your Windows Azure Storage, the code accessing it from web, and the worker roles are even closer together, with fewer router hops and less distance for data to traverse. This further reduces network latency.

Using the same affinity group across storage accounts and cloud services will ensure that they are all colocated in the same data center.


### Operational Logs and Metrics

You will also likely be gathering operational log files with Windows Azure Diagnostics (WAD) and Windows Azure Storage Analytics, available with Windows Azure Storage accounts. Apply the same affinity group to storage as you apply to compute instances.

It is a best practice to persist WAD into a special operational storage account (different from other storage accounts within your production system) both to minimize potential for contention and to make it easier to manage access to logs and metrics. Depending on the specific type of data items, individual diagnostic values are stored in either Windows Azure Blob or Windows Azure Table storage. Use the same affinity group for WAD storage to ensure it is stored in the same data center as the rest of the application.

Windows Azure Storage Analytics data are stored alongside the regular data, and are not stored in a separate storage account, so colocation is automatic.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="150">
        </td>
        <td>
            More details about the capabilities of Windows Azure Diagnostics and Windows Azure Storage Analytics can be found in <i>Horizontally Scaling Compute Pattern</i> (Chapter 2) in the <i>Example</i> section under <i>Operational Logs and Metrics</i>. Both features support allow applications to set a basic data retention policy that Windows Azure Storage uses to automatically purge data.
        </td>
    </tr>
</table>

When colocation is not possible due to technical or business reasons, Windows Azure has some services that can help. These services are mentioned in the *Example* section of *Multisite Deployment Pattern* (Chapter 15).

> <h3 align="center">Related Chapters</h3>
>
> - *MapReduce Pattern* (Chapter 6)
>
> - *Network Latency Primer* (Chapter 11)
>
> - *Valet Key Pattern* (Chapter 13)
>
> - *CDN Pattern* (Chapter 14)
>
> - *Multisite Deployment Pattern* (Chapter 15)


## Summary

The simplest way to get started in the cloud is to colocate nodes, usually all in a single data center. This is appropriate for many applications, and should be the usual configuration. Only deviate for good reason, and avoid the mistake of accidental deployment across more than one data center, including for storage of operational data.

</div>