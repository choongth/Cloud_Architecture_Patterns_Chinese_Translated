# CHAPTER 9 &emsp; Busy Signal Pattern

<div align="justify">

This pattern focuses on how an application should react when a cloud service responds to a programmatic request with a busy signal rather than success.

This pattern reflects the perspective of a client, not the service. The client is programmatically making a request of a service, but the service replies with a busy signal. The client is responsible for correct interpretation of the busy signal followed by an appropriate number of retry attempts. If the busy signals continue during retries, the client treats the service as unavailable.

Dialing a telephone occasionally results in a busy signal. The normal response is to retry, which usually results in a successful telephone call.

Similarly, invoking a service occasionally results in a failure code being returned, indicating the cloud service is not currently able to satisfy the request. The normal response is to retry which usually results in the service call succeeding.

The main reason a cloud service cannot satisfy a request is because it is too busy. Sometimes a service is “too busy” for just a few hundred milliseconds, or one or two seconds. Smart retry policies will help handle busy signals without compromising user experience or overwhelming busy services.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="60">
        </td>
        <td>
            Applications that do not handle busy signals will be unreliable.
        </td>
    </tr>
</table>


## Context

The Busy Signal Pattern is effective in dealing with the following challenges:

- Your application uses cloud platform services that are not guaranteed to respond successfully every time

This pattern applies to accessing cloud platform services of all types, such as management services, data services, and more.

More generally, this pattern can be applied to applications accessing services or resources over a network, whether in the cloud or not. In all of these cases, periodic transient failures should be expected. A familiar non-cloud example is when a web browser fails to load a website fully, but a simple refresh or retry fixes the problem.


### Cloud Significance

For reasons explained in *Multitenancy and Commodity Hardware Primer* (Chapter 8), applications using cloud services will experience periodic transient failures that result in a busy signal response. If applications do not respond appropriately to these busy signals, user experience will suffer and applications will experience errors that are difficult to diagnose or reproduce. Applications that expect and plan for busy signals can respond appropriately.

The pattern makes sense for robust applications even in on-premises environments, but historically has not been as important because such failures are far less frequent than in the cloud.


## Impact

Availability, Scalability, User Experience


## Mechanics

Use the Busy Signal Pattern to detect and handle normal transient failures that occur when your application (the *client* in this relationship) accesses a cloud service. A *transient failure* is a short-lived failure that is not the fault of the client. In fact, if the client reissues the identical request only milliseconds later, it will often succeed.
 
Transient failures are expected occurrences, not exceptional ones, similar to making a telephone call and getting a busy signal.

> <h3 align="center">Busy Signals Are Normal</h3>
>
> Consider making a phone call to a call center where your call will be answered by one of hundreds of agents standing by. Usually your call goes through without any problem, but not every time. Occasionally you get a busy signal. You don’t suspect anything is wrong, you simply hit redial on your phone and usually you get through. This is a transient failure, with an appropriate response: retry.
>
> However, many consecutive busy signals will be an indicator to stop calling for a while, perhaps until later in the day. Further, we only will retry if there is a true busy signal. If we’ve dialed the wrong number or a number that is no longer in service, we do not retry.

Although network connectivity issues might sometimes be the cause of transient failures, we will focus on transient failures at the service boundary, which is when a request reaches the cloud service, but is not immediately satisfied by the service. This pattern applies to any cloud service that can be accessed programmatically, such as relational databases, NoSQL databases, storage services, and management services.


### Transient Failures Result in Busy Signals

There are several reasons for a cloud service request to fail: the requesting account is being too aggressive, an overall activity spike across all tenants, or it could be due to a hardware failure in the cloud service. In any case, the service is proactively managing access to its resources, trying to balance the experience across all tenants, and even reconfiguring itself on the fly in reaction to spikes, workload shifts, and internal hardware failures.

Cloud services have limits; check with your cloud vendor for documentation. Examples of limits are the maximum number of service operations that can be performed per second, how much data can be transferred per second, and how much data can be transferred in a single operation.

