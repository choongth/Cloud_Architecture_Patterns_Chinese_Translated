# CHAPTER 6 &emsp; MapReduce Pattern

<div align="justify">

This pattern focuses on applying the MapReduce data processing pattern.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="120">
        </td>
        <td>
            MapReduce in this chapter is explicitly tied to the use of Hadoop since that helps pin down its capabilities and avoid confusion with other variants. The term MapReduce is used except when directly referencing the Hadoop project (which is introduced below).
        </td>
    </tr>
</table>

MapReduce is a data processing approach that presents a simple programming model for processing highly parallelizable data sets. It is implemented as a cluster, with many nodes working in parallel on different parts of the data. There is large overhead in starting a MapReduce job, but once begun, the job can be completed rapidly (relative to conventional approaches).

MapReduce requires writing two functions: a mapper and a reducer. These functions accept data as input and then return transformed data as output. The functions are called repeatedly, with subsets of the data, with the output of the mapper being aggregated and then sent to the reducer. These two phases sift through large volumes of data a little bit at a time.

MapReduce is designed for batch processing of data sets. The limiting factor is the size of the cluster. The same map and reduce functions can be written to work on very small data sets, and will not need to change as the data set grows from kilobytes to megabytes to gigabytes to petabytes.

Some examples of data that MapReduce can easily be programmed to process include text documents (such as all the documents in Wikipedia), web server logs, and users’ social graphs (so that new connection recommendations can be discovered).

Data is divvied up across all nodes in the cluster for efficient processing.

 
## Context

The MapReduce Pattern is effective in dealing with the following challenges:

- Application processes large volumes of relational data stored in the cloud
- Application processes large volumes of semi-structured or irregular data stored in the cloud
- Application data analysis requirements change frequently or are *ad hoc*
- Application requires reports that traditional database reporting tools cannot efficiently create because the input data is too large or is not in a compatible structure

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            Other cloud platforms may support Hadoop as an on-demand service. This pattern assumes a Hadoop-based service.
        </td>
    </tr>
</table>

Hadoop implements MapReduce as a batch-processing system. It is optimized for the flexible and efficient processing of massive amounts of data, not for response time.

The output from MapReduce is flexible, but is commonly used for data mining, for reporting, or for shaping data to be used by more traditional reporting tools.

