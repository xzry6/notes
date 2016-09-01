# Note: Spark
**Apache Spark** is a fast and general engine for large-scale data processing.
Spark was originally developed in *UC Berkely* and then was donated to Apache, who has maintained it since.
Apache Spark provides high-level APIs in *Scala*, *Java*, *Python* and *R*. It combines a stack of tools including SQL, Streaming, MLlib and GraphX/
Due to its advanced DAG engine and *Resilient Distributed Dataset(RDD)* data structure, Spark could run programs up to 100x faster than *Hadoop MapReduce* in memory, or 10x faster on disk.
Here is a learning note of Spark.

### Agenda
* [Structure](#structure)
* [Components](#components)
* [RDD(under construction!!)](#rdd)
* [Cheat Sheet(under construction!!)](#cheatsheet)


### Structure
Spark applications run as independent sets of processes on a cluster. See the following figure of Spark cluster components.

![sparkClusterComponents](http://spark.apache.org/docs/latest/img/cluster-overview.png)

Few tips of this architecture:
1. Each Spark application runs seperately;
2. Worker nodes are preferred to run close to driver program;
3. Driver is agnostic to cluster manager. Thus, cluster manager could be either of *Standalone*, *Apache Mesos* or *Hadoop Yarn*;
4. When Spark application creates a *SparkContext*, the new instance will connect to cluster manager. Cluster manager will then distribute workload to work nodes according to pre-set CPU and memory information;
5. Driver will split processes into different execution periods. Each period has exactly the same tasks;

### Components
Spark contains following main components:
- Spark Streaming: Provide extensible, high-capacity real-time stream processing. Support *Kafka*, *Flume*, *Twitter*, *ZeroMQ*, *Kinesis*, *HDFS*, *S3* and *TCP Socket* as data source;
- MLlib: A library of *Machine Learning* Tools, including *SVM*, *Logistic Regression*, *Linear Regression*, *Naive Bayes Classification*, *Decision Tree*, etc. in **classification**, *K-means*, *Gaussian Mixture*, *Power Iteration Clustering*, *Latent Dirichlet Allocation* in **clustering**, *Singular Value Decomposition* and *Principle Component Analysis* in **dimension reduction**;
- Spark SQL: Provide structural data storage, supporting *Hive*, *Avro*, *CSV*, *Parquet*, *JDBC*, *HBase*, etc. as data source;
- GraphX: Parallel processing API for graph computing;

### RDD
A **Resilient Distributed Dataset(RDD)** is the *basic abstraction* in Spark, representing an *immutable*, *partitioned* collection of elements that can be operated on in parellel. See [this paper](https://www.usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf) for more information.

#### Featues of RDD
An RDD is consisted of the following 5 parts:
- A *partition* is a basic unit of RDD. *Partition* could be initialized when an RDD is created. *BlockManager* is respddatasetatasetonsible for distributing *patitions*. *Partitions* are mapped as *Blocks*, which will be calculated by *Tasks* by then;
- A *compute* function;
- Dependencies of RDDs relating to current RDD;
- A *partitioner* calculating partitions;
- A *table* storing *preferred location* and *priority* of every partition;

#### Create an RDD
An RDD is a *read-only*, *partitioned* collection of records.
RDDs are only allowed to created from:
- Data in storage;
- Other RDDs;

#### Advantages of RDDs
The main difference of RDD and DSM(Distributed Shared Memory) is that RDDs can only be created by *coarse-grained transformations*, while DSMs allow transformations happening everywhere.
Benefits of RDD structure include:
- Allowing for more efficient fault tolerance;
- Mitigating slow notes by running backup copies of slow tasks;
- Degrading gracefully when there is not enough memory to store them;

#### Dependencies between RDDs
There are two kind of dependencies between RDDs and parent RDDs,
- *Narrow Dependency* means the parent RDD are used at most by one son RDD.
- *Wide Dependency* means the parent RDD can be depended by several son RDDs.

<img src="http://img.blog.csdn.net/20160806104849595" height="240">

#### Fault Tolerence
1. Check Cache;
2. Check checkpoint;
3. Lineage compute chain;

### Cheat Sheet
Here are some example code of anything everything in *Scala*.
```scala
// DataFrame
dataset.select(c).show() // show selected column
dataset.printSchema() // show schema

// String interpolation
val name = "James"
println(s"Hello, $name")  // Hello, James
```