In the first two examples, operations per second and data transferred per second, even with no individual service operation at fault it is possible that multiple operations will cumulatively exceed the limits. In contrast, the third example, amount of data transferred in a single operation, is different. If this limit is exceeded, it will not be due to a cumulative effect, but rather it is an invalid operation that should always be refused. Because an invalid operation should always fail, it is different from a transient failure and will not be considered further with this pattern.

> <h3 align="center">Handling Busy Signals Does Not Replace Addressing Scalability Challenges</h3>
>
> For cloud services, limits are not usually a problem except for very busy applications. For example, a Windows Azure Storage Queue is able to handle up to 500 operations per second for any individual queue. If your application needs to sustain more than 500 queue operations per second on an individual queue, this is no longer a transient failure, but rather a scalability challenge. Techniques for overcoming such a scalability challenge are covered under *Horizontally Scaling Compute Pattern* (Chapter 2) and *Auto-Scaling Pattern* (Chapter 4).

Limits in cloud services can be exceeded by an individual client or by multiple clients collectively. Whenever your use of a service exceeds the maximum allowed throughput, this will be detected by the service and your access would be subject to throttling. *Throttling* is a self-defense response by services to limit or slow down usage, sometimes delaying responses, other times rejecting all or some of an application’s requests. It is up to the application to retry any requests rejected by the service.

Multiple clients that do not exceed the maximum allowed throughput *individually* can still exceed throttling limits *collectively*. Even though no individual client is at fault, aggregate demand cannot be satisfied. In this case the service will also throttle one or more of the connected clients. This second situation is known as the *noisy neighbor problem* where you just happen to be using the same service instance (or virtual machine) that some other tenant is using, and that other tenant just got real busy. You might get throttled even if, technically, you do nothing wrong. The service is so busy it needs to throttle *someone*, and sometimes that someone is you.

Cloud services are dynamic; a usage spike caused by a bunch of noisy neighbors might be resolved milliseconds later. Sustained congestion caused by multiple active clients who, as individuals, are compliant with rate limits, should be handled by the sophisticated resource monitoring and management capabilities in the cloud platform. Resource monitoring should detect the issue and resolve it, perhaps by spreading some of the load to other servers.

Cloud services also experience internal failures, such as with a failed disk drive. While the service automatically repairs itself by failing over to a healthy disk drive, redirecting traffic to a healthy node, and initiating replication of the data that was on the failed disk (usually there are three copies for just this kind of situation), it may not be able to do so instantaneously. During the recovery process, the service will have diminished capacity and service calls are more likely to be rejected or time out.


### Recognizing Busy Signals

For cloud services accessed over HTTP, transient failures are indicated by the service rejecting the request and usually responded to with an appropriate HTTP status code such as: *503 Service Unavailable*. For a relational database service accessed over TCP, the database connection might be closed. Other short-lived service outages may result in different error codes, but the handling will be similar. Refer to your cloud service documentation for guidance, but it should be clear when you have encountered a transient failure and documentation may also prescribe how best to respond. Handle (and log) unexpected status codes.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="130">
        </td>
        <td>
            It is important that you clearly distinguish between busy signals and errors. For example, if code is attempting to access a resource and the response indicates it has failed because the resource does not exist or the caller does not have sufficient permissions, then retries will not help and should not be attempted.
        </td>
    </tr>
</table>


### Responding to Busy Signals

Once you have detected a busy signal, the basic reaction is to simply retry. For an HTTP service, this just means reissuing the request. For a database accessed over TCP, this may require reestablishing a database connection and then reissuing the query.

How should your application respond if the service fails again? This depends on circumstances. Some responses to consider include:

- Retry immediately (no delay).
- Retry after delay (fixed or random delay).
- Retry with increasing delays (linear or exponential backoff) with a maximum delay.
- Throw an exception in your application.

