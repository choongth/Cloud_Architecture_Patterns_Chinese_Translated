# Index

## Symbols

- 503 Service Unavailable status code, 87

## A

- Access Control Service (ACS), 140
- access policy, 122
- ACID principles, 55–56
- ACS (Access Control Service), 140, 151
- Active Directory, 141
- active/active failover configuration, 137, 142
- active/passive failover configuration, 137
- affinity groups, 150
  - defined, 112
  - for Colocate Pattern, 112
  - support for, 112
- algorithmic improvements, 6, 7
- Amazon, 62
- Amazon Dynamo Database, 57, 147
- Amazon S3 (Simple Storage Service), 32, 56, 117
- Amazon Simple Queue Service, 146
- Amazon Web Services, xi, xiv, 10, 55, 80, 95, 97, 111, 134, 145, 147
  - costs, 136
  - Elastic Load Balancing for, 18, 134
  - MapReduce in, 60
- An Analysis of Application Performance Data and Trends, 106
- Android, 121
- anycast routing protocol, 127
- Application Request Routing (ARR), 18
- applications
  - logic for
    - and commodity hardware, 81
    - multitenancy, 79
  - tiers in, 17–18
  - upgrades initiated by, 100
- architecture vs. technology, x
- Areas of Impact
  - Availability
    - Busy Signal Pattern, 84
    - Horizontally Scaling Compute Pattern, 14
    - MapReduce Pattern, 61
    - Multisite Deployment Pattern, 134
    - Node Failure Pattern, 94
    - Queue-Centric Workflow Pattern, 28
  - Cost Optimization
    - Auto-Scaling Pattern, 44
    - Colocate Pattern, 110
    - Horizontally Scaling Compute Pattern, 14
    - MapReduce Pattern, 61
  - Reliability
    - Multisite Deployment Pattern, 134
    - Queue-Centric Workflow Pattern, 28
  - Scalability
    - Auto-Scaling Pattern, 44
    - Busy Signal Pattern, 84
    - CDN Pattern, 127
    - Colocate Pattern, 110
    - Database Sharding Pattern, 68
    - Horizontally Scaling Compute Pattern, 14
    - MapReduce Pattern, 61
    - Multisite Deployment Pattern, 134
    - Queue-Centric Workflow Pattern, 28
    - Valet Key Pattern, 116
  - User Experience
    - Busy Signal Pattern, 84
    - CDN Pattern, 127
    - Colocate Pattern, 110
    - Database Sharding Pattern, 68
    - Horizontally Scaling Compute Pattern, 14
    - Multisite Deployment Pattern, 134
    - Node Failure Pattern, 94
    - Queue-Centric Workflow Pattern, 28
    - Valet Key Pattern, 116
- ARR (Application Request Routing), 18
- ASP.NET MVC, 25, 38
- asynchronous model, 27, 29
- at-least-once processing, 32
- atomicity, 57
- audience for this book, x
- Auto-Scaling pattern
  - and responsiveness, 47
  - limits in
    - platform-enforced, 48
    - setting, 48
  - PoP application example in, 48–51
    - auto-scaling resources for, 50–51
    - throttling for, 50
  - purpose of, 43–44
  - rules and signals for, 45–46
  - using with Horizontally Scaling Compute pattern with, 13
- automation for Colocate Pattern, 111
- automobile roadway example, 2
- autonomous node, 5
- availability, 48

## B

- bandwidth, 106
- BASE principles, 55–56, 147
- beyond current rental period caveat, 16
- BI (Business Intelligence), 62
- big data, 62
- blob storage, 18, 117
- Boston Azure Cloud User Group, x
- bottlenecks, 6
- Brewer's CAP Theorem, 53
- business equivalence, 33
- Business Intelligence (BI), 62
- Busy Signal pattern
  - and user experience, 88–89
  - busy signals for logging, 89–89
  - PoP application example in, 90–91
  - purpose of, 83–84
  - testing, 89–90
  - transient failures for, 85–87

## C

- C#, 25, 39, 90
- C++, 25
- Cache-Control header, 128–129
- caching
  - and proxy servers, 128
  - in CDN pattern, 128
- canonical name (CNAME), 129
- CAP Theorem, 53–54
- capacity planning, 21
- CDN (Content Delivery Network) pattern, 110
  - and eventual consistency, 128
  - caches in, 128
  - load balancing for, 150
  - PoP application example in, 129–131
    - and cost, 130
    - security considerations, 130
  - purpose of, 126–127
  - vs. Multisite Deployment pattern, 133
