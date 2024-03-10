<h3> Thread blocking in synchronous processing of incoming requests from a web server </h3>
<p>
  Imagine a system that has powerful vertically scalable infrastructure, but can only handle a limited number of threads from the operating system (e.g. 2500 with equal priority, with no specific restriction on their nature or definition). On the other side of the system, a powerful web server like Nginx is set up to simply route requests to the designated endpoint, and internal requests in this system are handled completely synchronously with its subsystems and kept busy until the end of the request thread.
</p>
