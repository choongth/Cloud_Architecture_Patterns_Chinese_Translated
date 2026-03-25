# CHAPTER 5 &emsp; Eventual Consistency Primer

<div align="justify">

The Eventual Consistency primer introduces eventual consistency and explains some ways to use it. This primer uses the CAP Theorem to highlight the challenges of maintaining data consistency across a distributed system and explains how eventual consistency can be a viable alternative.

In an eventually consistent database, simultaneous requests for the same data value can return different values. This condition is temporary, as the values become “eventually” consistent.

Eventual consistency stems from a choice in the way data is updated. It is an alternative to the use of distributed transactions. It can lead to better scalability, higher performance, and lower cost. Using it or not is a business decision.

At any moment, most of an eventually consistent database is consistent, with some small number of values still being updated. It is common for data values to be inconsistent for only seconds, but is not required. It depends on the application and can vary depending upon current circumstances.


## CAP Theorem and Eventual 

*Brewer’s CAP Theorem* (or simply the *CAP Theorem*) considers three possible guarantees for data within a distributed application: *Consistency*, *Availability*, and *Partition Tolerance* (which spell CAPT, though the more pronounceable CAP is used). *Consistency* means everyone gets the same answer; *availability* means clients have ongoing access (even if there is partial system failure); and *partition tolerance* means correct operation, even if nodes within the application are cut off from the network and unable to communicate. The CAP Theorem posits that of these three possible guarantees, an application can only pick two.
 
Guaranteeing consistency is easy when data is on a single node, but once the data is distributed, partition tolerance needs to be considered. What happens if our nodes cannot communicate with each other due to failure, or simply cannot do so fast enough to meet our performance and scalability targets? Different tradeoffs are possible. One popular choice for cloud-native applications is to favor partition tolerance and availability and give up the guarantee of immediate consistency.

Applications that do not guarantee immediate consistency are said to be *eventually consistent*. The use of eventual consistency makes sense when the business value (risk, downside, or cost) is deemed superior to immediate consistency.

While this approach is not as familiar to those from the relational database world, eventual consistency can be a powerful feature that enables better scalability. Eventual consistency is not a deficiency or design flaw. When used appropriately, it is a feature.


## Eventual Consistency Examples