- Chaos Monkey, 90, 149
- clients, 84
- cloud computing, benefits of, ix
- cloud platform, 9–10
- cloud-native applications, ix–11
- CloudFront, 55, 147
- CNAME (canonical name), 129
- Colocate pattern
  - and cost, 111
  - and network latency, 109
  - automation for, 111
  - non-technical considerations, 111
  - PoP application example in, 111–113
    - affinity groups for, 112
    - logging, 112–113
    - metrics for, 112–113
  - purpose of, 109–110
- colocation alternatives, 142–143
- Command Query Responsibility Segregation (CQRS) pattern, 36–37, 147
- commands, 29, 37
- commodity hardware, 79–82
  - and application logic, 81
  - defined, 149
  - homogeneous hardware, 82
  - MTBF of, 80
  - MTTR of, 80
- compensating transaction, 34
- compression, data, 107
- compute nodes, 2, 13
- concurrent users, 5
- constraint rules, 49
- Content Delivery Network (CDN) pattern (see CDN (Content Delivery Network) pattern)
- controlled reboots, 103–104
- cookies, 19
- costs, 47, 76, 115
  - and CDN pattern, 130
  - and Colocate pattern, 111
  - and Multisite Deployment pattern, 136
  - calculating, xi
  - for Amazon Web Services, 136
  - for Windows Azure, 136
  - for Windows Azure Storage, 39
- Couchbase, 57, 70
- CQRS (Command Query Responsibility Segregation) pattern, 37, 147
- Cyber Monday, 8

## D

- data centers
  - choosing, 138
  - routing to closest, 138
- data nodes, 2
- data sovereignty, 135
- data tier, 18
- Database Sharding pattern
  - and database instances, 72
  - and reference data tables, 71
  - distributing shards, 70
  - PoP application example in, 72–76
    - fan-out queries across federations, 74–75
    - NoSQL alternative, 75–76
    - rebalancing federations, 73–74
  - purpose of, 67–68
  - shard keys, 70
  - when not to use, 71
- databases
  - NoSQL BASE principles, 55–56
  - programmatic differences in, 57
  - relational ACID principles, 55–56
- DDD (Domain Driven Design), 37
- dead letter queue, 35
- dequeue count, 33
- dequeuing, 29
- DevOps, 44
- disaster recovery (DR) plan, 137, 142
- distributed cache, 19
- distributed transactions, 53
- distributing shards, 70
- DNS (Domain Name System), 54
- Domain Driven Design (DDD), 37
- DR (disaster recovery) plan, 137, 142

## E

- edge caching, 127
- elastic, 15
- Elastic Load Balancing, 18, 134
- embarrassingly parallel problems, 62
- enqueuing, 29
- Enterprise Library 5.0 Integration Pack for Windows Azure, 147
- environmental signals, 44, 50
- EU (European Union), 136
- event sourcing, 37
- eventual consistency, 68, 76, 147
  - and CAP Theorem, 53–54
  - and CDN pattern, 128
  - and databases
    - NoSQL BASE principles, 55–56
    - programmatic differences in, 57
    - relational ACID principles, 55–56
  - examples of, 54–55
  - impact on application logic, 56–57
  - in PoP application example, 54
  - vs. distributed transactions, 53
  - vs. immediate consistency, 54
- exponential backoff, 88

## F

- F#, 25
- Facebook, 62, 140–141
- failover
  - and Multisite Deployment pattern, 136–137, 141–142
  - defined, 136
- failures, hardware, 81, 86
- failures, node, 22, 93–104
  - preparing for, 99–101
  - fault domains, 99–100
  - N+1 rule, 99
  - upgrade domains, 101
  - recovering from, 98–99, 104
  - resuming work-in-progress, 99
  - shielding users from, 98–99
  - treating all interruptions as, 95
- fault domains, 99–100
- federated authentication, 140
- federation keys, 73
- federation members, 73
- federations, 73
  - defined, 72
  - fan-out queries across, 74–75
  - rebalancing, 73–74
- FIFO (first in, first out) ordering, 29

## G

- Gatekeeper pattern, 116
- Google, 140–141
- Google App Engine, xi, 55, 60, 145
- Google App Engine Datastore service, 56
- Google BigTable, 147
- Google Mail, 89, 98

## H

- Hadoop, 59, 148
  - as a service, 61
  - capabilities of, 64
- handling poison messages, 34–36
- hardware
  - failures, 81
  - improvements to, 7