Access to a cloud service involves traversing a network that already introduces a short delay (longer when accessing over the public Internet, shorter when accessing within a data center). A *retry immediately* approach is appropriate if failures are rare and the documentation for the service you are accessing does not recommend a different approach.
 
When a service throttles requests, multiple client requests may be rejected in a short time. If all those clients retry quickly at the same time, the service may need to reject many of them again. A *retry after delay* approach can give the service a little time to clear its queue or rebalance. If the duration of the delay is random (e.g., 50 to 250ms), retries to the busy service across clients will be more distributed, improving the likelihood of success for all.

The least aggressive retry approach is *retry with increasing delays*. If the service is experiencing a temporary problem, don’t make it worse by hammering the service with retry requests, but instead get less aggressive over time. A retry happens after some delay; if further retries are needed, the delay is increased before each successive retry. The delay time can increase by a fixed amount (*linear backoff*), or the delay time can, for example, double each time (*exponential backoff*).

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="100">
        </td>
        <td>
            Cloud platform vendors routinely provide client code libraries to make it as easy as possible to use your favorite programming language to access their platform services. Avoid duplication of effort: some client libraries may already have retry logic built in.
        </td>
    </tr>
</table>

Regardless of the particular retry approach, it should limit the number of retry attempts and should cap the backoff. An aggressive retry may degrade performance and overly tax a system that may already be near its capacity limits. Logging retries is useful for analysis to identify areas where excessive retrying is happening.

After some reasonable number of delays, backoffs, and retries, if the service still does not respond, it is time to give up. This is both so the service can recover and so the application isn’t locked up. The usual way for application code to indicate that it cannot do its job (such as store some data) is to throw an exception. Other code in the application will handle that exception in an application-appropriate manner. This type of handling needs to be programmed into every cloud-native application.


### User Experience Impact

Handling transient failures sometimes impacts the user experience. The details of handling this well are specific to every application, but there are a couple of general guidelines.

The choice of a retry approach and the maximum number of retry attempts should be influenced by whether there is an interactive user waiting for some result or if this is a batch operation. For a batch operation, exponential backoff with a high retry limit may make sense, giving the service time to recover from a spike in activity, while also taking advantage of the lack of interactive users.

With an interactive user waiting, consider several retries within a small interval before informing the user that “the system is too busy right now – please try again later”. The social networking service Twitter is well-known for this behavior. Consider the *Queue-Centric Workflow Pattern* (Chapter 3) for ways to decouple time-consuming work from the user interface.

When a service does not succeed within a reasonable time or number of retries, your application should take action. Though unsatisfying, sometimes passing the information back to the user is a reasonable approach, such as “the server is busy and your update will be retried in ten seconds”. (This is similar to how Google Mail and Quora handle temporary network connectivity issues in their web user interfaces.)

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="180">
        </td>
        <td>
            Be careful with server-side code that ties up resources while retrying some operation, even when that code is retrying in an attempt to improve the user experience. If a busy web application has lots of user requests, each holding resources during retries, this could bump up against other resource constraints, reducing scalability.
        </td>
    </tr>
</table>


### Logging and Reducing Busy Signals

Logging busy signals can be helpful in understanding failure patterns. Robustly tracking and handling transient failures is extremely important in the cloud due to the innate challenges in debugging and managing distributed cloud applications.

Analysis of busy signal logs can lead to changes that will reduce future busy signals. For example, analysis may reveal that busy signals trend higher when accessing a cloud database service. Remember, the cloud provides the illusion of infinite resources, but this does not mean that each resource has infinite capacity. To access more capacity, you must provision more instances. When one database instance is not enough, it may be time to apply *Database Sharding Pattern* (Chapter 7).


### Testing

