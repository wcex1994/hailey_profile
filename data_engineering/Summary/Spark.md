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

**Transformations Versus Actions**:

There are two types of functions defined on RDDs: actions and transformations. Actions are functions that return something that is not an RDD, including a side effect, and transformations are functions that return another RDD.

Each Spark program **must** contain an action, since actions either bring information back to the driver or write the data to stable storage.

Transformation has two types:

* narrow dependencies:

    Conceptually, narrow transformations are those in which each partition in the child RDD has simple, finite dependencies on partitions in the parent RDD.

![alt text](https://learning.oreilly.com/library/view/high-performance-spark/9781491943199/assets/hpsp_0202.png "narrow dependencies")

* wide dependencies:

    transformations with wide dependencies cannot be executed on arbitrary rows and instead require the data to be partitioned in a particular way

![alt text](https://learning.oreilly.com/library/view/high-performance-spark/9781491943199/assets/hpsp_0203.png "wide dependencies")

**Immutable**: transofrming an RDD returns a new RDD

RDD can be created by:

* by transforming an existing RDD
* from a SparkContext, which is the API’s gateway to Spark for your application
* converting a DataFrame or Dataset (created from the SparkSession).

Three properties required for RDD:

* `partitions()`: list of partition objects that make up the RDD
* `iterator(p, parentIters)`: a function for computing an iterator of each partition
* `dependencies()`:and a list of dependencies on other RDDs

Optional properties:

* `partitioner()`
* `preferredLocations(p)`

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

## Job Scheduling

A Spark application consists of a driver process, which is where the high-level Spark logic is written, and a series of executor processes that can be scattered across the nodes of a cluster. It will send instruction to the executors. One cluster can run several app concurrently. The applications are scheduled by the cluster manager and correspond to one SparkContext.

**Allocation**:

Static allocation or dynamic allocation

**Application**:

A Spark application begins when a SparkContext is started.

Each executor is its own Java Virtual Machine (JVM), and an executor cannot span multiple nodes although one node may contain several executors.The SparkContext determines how many resources are allotted to each executor. When a Spark job is launched, each executor has slots for running the tasks needed to compute an RDD.

![alt text](https://learning.oreilly.com/library/view/high-performance-spark/9781491943199/assets/hpsp_0205.png "Anatomy")

![alt text](https://learning.oreilly.com/library/view/high-performance-spark/9781491943199/assets/hpsp_0206.png "Anatomy")

## Streaming DataFrame/Dataset


Source:

* <https://learning.oreilly.com/library/view/high-performance-spark/9781491943199/ch02.html>
* <https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html>
* <https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html>