- hashing, 120
- HD video, 121
- Hive, 63
- homogeneous hardware, 82
- homogeneous nodes, 4
- horizontal resource allocation, 4
- horizontal scaling, 1, 3–5
- Horizontal Scaling Compute pattern
  - impact for, 14
  - is reversible, 14–17
  - managing many nodes, 20–22
    - capacity planning, 21
    - efficient management of, 20–21
    - failure in, 22
    - operational data collection, 22
    - sizing virtual machines, 21–22
  - PoP application example in, 22–26
    - logs for, 25–26
    - metrics for, 25–26
    - service tier for, 24–25
    - stateless nodes, 23–24
    - web tier for, 23
  - purpose of, 13–14
  - session state in, 17–20
    - and application tiers, 17–18
    - stateful nodes, 18–19
    - stateless nodes, 20
    - sticky sessions, 18
    - without stateful nodes, 19
  - using with Auto-Scaling pattern with, 13
  - using with Node Termination pattern with, 13
- HTML5 (HyperText Markup Language 5), 36, 116
- HTTP (Hypertext Transfer Protocol)/HTTPS (Hypertext Transfer Protocol Secure), 130
- hypervisor updates, 100

## I

- IaaS (Infrastructure as a Service), 10, 20
- idempotent processing
  - defined, 33
  - for Queue-Centric Workflow pattern, 33–34
  - naturally idempotent operations, 33
- identity provider (IdP), 140
- IdP (identity provider), 140
- IIS (Internet Information Services), 18
- immediate consistency, 54
- immediately consistent, 32
- in-place upgrade feature, 101
- infinite resources, illusion of, 21, 89
- infinite scalability, illusion of, 9
- Infrastructure as a Service (IaaS), 10, 20
- instance scaling, 50
- integrated sharding, 76
- Internet Information Services (IIS), 18
- invisibility window, 31–32
- iOS (iPhone, iPad), 121

## J

- Java, 25, 90
- JavaScript, 63

## K

- Kb (kilobits), 106
- KB (kilobytes), 106
- key-value store, 75
- keys, 75

## L

- last write wins model, 57
- limits
  - for Cloud Services, 85–86
  - platform-enforced, 48
  - setting, 48
- linear backoff, 88
- load balancing
  - defined, 17
  - for CDN, 150
- logging
  - busy signals, 89
  - data, 22
  - files for, 64
  - in PoP application example, 25–26, 112–113
- long polling, 36, 147
- loose coupling, 29, 146

## M

- Mahout, 63
- MapReduce pattern, 61–64
  - abstractions for, 63
  - defined, 59
  - Hadoop capabilities, 64
  - PoP application example in, 64–65
  - purpose of, 60–61
  - use cases for, 62–63
- mean time between failures (MTBF), 80–81
- mean time to recovery (MTTR), 80–81
- measuring scalability, 5–6
- metrics for Colocate pattern, 112–113
- mixed security models, 130
- money leak, 39
- MongoDB, 57, 148
- Moore's Law, 7
- MTBF (mean time between failures), 80–81
- MTTR (mean time to recovery), 80–81
- Multisite Deployment pattern, 110
  - and cost, 136–136
  - and failover, 136–137
  - non-technical considerations, 135–136
  - PoP application example in, 137–143
    - and failover, 141–142
    - choosing data centers, 138
    - colocation alternatives, 142–143
    - replicating identity information, 140–141
    - replicating user data, 138–140
    - routing to closest data center, 138
  - purpose of, 133–134
  - vs. CDN pattern, 133
- multitenancy, 77–79
  - and application logic, 79
  - defined, 149
  - performance for, 78–79
  - security for, 78
- multitier application, 110

## N

- N+1 rule, 99
- naturally idempotent operations, 33
- Netflix, 90, 149
- network latency, 133
  - and Colocation pattern, 109
  - challenges for, 105–107
  - perceived network latency, 107
  - reducing, 107
  - reducing perception of, 107
- NIST Definition of Cloud Computing, 145
- node, xi, 13
- Node Failure pattern
  - capacity for failure, 96–96
  - handling shutdown, 96–98
    - with minimal impact to user experience, 97
    - without losing operational data, 98
    - without losing partially completed work, 97
  - PoP application example in, 99–104
    - preparing for failure, 99–101
    - recovering from failure, 104
    - role instance shutdown, 101–104
  - purpose of, 93–94
  - recovering from failure, 98–99
  - resuming work-in-progress, 99
  - shielding users from, 98–99
  - scenarios for, 94–95
  - treating all interruptions as node failures, 95–95
- Node.js, 25, 90
- nodes
  - defined, 2
  - managing, 20–22
    - capacity planning, 21
    - efficient management of, 20–21
    - failure in, 22
    - operational data collection, 22–22
    - sizing virtual machines, 21–22
