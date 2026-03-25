# CHAPTER 14 &emsp; CDN Pattern

<div align="justify">

This pattern focuses on reducing network latency for commonly accessed files through globally distributed edge caching.

The goal is to speed up delivery of application content to users. Content is anything that can be stored in a file such as images, videos, and documents. The Content Delivery Network (CDN) is a service that functions as a globally distributed cache. The CDN keeps copies of application files in many places around the world. When these places are close to users, content does not need to travel as far to be delivered, so it will arrive faster, improving the user experience.

CDN nodes are strategically located around the globe, hopefully close to application users. The CDN has its own URLs that are resolved by a geographic load balancer that directs users to the nearest node, regardless of where the user is.

The flow of files is shown in Figure 14-1. When a CDN URL is requested for the first time, the CDN will retrieve the file from the main source (usually known as the origin server), and then return that to the requesting user while also caching a local copy to satisfy subsequent requests for that file. When a CDN URL is requested and the file has already been cached, the CDN returns a locally cached copy of that file to the requesting user. This is faster than if the user retrieved it from the origin server, which is (presumably) further away. When many users access the same content from a CDN node, the initial request is slower (so the first user to request a file has to wait longer), but subsequent requests are much faster.

A CDN is only helpful for files that will be accessed multiple times. Files that are intended to be rarely accessed or that are for only a single user are usually not good candidates for a CDN.

Each CDN node operates independently of the others.

<div align="center">

![Local Image](../images/chapter_14/image1.png)

*Figure 14-1. Users get content directly from the nearest CDN node. The CDN node fetches content from the source as needed.*

</div>


## Context

The CDN Pattern is effective in dealing with the following challenges:

- Application data is accessed from geographic locations that may not be close to the data center from which it originates
- Multiple clients access the same application data objects (such as HTML, JavaScript, image, video, or other files)
- Application includes large downloads, streaming video, or other heavyweight content delivery

A CDN can effectively reduce the load on other types of nodes that might be serving up content.

 
### Cloud Significance

Cloud platform CDN services are easy to enable and are integrated into the cloud vendor’s web-hosted management tool, appear on the same bill, and are supported by the same organization as other services offered within the cloud platform. Some other services, such as blob storage, can be easily CDN-enabled with a few mouse clicks. For these reasons, cloud CDN services can be more convenient than working directly with an independent CDN service provider.


## Impact

Scalability, User Experience


## Mechanics

Generally speaking, transmitting data over the Internet is faster when the source data and recipient are closer together. One way to bring source data and recipients closer together is by caching copies of the source data in locations that are closer to the recipients. When a user needs the data, retrieving it from the closest location will be faster than retrieving it from the origin, which might potentially be much further away. This practice is commonly known as *edge caching* and is the role of the CDN. Specifically, we will focus on accessing files over HTTP.

To enable the CDN to cache our files, we need to make one change: instead of using our normal domain name in our file URLs, we use one designated for us by the CDN provider. All file resources that we want to be cached by the CDN are changed to the new scheme.

Whenever a user attempts to access a file being managed by the CDN, the request goes to the CDN service to be fulfilled. However, because one of the benefits of the CDN service is that it has cache nodes in many geographical locations, somehow the request needs to be directed to the location nearest to the requesting user. This is usually handled through the *anycast* routing protocol that helps identify the closest CDN node, and the request is smartly routed accordingly.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="80">
        </td>
        <td>
            Even though all users see the same URL for the file, different users will be routed to different CDN nodes. This is the whole point of the CDN: route requests to the nearest CDN node to improve responsiveness.
        </td>
    </tr>
</table>

A CDN is a specialized cache, and as such, is not necessarily already populated with every desired file. This would be the case if the file had never been requested before through this CDN node, or the file had been requested but subsequently expired from the CDN cache. Any resource stored in a CDN has a defined expiration time that your application sets, so this is something you will want to think about. Some resources, such as a company logo, may be stable for months or years, while other resources such as a user profile photo may have a shorter shelf life of perhaps one hour (users like to update them frequently). The expiration is expressed through the HTTP *Cache-Control* header. Because the Cache-Control caching directive is a long-established Internet standard, web browsers understand and handle this directive with ease. Beyond individual browsers, sometimes proxy servers or other intermediaries will cache, extending the caching benefits beyond a single user and beyond the CDN. Once the resource has expired, the CDN will remove it from its cache. (There are other possible behaviors with some CDN services, such as having the CDN check with the origin server to ascertain whether expired content has indeed changed, and still using the cached object if it has not.)

If a requested file is not available at the CDN node, the CDN service effectively acts as a middleman, and behind the scenes retrieves the file from the origin server, caches (stores) it in the CDN node, and then responds to the original request (which it can now do successfully).

> <h3 align="center">Limitations of CDN</h3>
>
> A CDN is not effective for use on resources that are infrequently accessed. A CDN is only efficient for use on resources that are usually accessed at least twice before expiring from the CDN cache.
>
> A CDN is not effective for use on resources that change constantly, such as those changing with each request.
>
> A CDN may be a poor choice for content that is not intended for public viewing.

A CDN really shines when used on frequently accessed files that seldom change.


### Caches Can Be Inconsistent

