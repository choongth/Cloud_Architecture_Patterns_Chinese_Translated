# CHAPTER 10 &emsp; Node Failure Pattern

<div align="justify">

This pattern focuses on how an application should respond when the compute node on which it is running shuts down or fails.

This pattern reflects the perspective of application code running on a node (virtual machine) that is shut down or suddenly fails due to a software or hardware issue. The application has three responsibilities: prepare the application to minimize issues when nodes fail, handle node shutdowns gracefully, and recover once a node has failed.

Some common reasons for shutdown are unresponsive application due to application failure, routine maintenance activities managed by the cloud vendor, and auto-scaling activities initiated by the application. Failures might be caused by hardware failure or an unhandled exception in your application code.

While there are many reasons for a node shutdown or failure, we can still treat them uniformly. Handling the various forms of failure is sufficient; all shutdown scenarios will also be handled. The pattern name derives from the more encompassing node failures.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="70">
        </td>
        <td>
            Applications that do not handle node shutdowns and failures will be unreliable.
        </td>
    </tr>
</table>


## Context

The Node Interruption Pattern is effective in dealing with the following challenges:

- Your application is using the Queue-Centric Workflow Pattern and requires *at-least-once* processing for messages sent across tiers
- Your application is using the Auto-Scaling Pattern and requires graceful shutdown of compute nodes that are being released
- Your application requires graceful handling of sudden hardware failures
- Your application requires graceful handling of unexpected application software failures
- Your application requires graceful handling of reboots initiated by the cloud platform
- Your application requires sufficient resiliency to handle the unplanned loss of a compute node without downtime

All of these challenges result in interruptions that need to be addressed and many of them have overlapping solutions.


### Cloud Significance

Cloud applications will experience failures that result in node shutdowns. Cloud-native applications expect these and respond appropriately when notified of shutdowns by the cloud platform.


## Impact

Availability, User Experience


## Mechanics

The goal of this pattern is to allow individual nodes to fail while the application as a whole remains available. There are several categories of failure scenarios to consider, but all of them share characteristics for preparing for failure, handling node shutdown (when possible), and then recovering.


### Failure Scenarios

When your application code runs in a virtual machine (especially on commodity hardware and on a public cloud platform), there are a number of scenarios in which a shutdown or failure could occur. There are other potential scenarios, but from the point of view of the application, the scenarios always look like one of those listed in Table 10-1.

*Table 10-1. Node Failure Scenarios*

| Scenario | Initiated By | Advanced Warning | Impact |
|-|-|-|-|
| Sudden failure + restart | Sudden hardware failure | Non | Local data is lost |
| Node shutdown + restart | Cloud platofrm (vacale failing hardware, software update) | Yes | Local data may be available |
| Node shutdown + restart | Application (application update, application bug, managed reboot) | Yes | Local data is available |
| Node shutdown + termination | Application (node released such as via auto-scale) | Yes | Local data is lost |

<br>

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="100">
        </td>
        <td>
            Table 10-1 is correct for the Windows Azure and Amazon Web Services platforms. It is not correct for every platform, though the pattern is still generally applicable.
        </td>
    </tr>
</table>

The scenarios have a number of triggers listed; some provide advanced warning, some don’t. Advanced warning can come in multiple forms, but amounts to a proactive signal, sent by the cloud platform that allows the node to gracefully shut down. In some cases, local data written to the local virtual machine disk is available after the interruption, sometimes it is lost. How do we deal with this?

Scenarios can be initiated by hardware or software failures, by the cloud platform, or by your application.


### Treat All Interruptions as Node Failures

None of the scenarios are predictable from the point of view of the individual node. Further, the application code does not know which scenario it is in, but it can handle all of the scenarios gracefully if it treats them all as failures.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            All application compute nodes should be ready to fail at any time.
        </td>
    </tr>
</table>

Given that failure can happen at any time, your application should not use the local disk drive of the compute node as the system of record for any business data. As mentioned in Table 10-1, it can be lost. Remember that an application using stateless nodes is not the same as a stateless application. Application state is persistent in reliable storage, not on the local disk of an individual node.

By treating all shutdown and failure scenarios the same, there is a clear path for handling them: maintain capacity, handle node shutdowns, shield users when possible, and resume work-in-progress after the fact.

 
### Maintain Sufficient Capacity for Failure with N+1 Rule