The term eventual consistency is relatively new, but the idea is not. An old example can be found with the Domain Name System (DNS). DNS powers the Internet name resolution that is responsible for turning human-friendly web addresses (such as *http://www.pageofphotos.com*) into a computer-friendly IP address (such as 12.34.56.789). When the IP Address for a domain name is changed, it usually takes hours before the update propagates to all DNS servers (which may have the old address cached) across the Internet. This is considered a good tradeoff; IP addresses change infrequently enough that we tolerate the occasional inconsistency in exchange for superb scalability. After the IP address has been changed but before it is fully propagated, some users will be directed to the old site IP address, and some to the new site IP address.

> <h3 align="center">Eventual Satisfaction</h3>
>
> Eventually consistent does not mean that the system doesn’t care about consistency. Most data is consistent virtually all of the time. There is just a business-tolerable delay before updates fully propagate. During this delay, different users may see different versions of the data.

The Page of Photos (PoP) application is eventually consistent because there is a delay after a photo is uploaded, but before it appears to visitors on the site. Furthermore, some viewers may see the photo before others do. Part of this is due to data replication across data centers, but some is just internal processing, such as the ingestion process for newly uploaded photos.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="100">
        </td>
        <td> 
            Data values that are no longer current are referred to as being <i>stale</i>. Sometimes stale data is visible to users. When a user sees data they know is stale, and there is a delay before they see the most current data, we call this resolution <i>eventual satisfaction</i>.
        </td>
    </tr>
</table>

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="60">
        </td>
        <td>
            If you've ever experienced "buying" tickets online, only to find out that they have already been sold, you've seen eventual consistency in action.
        </td>
    </tr>
</table>

Windows Azure, Amazon Web Services, Google App Engine, and other cloud platforms are themselves eventually consistent in a variety of circumstances. For example, it takes many minutes after activating global services such as CloudFront (a global CDN service from Amazon) and Traffic Manager (a global load-balancing service from Windows Azure) for them to propagate to nodes around the world.

The CAP Theorem has formalized these ideas and they have taken hold in distributed systems in the cloud and become popular with some databases.


## Relational ACID and NoSQL BASE

The traditional relational database offers four so-called *ACID* guarantees:

*Atomicity*  
&emsp; &ensp; All of a transaction completes, or none of it does.

*Consistency*  
&emsp; &ensp; Data is always valid according to schema constraints.

*Isolation*  
&emsp; &ensp; Transactions competing to change the same data are applied sequentially.

*Durability*  
&emsp; &ensp; Committed changes are not lost.

These guarantees originated in a world where databases ran on a single node. They become more complex and expensive if the database is distributed.

For a single-node application, the CAP Theorem is not interesting, as partition tolerance need not be considered. As databases become more distributed (clustered, or with a geographically distributed failover node), the CAP Theorem consideration comes into play.
 
The CAP Theorem informs us that we must pick two of the three guarantees, which can be written in shorthand as CA, AP, and CP. All three combinations result in different behaviors. The one we will focus on here is AP (availability and partition tolerance), also known as eventually consistent.

By definition, eventually consistent databases do not support ACID guarantees, though they do support BASE. A *BASE* database is:

*Basically Available*  
&emsp; &ensp; The system will respond even with stale data.

*Soft State*  
&emsp; &ensp; State might not be consistent and might be corrected.

*Eventually Consistent*  
&emsp; &ensp; We allow for a delay before any individual data change completely propagates through the system.

BASE is commonly associated with NoSQL databases, and NoSQL database services are popular in the cloud. *NoSQL*, or *Not Only SQL*, is a database style that has emerged in recent years. NoSQL databases tend to be designed for very high scale at the expense of some advanced features of traditional relational databases. For example, they tend to have limited transactional capabilities and no ACID guarantees. Notably, they are usually designed to support sharding, which is further explored in *Database Sharding Pattern* (Chapter 7).
Unlike the acid and base you may have learned about in high school chemistry class, ACID and BASE can be used together safely, even in the same application.


## Impact of Eventual Consistency on Application Logic

Previous examples have focused on eventual consistency scenarios that may be familiar or seem intuitive. Developers are often surprised when eventual consistency is used in a database. We have all come to expect that we can read a value back from a database after we’ve written it. This is not guaranteed if the database is eventually consistent.

Google’s App Engine Datastore service and Amazon’s S3 storage service are eventually consistent. Sometimes you get a choice: Amazon’s SimpleDB database service has configurable consistency (with different performance characteristics). Many NoSQL databases are eventually consistent.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="70">
        </td>
        <td>
            Windows Azure Storage is <i>immediately consistent</i>; you can immediately read back whatever you wrote. Sometimes this is also referred to as <i>strongly consistent</i> or <i>strictly consistent</i>.
        </td>
    </tr>
</table>
 
It is important to note that eventually consistent databases always support some level of atomicity. Check the documentation for your eventually consistent database to understand what is considered atomic, but typically a database operation that writes a single record that changes ten properties will propagate as an atomic unit. Eventually consistent does not extend inside this atomic unit. None of the ten changes will show up individually; there is no partial update. None of the updates are visible until all of the updates are visible.

How should a developer deal with data storage that is eventually consistent?


### User Experience Concerns

Sometimes, a reasonable approach is to act like it doesn’t matter. Just go with the data you have at the moment. Surprisingly, this works very well in many scenarios where eventually consistent data makes sense. Often, users can’t tell the difference.

However, sometimes that depends on who the user is. If the user is the one who just updated the data, it is more important to show the data the user expects, rather than wait for eventual satisfaction. In such cases, it may be sufficient for the user interface to update the user interface to reflect the most recent user-initiated change. In this case, the user interface intentionally does not refresh data from the database, knowing it may be stale.


### Programmatic Differences

Data storage systems vary, but there are some common threads. Optimistic concurrency and “last write wins” models are common. These two features go hand-in-hand because they allow an application to retrieve a value, update it in memory, and then conditionally write it back. The condition is the timestamp from the original value; if the timestamp in storage is the same as the timestamp on the original value, there have been no changes in the meantime, so the update does not lose data.

Other systems are more sophisticated than “last write wins.” The Amazon Dynamo Database was built for the shopping cart on Amazon.com. Dynamo is designed to merge multiple versions of the same shopping cart, such as might occur through temporary system partitions (the “P” in CAP), a sensible feature given the purpose.

If all writes go to a single location, dealing with eventual consistency is simplified. This is the case with the Couchbase and MongoDB NoSQL databases, which only accept writes to the master node for a particular data value. Once written, that updated value propagates to other nodes which are not allowed to modify it. In these scenarios, eventual consistency only matters during reads.

 
## Summary

The CAP Theorem provides the theoretical basis that explains why we cannot guarantee both consistency and availability in a distributed database. A useful compromise is to allow for eventual consistency in favor of better scalability. Determining if your application data is a suitable candidate for eventual consistency is a business decision. The choice is between displaying stale data and scaling more efficiently.

</div>