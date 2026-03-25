# CHAPTER 15 &emsp; Multisite Deployment Pattern

<div align="justify">

This advanced pattern focuses on deploying a single application to more than one data center.

Deploying to multiple data centers helps reduce network latency by routing a client to the nearest data center, which improves the user experience. This also provides the seeds of a solution that can handle failover across data centers and improve availability.

If multisite deployment does not improve user experience and your application does not need cross-data center failover, use of this pattern may be overkill.


## Context

The Multisite Deployment Pattern is effective in dealing with the following challenges:

- Users are not clustered near any single data center, but form clusters around multiple data centers or are widely distributed geographically
- Regulations limit options for storing data in specific data centers
- Circumstances require that the public cloud be used in concert with on-premises resources
- Application must be resilient to the loss of a single data center

This pattern helps deal with a user base that is not conveniently clustered in a single geographic area. Some of the reasons for this include use of the application from unpredictable locations during travel, globally distributed mobile applications, and companies with offices distributed across many geographical locations.

This pattern is similar in some ways to *CDN Pattern* (Chapter 14), in that we strive to bring our application closer to our users. The CDN focus is on bringing files closer to our users, and it only helps when sending data to our users, not users sending data to the application. This pattern is more powerful than a CDN because it brings more application facilities closer to users with lower latency, even when users send data to the application. This pattern is more limited in one way: there are many more CDN nodes than there are full-blown data centers. For these reasons, these two patterns are often used together.


### Cloud Significance

Networking cloud services are available for geographic load balancing and cross-data center failover. Data-oriented cloud services are also available for database synchronization and geo-replication of cloud storage. These services, plus access to multiple geographically distributed data centers, simplify some of the most complex aspects of multisite deployment.


## Impact

Availability, Reliability, Scalability, User Experience


## Mechanics

If all global traffic for an application is served from a single data center, then responsiveness is better near that data center than from more distant regions of the world. Generally speaking, the farther away, the poorer the responsiveness. Which data center to choose then? This pattern is about choosing more than one data center to offer the best available user experience throughout the world.

The major public cloud platforms have multiple data centers on multiple continents. With Windows Azure and Amazon Web Services, you select the specific data center to which you will deploy. Let’s assume you choose one data center location in Asia, one in Europe, and one in the United States, and deploy an instance of your application to each data center.

Once your application is deployed to multiple data centers, you need to decide how users will be directed to the appropriate one. In this pattern, we will strive to make this process transparent to users in order to provide the best user experience. In the most basic scenarios, this requires smart routing and data replication services.

To route users to the closest data center, use your cloud platform’s service for geographic load balancing and configure it to direct users to the closest data center. These services include the Windows Azure Traffic Manager and Elastic Load Balancing from Amazon Web Services.

You also need to replicate data across all data centers if you want users to see the same data, regardless of the data center to which they route. This is further explored in the example given later in this chapter. The end result can be seen in Figure 15-1, where data centers replicate data as needed. Since each data center has a local copy of any needed data, users can be conveniently routed to the one nearest to them for the best performance. As we shall see later, this topology also can be leveraged to handle rare scenarios when one of the data centers is not available.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            Another possibility is to assign each user to a single data center, though that scenario is not explored.
        </td>
    </tr>
</table>

With geographic load balancing and data replication in place, you have the basics of a multisite deployment. Users will experience better performance than if they always had to directly access data and services from a data center located on another continent.

<div align="center">

![Local Image](../images/chapter_15/image1.png)

*Figure 15-1. Data centers stay in sync so that users can access the closest one, or an alternate in case of failure. The connection spanning the continents represents data center-to-data center synchronization. External user devices are shown accessing the closest data center.*

</div>


### Non-Technical Considerations in Data Center Selection

Sometimes there are non-technical considerations that compel you to separate application tiers across data centers or to choose one data center over another. Sometimes a data center location is chosen in order to comply with government regulations, industry requirements, and other so-called *data sovereignty* issues. For example, some countries in the European Union (EU) require that applications operating in the EU store personally identifying information within the EU. As another example, the credit card industry has specific expectations of applications handling credit card data. These expectations may further limit or influence options in data center selection.

Finally, consider a business constraint in which a customer is not ready to move a database from its on-premises location into the cloud but wants to access it from cloud servers. Cloud platform services can extend a company’s internal network into the cloud with a Virtual Private Network (VPN). It securely handles this scenario so that applications running on cloud resources can easily query databases located elsewhere. This is yet another factor influencing your data center footprint.

Thus, business considerations may override purely performance-based data center selection criteria. While these considerations can impact cloud architecture, a treatment of this complex topic is not the focus of this pattern.


### Cost Implications