An application prepares first by assuming node failures will happen, then taking proactive measures to ensure failures don’t result in application downtime. The primary proactive measure is to ensure sufficient capacity.

How many node instances are needed for high availability? Apply the *N+1 rule*: If N nodes are needed to support user demand, deploy N+1 nodes. One node can fail or be interrupted without impact to the application. It is especially important to avoid any single points of failure for nodes directly supporting users. If a single node is required, deploy two.

The *N+1 rule* should be considered and applied independently for each type of node. For example, some types of node can experience downtime without anyone noticing or caring. These are good candidates for not applying the *N+1 rule*.

A buffer more extensive than a single node may be needed, though rarely, due to an unusual failure. A severe weather-related incident could render an entire data center unavailable, for example. A top-of-rack switch failure is discussed in the Example and cross-data center failover is touched on in *Multisite Deployment Pattern* (Chapter 15).

There is a time lag between the failure occurrence and the recognition of that failure by the cloud platform monitoring system. Your service will run at diminished capacity until a new node has been fully deployed. For nodes configured to accept traffic through the cloud platform load balancer, traffic will continue to be routed there; once the failed node is recognized it will be promptly removed from the load balancer, but there will still be more time until the replacement node is booted up and added to the load balancer. See *Auto-Scaling Pattern* (Chapter 4) for a discussion of borrowing already-deployed resources from less critical functions to service more important ones during periods of diminished capacity.


### Handling Node Shutdown

We can responsibly handle node shutdown, but there is no equivalent handling for sudden node failure because the node just stops in an instant. Not all hardware failures result in node failure: the cloud platforms are adept at detecting signals that hardware failure is imminent and can preemptively initiate a controlled shutdown to move tenants to different hardware.

Some node failures are also preventable: is your application code handling exceptions? At a minimum, do this for logging purposes.

Regardless of the reason the node is shutting down, the goals are to allow in-progress work to complete, gather operational data from that node before shutdown completes, and do so without negatively impacting the user experience.
 
Cloud platforms provide signals to let running applications know that a node is either about to be, or is in the process of being, shut down. For example, one of the signals provided by Amazon Web Services is an advanced alert, and one of the signals provided by Windows Azure is to raise an event within the node indicating that shutdown has begun.


#### **Node shutdown with minimal impact to user experience**

Cloud platforms managing both load balancing and service shutdown stop sending web traffic to nodes they are in the process of shutting down. This will prevent many types of user experience issues, as new web requests will be routed to other instances. Only those pending web requests that will not be complete by the time the service shuts down are problematic. Some applications will not have any problem as all requests are responded to nearly instantly.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="140">
        </td>
        <td>
            If your application is using sticky sessions, new requests will not always be routed to other instances. This topic was covered in Horizontally <i>Scaling Compute Pattern</i> (Chapter 2). However, cloud-native applications avoid sticky sessions.
        </td>
    </tr>
</table>

It is possible that a node will begin shutting down while some work visible to a user is in progress. The longer it takes to satisfy a web request, the more of a problem this can be. The goal is to avoid having users see server errors just because they were accessing a web page when a web node shuts down while processing their request. The application should wait for these requests to be satisfied before shutting down. How to best handle this is application-, operating system-, and cloud platform-specific; an example for a Windows Cloud Service running on Windows Azure is provided in the *Example*.

Failures that result in sudden termination can cause user experience problems if they happen in nodes serving user requests. One tactic for minimizing this is to design the user interface code to retry when there are failures, as described in *Busy Signal Pattern* (Chapter 9). Note that the busy signal here is not coming from a cloud platform service, but the application’s own internal service.


#### **Node shutdown without losing partially completed work**

The node should immediately stop pulling new items from the queue once shutdown is underway and let current work finish if possible.

*Queue-Centric Workflow Pattern* (Chapter 3) describes processing services that can sometimes be lengthy. If processing is too lengthy to be completed before the node is terminated, that node has the option of saving any in-progress work outside of the local node (such as to cloud storage) in a form that can be resumed.

 
#### **Node shutdown without losing operational data**

Web server logs and custom application logs are typically written to local disk on individual nodes. This is a reasonable approach, as it makes efficient use of available local resources to capture this log data.

