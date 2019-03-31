# Hadoop

## Why Parallel

Nowadays business has increasing volume of the data, but the Read and Write speed has not yet come along with the increasing rate of data expansion. To have a better understanding of the huge amount of data that we are collecting, people think of smart work-arounds to R/W data. Now, we R/W in parallel to or from multiple disks. However, working in parallel has several problems:

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

Interactive SQL:"always on" daemon (Impala) or container resue (Hive on Tez)
Iterative processing: Spark
Stream processing: Storm, Spark Streaming, Samza
Search: Solr search

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

## HDFS

Design
Concept
Command Line
Java Interface
Data Flow

## YARN

Yet Another Resource Negotiator 

## Batch

## Streaming

Source:

* <https://learning.oreilly.com/library/view/hadoop-the-definitive/9781491901687/>
* <https://learning.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/ch10.html#ch_batch>
* <https://learning.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/ch11.html#ch_stream>
* <https://learning.oreilly.com/library/view/hadoop-the-definitive/9781491901687/ch04.html>
* <https://www.tutorialspoint.com/hadoop/hadoop_mapreduce.htm>
* <https://thirdeyedata.io/hadoop-mapreduce/>
