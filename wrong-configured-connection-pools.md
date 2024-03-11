<h3> The incorrect configuration and low efficiency of the connection pool </h3>
<p>
  One common type of bottleneck is the limited operational capacity of an intermediate component, such as a connection pool, between two subsystems or powerful components. This limitation may not be an inherent characteristic of the component and the pool size may not have been properly tuned with a trade-off.
</p>


![image](https://github.com/MaysamPx/performance-obstacles-in-software-systems/assets/13215181/420b7fbe-60b9-434e-8633-c53f3bd16eb1)


<p>
Generally, it's not uncommon to encounter situations where a high-performing application needs to connect to powerful resources, such as databases, email servers, or cloud storage, to handle heavy traffic loads. Unfortunately, even with proper optimization, the limitations of the connection pool can prevent the app from achieving the desired level of operational capacity, leading to performance bottlenecks. This highlights the importance of carefully managing resource allocation and pool sizes to avoid potential issues down the line.
</p>

[In this blog post, an example of this issue has been presented.](https://medium.com/@kyle_martin/mongodb-in-production-how-connection-pool-size-can-bottleneck-application-scale-439c6e5a8424)

<p>
  In this [blog](https://highscalability.com/big-list-of-20-common-bottlenecks), a good classification of 20 common types of bottlenecks in various layers, including processors, disks, main memory, databases, caching frameworks, networks, etc., has been provided. Each of them has its details and specific features.
</p>
