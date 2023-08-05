# Reliable, Scalabale and Maintainable Applications

- [Reliable, Scalabale and Maintainable Applications](#reliable-scalabale-and-maintainable-applications)
  - [Thinking About Data Systems](#thinking-about-data-systems)
    - [Reliabilty](#reliabilty)


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