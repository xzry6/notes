# Note: Spark
**Apache Spark** is a fast and general engine for large-scale data processing.
Spark was originally developed in *UC Berkely* and then was donated to Apache, who has maintained it since.
Apache Spark provides high-level APIs in *Scala*, *Java*, *Python* and *R*. It combines a stack of tools including SQL, Streaming, MLlib and GraphX/
Due to its advanced DAG engine and *Resilient Distributed Dataset(RDD)* data structure, Spark could run programs up to 100x faster than *Hadoop MapReduce* in memory, or 10x faster on disk.
Here is a learning note of Spark.

### Agenda
* [Structure](#structure)
* [Components](#components)
* [RDD](#rdd)


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
