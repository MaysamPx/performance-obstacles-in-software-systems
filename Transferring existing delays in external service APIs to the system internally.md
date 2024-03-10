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