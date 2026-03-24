# CHAPTER 1 &emsp; Scalability Primer

<div align="justify">

This primer explains scalability with an emphasis on the key differences between vertical and horizontal scaling.

Scaling is about allocating resources for an application and managing those resources efficiently to minimize contention. The user experience (UX) is negatively impacted when an application requires more resources than are available. The two primary approaches to scaling are *vertical scaling* and *horizontal scaling*. Vertical scaling is the simpler approach, though it is more limiting. Horizontal scaling is more complex, but can offer scales that far exceed those that are possible with vertical scaling. Horizontal scaling is the more cloud-native approach.

This chapter assumes we are scaling a distributed multi-tier web application, though the principles are also more generally applicable.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            <blockquote>
                This chapter is not specific to the cloud except where explicitly stated.
            </blockquote>
        </td>
    </tr>
</table>


## Scalability Defined

The *scalability* of an application is a measure of the number of users it can effectively support at the same time. The point at which an application cannot handle additional users effectively is the limit of its scalability. Scalability reaches its limit when a critical hardware resource runs out, though scalability can sometimes be extended by providing additional hardware resources. The hardware resources needed by an application usually include CPU, memory, disk (capacity and throughput), and network bandwidth.
 
An application runs on multiple *nodes*, which have hardware resources. Application logic runs on *compute nodes* and data is stored on *data nodes*. There are other types of nodes, but these are the primary ones. A node might be part of a physical server (usually a virtual machine), a physical server, or even a cluster of servers, but the generic term *node* is useful when the underlying resource doesn’t matter. Usually it doesn’t matter.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="70">
        </td>
        <td>
            <blockquote>
                In the public cloud, a compute node is most likely a virtual machine, while a data node provisioned through a cloud service is most likely a cluster of servers.
            </blockquote>
        </td>
    </tr>
</table>

Application scale can be extended by providing additional hardware resources, as long as the application can effectively utilize those resources. The manner in which we add these resources defines which of two scaling approaches we take.

- To *vertically scale up* is to increase overall application capacity by increasing the resources within existing nodes.
- To *horizontally scale out* is to increase overall application capacity by adding nodes.

These scaling approaches are neither mutually exclusive nor all-or-nothing. Any application is capable of vertically scaling up, horizontally scaling out, neither, or both. For example, parts of an application might only vertically scale up, while other parts might also horizontally scale out.

> <center><h3>Increasing Capacity of Roadways</h3></center>
>
> Consider a roadway for automobile travel. If the roadway was unable to support the desired volume of traffic, we could improve matters in a number of possible ways. One improvement would be to upgrade the road materials (“the hardware”) from a dirt road to pavement to support higher travel speeds. This is vertically scaling up; the cars and trucks (“the software”) will be able to go faster. Alternatively, we could widen the road to multiple lanes. This is horizontally scaling out; more cars and trucks can drive in parallel. And of course we could both upgrade the road materials and add more lanes, combining scaling up with scaling out.

The horizontal and vertical scaling approaches apply to any resources, including both computation and data storage. Once either approach is implemented, scaling typically does not require changes to application logic. However, converting an application from vertical scaling to horizontal scaling usually requires significant changes.


### Vertically Scaling Up

Vertically scaling up is also known simply as *vertical scaling* or *scaling up*. The main idea is to increase the capacity of individual nodes through hardware improvements. This might include adding memory, increasing the number of CPU cores, or other single-node changes.

Historically, this has been the most common approach to scaling due to its broad applicability, (often) low risk and complexity, and relatively modest cost of hardware improvements when compared to algorithmic improvements. Scaling up applies equally to standalone applications (such as desktop video editing, high-end video games, and mobile apps) and server-based applications (such as web applications, distributed multiplayer games, and mobile apps connected to backend services for heavy lifting such as for mapping and navigation).

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            <blockquote>
                Scaling up is limited by the utilizable capability of available hardware.
            </blockquote>
        </td>
    </tr>
</table>

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="100">
        </td>
        <td>
            <blockquote>
                Vertical scaling can also refer to running multiple instances of software within a single machine. The architecture patterns in this book only consider vertical scaling as it relates to physical system resources.
            </blockquote>
        </td>
    </tr>
</table>

There are no guarantees that sufficiently capable hardware exists or is affordable. And once you have the hardware, you are also limited by the extent to which your software is able to take advantage of the hardware.

Because hardware changes are involved, usually this approach involves downtime.


### Horizontally Scaling Out

Horizontally scaling out, also known simply as *horizontal scaling* or *scaling out*, increases overall application capacity by adding entire nodes. Each additional node typically adds equivalent capacity, such as the same amount of memory and the same CPU.