As of this writing, both Windows Azure and Amazon platforms charge a fee for data leaving the data center, but not for data coming into the data center. Thus, traffic across data centers would incur these charges. Of course, this applies even within a single data center location because remotely accessing a cloud application from outside the data center results in data leaving the data center. In the context of this pattern, this also applies to keeping databases in sync across data centers and accessing on-premises databases.

Deploying your application across multiple data centers has cost implications. Such costs should be weighed against the user experience and the robustness limitations of a single data center. And of course, if multisite deployment is needed, consider the cost and complexity of doing this without the convenience of cloud services.


### Failover Across Data Centers

*Failover* is a feature of an application that enables the application to continue to function using secondary resources when there is a failure with the primary resources. In the context of this pattern, we are concerned with the loss of a data center and the ability of our application to continue to function using the other available data centers. We may want to failover from one data center to another due to a natural disaster, such as a hurricane, or due to a failure by the cloud provider, such as a software bug or configuration error that impacts services.

The tools and techniques used to create a multisite deployment can also help to make your application more failure-resistant. The same services used for geographic load balancing that route users to the closest data center can be used to account for failover scenarios. In particular, they can be configured to monitor the health of the services to which they are directing users, and if any service is unavailable, traffic will be routed to a healthy instance.

It takes time for the geographic load balancer to figure out that a service is not responding before it fails over to a healthy copy. During that time, application access may result in errors. Client-side retries, such as *Busy Signal Pattern* (Chapter 9), can help shield users from these errors. Once failover is complete, users will be routed to data centers; if these data centers are on other continents, their experience will be degraded, but it will work. This is usually better than not working at all, although that depends on the specific application.

As unhealthy services become healthy again, traffic can be delivered, returning system responsiveness to maximum levels.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="60">
        </td>
        <td>
            This scheme does not guarantee instant or seamless failover. There will be downtime.
        </td>
    </tr>
</table>

One approach to failover is to have the secondary resources already deployed, just in case they are needed. This arrangement is known as either *active/passive* or *active/ active*, depending on whether the secondary resources are just sitting around in case they are needed or are in active use (the primary data center accounts for the first “active” resource). Another option is to not have any spare resources that are already running, but to deploy those resources as needed for a failover. This obviously would take longer, but is also less costly. This topic is explored further in the *Example*.

This is part of a *larger disaster recovery (DR)* plan. Many of these concerns parallel those for non-cloud applications, but are beyond the scope of this chapter.


## Example: Building PoP on Windows Azure

The Page of Photos (PoP) application (which was described in the Preface) faces a few challenges in providing a seamless user experience across the globe:

- Routing users to the nearest data center so that they have the best user experience
- Replicating account data so that it is available at all data centers, both for account owners and visitors
- Replicating identity information so that we can correctly authenticate a user in any data center and know they are the same user
- Serving photos globally and efficiently


### Choosing a Data Center

As of this writing, Windows Azure offers eight geographically distributed data centers:

- Two in Asia: East (Hong Kong) and Southeast (Singapore)
- Two in Europe: North (Netherlands) and West (Ireland)
- Four in the United States: North Central (Illinois), East (Virginia), South Central (Texas), and West (California)

Since PoP users are all over the world, we will deploy to one data center in Asia (Singapore), one in Europe (Ireland), and one in North America (Virginia). However, PoP users should not need to know about the three data centers; using PoP should just work, regardless of where the user is. In other words, *http://www.pageofphotos.com* should be the only address a user ever needs to remember.


### Routing to the Closest Data Center

Windows Azure Traffic Manager is a scalable, highly available, easily configured, geographic load balancing service that continually monitors the responsiveness of all application deployments behind the scenes. It ensures that a visitor to *http://www.pageofphotos.com* is transparently directed to the data center that will give the visitor the best response.

