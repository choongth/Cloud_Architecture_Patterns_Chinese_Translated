# 附录 A &emsp; 进一步阅读

<div align="justify">

## Page of Photos (PoP) 示例

本书通篇使用 Page of Photos 示例应用程序。其中部分功能已使用 Windows Azure 实现，用于演示书中的概念。Page of Photos 的代码（最初非常简洁，之后由作者及其合作者逐步完善）将在 GitHub 的公共代码库中共享。

- 运行 Page of Photos (PoP) 示例应用程序：*http://www.pageofphotos.com*

- 查看 Page of Photos (PoP) 示例应用程序的源代码：*http://www.github.com/codingoutloud/pageofphotos*

- 查找有关本书及未来可能相关内容的信息：*http://www.cloudarchitecturebook.com*


## 序言和各章节中的资源

- Windows Azure Platform: *http://www.windowsazure.com*
- Amazon Web Services: *http://aws.amazon.com/*
- Google App Engine: *http://developers.google.com/appengine/*
- A NIST Definition of Cloud Computing (SP 800-145 Sept. 2011): *http://csrc.nist.gov/publications/nistpubs/800-145/SP800-145.pdf*
- (NIST) Cloud Computing Synopsis and Recommendations (SP 800-146 May 2012): *http://csrc.nist.gov/publications/nistpubs/800-146/sp800-146.pdf*


### **第一章**

- “A Compuware analysis of 33 major retailers across 10 million home page views showed that a 1-second delay in page load time reduced conversions by 7%.” Source: Compuware, April 2011.
- “Google observed that adding a 500-millisecond delay to page response time caused a 20% decrease in traffic” Source: Marissa Mayer, “What Google Knows” talk at Web 2.0 Conf 2006 (11/09/2006): *http://conferences.oreillynet.com/presentations/web2con06/mayer.ppt*
- “Yahoo! observed a 400-millisecond delay caused a 5-9% decrease [in traffic].” And “Amazon.com reported that a 100-millisecond delay caused a 1% decrease in retail revenue.” Source: *http://blog.yottaa.com/2010/11/secret-sauce-for-successful-web-site-web-performance-optimization-wpo*
- “Google has started using web-site performance as a signal in its search engine rankings.” Source: Using site speed in web search ranking: *http://googlewebmastercentral.blogspot.com/2010/04/using-site-speed-in-web-search-ranking.html*
- Example of a self-inflicted scaling failure: *http://glinden.blogspot.com/2006/11/amazon-crashes-itself-with-promotion.html*
- ITIL: *http://en.wikipedia.org/wiki/Information_Technology_Infrastructure_Library*


### **第二章**

- Windows Azure Storage Analytics: Logs and Metrics: *http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-analytics.aspx*
- Windows Azure Diagnostics: *http://msdn.microsoft.com/en-us/library/windowsazure/gg433048.aspx*
- About Load Balancers: *http://1wt.eu/articles/2006_lb/*


### **第三章**

- Comparing Windows Azure Storage Queues with Amazon Simple Queue Service: *http://gauravmantri.com/2012/04/15/comparing-windows-azure-queue-service-and-amazon-simple-queue-servicesummary/*
- Comparing the two queue services offered by Windows Azure: *http://msdn.microsoft.com/en-us/library/windowsazure/hh767287.aspx*
- Update Message on a Windows Azure Queue: *http://msdn.microsoft.com/en-us/library/windowsazure/hh452234*
- Loose coupling: *http://en.wikipedia.org/wiki/Loose_coupling*
- Long Polling with SignalR for ASP.NET: *http://signalr.net*
- Long Polling with Socket.IO for Node.js: *http://socket.io*
- CQRS Pattern: *http://martinfowler.com/bliki/CQRS.html*
- CQRS: *http://www.cqrsinfo.com/*
- Event Sourcing: *http://martinfowler.com/eaaDev/EventSourcing.html*


### **第四章**

