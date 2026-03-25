# CHAPTER 8 &emsp; Multitenancy and Commodity Hardware Primer

<div align="justify">

This primer introduces multitenancy and commodity hardware and explains why they are used by cloud platforms.

Cloud platforms are optimized for cost-efficiency. This optimization is partially driven by the high utilization of services running on cost-efficient hardware that manifests as multitenant services running on commodity hardware.

The decisions made in building the cloud platform also influence the applications that run on it. The impact to the application architecture of cloud-native applications manifests through horizontal scaling and handling failure.


## Multitenancy

*Multitenancy* means there are multiple tenants sharing a system. Usually the system is a software application operated by one company, the host, for use by other companies, the tenants. Each tenant company has individual employees who access the software. All employees of a tenant company can be connected within the application while other tenants are invisible; this creates the illusion for each tenant that they are the only customers using the software.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="120">
        </td>
        <td>
            In a single tenant model, if an application needs a database, it gets its own instance. This simplifies capacity management (for individual applications), but at the expense of overall efficiency, as many database servers (and other types of servers) will be running with low overall utilization much of the time.
        </td>
    </tr>
</table>

In the cloud, multitenant services are standard: data services, DNS services, hardware for virtual machines, load balancers, identity management, and so forth. Cloud data centers are optimized for high hardware utilization and that drives down costs.

> <h3 align="center">Multitenancy: Not Just for Cloud Platform Services</h3>
>
> Cloud platforms have embraced multitenant services, so why not you? Software as a Service (SaaS) is a delivery model in which a software application is offered as a managed service; customers simply rent access. You may wish to build your SaaS application as multitenant on the cloud so that you can leverage the cost-efficiencies of shared instances. You can choose to be multitenant all the way through for maximum savings, or just in some areas but not others, such as with compute nodes but not database instances, for example.
>
> Sometimes SaaS applications are also able to (perhaps anonymously) glean valuable business insights and analytics from the aggregate data they are managing across many customers.
>
> There are also downsides to multitenant services. Your architecture will need to ensure *tenant isolation* so that one customer cannot access another customer’s data, while still allowing individual customers access to their own data and reporting.

Two common areas of concern are security and performance management.


### Security

Any individual tenant on a multitenant service is placed in a security sandbox that limits its ability to know anything about the other tenants, even the existence of other tenants. This is handled in different ways on different services. For example, hypervisors manage security on virtual machines, relational databases have robust user management features, and cryptographically secure keys are used as controls for cloud storage.

Unlike a tenant in an apartment building, you won’t be running into neighbors, and won’t need to remember their names. If tenant isolation is successful, you operate under the illusion that you are the only tenant.


### Performance Management

Applications in a multitenant environment compete for system resources. The cloud platform is responsible for fairly managing competing resource needs among tenants. The goal is to achieve high hardware utilization in all service instances without compromising the performance or behavior of the tenants. One strategy employed is to enforce quotas on individual tenants to prevent them from overwhelming specific shared resources. Another strategy is to deploy resource-hungry tenants alongside tenants with low resource demands. Of course resource needs are dynamic and therefore unpredictable. The cloud platform is continuously monitoring, reorganizing (moving tenants around), and horizontally scaling service instances—but it’s all done transparently. See *Auto-Scaling Pattern* (Chapter 4).

This type of automated performance management is less common in the non-cloud world, but the approach is important to understand as it will impact your cloud-native application.


### Impact of Multitenancy on Application Logic

While the cloud platform can do a very good job of monitoring active tenants and continually rebalancing resources, there are scenarios where a burst of activity can temporarily overwhelm a service instance. This can happen when multiple applications get really busy all of a sudden. What happens? The cloud platform will proactively decide how to redistribute tenants as needed, but in the meantime (usually a few seconds to a few minutes), attempts to access these resources may experience transient failures that manifest as busy signals. For more information about responding to *transient failures* or busy signals, refer to *Busy Signal Pattern* (Chapter 9).

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="80">
        </td>
        <td>
            Multitenancy services get busy, occasionally responding to service calls with a busy signal. Plan on it happening to your application and plan on handling it.
        </td>
    </tr>
</table>


## Commodity Hardware

Cloud platforms are built using commodity hardware. In contrast to either low-end hardware or high-end hardware, commodity hardware is in the middle, chosen because it has the most attractive value-to-cost ratio; it’s the-biggest-bang-for-the-buck hardware. High-end hardware is more expensive than commodity hardware. Twice as much memory and twice as many CPU cores typically will be more than twice the total cost. A dominant driver for using cloud data centers is cost-efficiency.