- noisy neighbor problem, 86
- NoSQL databases, 28, 56, 75, 85, 122
  - BASE principles for, 55–56
  - in PoP application example, 75–76

## O

- OnStop method, 102, 103
- OnStopping event, 103
- optimistic concurrency model, 57

## P

- PaaS (Platform as a Service), 10, 20
- Page of Photos (PoP) application example (see PoP (Page of Photos) application example)
- partition keys, 75
- partition tolerance, 53
- patterns, ix
- perceived network latency, 107
- performance
  - and DR plan, 142
  - defined, 8
  - for multitenancy, 78–79
- PHP, 25
- Pig, 63
- Pig Latin, 63
- ping utility, 106, 149
- Platform as a Service (PaaS), 10, 20
- poison messages, 34–36
- PoP (Page of Photos) application example, xiii, 145
  - eventual consistency in, 54
  - in Auto-Scaling pattern, 48–51
    - auto-scaling resources for, 50–51
    - throttling for, 50
  - in Busy Signal pattern, 90–91
  - in CDN pattern, 129–131
    - and cost, 130
    - security considerations, 130
  - in Colocate pattern, 111–113
    - affinity groups for, 112
    - logging, 112–113
    - metrics for, 112–113
  - in Database Sharding pattern, 72–76
    - fan-out queries across federations, 74–75
    - NoSQL alternative, 75–76
    - rebalancing federations, 73–74
  - in Horizontal Scaling Compute pattern, 22–26
    - logs for, 25–26
    - service tier for, 24–25
    - stateless nodes, 23–24
    - web tier for, 23
  - in MapReduce pattern, 64–65
  - in Multisite Deployment pattern, 137–143
    - and failover, 141–142
    - choosing data centers, 138
    - colocation alternatives, 142–143
    - replicating identity information, 140–141
    - replicating user data, 138–140
    - routing to closest data center, 138
  - in Node Failure pattern, 99–104
    - preparing for failure, 99–101
    - recovering from failure, 104
    - role instance shutdown, 101–104
  - in Queue-Centric Workflow pattern, 38–41
    - service tier for, 39–40
    - user interface tier for, 38–39
  - in Valet Key pattern, 121–123
    - public read access in, 121
    - shared access signatures, 122–123
- pre-fetching objects, 130
- proactive rules, 49
- properties, 75
- proxy servers, 128
- public read access, 117
  - in PoP application example, 121
  - in Valet Key pattern, 118
- public, defined, 129
- Python, 25, 90

## Q

- queries, 37
- Queue-Centric Workflow pattern
  - PoP application example, 38–41
    - service tier for, 39–40
    - user interface tier for, 38–39
  - purpose of, 28
  - queues in, 30
  - receiver for, 31–36
    - at-least-once processing, 31–32
    - handling poison messages, 34–36
    - idempotent processing, 33–34
    - invisibility window, 31–32
  - scaling tiers independently, 37–38
  - user experience, 36–37
  - vs. Command Query Responsibility Segregation pattern, 36
- queues, 30
- Quora, 89

## R

- Rackspace, 117
- Rai, Rahul, 150
- reactive rules, 49
- read-mostly data, 71
- Reboot Role Instance operation, 103
- receivers, 31–36
  - handling poison messages, 34–36
  - idempotent processing, 33–34
  - invisibility window, 31–32
- reducing network latency, 107
- reference data tables, 71
- relational databases, 55–56
- reliable queue, 30
- replicating
  - identity information, 140–141
  - user data, 138–140
- resource bottlenecks, 6
- resource contention, 6–7
- resources, 145–151
- responsiveness, 5, 47
- retry policies, 83
  - on Windows Azure, 149
  - retry after delay, 88
  - retry immediately, 87
  - retry with increasing delays, 88
- role instance, 23
- round-robin load balancing, 4

## S

- SaaS (Software as a Service), 45, 78
- SAS (Shared Access Signatures), 121–123, 150
- scalability
  - business concerns, 7–8
  - defined, 1, 8
  - horizontal scaling, 3–5
  - measuring, 5–6
  - resource contentions, 6–7
  - scale units, 6
  - vertical scaling, 3
- scale units, 6
- scaling, 37–38 (see also horizontal scaling; vertical scaling)
- scenarios for Node Failure pattern, 94–95
- security considerations
  - for multitenancy, 78
  - for Valet Key pattern, 120
  - in CDN pattern, 130
- self-inflicted failures, 8
- Service Level Agreement (SLA), 48
- Service Oriented Architecture (SOA), 24
- service tier
  - defined, 17
  - for PoP application example, 24–25, 39–40
- services
  - defined, 17
  - usage of, xiv