- Enterprise Library 5.0 Integration Pack for Windows Azure (contains WASABi): *http://msdn.microsoft.com/en-us/library/hh680918*
- Hosted services that can be used to monitor and autoscale your Windows Azure applications include AzureWatch from Paraleap Technologies and AzureOps from Opstera.
- Auto-Scaling on Amazon Web Services: *http://aws.amazon.com/autoscaling*
- Ticket Direct case study that sharded databases then consolidated depending on ticket sales: New Zealand-based TicketDirect International: *http://www.microsoft.com/casestudies/Case_Study_Detail.aspx?CaseStudyID=4000005890*


### **第五章**

- Life beyond Distributed Transactions: an Apostate’s Opinion (by Pat Helland): *http://www-db.cs.wisc.edu/cidr/cidr2007/papers/cidr07p15.pdf*
- CAP Twelve Years Later: How the “Rules” Have Changed (by Eric Brewer): *http://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed*
- Introducing BASE: *http://queue.acm.org/detail.cfm?id=1394128*
- Introducing Eventual Consistency: *http://queue.acm.org/detail.cfm?id=1466448*
- Eventual consistency in CloudFront: *http://docs.amazonwebservices.com/AmazonCloudFront/latest/DeveloperGuide/Concepts.html*
- Google BigTable: *https://developers.google.com/appengine/docs/python/datastore/overview*
- Amazon Dynamo SOSP paper: *http://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf*
- Azure Storage SOSP paper: *http://sigops.org/sosp/sosp11/current/2011-Cascais/printable/11-calder.pdf*


### **第六章**

- Hadoop: *http://hadoop.apache.org*
- Hadoop on Windows Azure: *http://www.hadooponazure.com*


### **第七章**

- An Unorthodox Approach to Database Design: The Coming of the Shard from High Scalability blog: *http://highscalability.com/unorthodox-approach-database-design-coming-shard.*
- The official source for learning about Federations in Windows Azure SQL Database.
- For anyone interested in Federations for Windows Azure SQL Database, Cihan Biyikoglu's blog is a must-read. Some particularly useful posts are listed below.

    + Implementing MERGE command using SQL Azure Migration Wizard by @gihuey: *http://blogs.msdn.com/b/cbiyikoglu/archive/2012/02/20/implementing-alter-federation-merge-at-command-using-sql-azure-migration-wizard-by-gihuey.aspx.*

    +  Introduction to Fan-out Queries for Federations in SQL Azure (Part 1): Scalable Queries over Multiple Federation Members, MapReduce Style!: *http://blogs.msdn.com/b/cbiyikoglu/archive/2011/12/29/introduction-to-fan-out-queries-querying-multiple-federation-members-with-federations-in-sql-azure.aspx.*

- Integrated sharding support with Windows Azure SQL Database Federations: *http://blogs.msdn.com/b/cbiyikoglu/archive/2012/02/08/connection-pool-fragmentation-scale-to-100s-of-nodes-with-federations-and-you-won-t-need-to-ever-learn-what-these-nasty-problems-are.aspx*
- Federations: *http://msdn.microsoft.com/en-us/magazine/hh848258.aspx*
- Choosing a shard key in MongoDB: *http://www.mongodb.org/display/DOCS/Choosing+a+Shard+Key*
- SQL Azure Data Sync: *http://msdn.microsoft.com/en-us/library/windowsazure/hh667301.aspx*
- Windows Azure Table Storage service: *http://www.windowsazure.com/en-us/develop/net/how-to-guides/table-services/*
- Generating a GUID as a cluster key with NEWID for Federations on SQL Database: *http://msdn.microsoft.com/en-us/library/ms190348.aspx*


### **第八章**

- Definition of multitenancy: *http://en.wikipedia.org/wiki/Multitenancy*
- Definition of commodity hardware: *http://en.wikipedia.org/wiki/Commodity_hardware*


### **第九章**

