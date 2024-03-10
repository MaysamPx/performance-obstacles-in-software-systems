<h3> Limitations in key system operations can result in hidden operational bottlenecks </h3>

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