The architectural challenges in vertical scaling differ from those in horizontal scaling; the focus shifts from maximizing the power of individual nodes to combining the power of many nodes. Horizontal scaling tends to be more complex than vertical scaling, and has a more fundamental influence on application architecture. Vertical scaling is often hardware- and infrastructure-focused—we “throw hardware at the problem”—whereas horizontal scaling is development- and architecture-focused. Depending on which scaling strategy is employed, the responsibility may fall to specialists in different departments, complicating matters for some companies.

> <center><h3>Imperfect Terminology</h3></center>
>
> To align with well-established industry practice, we use the term *horizontal scaling*, although *horizontal resource allocation* is more descriptive. The pattern is really about how resources are allocated and assembled; application scalability is simply one of many pos-sible outcomes from the use of this pattern. Some specific benefits are listed later in the section that defines *cloud-native application*.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="100">
        </td>
        <td>
            <blockquote>
                Parallel or multicore programming to fully leverage CPU cores within a single node should not be confused with using multiple nodes together. This book is concerned only with the latter.
            </blockquote>
        </td>
    </tr>
</table>

Applications designed for horizontal scaling generally have nodes allocated to specific functions. For example, you may have web server nodes and invoicing service nodes. When we increase overall capacity by adding a node, we do so by adding a node for a specific function such as a web server or an invoicing service; we don’t just “add a node” because node configuration is specific to the supported function.

When all the nodes supporting a specific function are configured identically—same hardware resources, same operating system, same function-specific software—we say these nodes are *homogeneous*.

Not all nodes in the application are homogeneous, just nodes within a function. While the web server nodes are homogeneous and the invoicing service nodes are homogeneous, the web server nodes don’t need to be the same as the invoicing service nodes.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            <blockquote>
                Horizontal scaling is more efficient with homogeneous nodes.
            </blockquote>
        </td>
    </tr>
</table>

Horizontal scaling with homogeneous nodes is an important simplification. If the nodes are homogeneous, then basic round-robin load balancing works nicely, capacity planning is easier, and it is easier to write rules for auto-scaling. If nodes can be different, it becomes more complicated to efficiently distribute requests because more context is needed.

Within a specific type of node (such as a web server), nodes operate autonomously, independent of one another. One node does not need to communicate with other similar nodes in order to do its job. The degree to which nodes coordinate resources will limit efficiency.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            <blockquote>
                An <i>autonomous node</i> does not know about other nodes of the same type.
            </blockquote>
        </td>
    </tr>
</table>

Autonomy is important so that nodes can maintain their own efficiency regardless of what other nodes are doing.

Horizontal scaling is limited by the efficiency of added nodes. The best outcome is when each additional node adds the same incremental amount of usable capacity.


### Describing Scalability

Descriptions of application scalability often simply reference the number of application users: “it scales to 100 users.” A more rigorous description can be more meaningful. Consider the following definitions.

- *Concurrent users*: the number of users with activity within a specific time interval (such as ten minutes).
- *Response time*: the elapsed time between a user initiating a request (such as by clicking a button) and receiving the round-trip response.

Response time will vary somewhat from user to user. A meaningful statement can use the number of concurrent users and response time collectively as an indicator of overall system scalability.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="70">
        </td>
        <td>
            <blockquote>
                Example: With 100 concurrent users, the response time will be under 2 seconds 60% of the time, 2-5 seconds 38% of the time, and 5 seconds or greater 2% of the time.
            </blockquote>
        </td>
    </tr>
</table>

This is a good start, but not all application features have the same impact on system resources. A mix of features is being used: home page view, image upload, watching videos, searching, and so forth. Some features may be low impact (like a home page view), and others high impact (like image upload). An average usage mix may be 90% low impact and 10% high impact, but the mix may also vary over time.

An application may also have different types of users. For example, some users may be interacting directly with your web application through a web browser while others may be interacting indirectly through a native mobile phone application that accesses resources through programmatic interfaces (such as REST services). Other dimensions may be relevant, such as the user’s location or the capabilities of the device they are using. Logging actual feature and resource usage will help improve this model over time.

The above measures can help in formulating scalability goals for your application or a more formal *service level agreement (SLA)* provided to paying users.


### The Scale Unit

When scaling horizontally, we add homogeneous nodes, though possibly of multiple types. This is a predictable amount of capacity that ideally equates to specific application functionality that can be supported. For example, for every 100 users, we may need 2 web server nodes, one application service node, and 100 MB of disk space.

These combinations of resources that need to be scaled together are known as a *scale unit*. The scale unit is a useful modeling concept, such as with *Auto-Scaling Pattern* (Chapter 4).

For business analysis, scalability goals combined with resource needs organized by scale units are useful in developing cost projections.


## Resource Contention Limits Scalability