The CDN stores copies of resources with a specified expiration date; thus a cached image file might be declared to be valid for one hour, one month, or whatever is appropriate for that resource. Any resource cached in a CDN is potentially stale because there is always a delay between updates to the source copy and propagation to the copy that is cached at the CDN. This is not necessarily a problem, but is a factor to consider carefully when deciding what to cache and for how long.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            Caching resources in a CDN is an application of <i>eventual consistency</i>: immediate consistency is traded off for performance and scalability benefits.
        </td>
    </tr>
</table>


## Example: Building PoP on Windows Azure

The Page of Photos (PoP) application (which was described in the Preface) stores all photos in Windows Azure Blob Storage. To improve the download experience for PoP users, all photo downloads are CDN-enabled.

In Windows Azure Blob Storage, individual blobs are stored in a *blob container* which, among other benefits, establishes a default security context for the blobs within it. Only blobs stored in public containers—where public means anyone knowing the URL to a file within the container can view that file—are eligible for caching in the CDN.

> <h3 align="center">Configuring the CDN</h3>
>
> Enabling the CDN for blobs is a trivial configuration change at the storage account level. In order for the CDN to sit between users and blob storage, a new domain name is created. This new domain name is system generated. That is, rather than using our own domain name (such as example.com), we use one assigned to us by the CDN service (such as jaromijo1213.vo.msecnd.net). A URL that used to be *http://example.com/maura.png* before the CDN becomes *http://jaromijo.vo.msecnd.net/maura.png* with the CDN. You can optionally configure a vanity domain name of your own to make the CDN addresses appear less chaotic; this vanity domain name is also known as *canonical name* (or CNAME) in DNS terms. PoP takes advantage of this vanity feature, creating *http:// cdn.pageofphotos.com* as the CDN domain so that photo URLs will be more consistent with the URL for the main application.

As of this writing, there are 8 Windows Azure data centers worldwide, but 24 Windows Azure CDN node locations. Since there are so many additional geographic locations brought into the mix, use of the CDN network greatly increases global coverage for lower latency content distribution. A resource needs to have been requested at least once in order to be loaded into the CDN cache. The first request for that resource will therefore have a poorer user experience than subsequent requestors. This scenario plays out at each CDN node, as each node needs to fill its own local cache. The scenario also repeats any time a resource is requested, but has expired from the CDN cache.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="40">
        </td>
        <td>
            Each of the 24 CDN nodes operates independently, so each populates independently.
        </td>
    </tr>
</table>

PoP photos do not change frequently, so when the photos are saved in blob storage, a Cache-Control header is set as a property on the individual blob, with the duration set to one month. If we ever needed to update a photo sooner, we would need to change the filename.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="350">
        </td>
        <td>
            The Windows Azure CDN does not currently have built-in support for pre-fetching an object to warm up a particular CDN node. Further, once an object is cached by the CDN, you can’t easily change your mind about it since there is no way to evict a particular object from the CDN before its specified cache expiration. Thus, tricks like file renaming or appending a “cache-buster” like a query string are used. For example, we could append a query string so that the cached image <i>kd1hn.png</i> becomes <i>kd1hn.png?v=1</i> to cause it to reload without finding the one in the current CDN. This requires a change to the reference used to access the image (such as the HTML page referencing it). These are cache tricks that happen to work with the CDN (since it is also a cache honoring HTTP headers); nothing is specific to the Windows Azure Cache.
        </td>
    </tr>
</table>


### Cost Considerations

The Windows Azure CDN access charges are the same as directly from blob storage, with no additional at-rest storage charge. Other than the cost of the one additional request needed to populate the CDN node, the cost will be identical to direct-from-blob access. Note that this “one additional request” happens for (a) each resource, (b) each time it is loaded from blob storage (the first time as well as the next time after cache expiration), and (c) in each CDN node from which the resource is accessed.


### Security Considerations

The Windows Azure CDN only supports caching of files that are already freely visible to the public, so they do not decrease security or increase the attack surface.

The Windows Azure CDN supports access using HTTP or HTTPS. This is primarily a user experience benefit (because the files are already freely visible to the public) and comes into play when a user loads a page using HTTPS. Modern browsers typically display a warning if an HTTPS page references an image as HTTP, complaining about mixed security models. Such images can instead be accessed as HTTPS to improve the user experience.


### Additional Capabilities

Though not explored in this chapter, there are other Windows Azure services related to CDN. For example, the CDN can serve files directly from a Web Role. Also, Windows Azure Media Services provide video streaming to the Windows Azure CDN. There are also other integrated services that help applications prepare media for download by many types of devices, such as transcoding services that make content equally accessible across mobile phone brands and desktop operating systems.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="50">
        </td>
        <td> 
            As of this writing, the Windows Azure Media Services are currently in preview and not yet supported for production use.
        </td>
    </tr>
</table>

> <h3 align="center">Related Chapters</h3>
>
> - *Eventual Consistency Primer* (Chapter 5)
>
> - *Network Latency Primer* (Chapter 11)
>
> - *Colocate Pattern* (Chapter 12)
>
> - *Valet Key Pattern* (Chapter 13)
>
> - *Multisite Deployment Pattern* (Chapter 15)


## Summary

Adding CDN support to a cloud application is a great example of a low-friction adoption of a cloud service. Enabling a CDN can be accomplished either programmatically or through a one-time manual configuration via the cloud vendor’s web-hosted management tool. This is substantially easier to get started with than traditional CDNs due to the degree of convenience and integration.

Once enabled, this is a great technique for reducing the load on web servers, distributing load across many servers (there are many more CDN locations than data centers), while decreasing network latency. All this helps to both improve scalability and improve user experience.

</div>