As described in *Horizontally Scaling Compute Pattern* (Chapter 2), part of managing many nodes is the challenge of consolidating operational data. Usually this data is collected periodically. If the collection process is not aware that a node is in the process of shutdown, that data may not be collected in time, and may be lost. This is your opportunity to trigger collection of that operational data.


### Recovering From Node Failure

There are aspects to recovering from node failure: maintaining a positive user experience during the failure process, and resuming any work-in-progress that was interrupted.

#### **Shielding interactive users from failures**

Some node instances support interactive users directly. These nodes might be serving up web pages directly or providing web services that power mobile apps or web applications. A positive user experience would shield users from the occasional failure. This happens in a few ways.

If a node fails on the server, the cloud platform will detect this and stop sending traffic to that node. Once detected, service calls and web page requests will be routed to healthy node instances. You don’t need to do anything to make this happen (other than follow the *N+1 rule*), and this is sometimes good enough.

However, to be completely robust, you should consider the small window of exposure to failure that can occur while in the middle of processing a request for a web page or while executing a service call. For example, the disk drive on the node may suddenly fail. The best response to this edge case is to place retry logic on the client, basically applying *Busy Signal Pattern* (Chapter 9). This is straightforward for a native mobile app or a single-page web application managing updates through web service calls (Google Mail is an example of a single-page web application that handles this scenario nicely). For a traditional web application that redisplays the entire page each time, it is possible that users could see an error surface through their web browser. This is out of your control. It will be up to the users to retry the operation.

> <h3 align="center">Don’t Be Overprotective</h3>
>
> The strategy of shielding users from errors only makes sense up to a point. Eventually you will need to communicate errors to the user, though “service is not available, please retry in a couple of minutes” is sometimes the best we can do. Further, make sure any retry policies are appropriate for interactive users; in other words, a policy that retries 10 times with a 30-second sleep in between may be fine for a nightly batch process, but totally inappropriate when supporting an interactive user.


#### **Resuming work-in-progress on backend systems**

In addition to user experience impact, sudden node failure can interrupt processing in the service tier. This can be robustly handled by building processes to be idempotent so they can safely execute multiple times on the same inputs. Successful recovery depends on nodes being stateless and important data being stored in reliable storage rather than on a local disk drive of the node (assuming that disk is not backed by reliable storage). A common technique for cloud-native applications is detailed in *Queue-Centric Workflow Pattern* (Chapter 3), which covers restarting interrupted processes as well as saving in-progress work to make recovery faster.


## Example: Building PoP on Windows Azure

Page of Photos (PoP) application (which was described in the Preface) is architected to provide a consistently reliable user experience and to not lose data. In order to maintain this through occasional failures and interruptions, PoP prepares for failure, handles role instance (node) shutdowns gracefully, and recovers from failures as appropriate.


### Preparing PoP for Failure

PoP is constantly increasing and decreasing capacity to take advantage of the cost savings; we only want to pay for the capacity needed to run well, and no more. PoP also does not wish to sustain downtime or sacrifice the user experience due to diminished capacity in the case of an occasional failure or interruption.


#### **N+1 rule**
The *N+1 rule* is honored for roles that directly serve users, because user experience is very important. However, because no users are directly impacted by occasional interruptions, PoP management made the business decision to not apply the *N+1 rule* for worker roles which make up the service tier.

These decisions are accounted for in PoP auto-scaling rules.


#### **Windows Azure fault domains**

Windows Azure (through a service known as the Windows Azure Fabric Controller) determines where in the data center to deploy each role instance of your application within specific constraints. The most important of these constraints for failure scenarios is a fault domain. A *fault domain* is a potential single point of failure (SPoF) within a data center, and any roles with a minimum of two instances are guaranteed to be distributed across at least two fault domains.

The discerning reader will realize that this means up to half of your application’s web and worker role instances can go down at once (assuming two fault domains). While it is possible, it is unlikely. The good news is that if it does happen, the Fabric Controller begins immediate remediation, although your application will run with diminished capacity during recovery.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="150">
        </td>
        <td>
            <i>Auto-Scaling Pattern</i> (Chapter 4) introduced the idea of internally throttling some application features during periods of diminished or insufficient resources. This is another scenario where that tactic may be useful. Note that you may not want to auto-scale in response to such a failure, but rather allow the Fabric Controller to recover for you.
        </td>
    </tr>
