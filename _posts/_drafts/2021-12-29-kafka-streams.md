---
layout: post
title:  Apache Kafka - Part II - Kafka Streams
categories: [Apache Kafka,Data,Processing,Streaming,Kafka Streams]
excerpt: Kafka Streams technology enables to implement Microservices Architecture (MSA) compliant stream processing/event streaming applications using Apache Kafka as a data backbone with all of the scalability and availability features of Kafka cluster. 
---
A stream is a data sequence that has no bound, no ending and keeps flowing continuously. Each record in a stream comprises of key and value. The Kafka Streams API manifests itself as a way to transform and enrich (consume and produce) this data flow in record level (no micro-batching) with a very low latency (in milliseconds) via taking the processing effort out of Kafka cluster. The API brings methods for stateless processing such as `filter` and `map` with methods for stateful processing such as `join` and `aggregate` as well. There are also out-of-box support for windowing operations with analyzing set of records in a definite time frame/boundary.

One of the most charming capabilities of Kafka Streams API is a real-time stream processing application can be implemented as a cross-platform, easy-to-deploy microservice using standard Java language without need of a disparate processing cluster. It leads to having an elastic, scalable and fault-tolerant event stream processing stack in application layer. Being backed by a state-of-art distributed system like Kafka with embracing Microservices Architecture (MSA) lends itself to the main tenet of a stream processing application. The computation of streaming needs to be done on a multi node system for the sake of performance and parallelization (partition, replication, grouping, etc.). To sum up Kafka Streams provides a stream processing API that has:
- Local (on an embedded key-value store) and fault-tolerant state management
- Rich set of operators for transforming streams of data via enrichment, transformation and processing
- Advanced representations of streams such as aggregations and tables
- Sophisticated handling of time such as windowing

{% include image.html url="/images/apache-kafka/kafka_streams.png" caption="Kafka Streams App" %}

# Kafka Streams as a Data Processing System
A data processing system should cover three main non-functional requirements:
- Scalability
- Reliability
- Maintainability

## Scalability
A data processing system should perform well under load. The unit of work in Kafka Streams app is the topic partition that it subscribed to. The topics can be expanded by adding more partitions. Increasing number of partitions on source topics enable to distribute load across these partitions and across the apps that consume from them. Kafka Streams API also supports consumer groups. An event processing computation on topic can be distributed across multiple, cooperating instances of app in a group.

## Reliability
A data processing system should behave consistent in case of error with no crash in any noticeable way (no outage) and with no data loss or corruption. If any of Kafka Streams app in a group crashes, Kafka automatically re-distributes the load to other healthy instances. The consumer group also makes parallel processing and load balancing possible for a topic.

## Maintainability
On-going maintenance, bug fixing, debugging and keeping system operational should be straightforward for a data processing system. Kafka Streams API is a Java based library that has huge community, myriad of best practices, and debugging, monitoring and analyzing easiness. Well known data processing systems like Apache Spark and Apache Flink need a dedicated processing cluster for submitting and running stream processing tasks. They also use micro-batching to achieve greater throughput that turns to be latency in processing completion. On the contrary, Kafka Streams apps externalize processing from Kafka cluster and maintains high throughput with low latency by parallelizing processing via splitting data across many partitions for a topic.

Relations or tables are first-class citizens in Kafka Streams API, where each has an independent identity. Relations can be transformed into other relations:
```java
// Compute the number of clicks per region, e.g. "europe".
//
// The resulting KTable is continuously being updated as new data records are arriving in the
// input KStream `userClicksStream` and input KTable `userRegionsTable`.
final KTable<String, Long> clicksPerRegion = userClicksStream
    // Join the stream against the table.
    //
    // Null values possible: In general, null values are possible for region (i.e. the value of
    // the KTable we are joining against) so we must guard against that (here: by setting the
    // fallback region "UNKNOWN").  In this specific example this is not really needed because
    // we know, based on the test setup, that all users have appropriate region entries at the
    // time we perform the join.
    //
    // Also, we need to return a tuple of (region, clicks) for each user.  But because Java does
    // not support tuples out-of-the-box, we must use a custom class `RegionWithClicks` to
    // achieve the same effect.
    .leftJoin(userRegionsTable, (clicks, region) -> new RegionWithClicks(region == null ? "UNKNOWN" : region, clicks))
    // Change the stream from <user> -> <region, clicks> to <region> -> <clicks>
    .map((user, regionWithClicks) -> new KeyValue<>(regionWithClicks.getRegion(), regionWithClicks.getClicks()))
    // Compute the total per region by summing the individual click counts per region.
    .groupByKey(Grouped.with(stringSerde, longSerde))
    .reduce(Long::sum);
```

## Data Processors in Kafka Streams API