> <h3 align="center">Data Center Is a Competitive Differentiator</h3>
>
> It is not credible to claim that traditional data centers were developed without cost concerns. But with more heterogeneous and higher-end hardware populating those data centers, the emphasis was certainly different. These data centers were there to serve as home for the applications we already had on the hardware we were already using, optimized for individual vertically scaling applications rather than the far more ambitious goal of optimizing across all applications.
>
>The larger cloud platform vendors are tackling this ambitious goal of optimizing across the whole data center. While Windows Azure, Amazon Web Services, and other cloud platforms support virtual machine rentals that can run legacy software on Windows Server or Linux, the greatest runtime efficiency lies with cloud-native applications. This model should become attractive to more and more customers over time as it becomes increasingly cost-efficient as cloud platform vendors drive further efficiencies and pass along the cost savings.
>
> In particular, Microsoft enjoys economies of scale not available to most companies. Partly this is because it is a very large technology company in its own right, but also stems from its broad, mature product lines and platforms. By methodically updating its own internal applications and existing products to leverage Windows Azure, while also adding new cloud offerings, Microsoft benefits from a practice known as *eating your own dogfood*, or *dogfooding*. Through dogfooding, Microsoft's internal product teams use the Windows Azure platform as customers would, identify feature gaps or other concerns, and then work directly with the Windows Azure team so that more features can be developed using real world scenarios, resulting in a more mature platform sooner than might otherwise be possible.
>
> The largest cloud platform vendors are in a battle to produce and offer advanced features more efficiently than their competitors so that they can offer competitive pricing. Although I don't know which cloud platform vendors will win in the end (and I don't envision a world where Windows Azure and Amazon Web Services aren't both big players), the clear winners in this battle are the customers—that's us.

This is an economic decision that helps optimize for cost in the cloud. The main challenge to applications is that commodity hardware fails more frequently than high-end hardware.


### Shift in Emphasis from MTBF to MTTR

The ethos in the traditional application development world emphasized minimizing the *mean time between failures (MTBF)*, meaning that we worked hard to ensure that hardware did not fail. This translated into high-end hardware, redundant components (such as RAID disk drives and multiple power supplies), and redundant servers (such as secondary servers that were not put into use unless the primary server failed for the most critical systems). On occasion when hardware did fail, the application was down until a human fixed the problem. It was expensive and complex to build software that effortlessly survived a hardware failure, so for the most part we attacked that problem with hardware.

The new ethos in the cloud-native world emphasizes minimizing the *mean time to recovery (MTTR)*, meaning that we work hard to ensure that when hardware fails, only some application capacity is impacted, and the application keeps on working. In concert with patterns in this book and in alignment with the services offered by the major cloud platforms, this approach is not only viable, but also attractive due to the great reduction in complexity and new economic efficiencies.

> <h3 align="center">Hardware Failure Is Inevitable, but Not Frequent</h3>
>
> Discussion of recovering from failures in commodity hardware can be misleading. Just because commodity hardware fails *more frequently* than high-end hardware does not mean it fails *frequently*. Hardware failures impact only a small percentage of the commodity servers in the data center every year. But be ready: eventually it will be your turn.

The cloud platform assumes that much of the MTTR duties are completed through automation, but also imposes requirements on your application, forming something of a partnership in handling failure.


### Impact of Commodity Hardware on Application Logic

Cloud-native applications expect failure and are able to detect and automatically recover from common scenarios. Some of these failure scenarios are present because the application is relying on commodity hardware.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="60">
        </td>
        <td>
            Commodity hardware fails occasionally. Plan on it happening to your compute nodes and plan on handling it.
        </td>
    </tr>
</table>

Failure may simply be due to an issue with a specific physical server such as bad memory, a crashed disk drive, or coffee spilled on the motherboard. Other scenarios originate from software failures. For more information about responding to failures at the individual node level, refer to *Node Failure Pattern* (Chapter 10).

The failure scenario just described may be obvious: your application code is running on commodity hardware, and when that hardware fails your application is impacted. What is less obvious is that cloud services on which your application also depends (databases, persistent file storage, messaging, and so on) on are also running on commodity hardware. When these services experience a disruption due to a hardware failure, your application may also be impacted. In many scenarios, the cloud platform service recovers without any visible degradation, but sometimes capacity is temporarily reduced, forcing the calling application to handle transient failure. For more information about responding to failures encountered during calls to a cloud service, refer to Chapter 3.

 
### Homogeneous Hardware

Cloud data centers also strive to use homogeneous hardware for easier management and maintenance of resources. Procurement of large scale homogeneous hardware is possible through inexpensive and readily available commodity hardware.

The level of homogeneity in the hardware is unlikely to directly impact applications as long as the allocated capacity in a virtual machine remains predictable.

> <h3 align="center">Homogeneous Hardware Benefits in the Real World</h3>
>
> Southwest Airlines is one of the most consistently profitable airlines in the world, in part fueled by their insistence on homogeneous commodity hardware: the Boeing 737. This is the only type of plane in the whole fleet, vastly reducing complexity in breadth of skills needed by mechanics and pilots, streamlining parts inventory, and probably even simplifying software that runs the airlines since there are fewer differences between flights.


## Summary

Cloud platform vendors make choices around cost-efficiency that directly impact the architecture of applications. Architecting to deal with failure is part of what distinguishes a cloud-native application from a traditional application. Rather than attempting to shield the application from all failures, dealing with failure is a shared responsibility between the cloud platform and the application.

</div>