</table>

For the most part, a hardware failure results in the outage of a single role instance. The *N+1 rule* accounts for this possibility.

PoP makes the business decision that N+1 is sufficient, as described previously.

> <h3 align="center">When N + 1 Is Not Enough</h3>
>
> If a failure impacts more than a single hardware node (for example, a failure at the rack level), N+1 may not be enough, depending on your business requirements. If this level of potential (albeit rare) downtime is too much, consider even more capacity, depending on how many fault domains are in effect. Currently, all Windows Azure cloud services run with two fault domains and (as of this writing) the number cannot be changed. Extra capacity for critical times can be scheduled as a proactive auto-scaling task. Also consider *Multisite Deployment Pattern* (Chapter 15).

Note that fault domains only apply with unexpected hardware failures. For operating system updates, hypervisor updates, or application-initiated upgrades, downtime is distributed across upgrade domains.

 
#### **Upgrade domains**

Conceptually similar to fault domains, Windows Azure also has *upgrade domains* that provide logical partitioning of role instances into groups (by default, five) which are considered to be independently updatable. This goes hand-in-hand with the *in-place upgrade* feature, which upgrades your application in increments, one upgrade domain at a time.

If your application has N upgrade domains, the Fabric Controller will manage the updates such that 1/N role instances will be impacted at a time. After these updates complete, updating the next 1/N role instances can begin. This continues until the application is completely updated. Progressing from one upgrade domain to the next can be automated or manual.

Upgrade domains are useful to the Fabric Controller since it uses them during operating system upgrades (if the customer has opted for automatic updates), hypervisor updates, emergency moves of role instances from one machine or rack to another, and so forth. Your application uses upgrade domains if it updates itself using the *in-place upgrade* process where the application is upgraded one upgrade domain at a time. You may choose to manually approve the progression from one upgrade domain to the next, or have the Fabric Controller proceed automatically.

> <h3 align="center">Multiple Concurrent Versions</h3>
>
> Cloud-native applications commonly have a single deployment with mixed releases (and features) across the role instances. This staged upgrade can be managed using upgrade domains with an in-place upgrade. Another way to look at this: an application upgrade does not happen at a particular time, rather it spans time. During that time span the application is running two versions. Two customers visiting the application at the same time may be routed to different versions. If you wish to avoid deploying multiple concur¬rent versions, look into Windows Azure's VIP Swap feature.

What is the right number of upgrade domains for my application? There is a tradeoff: more upgrade domains take longer to roll out, while fewer upgrade domains increase the level of diminished capacity. For PoP, the default of five upgrade domains works well.


### Handling PoP Role Instance Shutdown

Graceful role shutdown avoids loss of operational data and degradation of the user experience.

When we instruct Windows Azure to release a role instance, there is a defined process for shutdown; it does not shut off immediately. In the first step, Windows Azure chooses the role instance that will be terminated

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            You don’t get to decide which role instances will be terminated.
        </td>
    </tr>
</table>

First, let’s consider terminating a web role instance.


#### **Web role instance shutdown**

Once a specific web role instance is selected for termination, Windows Azure (specifically the Fabric Controller) removes that instance from the load balancer so that no new requests will be routed to it. The Fabric Controller then provides a series of signals to the running role that a shutdown is coming. The last of these signals is to invoke the role’s ```OnStop``` method, which is allowed five minutes for a graceful shutdown. If the instance finishes the cleanup within five minutes, it can return from ```OnStop``` at that point; if your instance does not return from ```OnStop``` within five minutes, it will be forcibly terminated. Only after ```OnStop``` has completed does Azure consider your instances to be released for billing purposes, providing an incentive for you to make ```OnStop``` exit as efficiently as possible. However, there are a couple of steps you could take to make the shutdown more graceful.

Although the first step in the shutdown process is to remove the web role from the load balancer, it may have existing web requests being handled. Depending on how long it takes to satisfy individual web requests, you may wish to take proactive steps to allow these existing web requests to be satisfied before allowing the shutdown to complete. One example of a slower-than-usual operation is a user uploading a file, though even routine operations may need to be considered if they don’t complete quickly enough. A simple technique to ensure your Web Role is gracefully shutting down is to monitor an IIS performance counter such as *Web Service\Current Connections* in a loop inside your ```OnStop``` method, continuing to termination only when it has drained to zero.

