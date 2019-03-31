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

Map
    Scale out
Reduce
    Combine function

Java MapReduce Code Example

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
