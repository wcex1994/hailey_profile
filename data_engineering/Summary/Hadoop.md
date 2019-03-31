# Hadoop

## Why Parallel

Nowadays business has increasing volume of the data, but the Read and Write speed has not yet come along with the increasing rate of data expansion. To have a better understanding of the huge amount of data that we are collecting, people think of smart work-arounds to R/W data. Now, we R/W in parallel to or from multiple disks. File systems that manage the storage across a network of machines are called distributed filesystems. However, working in parallel has several problems:

* Hardware failure:

    If one of pieces of the hardware fail, then you might forever lose that part of the data. The solution is to have data replication (how RAID works). Meanwhile, Hadoop Distributed Filesystem (HDFS) has another approach.

* Combine from multiple sources for analysis:

    Now with many disks as the origin, we might need to combine data from one disk with the rest, which can be challenging. MapReduce helps us to abstract the problem of disk R/W, and focus on computation over sets of keys and values. MapReduce is a batch query processor while able to run ad hoc queries against the whole dataset within a reasonable time. However, it is not suitable for interactive analysis.

Hadoop has evoleved beyond batch processing, as referring to a larger ecosystem of projects under the umrella of infrastructure for distributed computing and large-scale data processing.

**HBase**:

* key-value store using HDFS for underlying storage
* online R/W access of individual rows and batch operation

**YARN**:

* cluster resource management system
* allow any distributed program (e.g. MapReduce) to run on data in a Hadoop cluster

Now Hadoop can act like many things:

* Interactive SQL: "always on" daemon (Impala) or container resue (Hive on Tez)
* Iterative processing: Spark
* Stream processing: Storm, Spark Streaming, Samza
* Search: Solr search

## Comparison

**Relational DB**:

* Seeking time is improving much more slowly than transfer time. When R/W large portion of the dataset, it takes much longer than just streaming through
* Updating large portion of DB can be less efficient with B-Tree (Sort/Merge).
* MapReduce is schema-on-read hile RDBMS is schema-on-write.
* The difference is blurring

**Grid Computing**:

* HPC works well for compute intensive job but has a problem when nodes need to access larger data volumes since network bandwidth is the bottlenect and compute nodes become idle. This is where Hadoop is really good at.
* Hadoop co-locates the data with the compute nodes, so data access is fast as it is local. **data locality**
* MPI has great control but it requries complicated handlement of data flow (in low-level C routine), while Hadoop operates at a higher level in terms of data model.

## MapReduce

MapReduce is a programming model for data processing. You can run it in multiple languages: Java, Ruby and Python. It is inherently parallel. It works by breaking into two phases: map and reduce. Each phase has key-value pairs as input and output, and the types can be chosen by the programmer. The programmer specifies two functions:

Map takes a set of data and converts it into another set of data, where individual elements are broken down into tuples (key/value pairs). Secondly, reduce task, which takes the output from a map as an input and combines those data tuples into a smaller set of tuples. As the sequence of the name MapReduce implies, the reduce task is always performed after the map job.