Application diagnostic information from role instances is logged to the current virtual machine and collected by the Diagnostics Manager on a schedule of your choosing, such as every ten minutes. Regardless of your schedule, triggering an unscheduled collection within your ```OnStop``` method will allow you to wrap up safely and exit as soon as you are ready. An on-demand collection can be initiated using Service Management features.

 
#### Worker role instance shutdown

Gracefully terminating a worker role instance is similar to terminating a web role instance, although there are some differences. Because Azure does not load balance PoP worker role instances, there are no active web requests to drain. However, triggering an on-demand collection of application diagnostic information does make sense, and you would usually want to do this.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="250">
        </td>
        <td>
            Although PoP worker role instances do not have public-facing end-points, Windows Azure does support it, so applications may have active connections when instance shutdown begins. A worker role can define public-facing endpoints that will be load balanced. This feature is useful in running your own web server such as Apache, Mongoose, or Nginx. It may also be useful for exposing self-hosted WCF endpoints. Further, public endpoints are not limited to HTTP, as TCP and UDP are also supported.
        </td>
    </tr>
</table>

A worker role instance will want to stop pulling new items from the queue for processing as soon as possible once shutdown is underway. As with the web role, instance shutdown signals such as ```OnStop``` can be useful; a worker role could set a global flag to indicate that the role instance is shutting down. The code that pulls new items from the queue could check that flag and not pull in new items if the instance is shutting down. Worker roles can use ```OnStop``` as a signal, or hook an earlier signal such as the ```OnStopping``` event.

If the worker role instance is processing a message pulled from a Windows Azure Storage queue and it does not complete before the instance is terminated, that message will time out and be returned to the queue so it can be pulled again. (Refer to *Queue-Centric Workflow Pattern* (Chapter 3) for details.) If there are distinct steps in processing, the underlying message can be updated on the queue to indicate progress. For example, PoP processes newly uploaded photos in three steps: extract geolocation information from the photo, create two sizes of thumbnail, and update all SQL Database references. If two of these steps were completed, but not the third, it is indicated by a ```LastCompletedStep``` field in the message object that was pulled from the queue, and that message object can be used to update the copy on the queue. When this message is pulled again from the queue (by a subsequent role instance), processing logic will know to skip the first two steps because LastCompletedStep field is set to two. The first time a message is pulled, the value is set to zero.


#### **Use controlled reboots**

Sometimes you may wish to reboot a role instance. The controlled shutdown that allows draining active work is supported if reboots are triggered using the Reboot Role Instance operation in the Windows Azure Service Management service.
 
If other means of initiating a reboot are used, such as directly on the node using Windows commands, Windows Azure is not able to manage the process to allow for a graceful shutdown.


### Recovering PoP From Failure

*Queue-Centric Workflow Pattern* (Chapter 3) details recovering from a long-running process.

Otherwise, there is usually little to do in recovering a stateless role instance, whether a web role or worker role.

Some user experience glitches are addressed in PoP because client-side JavaScript code that fetches data (though AJAX calls to REST services) implements *Busy Signal Pattern* (Chapter 9). This code will retry if a service does not respond, and the retry will eventually be load balanced to a different role instance that is able to satisfy the request.

> <h3 align="center">Related Chapters</h3>
>
> - *Queue-Centric Workflow Pattern* (Chapter 3)
>
> - *MapReduce Pattern* (Chapter 6)
>
> - *Multitenancy and Commodity Hardware Primer* (Chapter 8)
>
> - *Busy Signal Pattern* (Chapter 9)
>
> - *Multisite Deployment Pattern* (Chapter 15)


## Summary

Failure in the cloud is commonplace, though downtime is rare for cloud-native applications. Using the Node Failure Pattern helps your application prepare for, gracefully handle, and recover from occasional interruptions and failures of compute nodes on which it is running. Many scenarios are handled with the same implementation.

Cloud applications that do not account for node failure scenarios will be unreliable: user experience will suffer and data can be lost.

</div>