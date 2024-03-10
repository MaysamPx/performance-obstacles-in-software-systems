<h3> Thread blocking in synchronous processing of incoming requests from a web server </h3>
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

<p>
  In this situation, two bottlenecks have occurred, correlated operationally with each other. It is somewhat one of the types of multi-level bottlenecks that need to be considered in system design and management. In general, it is important to have a mechanism in place to manage these types of bottlenecks and prevent them from causing major disruptions in the system.
</p>