![alt text](https://4zy7s42hws72i51dv3513vnm-wpengine.netdna-ssl.com/wp-content/uploads/2018/02/Traditional-Way-MapReduce-Tutorial-Edureka-4.png "MapReduce Example")

**Map Function**:

Map takes a set of data and converts it into another set of data, where individual elements are broken down into tuples (key/value pairs). The output from the map function is processed by the MapReduce framework before sending to Reduce with *sort* and *group by key*.

**Reduce Function**:

Reduce task takes the output from a map as an input and combines those data tuples into a smaller set of tuples. As the sequence of the name MapReduce implies, the reduce task is always performed after the map job.

### Scale Out

In distributed filesystem, YARN helps to move MapReduce computation to each machine hosting that part of the data.

A MapReduce *job* is a unit of work to perform with input data, MapReduce program and config info. Hadoop runs job by splitting into *tasks*: map task or reduce task. 

### Map in Hadoop

YARN schedules which node runs which task. If a task fails, YARN will reschedule to run it on another node. Haddop divides input to a job into fixed-size pieces as *input splites*. Hadoop will run one map task for each split, which runs the map function for each *record* in the split. Because of the data locality optimization, Hadoop run the map task on a node where the input data resides in HDFS. If the node is busy, YARN will llok for a free node on the same rack, it again not possible, will look for an off-rack node. The opitmal split size is the same as the block size. The result of the map task will be stored in local disks instead of in HDFS because it is a middle product.

### Reduce in Hadoop

The reduce doesn't have data locality.You can speicfy how many reduce tasks you want. When we have multiple reducer, map tasks partition their output. Each create one partition for each reduce task. The records for any given key are all in a single partition. The partitioning can be controlled by user-defined partitioning functions (bucket key with hash function).It is call "the shuffle" for data flow between map and reduce as each reduce task is fed by many map tasks. You can also have zero reduce task, where only off-node data transfer is when map writes to HDFS.

### Combine functions in Hadoop

You can define a combiner function to run on the map output which will be the input for reduce function. Calling the combiner function zero, one, or many times should produce the same output from the reducer.This can cut down the data shuffle.

You can also have Hadoop Streaming.

## HDFS (Hadoop Distributed Filesystem)

### Design

* Very large file
* Streaming data access (write-once, read-many-time pattern)
* Commodity hardware
* Low-latency data access (high throughput of data)
* Lots of small files (150 bytes)
* Multiple writer, arbitrary file modifications

### Concept

**Blocks**:

It is the min amount of data that disk can R/W. Filesystem blocks is usually few kilobytes in size, where disk blocks are normally 512 bytes. HDFS has 128MB blocks.

Benefit:

* a file can be larger than any single disk in the network
* make the unit of abstraction a block rather than a file simplifies the storage subsystem
* fault tolerance and availability

```bash
% hdfs fsck / -files -blocks
```

**Namenodes and Datanodes**:

It has a mster-worker pattern where namenode is the master and the datanodes as workers. The namenodes manages the filesystem namespace (filesys tree and metadata), which stores in form of two files: namespace image and the edit log. The namenodes knows the location of each datanodes, and grab it when the system starts. Datanodes store and retrive bloacks when they are told to, and report back to the namenode periodically with lists of blocks that they are storing. It is important to keep the namenode safe. Usually, we configure a write to local disk as well as a remote NFS mount. You can also have a secodnary namenodes.

**Block Caching**:

frequently-used block can be cached in one datanode's memory on per-file basis. Job scheduler can use the cache file to have better read performance. Namenode needs to have the cache directive to a cache pool. Cache pools are an administrative grouping for managing cache permissions and resource usage.

**HDFS Federation**:

Namenode keeps reference to every file and block in the filesystem in memory, and memory can be a bottleneck, which is why HDFS federation allows a cluster to scale with mroe namenodes where each manage a portion of the filesystem namespace. Each namenode manages a namespace volume (metadata for the namespace) and a block pool containing all the blocks for the files in the namespace. Namespace volumes are independent of each other. See `ViewFileSystem` and the `viewfs:// URIs`.

**HDFS High Availability**:

The namenode is still a single point of failure (SPOF), which takes a long time to recover. Hadoop 2 uses pair of namenodes to support HA. Transition between active and standby is maanged by failover controller, which by default is using ZooKeeper to ensure only one namenode is active. It can be a graceful failover by manually triggerd. During ungraceful failover, HA implementation uses fencing to ensure the previous active namenode is not doing anything damage or corruption.

## Command Line

There are two properties set in pseudodistributed config. 

* We set `fs.defaultFS` to hdfs://localhost/, which is used to set a default filesystem for Hadoop. Filesystems are specified by a URI, and we use hdfs URI to config Hadoop to use HDFS by default. The HDFS daemons will use this property to determine the host and port for namenode. Here we use localhost on default HDFS port 8020.

* We se `dfs.replication` to 1 so HDFS doesn't replicate filesystem blocks by the default factor of three. It is because we are running it on a single datanode.

HDFS is just one implementation of filesystem for Hadoop.

```bash
# help
fs -help

# copy from local filesystem to HDFS
hadoop fs -copyFromLocal input/docs/quangle.txt /user/tom/quangle.txt

# copy to local
hadoop fs -copyToLocal quangle.txt quangle.copy.txt

# create a folder
hadoop fs -mkdir books
hadoop fs -ls .

#list files in root directory of the local filesystem

hadoop fs -ls file:///

# parallel copying 
hadoop distcp file1 file2
hadoop distcp dir1 dir2
hadoop distcp -update dir1 dir2
hadoop distcp -update -delete -p hdfs://namenode1/foo hdfs://namenode2/foo
hadoop distcp webhdfs://namenode1:50070/foo webhdfs://namenode2:50070/foo
```

## Data Flow

![alt text](https://www.researchgate.net/publication/275068128/figure/fig7/AS:324975367081993@1454491573612/Instruction-invoking-path-of-HDFS-read.png "Hadoop Read Data Flow")

![alt text](https://www.researchgate.net/publication/275068128/figure/fig3/AS:322163497291784@1453821171340/Instruction-invoking-path-of-HDFS-write.png "Hadoop Write Data Flow")

## YARN

Yet Another Resource Negotiator is Hadoop's cluster resource management system. In MapReduce 1, jobtracker takes care of both job scheduling and task progress monitoring, while YARN splites them up. At the same time, jobtracker needs to store job history. YARN takes the load off to timeline server. It provides APIs for working with clustering resources. Users typically uses higher-level API built upon the YARN api to manage resources.

![alt text](https://d1jnx9ba8s6j9r.cloudfront.net/blog/content/ver.1553858763/uploads/2018/06/Hadoop-v1.0-vs-Hadoop-v2.0.png "YARN")

YARN runs core services vi two long-running daemon: *resource manager* one per cluster and *node managers* running on all nodes in the cluster to launch and monitor containers. A container executes app-specific process with a constrained set of resources (memory etc).

![alt text](https://www.oreilly.com/library/view/hadoop-the-definitive/9781491901687/images/hddg_0402.png "YARN flow")

YARN allows an app to specify locality constraint for the containers it is requesting. YARN app can make resource requests any time while it is running. Or it can be dynamic approach.

### Lifespan

* one app per user job (MapReduce)
* one app per workflow or user session of jobs
* long-running app shared by different users

### YARN App

* Directed acyclic graph (DAG): Spark, Tez
* Stream processing: Spark, Samza, Storm
* Different users can run different versions of the same app: Apache Slider, Apache Twill.

### Design of YARN

* Scalability
* Availability
* Utilization
* Multitenancy

### YARN Scheduler

* FIFO
* Capacity
* Fair Scheduler

![alt text](https://learning.oreilly.com/library/view/hadoop-the-definitive/9781491901687/images/hddg_0403.png "YARN Scheduler")

## Source

* <https://learning.oreilly.com/library/view/hadoop-the-definitive/9781491901687/>
* <https://learning.oreilly.com/library/view/hadoop-the-definitive/9781491901687/ch04.html>
* <https://www.tutorialspoint.com/hadoop/hadoop_mapreduce.htm>
* <https://thirdeyedata.io/hadoop-mapreduce/>
* <https://www.researchgate.net/figure/Instruction-invoking-path-of-HDFS-read_fig7_275068128>
* <https://www.edureka.co/blog/hadoop-yarn-tutorial/>