It is common to test cloud applications in non-cloud environments. Code that runs in a development or test environment, especially at lower-than-production volumes or with dedicated hardware, may not experience the transient failures seen in the cloud. Be sure to test and load test in an environment as close to production as possible.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="250">
        </td>
        <td>
            It is becoming more common for companies to test against the production environment because it is the most realistic. For example, load testing against production, though perhaps at non-peak times. Netflix goes even further by continually stressing their production (cloud) environment with errors using a home-grown tool they call Chaos Monkey. To ensure they can handle any kind of disruption, Chaos Monkey continually and randomly turns off services and reboots servers in the production environment.
        </td>
    </tr>
</table>


## Example: Building PoP on Windows Azure

The Page of Photos (PoP) application (which was described in the Preface) uses a number of Windows Azure cloud services. Two of these are Windows Azure SQL Databases for storing account profile information and Windows Azure Storage blobs for storing photos. Because these are cloud services, PoP code is written to handle transient failures.

The Windows Azure Platform includes software libraries for use with a variety of programming environments such as C#, Java, Node.js, Python, and mobile devices. These libraries all simplify accessing blobs by automatically detecting transient failures and retrying up to three times. There are a number of other predefined retry behaviors available, and it is also possible to create a custom retry policy. Most applications won’t need anything beyond the default behavior.

> <h3 align="center">Busy Signals in the Real World</h3>
>
> How frequently do retries happen in practice? Here are some real numbers gathered in running an upload utility that saturated a network connection for an extended period of time (measured in days), pushing data over the public Internet into Windows Azure Blob storage. A custom retry policy was used to implement exponential backoff while also logging each retry attempt. During the uploading of nearly a million files averaging four megabytes in size, around 1.8% of the files required at least one retry. Note that because the files were so large, each file upload involved many storage operations, so retries on storage operations were needed much less frequently than 0.1% of the time. However, because logging was at the granularity of a file upload level, exact statistics are not available. Further, many factors can impact retry behavior, such as competition for the network resources locally and network connectivity across the public Internet. Your results will vary.

Applications accessing Windows Azure SQL Databases use the same TCP protocol used for accessing SQL Server. However, a more robust approach to transient failure detection and retry logic is needed. This comes in the form of a library known as the Transient Fault Handling Application Block, also known as Topaz. Like the retry support in the libraries mentioned previously for blob access, Topaz has some predefined retry behaviors, but also a rich model for customization. The easiest way to take advantage of Topaz features when writing database access code is to replace standard database objects with the transient-failure-aware equivalents that ship with Topaz. For example, use a ```ReliableSqlConnection``` object in place of the standard .NET ```SqlConnection``` object.

Some SQL Database transient failures are due to throttling and inform you of this by returning a well-known error code. Sometimes SQL Database will drop your database connection, requiring that you reconnect.

> <h3 align="center">Expect Your Database Connection to Drop</h3>
>
> Why would SQL Database drop your connection? When you connect, behind the scenes it is not a single instance but rather a cluster of three collaborating servers. This provides multiple benefits. One benefit is resilience to disk drive failure as each byte written to SQL Database is written to all three servers within the cluster. Another benefit is that, as a multitenant service, SQL Database can distribute (and redistribute) load across servers to keep it balanced. These benefits come at a cost. If SQL Database chooses your connection to move to another of the three servers in the cluster, it needs to first disconnect you; when you reconnect, you will connect to your newly assigned cluster node, though you won’t be able to tell the difference.

While the focus here has been on using Topaz with SQL Database, it also supports Windows Azure Storage, Windows Azure Caching, and Windows Azure Service Bus, including advanced retry possibilities if you need them.

> <h3 align="center">Related Chapters</h3>
>
> - *Queue-Centric Workflow Pattern* (Chapter 3)
>
> - *Multitenancy and Commodity Hardware Primer* (Chapter 8)
>
> - *Node Failure Pattern* (Chapter 10)


## Summary

Handling transient failures is essential for building reliable cloud-native applications. Using the Busy Signal Pattern, your application can detect and handle transient failures appropriately. Further, your approach can be tuned for batch or interactive user scenarios.

It may be difficult to test your application’s response to transient failure conditions if running on non-cloud hardware or with an unrealistically light load.

</div>