# CHAPTER 11 &emsp; Network Latency Primer

<div align="justify">

This basic primer explains network latency and why delays due to network latency matter.

The time it takes to transmit data across a network is known as *network latency*. Network latency slows down our application.

While individual networking devices like routers, switches, wireless access points, and network cards all introduce latencies of their own, this primer blends them all together into a bigger picture view, the total delay experienced by data having to travel over the network.

As cloud application developers, we can decrease the impact of network latency through caching, compression, moving nodes closer together, and shortening the distance between users and our application.


## Network Latency Challenges

Highly scalable and high performing (even infinitely fast!) servers do not guarantee that our application will perform well. This is due to the main performance challenge that lies outside of raw computational power: movement of data. Transmitting data across a network does not happen instantly and the resultant delay is known as *network latency*.

Network latency is a function of distance and bandwidth: how far the data needs to travel and how fast it moves. The challenge is that any time compute nodes, data sources, and end users are not all using a single computer, network latency comes into play; the more distribution, the greater the impact. Network quality plays an important role, although it is one you might not be able to control; quality may vary as users connect through networks from disparate locations.

Application performance and scalability will suffer if it takes too long for data to get to the client computer. The effects of network latency can also vary based on the user’s geographical location: to some users of the system, it may seem blazingly fast, while to other users it may seem as slow as cold molasses (which means *really slow*).

Admittedly, understanding the actual distance traveled by the data and the effective bandwidth can be challenging. The actual path traveled is not the same as the ideal great circle path you might imagine from a map. The path from router to router is jagged, and indeed may even be impacted by the capability of individual routers, and individual legs of the route may have different bandwidths. And it can vary over time.

A simple way to estimate effective bandwidth (at some point in time) is to use ping. *Ping* is a simple but useful program that calculates the time it takes to travel from one point on the Internet to another point, by actually sending network packets, counting the nodes along the way, and showing how long it takes the packets to get to the destination and back. Some measured ping times are shown in Table 11-1. These pings originated from Boston which is in the eastern USA. The fastest networks today use fiber optic cable, which supports data transmission at around 66% of the speed of light (186,000 miles/sec × 66% = 122,760 miles/sec). As you can see from Table 11-1, shorter distances are dominated by factors other than pure distance traveled, and none are close to the theoretical maximum speed. HTTP and TCP traffic will be slower than pings. Network latency impact can add up quickly, especially as web pages get heavier with more objects to download. In *An Analysis of Application Performance Data and Trends* (Jan 2012), Compuware reports that the average (yes, average) page download size for a non-mobile news site is more than a megabyte and 790 KB for a non-mobile travel site.

<br>

*Table 11-1. Ping Times from Boston - distance matters*

| Domain | Location | Elapsed Time each way (ms) | Approximate Distance (miles) | Speed (miles/sec) |
|-|-|-|-|-|
| www.bu.edu | Boston, USA | 17 | 10 | 1,000 |
| www.gsc.edu | Atlanta, USA | 75 | 900 | 12,000 |
| www.usc.edu | Los Angeles, USA | 51 | 2600 | 52,000 |
| www.cam.ac.uk | Cambridge, UK | 50 | 3300 | 67,000 |
| www.msu.ru | Moscow, RU | 81 | 4500 | 56,000 |
| www.u-tokyo.ac.jp | Tokyo, JP | 107 | 6700 | 63,000 |
| www.auckland.ac.nz | Auckland, NZ | 115 | 9000 | 79,000 |

> <h3 align="center">Doing the Math</h3>
>
> How much data transfer can I get for my bandwidth? Let’s do some math. Consider a T1 connection that provides 1.544 Mb/sec of bandwidth. Translating from kilobits (Kb) to kilobytes (KB) we end up with about 193 KB/sec (1544/8=193 kilobytes per second, ig-noring errors, retries, speed degradation, and assuming no other uses of the connection). Uploading a full CD-ROM (~660 MB) would take nearly an hour. Uploading a full DVD (~4.7 GB) would take around 7 hours. These calculations assume ideal network conditions, which of course you will never have.


## Reducing Perceived Network Latency

The *perceived network latency*, the network latency as experienced by a user, can be reduced through techniques such as:

- Data compression
- Background processing, where screen updates don’t happen until the data arrives (though not actually faster, it may improve a user’s subjective experience, as with single page web applications)
- Predictive fetching, where data is loaded in anticipation of need (as with map tiles in a mapping app, although it is possible that not all the map tiles will be referenced)

Eventual consistency is another tool we can use, if we can serve users with slightly stale data. These are reasonable approaches for reducing the *impact* of network latency, but do not *change* the network latency. They are essentially the same for cloud-native applications as they are for non-cloud applications.


## Reducing Network Latency

We can reduce network latency by:

- Moving application closer to users
- Moving application data closer to users
- Ensuring nodes within our application are close together

These reduction techniques are the topics of the Colocate, Valet Key, CDN, and Multisite Deployment patterns.


## Summary

A comprehensive strategy for dealing with network latency will use multiple strategies. One set of strategies focuses on reducing the perceived network latency. Another set of strategies focuses on actually reducing network latency by shortening the distance between users and the instances of our application.

</div>