Scalability problems are *resource contention* problems. It is not the number of concurrent users, per se, that limits scalability, but the competing demands on limited resources such as CPU, memory, and network bandwidth. There are not enough resources to go around for each user, and they have to be shared. This results in some users either being slowed down or blocked. These are referred to as *resource bottlenecks*.

For example, if we have high performing web and database servers, but a network connection that does not offer sufficient bandwidth to handle traffic needs, the resource bottleneck is the network connection. The application is limited by its inability to move data over the network quickly enough.

To scale beyond the current bottleneck, we need to either reduce demands on the resource or increase its capacity. To reduce a network bandwidth bottleneck, compressing the data before transmission may be a good approach.

Of course, eliminating the current bottleneck only reveals the next one. And so it goes.


### Easing Resource Contention

There are two ways to ease contention for resources: don’t use them up so fast, and add more of them.

An application can utilize resources more or less efficiently. Because scale is limited by resource contention, if you tune your application to more efficiently use resources that could become bottlenecks, you will improve scalability. For example, tuning a database query can improve resource efficiency (not to mention performance). This efficiency allows us to process more transactions per second. Let’s call these *algorithmic improvements*.

Efficiency often requires a trade-off. Compressing data will enable more efficient use of network bandwidth, but at the expense of CPU utilization and memory. Be sure that removing one resource bottleneck does not introduce another.

Another approach is to improve our hardware. We could upgrade our mobile device for more storage space. We could migrate our database to a more powerful server and benefit from faster CPU, more memory, and a larger and faster disk drive. *Moore’s Law*, which simply states that computer hardware performance approximately doubles every couple of years, neatly captures why this is possible: hardware continuously improves. Let’s call t*hese hardware improvements*.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="70">
        </td>
        <td>
            <blockquote>
                Not only does hardware continuously improve year after year, but so does the price/performance ratio: our money goes further every year.
            </blockquote>
        </td>
    </tr>
</table>

Algorithmic and hardware improvements can help us extend limits only to a certain point. With algorithmic improvements, we are limited by our cleverness in devising new ways to make better use of existing hardware resources. Algorithmic improvements may be expensive, risky, and time consuming to implement. Hardware improvements tend to be straightforward to implement, though ultimately will be limited by the capability of the hardware you are able to purchase. It could turn out that the hardware you need is prohibitively expensive or not available at all.

What happens when we can’t think of any more algorithmic improvements and hardware improvements aren’t coming fast enough? This depends on our scaling approach. We may be stuck if we are scaling vertically.


## Scalability is a Business Concern

A speedy website is good for business. A Compuware analysis of 33 major retailers across 10 million home page views showed that a 1-second delay in page load time reduced conversions by 7%. Google observed that adding a 500-millisecond delay to page response time caused a 20% decrease in traffic, while Yahoo! observed a 400-millisecond delay caused a 5-9% decrease. Amazon.com reported that a 100-millisecond delay caused a 1% decrease in retail revenue. Google has started using website performance as a signal in its search engine rankings. (Sources for statistics are provided in Appendix A.)

There are many examples of companies that have improved customer satisfaction and increased revenue by speeding up their web applications, and even more examples of utter failure where a web application crashed because it simply was not equipped to handle an onslaught of traffic. Self-inflicted failures can happen, such as when large retailers advertise online sales for which they have not adequately prepared (this happens routinely on the Monday after Thanksgiving in the United States, a popular online shopping day known as Cyber Monday). Similar failures are associated with Super Bowl commercials.

> <center><h3>Comparing Performance and Scalability</h3></center>
>
> Discussions of web application speed (or “slowness”) sometimes conflate two concepts: performance and scalability.
>
> Performance is what an individual user experiences; scalability is how many users get to experience it.
>
> *Performance* refers to the experience of an individual user. Servicing a single user request might involve data access, web server page generation, and the delivery of HTML and images over an Internet connection. Each of these steps takes time. Once the HTML and images are delivered to the user, a web browser still needs to assemble and render the page. The elapsed time necessary to complete all these steps limits overall performance. For interactive web applications, the most important of the performance-related measure-ments is response time.
>
> *Scalability* refers to the number of users who have a positive experience. If the application sustains consistent performance for individual users as the number of concurrent users grows, it is scaling. For example, if the average response time is 1 second with 10 concurrent users, but the average response time climbs to 5 seconds with 100 concurrent users, then the application is not scaling. An application might scale well (handling many concurrent users with *consistent* performance), but not perform well (that consistent performance might be *slow*, whether with 100 concurrent users or just one). There is always a threshold at which scalability problems take hold; an application might perform well up to 100 concurrent users, and then degrade as the number of concurrent users increases beyond 100. In this last scenario, the application does not scale beyond 100 concurrent users.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="70">
        </td>
        <td>
            <blockquote>
                Network latency can be an important performance factor influencing user experience. This is considered in more depth starting with <i>Network Latency Primer</i> (Chapter 11).
            </blockquote>
        </td>
    </tr>
