# performance-obstacles-in-software-systems
Limitations and obstacles that cause slowdowns in software systems

<p> Efficiency is a critical consideration when dealing with resource limitations in software systems. These constraints, which are typically present in most systems, including hardware and software aspects such as disk, processor, and main memory, can restrict the performance of the overall system or a critical subsystem moment by moment or continuously. In the context of software systems, components that impede performance in this way are commonly referred to as bottlenecks.
For instance, a large accounting system may feature a powerful processing core for calculating complex payroll details, taxes, and insurance rules, but its performance may be limited by a relatively simple reporting subsystem that is utilized by users. This may result in low utilization of underlying resources and reduced efficiency of the system. In the following sections, we will discuss several common types of bottlenecks in software systems. </p>

<h3> Transferring existing delays in external service APIs to the system internally </h3>
<p> 
One of the potential bottlenecks that designers face within a system, especially a monolithic one, is the reliance on external APIs to hold and maintain resources (such as threads). The assumption underlying this design is that the external system is always in a stable state and guarantees a response time for every request. However, this assumption can often be violated in practice, and the external system can suffer from delay, downtime, and failure. At that moment, the abstraction that has been created makes it difficult for the current system designer to understand the details of the delay in the external system. Although this abstraction is created for simplicity and to provide a unified picture, it hides the details of the problem by drawing a veil of ambiguity. Therefore, the subsystem or module that is responsible for the primary task of connecting to the external system's APIs (even synchronously) can potentially become a bottleneck.
</p>

<p>
For example, consider a system that receives the prices of various cryptocurrencies from different external systems and instantaneously draws and updates a graphical chart on a page. In this system, if the asynchronous and data-pushing approaches are not used, the chart drawing module must wait for the maximum possible delay in the set of external APIs to draw the chart.
</p>

<p>
One of the most straightforward solutions in such situations is to switch from synchronous to asynchronous approaches with suitable queuing and persistence using Callback methods or other popular techniques. In this situation, each request-carrying thread becomes free and returns resources to the system upon asynchronous endpoint invocation, while the system experiences relatively stable and low oscillation.
</p>

<h3>Memory leak, turning memory latency into an actual bottleneck</h3>
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

