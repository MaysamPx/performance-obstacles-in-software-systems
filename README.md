# Five performance obstacles in software systems
I'm going to discuss five limitations and obstacles that cause slowdowns in software systems (based on actual cases).


<p> Efficiency is a critical consideration when dealing with resource limitations in software systems. These constraints, which are typically present in most systems, including hardware and software aspects such as disk, processor, and main memory, can restrict the performance of the overall system or a critical subsystem moment by moment or continuously. In the context of software systems, components that impede performance in this way are commonly referred to as bottlenecks.
For instance, a large accounting system may feature a powerful processing core for calculating complex payroll details, taxes, and insurance rules, but its performance may be limited by a relatively simple reporting subsystem that is utilized by users. This may result in low utilization of underlying resources and reduced efficiency of the system. In the following sections, we will discuss several common types of bottlenecks in software systems. </p>

### Transferring external APIs delay to the system
<p> 
One of the potential bottlenecks that designers face within a system, especially a monolithic one, is the reliance on external APIs to hold and maintain resources (such as threads). The assumption underlying this design is that the external system is always in a stable state and guarantees a response time for every request. However, this assumption can often be violated in practice, and the external system can suffer from delay, downtime, and failure. At that moment, the abstraction that has been created makes it difficult for the current system designer to understand the details of the delay in the external system. Although this abstraction is created for simplicity and to provide a unified picture, it hides the details of the problem by drawing a veil of ambiguity. Therefore, the subsystem or module that is responsible for the primary task of connecting to the external system's APIs (even synchronously) can potentially become a bottleneck.
</p>

<p>
For example, consider a system that receives the prices of various cryptocurrencies from different external systems and instantaneously draws and updates a graphical chart on a page. In this system, if the asynchronous and data-pushing approaches are not used, the chart drawing module must wait for the maximum possible delay in the set of external APIs to draw the chart.
</p>

<p>
One of the most straightforward solutions in such situations is to switch from synchronous to asynchronous approaches with suitable queuing and persistence using Callback methods or other popular techniques. In this situation, each request-carrying thread becomes free and returns resources to the system upon asynchronous endpoint invocation, while the system experiences relatively stable and low oscillation.
</p>

### Memory leak turning memory latency into an actual bottleneck

This issue, if not addressed properly, can lead to a system failure over time gradually and calmly. Generally, if the fundamental testing is not performed at the beginning of the process, the issue may not be apparent. This can result in a decrease in performance and efficiency indicators, sometimes after developers have made changes and modifications and after the deployment of the system. As a result, it is crucial to implement proper testing procedures and performance monitoring to prevent memory leaks and ensure the system is functioning efficiently and effectively.