</table>


## The Cloud-Native Application

This is a book for building *cloud-native applications*, so it is important that the term be defined clearly. First, we spell out the assumed characteristics of a *cloud platform*, which enables cloud-native applications. We then cover the expected characteristics of cloud-native applications that are built on such a platform using the patterns and ideas included in this book.


### Cloud Platform Defined

The following characteristics of a cloud platform make cloud-native applications possible:

- Enabled by (the illusion of) infinite resources and limited by the maximum capacity of individual virtual machines, cloud scaling is horizontal.
- Enabled by a short-term resource rental model, cloud scaling releases resources as easily as they are added.
- Enabled by a metered pay-for-use model, cloud applications only pay for currently allocated resources and all usage costs are transparent.
- Enabled by self-service, on-demand, programmatic provisioning and releasing of resources, cloud scaling is automatable.
- Both enabled and constrained by multitenant services running on commodity hardware, cloud applications are optimized for cost rather than reliability; failure is routine, but downtime is rare.
- Enabled by a rich ecosystem of managed platform services such as for virtual machines, data storage, messaging, and networking, cloud application development is simplified.

While none of these are impossible outside the cloud, if they are all present at once, they are likely enabled by a cloud platform. In particular, Windows Azure and Amazon Web Services have all of these characteristics. Any significant cloud platform—public, private, or otherwise—will have most of these properties.

The patterns in this book apply to platforms with the above properties, though many will be useful on platforms with just some of these properties. For example, some private clouds may not have a metered pay-for-use mechanism, so pay-for-use may not literally apply. However, relevant patterns can still be used to drive down overall costs allowing the company to save money, even if the savings are not directly credited back to specific applications.

Where did these characteristics come from? There is published evidence that companies with a large web presence such as eBay, Facebook, and Yahoo! have internal clouds with some similar capabilities, though this evidence is not always as detailed as desired. The best evidence comes from three of the largest players—Amazon, Google, and Microsoft—who have all used lessons learned from years of running their own internal high-capacity infrastructure to create public cloud platforms for other companies to use as a service.

These characteristics are leveraged repeatedly throughout the book.


### Cloud-Native Application Defined

A *cloud-native application* is architected to take full advantage of cloud platforms. A cloud-native application is assumed to have the following properties, as applicable:

- Leverages cloud-platform services for reliable, scalable infrastructure. (“Let the platform do the hard stuff.”)
- Uses non-blocking asynchronous communication in a loosely coupled architecture.
- Scales horizontally, adding resources as demand increases and releasing resources as demand decreases.
- Cost-optimizes to run efficiently, not wasting resources.
- Handles scaling events without downtime or user experience degradation.
- Handles transient failures without user experience degradation.
- Handles node failures without downtime.
- Uses geographical distribution to minimize network latency.
- Upgrades without downtime.
- Scales automatically using proactive and reactive actions.
- Monitors and manages application logs even as nodes come and go.

As these characteristics show, an application does not need to support millions of users to benefit from cloud-native patterns. Architecting an application using the patterns in this book will lead to a cloud-native application. Applications using these patterns should have advantages over applications that use cloud services without being cloud-native. For example, a cloud-native application should have higher availability, lower complexity, lower operational costs, better performance, and higher maximum scale.

Windows Azure and Amazon Web Services are full-featured public cloud platforms for running cloud-native applications. However, just because an application runs on Azure or Amazon does not make it cloud-native. Both platforms offer *Platform as a Service (PaaS)* features that definitely facilitate focusing on application logic for cloud-native applications, rather than plumbing. Both platforms also offer *Infrastructure as a Service (IaaS)* features that allow a great deal of flexibility for running non-cloud-native applications. But using PaaS does not imply that the application is cloud-native, and using IaaS does not imply that it isn’t. The architecture of your application and how it uses the platform is the decisive factor in whether or not it is cloud-native.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            <blockquote>
                It is the application architecture that makes an application cloud-native, not the choice of platform.
            </blockquote>
        </td>
    </tr>
</table>

A cloud-native application is not the best choice for every situation. It is usually most cost-effective to architect new applications to be cloud-native from the start. Significant (and costly) changes may be needed to convert a legacy application to being cloud-native, and the benefit may not be worth the cost. Not every application should be cloud-native, and many more cloud applications need not be 100% cloud-native. This is a business decision, guided by technical insight.

Patterns in this book can also benefit cloud applications that are not fully cloud-native.


## Summary

Scalability impacts performance and efficiency impacts scalability. Two common scaling patterns are vertical and horizontal scaling. Vertical scaling is generally easier to implement, though it is more limiting than horizontal scaling. Cloud-native applications allocate resources horizontally, and scalability is only one benefit.

</div>