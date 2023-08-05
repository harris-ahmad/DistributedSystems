# Reliable, Scalabale and Maintainable Applications

- [Reliable, Scalabale and Maintainable Applications](#reliable-scalabale-and-maintainable-applications)
  - [Thinking About Data Systems](#thinking-about-data-systems)
    - [Reliabilty](#reliabilty)
    - [Scalability](#scalability)


## Thinking About Data Systems
Data is everywhere. Many modern and state-of-the-art distributed systems are more data-intensive than being compute-intensive implying that raw CPU power
is rarely considered to be a bottleneck anymore. Some examples of common data-intensive applications include:

- **Databases:** Store data to be used by systems querying them.
- **Caches:** Remember the result of an expensive read-intensive operation.
- **Search Indexes:** Allow users to search data by a keyword or filter it in various ways. A practical use-case of search indexes are vector databases.

In the following chapters, I shall be summarizing different design decisions that need to be considered when working on a data-intensive application. 

The boundaries between systems like databases or message queues are becoming blurred. For example, there are datastores that are also being used as
message queues (Redis) and there are message queues that are also being used as databases (Apache Kafke). Technically, both are responsible for storing data temporarily, but they have very different access patterns which means different performance characteristics and thus very different implementations.

When you're designing a data-intensive system, several questions may arise:
- How do we ensure consistency, i.e., data remains correct even when things go horribly wrong?
- How do you continue to provide consistenly good performance to clients even when some parts of the system are degraded?
- How do you scale the system to handle the increasing workload?

In these notes, I'll dig deeper into three broader areas of concern:
- **Reliability:** The system should continue to work correctly even in the face of adversity (failure or power outages, etc.)
- **Scalability:** As the system grows (more computers or systems are added), there should be reasonable ways to balance the workload.
- **Maintainability:** Task of maintaining the current system and adapting the system to deal with new use-cases.

### Reliabilty

Reliability, in plain english, means that a system should continue to function even when something has gone wrong or one of the remote servers in the system has failed. For a distributed system to be reliable, it must follow the typical expectations:
- The system performs the functions that the user expected.
- It should be able to tolerate any mmistakes made by the user and not crash unexpectedly.
- Its performance is good enough to handle the required number of users by balancing the load efficiently across servers involved.

It's very important to note that *faults* are not the same as *failures*. A fault is when a component or two within a system deviate from their spec, while
a failure is when the system as a whole fails to provide the required functionality to the user. In these notes, I've covered several techniques for building reliable distributed systems from unreliable parts.

Many critical bugs are actually due to poor error handling; by deliberately inducing faults in the system, we can test the system exhausitvely so it can handle most errors. This can increase a system designer's confidence that faults will be handled and dealt off nicely when any errors occur.

**Hardware Faults**

Hard disk crashes, RAM becomes faulty, or damage of under-sea cables all constitute *Hardware Faults*. The quickest way of handling hardware faults is replication: add redundancy to the individual hardware components in order to reduce the faulure rate of the system. 
The downtime in case of a failure is not catastrophic in most applications if in a working system, the failed node is replaced fairly quickly by another working node. If we want to upgrade the software of a single-server system, we can do so by bringing the system to a halt for a very short period of time, whereas for a multi-node system, nodes must be matched one node at a time without the downtime of the entire system (also known as *rolling upgrade*).

We typically think of hardware faults as independent failure events since the failure of one disk would not cause the entire system to go down. It is highly unlikely that multiple nodes (for example 4 out of 5 nodes in a 5-node system) fail at the same time.

**Software Errors**

These are systematic errors within systems which are harder to anticipate. Some example include a software bug that causes every instance of an application
server to crash, a runaway process that uses up the shared resources (RAM, CPU, etc), or a service that a system depends on that slows down or becomes unresponsive. If a system is expected to provide some form of guarantee, it can constantly check itself while it is running and raise an alert if a discrepency is found.

**Human Errors**

Humans are known to be unreliable. Considering this fact, how can we make our systems reliable inspite of unreliable humans? Some factors that we could consider are as follows:

- Create easy-to-use and properly configured APIs for interaction with distributed systems so people don't have to work around the poor abstractions while ending up negating their benefits.
- Provide *sandbox* environments for people to explore the systems freely without harming the real-world users.
- Test thoroughly at whole levels - from unit tests to the whole-system integration tests.
- Detailed and clear moniorting of performance metrics and error rates.

### Scalability
*Scalibility* is a term that we use to describe the system's ability to cope with increased load - be it more users or larger volumes of data.

**Describing Load**