> <h3 align="center">Configuring Traffic Manager</h3>
>
> Traffic Manager configuration includes specifying a public domain name (*http:// www.pageofphotos.com*) that users see, listing the individual instances (*http://pop-asia.cloudapp.net*, *http://pop-eur.cloudapp.net*, *http://pop-usa.cloudapp.net*), and configuring the routing rules for the closest instance. The endpoint names listed are just examples, but show use of a consistent naming pattern that can help avoid confusion and errors.

In our case, as an example, visiting *http://www.pageofphotos.com/timothy* from Asia would be satisfied by the *http://pop-asia.cloudapp.net* instance.


### Replicating User Data for Performance

Consider a PoP user in Ireland who posts some new photos. They can be neatly and efficiently stored in the Ireland data center with a minimum of network latency. The collection can be shared with friends in Ireland who also access them from the Ireland data center. So far, so good.
 
Now consider that a link to this page of photos is emailed to some friends in Boston. Traffic from Boston will be routed to the Virginia data center. What will happen? To make it work, the Virginia data center needs access to the same data that was saved to the Ireland data center. Our approach with PoP is split into two complementary schemes: one for uploaded photos, another for the rest of the account data.

The photos are uploaded to blobs in Window Azure Storage in the nearest data center. The application makes no effort to explicitly replicate these photos to other data centers. Thumbnails for these photos are generated in the same data center [using *Queue-Centric Workflow Pattern* (Chapter 3)] and also stored as blobs. Photos are not replicated to all data centers as we plan to rely on *CDN Pattern* (Chapter 14) to deliver them most efficiently to users.

In addition to the photos, there is metadata around the photos such as:

- The account responsible for uploading the photo
- The URL needed for accessing the photo
- The text description of the photo
- And so on.

This data is stored in a Windows Azure SQL Database. The new photo is uploaded to the nearest data center. The metadata is saved to SQL Database in the nearest data center. But, unlike the photo blobs, the SQL Database data is replicated across all three data centers using SQL Database Data Sync service.

> <h3 align="center">Configuring Data Sync</h3>
>
> SQL Data Sync service configuration requests the provisioning of a Data Sync Server, creates a Sync Group that will specify the database instances to be kept in sync, then adds the database names of the three instances that will be kept in sync to the Sync Group. It selects the desired sync rules, and lets it run. Syncs happen on a schedule; the most aggressive supported frequency is five minutes between synchronization jobs.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="60">
        </td>
        <td>
            As of this writing, the SQL Database Data Sync service is currently in preview. It is not yet supported for production use.
        </td>
    </tr>
</table>

A user from anywhere in the world will be directed to the nearest data center when accessing a page of photos. It returns the HTML page of photos; the individual photos will be served up from one of the 24 global CDN nodes.
 
Note the eventual consistency of SQL Database instances. Consider our earlier example of a PoP user from Ireland sharing a page of photos with a friend in Boston. There will be a short window of time during which PoP users in Ireland see new photos, but PoP users in the United States (and Asia) do not; this is eventually resolved as the SQL Database Data Sync service catches up.


### Replicating Identity Information for Account Owners

While any anonymous user can view any page of photos, all uploaded photos belong to a PoP account holder. An account on PoP is easily created because it does not manage user credentials, but rather relies on *federated authentication*. PoP is configured to allow users to log in using an *identity provider (IdP)* that they are likely already using. Supported IdPs leveraged by PoP include Google, Yahoo!, and Facebook. (Many other identity providers can be supported, but PoP chooses not to make use of them.) The first time a user signs into their IdP, they give their IdP permission to share login information with PoP. (Actually, the IdP shares with the Access Control Service (ACS), not directly with PoP. PoP gets the information from ACS.) For any IdP supported by PoP, the important item we want is a validated email address. Because each IdP trusted by PoP is reputable, we trust it. We then use the email address as a unique account key in our SQL Database instances. This way, we can tie uploaded photos to a specific user account.

Note further that PoP does not know or store the user’s password. Only the underlying IdP does. Using cryptographic techniques, the IdP can securely communicate to PoP that users are who they say they are and pass along a cryptographically secure *claim* that provides their email address. This is all PoP needs. The PoP development effort can be focused on features, not infrastructure.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="200">
        </td>
        <td>
            Federated identity, claims, and identity providers are important concepts for building applications that deal with identity in a cloud-native manner. A whole book could be written to explain these important topics. (In fact, Appendix A references such a book.) The key takeaway for this chapter is that some external entity (an IdP) is telling us the current user owns a specific email address (indicated by a claim) and that we can trust that this is accurate.
        </td>
    </tr>
</table>

The Windows Azure Access Control Service (ACS) acts as an intermediary between PoP and each IdP. It turns out that this is a huge simplification for PoP because there is so much variation in how an IdP can behave. It can be fairly complicated if your application needs to interact with the IdP directly. ACS helps applications manage the relationship with one or more identity providers, abstracting away the implementation details and normalizing them so that claims are consistent when they reach your application.

> <h3 align="center">Configuring ACS</h3>
>
> Access Control Service configuration consists of establishing a set of rules for each supported provider. The basic process is mostly handled through a set of well-defined steps in a wizard-like user interface; this is appropriate as such configurations are routine. Google and Yahoo! configurations can be completed in the ACS interface, though Facebook requires additional configuration at the Facebook site. ACS also can be configured programmatically. The configuration for PoP is simple: pass through all claims from the IdP (which will include an email address claim), and inject an "Admin" role claim for a few special email accounts (for PoP site administrators).
 
This description of how ACS helps authentication work for PoP is necessary to provide context for the multisite deployment. The upshot is that this configuration needs to be repeated for each identity provider in each supported data center.


### Data Center Failover

Given the measures we have taken to make PoP available in three data centers, what more do we need to do to ensure we can failover in the event one of the data centers fails?

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="130">
        </td>
        <td>
            Supporting data center failover is a big decision with many risks and tradeoffs. The approach outlined here is appropriate for the PoP application and business. These risks and tradeoffs may or may not be acceptable for your application and business.
        </td>
    </tr>
</table>

PoP replicates Windows Azure SQL Database instances. As mentioned previously, there is still a window within which data loss could occur. If this is not good enough, it may be challenging to overcome.

PoP has Access Control Service identity providers configured in each data center so that PoP will recognize the data center, regardless of which one is used to authenticate.

PoP uses Windows Azure Storage to save photos and thumbnails as blobs. We have taken no measures to replicate them because the Windows Azure Platform geo-replicates blobs on our behalf. (Windows Azure Storage Tables are also geo-replicated.) Each data center is paired with another on the same continent. In the list of data centers above, the two Asia data centers replicate, the two Europe data centers replicate, and in the United States, North Central replicates with South Central, and East replicates with West. In the extremely rare event of a disaster so great that a data center is lost or unavailable for an extended period of time, storage will failover to its replication pair. You do not need to do anything. There will be a period of time when blob requests fail, but once the failover is complete, they will resume working correctly. Note, however, that there is still a window within which data loss could occur.

PoP uses the CDN to deliver photos and thumbnails close to the users. Because all of the eight Windows Azure data centers also host CDN nodes, a data center failure could likewise impact the CDN. Furthermore, the other CDN nodes could be impacted independently. The built-in CDN routing logic will notice a failure of a CDN node and change where traffic is routed. There is still a window within which files may not be delivered. Given the nature of the CDN, it is likely that the many distributed CDN nodes will somewhat shield users during the interval where blobs are unavailable and in the process of being failed over because many photos will already be cached around the world.

The final step is to update the configuration in Traffic Manager. Earlier we described how Traffic Manager routes to the closest (most responsive) data center. Traffic Manager also handles failover scenarios. For example, if a hurricane rendered the Virginia data center unavailable, Traffic Manager can route users to Singapore or Ireland (whichever it determines to be faster for any individual user). While the user experience is degraded compared to direct access to the North American data center, it is a better user experience than no availability. There is still a window within which Traffic Manager may route to a failing data center.

Because the data center can failover to resources that are already provisioned and in active use, this is an *active/active* failover configuration.

> <h3 align="center">Considering Disaster Recovery and User Experience</h3>
>
> How should use of multisite deployment balance disaster recovery (DR) and user experience (primarily performance) benefits? There is no single right answer, but it is generally helpful to look at DR and user experience as independent concerns that happen to have overlapping solutions. Either could drive the decision to go multisite, or the combined value may be needed to justify a multisite deployment.


### Colocation Alternatives

As mentioned earlier, there are a number of non-technical business factors that may limit location options for data storage. In Windows Azure, there are a number of features that might be of use in making the most of such constraints. Briefly, these include:

- Windows Azure Virtual Networking and Windows Azure Connect support a secure Virtual Private Network (VPN) connection between your Windows Azure service and another network. This enables many integration scenarios, such as allowing compute resources running in Windows Azure to access a database running in a private data center.
- Windows Azure Service Bus supports several secure scenarios for communication across firewalls, publish/subscribe to connected devices, and publish/subscribe to disconnected devices. These capabilities enable secure and efficient communication across applications regardless of firewall or data center configurations.
- SQL Data Sync Service use for cloud-to-cloud synchronization was already described. This service can also be used to synchronize between the cloud and on-premises databases.

These services can help integrate with a database or services that are on premises or in another data center.

> <h3 align="center">Related Chapters</h3>
>
> - *Horizontally Scaling Compute Pattern* (Chapter 2)
>
> - *Queue-Centric Workflow Pattern* (Chapter 3)
>
> - *Eventual Consistency Primer* (Chapter 5)
>
> - *Node Failure Pattern* (Chapter 10)
>
> - *Network Latency Primer* (Chapter 11)
>
> - *Colocate Pattern* (Chapter 12)
>
> - *Valet Key Pattern* (Chapter 13)
>
> - *CDN Pattern* (Chapter 14)


## Summary

Using the Multisite Deployment Pattern primarily helps improve the user experience for a geographically distributed user base. Users need not be all over the world, but at least distributed such that more than one data center provides sufficient value if the goal is to improve performance.

This pattern is also useful for applications requiring a failover strategy in case one data center becomes unavailable. This is a complex subject, but many of the components in this chapter will get you started.

Because the use of this pattern will result in a more complex and more expensive application than a single data center solution, the business value needs to be assessed and compared with the cost.

</div>