> <h3 align="center">MapReduce as a Service</h3>
>
> Amazon Web Services, Google, and Windows Azure all offer MapReduce as an on-demand service.
>
> The Amazon Web Services and Windows Azure services are based on the open source Apache Hadoop project (*http://hadoop.apache.org*).
>
> The Google service also uses the MapReduce pattern, but not with Hadoop. In fact, Google invented MapReduce to solve problems it faced in processing the vast data it was collecting, such as page-to-page hyperlinks that were gathered by crawling public websites. In particular, MapReduce was used to analyze these links to apply Google’s famous PageRank algorithm to decide which websites are most worthy of showing up in searches. Afterwards, they published some academic papers explaining their approach, and the Hadoop project was modeled after it. (Relevant links are in Appendix A.)

 
### Cloud Significance

Cloud platforms are good at managing large volumes of data. One of the tenets of big data analysis is bring the *compute to the data*, since moving large amounts of data is expensive and slow. A cloud service for the analysis of data already nearby in the cloud will usually be the most cost-effective and efficient.

<table>
    <tr>
        <td>
            <img src="../images/preface/blue.png" width="50">
        </td>
        <td>
            Large volumes of data stored on-premises are more effectively analyzed by using a Hadoop cluster that is also on-premises.
        </td>
    </tr>
</table>

Hadoop greatly simplifies building distributed data processing flows. Running *Hadoop as a Service* in the cloud takes this a step further by also simplifying Hadoop installation and administration. The Hadoop cloud service can access data directly from certain cloud storage services such as S3 on Amazon and Blob Storage on Windows Azure.

Using MapReduce through a Hadoop cloud platform service lets you rent instances for short amounts of time. Since a Hadoop cluster can involve many compute nodes, even hundreds, the cost savings can be substantial. This is especially convenient when data is stored in cloud storage services that integrate well with Hadoop.


## Impact

Cost Optimization, Availability, Scalability


## Mechanics 
 
> <h3 align="center">Map and Reduce from Computer Science</h3>
>
> For context, it is helpful to be aware that the name of this pattern derives from the functional programming concepts of *map* and *reduce*. In computer science, map and reduce describe functions that can be applied to lists of elements. A map function is applied to each element in a list, resulting in a new list of elements (sometimes called a *projection*). A reduce function is applied to all elements in a list, resulting in a single scalar value.
>
> Consider the list [“foo”, “bar”]. A map function that converts a single string to uppercase would result in a list where the letters have all been converted to uppercase ([“FOO”, “BAR”]); a corresponding reduce function that accepts a list of strings and concatenates them would produce a single result string (“FOOBAR”). A map function that returns the length of a string would result in a list of word lengths ([3, 3]); a corresponding reduce function that accepts a list of integers and adds them up would produce a sum (6).
>
> Map produces a new list, while reduce produces a scalar result. While simple concepts, they are powerful.

The map and reduce functions implemented in this pattern are conceptually similar to the computer science versions, but not exactly the same. In the MapReduce Pattern, the lists consist of key/value pairs rather than just values. The values can also vary widely: a text block, a number, even a video file.

Hadoop is a sophisticated framework for applying map and reduce functions to arbitrarily large data sets. Data is divvied up into small units that are distributed across the cluster of data nodes. A typical Hadoop cluster contains from several to hundreds of data nodes. Each data node receives a subset of the data and applies the map and reduce functions to locally stored data as instructed by a *job tracker* that coordinates jobs across the cluster. In the cloud, "locally stored" may actually be durable cloud storage rather than the local disk drive of a compute node, but the principle is the same.

Data may be processed in a workflow where multiple sets of map and reduce functions are applied sequentially, with the output of one map/reduce pair becoming the input for the next. The resulting output typically ends up on the local disk of a compute node or in cloud storage. This output might be the final results needed or may be just a data shaping exercise to prepare the data for further analytical tools such as Excel, traditional reporting tools, or Business Intelligence (BI) tools.


### MapReduce Use Cases

MapReduce excels at many data processing workloads, especially those known as *embarrassingly parallel problems*. Embarrassingly parallel problems can be effortlessly parallelized because data elements are independent and can be processed in any order. The possibilities are extensive, and while a full treatment is out of scope for this brief survey, they can range from web log file processing to seismic data analysis.

> <h3 align="center">MapReduce and You</h3>
>
> You may see the results of MapReduce without realizing it. LinkedIn uses it to suggest contacts you might want to add to your network. Facebook uses it to help you find friends you may know. Amazon uses it to recommend books. It is heavily used by travel and dating sites, for risk analysis, and in data security. The list of uses is extensive.

This pattern is not typically used on small data sets, but rather on what the industry refers to as *big data*. The criteria for what is or is not big data is not firmly established, but usually starts in the hundreds of megabytes or gigabytes range and goes up to petabytes. Since MapReduce is a distributed computing framework that simplifies data processing, one might reasonably conclude that big data begins when the data is too big to handle with a single machine or with conventional tooling.

If the data being processed will grow to those levels, the pattern can be developed on smaller data and it will continue to scale. From a programming point of view, there is no difference between analyzing a couple of small files and analyzing petabytes of data spread out over millions of files. The map and reduce functions do not need to change.


### Beyond Custom Map and Reduce Functions

Hadoop supports expressing map and reduce functions in Java. However, any programming language with support for standard input and standard output (such as C++, C#, Python) can be used to implement map and reduce functions using Hadoop streams. Also, depending on details of the cloud environment, other higher-level programming languages may be supported in some cases. For example, in Hadoop on Azure, JavaScript can be used to script *Pig* (Pig is introduced shortly).

Hadoop is more than a robust distributed map/reduce engine. In fact, there are so many other libraries in the Apache Hadoop Project, that it is more accurate to consider Hadoop to be an ecosystem. This ecosystem includes higher-level abstractions beyond map/ reduce.

For example, the *Hive* project provides a query language abstraction that is similar to traditional SQL; when one issues a query, Hive generates map/reduce functions and runs them behind the scenes to carry out the requested query. Using Hive interactively as an *ad hoc* query tool is a similar experience to using a traditional relational database. However, since Hadoop is a batch-processing environment, it may not run as fast.

*Pig* is another query abstraction with a data flow language known as *Pig Latin*. Pig also generates map/reduce functions and runs them behind the scenes to implement the higher-level operations described in Pig Latin.

*Mahout* is a machine-learning abstraction responsible for some of the more sophisticated jobs, such as music classification and recommendations. Like Hive and Pig, Mahout generates map/reduce functions and runs them behind the scenes.

Hive, Pig, and Mahout are abstractions that, comparable to a compiler, turn a higher-level abstraction (such as Java code) into machine instructions.

The ecosystem includes many other tools, not all of which generate and execute map/ reduce functions. For example, *Sqoop* is a relational database connector that gives you access to advanced traditional data analysis tools. This is often the most potent combination: use Hadoop to get the right data subset and shape it to the desired form, then use Business Intelligence tools to finish the processing.

 
### More Than Map and Reduce

Hadoop is more than just capable of running MapReduce. It is a high-performance operating system for building distributed systems cost-efficiently.

Each byte of data is also stored in triplicate, for safety. This is similar to cloud storage services that typically store data in triplicate, but refers to Hadoop writing data to the local disk drives of its data nodes. Cloud storage can be used to substitute for this, but that is not required.

Automatic failure recovery is also supported. If a node in the cluster fails, it is replaced, any active jobs restarted, and no data will be lost. Tracking and monitoring administrative features are built in.


## Example: Building PoP on Windows Azure

A new feature we want to add to the Page of Photos (PoP) application (which was described in the Preface) is to highlight the most popular page of all time. To do this, we first need data on page views. These are traditionally tracked in web server logs, and so can easily be parsed out. As described in *Horizontally Scaling Compute Pattern* (Chapter 2), the PoP IIS web logs are collected and conveniently available in blob storage.

We can set up Hadoop on Azure to use our web log files as input directly out of blob storage. We need to provide map and reduce functions to process the web log files. These map and reduce functions would parse the web logs, one line at a time, extracting just the visited page from that line. Each line of the log file would contain a reference to a page; for example, a row in the web log indicating a visit to *http://www.pageofphotos.com/jebaka* would include the string “/jebaka.” Our map function can remove the leading forward slash character. We could also have our map function ignore any rows that were not for visited pages, such as rows that were for image downloads. Because MapReduce expects a map function to return an attribute value pair, our simple map function would output a single string such as “jebaka, 1” where the “1” indicates a count of 1.

MapReduce will collect all instances of “jebaka, 1” and pass them on as a single list to our reduce function. The key here is “jebaka” and the list passed to the reduce function is a key followed by many values. The input to the reduce function would be “jebaka, 1 1 1 1 1 1 1 1 1 1” (and so on, depending upon how many views that page got). The reduce function needs to add up all the hits (10 in this example) and output it as “jebaka, 10” and that’s all.

MapReduce will take care of the rest of the bookkeeping. In the end, there will be a bunch of output files with totals in them. While more map/reduce functions could be written to further simplify, we’ll assume that a simple text scan (not using MapReduce) could find the page with the greatest number of views and cache that value for use by the PoP logic that displays the most popular site on the home page.
 
If we wanted to update this value once a week, we could schedule the launching of a MapReduce job. The individual nodes (which are worker role instances under the covers) in the Hadoop on Azure cluster will only be allocated for a short time each week.

Enabling clusters to be spun up only periodically without losing their data depends on their ability to persist data into cloud storage. In our scenario, Hadoop on Azure reads web logs from blob storage and writes its analysis back to blob storage. This is convenient and cuts down on the cost of compute nodes since they can be allocated on demand to run a Hadoop job, then released when done. If the data was instead maintained on local disks of compute nodes (which is how traditional, non-cloud Hadoop usually works), the compute nodes would need to be kept running continually.

<table>
    <tr>
        <td>
            <img src="../images/preface/red.png" width="70">
        </td>
        <td>
            As of this writing, the Hadoop on Azure service is in preview. It is not yet supported for production use.
        </td>
    </tr>
</table>

> <h3 align="center">Related Chapters</h3>
>
> - *Horizontally Scaling Compute Pattern* (Chapter 2)
>
> - *Eventual Consistency Primer* (Chapter 5)
>
> - *Colocate Pattern* (Chapter 12)


## Summary

The MapReduce Pattern provides simple tools to efficiently process arbitrary amounts of data. There are abundant examples of common use that are not economically viable using traditional means. The Hadoop ecosystem provides higher-level libraries that simplify creation and execution of sophisticated map and reduce functions. Hadoop also makes it easy to integrate MapReduce output with other tools, such as Excel and BI tools.

</div>