- Definition of multitenancy: *http://en.wikipedia.org/wiki/Multitenancy*
- Transient Fault Handling Application Block (Topaz): *http://msdn.microsoft.com/en-us/library/hh680934(v=PandP.50).aspx*
- Scalability Targets for Windows Azure Storage: *http://blogs.msdn.com/b/windowsazurestorage/archive/2010/05/10/*windows-azure-storage-abstractions-and-their-scalability-targets.aspx
- Implementing Retry Logic on Windows Azure: *http://www.davidaiken.com/2011/10/10/implementing-windows-azure-retry-logic/*
- Fault isolation and recovery: *http://www.faqs.org/rfcs/rfc816.html*
- Chaos Monkey from Netflix: *http://techblog.netflix.com/2011/07/netflix-simian-army.html*


### **第十章**

- Understanding Network Failures in Data Centers: Measurement, Analysis, and Im¬plications: *http://research.microsoft.com/en-us/um/people/navendu/papers/greenberg09vl2.pdf*
- How Windows Azure knows a Role Instance (node) is faulty: *http://blogs.msdn.com/b/mcsuksoldev/archive/2010/05/10/how-does-azure-identify-a-faulty-role-instance.aspx*
- Windows Azure Troubleshooting Best Practices: *http://msdn.microsoft.com/en-us/library/windowsazure/hh771389.aspx*
- Updating a Windows Azure deployment, including Fault Domains and Update Domains: *http://msdn.microsoft.com/en-us/library/ff966479.aspx*


### **第十一章**

- Ping utility: *http://en.wikipedia.org/wiki/Ping* 
- It’s the Latency Stupid essay: *http://rescomp.stanford.edu/~cheshire/rants/Latency.html*


### **第十二章**

- On the importance of affinity groups: *https://msmvps.com/blogs/nunogodinho/archive/2012/03/04/importance-of-affinity-groups-in-windows-azure.aspx*


### **第十三章**

- Windows Azure Toolkits for Mobile Devices (Android, iOS, Windows Phone, and more): *https://github.com/WindowsAzure-Toolkits*
- Restricting Access to Containers and Blobs Windows Azure: *http://msdn.microsoft.com/en-us/library/windowsazure/dd179354*
- Web Browser Same Origin Policy: *http://en.wikipedia.org/wiki/Same_origin_policy*
- Using a Shared Access Signature (REST API): *http://msdn.microsoft.com/en-us/library/windowsazure/ee395415.aspx*
- Rahul Rai’s sample code showing access to Windows Azure Blob Storage from HTML 5 Web Browser: *http://code.msdn.microsoft.com/windowsazure/Silverlight-Azure-Blob-3b773e26*
- Trusted Subsystem Design: *http://msdn.microsoft.com/en-us/library/aa905320.aspx*


### **第十四章**

- Anycast protocol enables geographic load balancing for CDN: *http://en.wikipedia.org/wiki/Anycast*
- Windows Azure Media Service: *https://www.windowsazure.com/en-us/home/features/media-services/*
- Recorded talk on Windows Azure CDN: *http://channel9.msdn.com/Events/TechEd/NorthAmerica/2011/COS401*


### **第十五章**

- Windows Azure SQL Data Sync service: *http://msdn.microsoft.com/en-us/library/windowsazure/hh456371.aspx*
- Windows Azure Traffic Manager: *http://msdn.microsoft.com/en-us/wazplatformtrainingcourse_windowsazuretrafficmanager.aspx*
- A Guide to Claims-Based Identity and Access Control: *http://msdn.microsoft.com/en-us/library/ff423674.aspx*
- Windows Azure Access Control Service: *http://msdn.microsoft.com/en-us/library/windowsazure/gg429786.aspx*
- Automating the Windows Azure Access Control Service (ACS): *http://msdn.microsoft.com/en-us/library/gg185927.aspx*
- ACS automation sample code: *http://acs.codeplex.com/releases/view/57595*
- SQL Azure Point In Time Restore now available in preview: *http://www.microsoft.com/en-us/download/details.aspx?id=28364*
- Business Continuity in Windows Azure SQL Database: *http://msdn.microsoft.com/en-us/library/windowsazure/hh852669.aspx*

</div>