- session state, 17–20
  - and application tiers, 17–18
  - stateful nodes, 18–19
  - stateless nodes, 20
  - sticky sessions, 18
  - without stateful nodes, 19
- shard keys, 70
- shards, 51, 67, 69, 73
- Shared Access Signatures (SAS), 121–123, 150
- shared nothing architecture, 69
- shutdown
  - handling, 96–98
    - with minimal impact to user experience, 97
    - without losing operational data, 98
    - without losing partially completed work, 97
  - of role instance, 101–104
    - using controlled reboots, 103–104
    - web role instance shutdown, 102
    - worker role instance shutdown, 103
- SignalR for ASP.NET, 36, 147
- SimpleDB database, 56
- single point of failure (SPoF), 99
- SLA (Service Level Agreement), 48
- slave nodes, 68
- SOA (Service Oriented Architecture), 24
- SOAP, 17
- Socket.IO for Node.js, 36, 147
- Software as a Service (SaaS), 45, 78
- Southwest Airlines, 82
- speed, 8, 106
- SPoF (single point of failure), 99
- SQL Azure, 24
- SQL Azure Data Sync, 148
- SQL Azure Point In Time Restore, 151
- SQL Data Sync service, 139
- SQL Data Sync Service, 143
- Sqoop, 63
- Startup Tasks, 23
- stateful nodes, 18–19
- stateless nodes
  - for PoP application example, 23–24
  - in Horizontal Scaling Compute pattern, 20
- sticky sessions, 18, 97
- storage access key, 117
- Super Bowl commercials, 8
- systems, xiv

## T

- technology vs. architecture, x
- temporary access, 117–119
- tenant isolation, 78
- terminology, xiv, 16–17
- testing, 89–90
- throttling
  - defined, 50, 86
  - for PoP application example, 50
- TicketDirect case study, 51, 147
- time lag between failure and recognition of, 96
- Topaz, 90, 149
- transient failures, 79, 85–87
- Transient Fault Handling Application Block, 90, 149
- trusted subsystems, 117
- Twitter, 89

## U

- Universal Coordinated Time (UTC), 72
- upgrade domains, 101
- use cases for MapReduce pattern, 62–63
- user experience, 99
  - and Busy Signal pattern, 88–89
  - and eventual consistency, 57
  - and Queue-Centric Workflow pattern, 36–37
  - handling shutdown without impacting, 97
- user interface tier, 38–39
- UTC (Universal Coordinated Time), 72
- UX, 99

## V

- Valet Key pattern
  - PoP application example in, 121–123
    - shared access signatures, 122–123
  - public read access in, 118
  - purpose of, 115–116
  - security considerations for, 120
  - temporary access, 119
  - vs. Gatekeeper pattern, 116
- vertical scaling, 1–3
- virtual machines, 21–22
- VPN (Virtual Private Network), 136

## W

- WAD (Windows Azure Diagnostics), 25, 26, 112, 146
- WASABi (Windows Azure Autoscaling Application Block), 48
- web applications, xiv
- Web Role, 23, 102, 111, 130
- web service, 17
- Web Service\Current Connections counter, 102
- web tier
  - defined, 17
  - for PoP application example, 23
- Windows Azure, xi, xiv, 10, 55, 80, 95, 97, 101, 102, 111, 134, 145, 146
  - costs, 136
  - Enterprise Library 5.0 Integration Pack for, 147
  - for mobile devices, 150
  - MapReduce in, 60
  - Retry Logic on, 149
- Windows Azure Autoscaling Application Block (WASABi), 48
- Windows Azure Blob Storage Service, 24, 112, 129, 150
- Windows Azure Caching, 91
- Windows Azure Compute, 112
- Windows Azure Connect, 143
- Windows Azure Diagnostics (WAD), 25, 112, 146
- Windows Azure Fabric Controller, 99
- Windows Azure Media Services, 130, 150
- Windows Azure Pricing Calculator, 76
- Windows Azure Service Bus, 91, 143
- Windows Azure SQL Data Sync service, 150
- Windows Azure SQL Database Federations, 148
- Windows Azure SQL Databases, 72, 90, 139
- Windows Azure Storage, 25, 39, 56, 90, 91, 103, 112, 141
- Windows Azure Storage Analytics, 26, 112, 146
- Windows Azure Storage service, 146
- Windows Azure Table Storage service, 24, 112, 148
- Windows Azure Traffic Manager, 55, 134, 138, 142, 151
- Windows Azure Virtual Networking, 143
- Windows Live ID, 141
- Windows Phone, 121
- Worker Role, 23, 24, 103, 111

## Y

- Yahoo!, 140