![image](https://github.com/MaysamPx/performance-obstacles-in-software-systems/assets/13215181/aade0f44-e1c9-469c-a256-6cfe8d990090)

<p>
  Naturally, without proper profiling, finding and fixing issues can be a difficult task. If the application has complex business logic, guessing and trying to locate the problem can be a time-consuming process. Interestingly, garbage collection is not a panacea for all issues, and the problem must be precisely identified, including the specific system component and the nature of the issue.
</p>

<p>
  As an example, consider a Java application that acts as a proxy component in a telecommunications system between two other applications, processing any streaming data passing through and adding a series of metadata or payloads to the end of data packets. Assume that all processing is also in-memory and the disk is not involved. In this scenario, with only a minor issue in memory management and failure to manually release resources that the GC is unable to recognize (such as unclosed resources, static fields, and other issues in Java), memory can turn into a bottleneck.
</p>

Some examples of potential issues that may arise:

https://www.baeldung.com/java-memory-leaks

https://stackify.com/memory-leaks-java/

### Blocked threads in synchronous processing
<p>
  Imagine a system that has powerful vertically scalable infrastructure, but can only handle a limited number of threads from the operating system (e.g. 2500 with equal priority, with no specific restriction on their nature or definition). On the other side of the system, a powerful web server like Nginx is set up to simply route requests to the designated endpoint, and internal requests in this system are handled completely synchronously with its subsystems and kept busy until the end of the request thread.
</p>

<p>
For convenience, let's assume that most of these requests are also mapped to a limited-capacity database transaction (e.g. 200 requests per second at best). In this situation, if the system at the app layer experiences a higher number of requests than the relational database throughput, delays gradually spread from the database to higher layers, and the average response time for requests on the app side increases to the point where the system constantly faces a higher number of requests than 2500, encountering a specific delay. In this case, the system (or rather the OS) probably tries to context switch between threads to be able to handle requests and send them back to the database, which does not have its normal capacity (without a backpressure mechanism) and has also reached its connection pool ceiling through the app.
</p>

<h4>
  Now, two things have probably happened:
</h4>

<p>
  Firstly, the database is not responding normally to 200 requests per second and the execution time of the query has significantly increased. Secondly, all threads are in a busy state and waiting for a response from the database, and the system is generally locked.
</p>

### Hidden bottlenecks
<p>
Limited functionality of a key operation in a system results in hidden operational bottlenecks. Consider an online shopping system where one of the main operations is registering a new order in the shopping cart and a top-level metric for measuring system efficiency is the "time taken to register an order until electronic payment," regardless of browsing time. Under normal conditions, this operation and others occur with less delay, but during peak periods, such as "sales" or "calendar events like holidays and Black Friday," the system may slow down, causing the number of "cart registrations per unit time" to decrease or the upper limit of the expected number to drop to a lower value. By "slowdown," we mean a significant increase in this time metric and its correlation with the number of orders mentioned.
</p>

<p>
  Examining the logs reveals that the slowdown in the "registering a new item in the cart" process occurs at the disk and database level [bottleneck 1], and the technical team, for example, resolves this delay with a caching approach that significantly increases the number of "orders per unit time." Interestingly, even after eliminating this bottleneck, the delay in the mentioned metric is still significant [above the expected level]. After further investigation, it becomes clear that this time, the "shopping cart loading" operation for the customer also has a considerable delay that was previously inaccessible [bottleneck 2], and after addressing it, another delay is observed in registering the financial transaction in the database after returning from the payment gateway [bottleneck 3].
</p>

<p>
  The image below provides an abstract representation [not entirely accurate] of this issue.
</p>

![image](https://github.com/MaysamPx/performance-obstacles-in-software-systems/assets/13215181/8c02e30a-90b4-49b8-9e7b-175c75157baa) 

<p>
The main reason for this type of problem is the cascaded hiding of bottlenecks behind the main operations. Until the number of orders is not high, the metrics of "basket loads per unit time" and "successful payment returns per unit time" will not experience high traffic, making it difficult to feel their delays separately.
</p>

> In practice, bottlenecks are not always apparent at the surface level and may have multiple layers and somewhat of a causal relationship, as depicted in the image above. Identifying bottlenecks B and C, which are behind bottleneck A, is not always straightforward, and their exposure is usually a consequence of removing the bottleneck at the previous n-1 level.


### Wrong assumptions in configurations
<p>
  One common type of bottleneck is the limited operational capacity of an intermediate component, such as a connection pool, between two subsystems or powerful components. This limitation may not be an inherent characteristic of the component and the pool size may not have been properly tuned with a trade-off.
</p>


![image](https://github.com/MaysamPx/performance-obstacles-in-software-systems/assets/13215181/420b7fbe-60b9-434e-8633-c53f3bd16eb1)


<p>
Generally, it's not uncommon to encounter situations where a high-performing application needs to connect to powerful resources, such as databases, email servers, or cloud storage, to handle heavy traffic loads. Unfortunately, even with proper optimization, the limitations of the connection pool can prevent the app from achieving the desired level of operational capacity, leading to performance bottlenecks. This highlights the importance of carefully managing resource allocation and pool sizes to avoid potential issues down the line.
</p>

[In this blog post, an example of this issue has been presented.](https://medium.com/@kyle_martin/mongodb-in-production-how-connection-pool-size-can-bottleneck-application-scale-439c6e5a8424)


  In this [blog](https://highscalability.com/big-list-of-20-common-bottlenecks), a good classification of 20 common types of bottlenecks in various layers, including processors, disks, main memory, databases, caching frameworks, networks, etc., has been provided. Each of them has its details and specific features.
