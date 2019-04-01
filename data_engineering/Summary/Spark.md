# Spark

Spark is an open source framework that provides methods to process data in parallel that are generalizable. It is ually working with distributed storage system.Spark currently supports three kinds of cluster managers: Standalone Cluster Manager, Apache Mesos, and Hadoop YARN.

## Component

**Spark Core**:

Spark Core is the main data processing framework in the Spark ecosystem. It has API in various languages such as Python.

**Resilient Distributed Datasets**:

Spark is built around data abstraction RDD, which is a representation of lazily evaluated, statically typed, distributed collections. RDD has many predefined transformation such as `map`,`join`,and `reduce`.

**Spark SQL**:

Spark SQL has quite different query optimizer than Spark Core. Spark SQL defines an interface for a semi-structured data type - *Dataframes* and *Datasets* for after Spark 1.6 with typed version.

**Spark MLlib, Spark ML**:

Spark MLlib is writtend almost entirely on the Spark API.MLlib is a package of ML and stats algorithms witten with Spark. Spark ML is a higher-level API, easier to build ML pipeline. Spark MLlib is built upon RDDs with functions from Spark Core, while ML is built upon Spark SQL Dataframes. Eventually, Spark will deprecate MLlib to ML.

**Spark Streaming**:

Spark Streaming uses the scheduling of the Spark Core for streaming analytics on minibatches of data. Spark Streaming has a number of unique considerations, such as the *window sizes* used for batches.

**GraphX**:

For graph computation.

## Parallel Computing: RDD

**driver - executor style**:

Master node will have programs written by the users while Spark represents large datasets as RDDs - immutable, distributed collections of objects - in the executors.

The objects that comprise RDDs are called *partitions* and may be (but do not need to be) computed on different nodes of a distributed system. Spark cluster manager handle the starting and distributing the Spark executors across a distributed sys according to config. The Spark execution engine itself distributes data across the executors for a computation.

**Immutable**: transofrming an RDD returns a new RDD

**In-memory**: keep an RDD loaded in memory on executor nodes through life time.

**Lazy Evaluation**:

RDDs is completely lazy. Spark does not begin computing the partitions until an action is called. An *action* is a Spark operation that returns something other than an RDD, triggering evaluation of partitions and possibly returning some output to a non-Spark system (outside of the Spark executors). Actions trigger the *scheduler*, which builds a *directed acyclic graph* (called the DAG), based on the dependencies between RDD transformations

Lazy evaluation allows Spark to combine operations that don’t require communication with the driver (called transformations with one-to-one dependencies) to avoid doing multiple passes through the data. It is also easier to implement: we can chain together operations with narrow dependencies and let the Spark evaluation engine do the work of consolidating them.

**Fault Tolerance**:

Spark is fault-tolerant, meaning Spark will not fail, lose data, or return inaccurate results in the event of a host machine or network failure. It is achieved because you can recalculate.

**Debugging**:

Debugging might be hard as it is a one-stop process. 

**In-Memory Persistence**:

* In memory as deserialized Java objects: This form of in-memory storage is the fastest, since it reduces serialization time; however, it may not be the most memory efficient, since it requires the data to be stored as objects.
* As serialized data: This approach may be slower, since serialized data is more CPU-intensive to read than deserialized data; however, it is often more memory efficient, since it allows the user to choose a more efficient representation.
* On disk: This strategy is obviously slower for repeated computations, but can be more fault-tolerant for long sequences of transformations, and may be the only feasible option for enormous computations.

## Streaming DataFrame/Dataset


Source:

* <https://learning.oreilly.com/library/view/high-performance-spark/9781491943199/ch02.html>
* <https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html>
* <https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html>