# Reliable, Scalabale and Maintainable Applications

- [Reliable, Scalabale and Maintainable Applications](#reliable-scalabale-and-maintainable-applications)
  - [Thinking About Data Systems](#thinking-about-data-systems)


## Thinking About Data Systems
Data is everywhere. Many modern and state-of-the-art distributed systems are more data-intensive than being compute-intensive implying that raw CPU power
is rarely considered to be a bottleneck anymore. Some examples of common data-intensive applications include:

- **Databases:** Store data to be used by systems querying them.
- **Caches:** Remember the result of an expensive read-intensive operation.
- **Search Indexes:** Allow users to search data by a keyword or filter it in various ways. A practical use-case of search indexes are vector databases.

In the following chapters, I shall be summarizing different design decisions that need to be considered when working on a data-intensive application. 

The boundaries between systems like databases or message queues are becoming blurred. For example, there are datastores that are also being used as
message queues (Redis) and there are message queues that are also being used as databases (Apache Kafke). Technically, both are responsible for storing data temporarily, but they have very different access patterns which means different performance characteristics and thus very different implementations.