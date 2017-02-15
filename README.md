# assignment-2.2

Q1.  HDFS :

Hadoop File System was developed using distributed file system design. It is run on commodity hardware. Unlike other distributed systems, HDFS is highly faulttolerant and designed using low-cost hardware.

HDFS holds very large amount of data and provides easier access. To store such huge data, the files are stored across multiple machines. These files are stored in redundant fashion to rescue the system from possible data losses in case of failure. HDFS also makes applications available to parallel processing.

Features of HDFS
It is suitable for the distributed storage and processing.
Hadoop provides a command interface to interact with HDFS.
The built-in servers of namenode and datanode help users to easily check the status of cluster.
Streaming access to file system data.
HDFS provides file permissions and authentication.

Goals of HDFS

1.Fault detection and recovery : Since HDFS includes a large number of commodity hardware, failure of components is frequent. Therefore HDFS should have mechanisms for quick and automatic fault detection and recovery.

2.Huge datasets : HDFS should have hundreds of nodes per cluster to manage the applications having huge datasets.

3.Hardware at data : A requested task can be done efficiently, when the computation takes place near the data. Especially where huge datasets are involved, it reduces the network traffic and increases the throughput.



Q2 Hadoop cluster : 

Hadoop Cluster: 
Normally any set of loosely connected or tightly connected computers that work together as a single system is called Cluster. In simple words, a computer cluster used for Hadoop is called Hadoop Cluster. 

Hadoop cluster is a special type of computational cluster designed for storing and analyzing vast amount of unstructured data in a distributed computing environment. These clusters run on low cost commodity computers. 



Hadoop Cluster - Architecture and Components 

In this case Hadoop Cluster would consists of
110 different racks
Each rack would have around 40 slave machine
At the top of each rack there is a rack switch
Each slave machine(rack server in a rack) has cables coming out it from both the ends
Cables are connected to rack switch at the top which means that top rack switch will have around 80 ports
There are global 8 core switches
The rack switch has uplinks connected to core switches and hence connecting all other racks with uniform bandwidth, forming the Cluster
In the cluster, you have few machines to act as Name node and as JobTracker. They are referred as Masters. These masters have different configuration favoring more DRAM and CPU and less local storage.
The majority of the machines acts as DataNode and Task Trackers and are referred as Slaves. These slave nodes have lots of local disk storage and moderate amounts of CPU and DRAM

Core Components of Hadoop Cluster:

Hadoop cluster has 3 components:
Client
Master
Slave
The role of each components are shown in the below image. 

Masters:

The Masters consists of 3 components NameNode, Secondary Node name and JobTracker.

NameNode:
NameNode does NOT store the files but only the file's metadata. In later section we will see it is actually the DataNode which stores the files. 

NameNode oversees the health of DataNode and coordinates access to the data stored in DataNode. 
Name node keeps track of all the file system related information such as to
Which section of file is saved in which part of the cluster
Last access time for the files
User permissions like which user have access to the file
JobTracker:
JobTracker coordinates the parallel processing of data using MapReduce. 

Secondary Name Node:
Don't get confused with the name "Secondary". Secondary Node is NOT the backup or high availability node for Name node. 

The job of Secondary Node is to contact NameNode in a periodic manner after certain time interval(by default 1 hour). 
NameNode which keeps all filesystem metadata in RAM has no capability to process that metadata on to disk. So if NameNode crashes, you lose everything in RAM itself and you don't have any backup of filesystem. What secondary node does is it contacts NameNode in an hour and pulls copy of metadata information out of NameNode. It shuffle and merge this information into clean file folder and sent to back again to NameNode, while keeping a copy for itself. Hence Secondary Node is not the backup rather it does job of housekeeping. 
In case of NameNode failure, saved metadata can rebuild it easily. 

Slaves:

Slave nodes are the majority of machines in Hadoop Cluster and are responsible to
Store the data
Process the computation


Q3 HDFS blocks :

HDFS Block Concepts -
Filesystem Blocks: A block is the smallest unit of data that can be stored or retrieved from the disk. Filesystems deal with the data stored in blocks. Filesystem blocks are normally in few kilobytes of size. Blocks are transparent to the user who is performing filesystem operations like read and write.

HDFS Block

Hadoop distributed file system also stores the data in terms of blocks. However the block size in HDFS is very large. The default size of HDFS block is 64MB. The files are split into 64MB blocks and then stored into the hadoop filesystem. The hadoop application is responsible for distributing the data blocks across multiple nodes. 

Advantages of HDFS Block

The benefits with HDFS block are: 
The blocks are of fixed size, so it is very easy to calculate the number of blocks that can be stored on a disk.
HDFS block concept simplifies the storage of the datanodes. The datanodes doesn’t need to concern about the blocks metadata data like file permissions etc. The namenode maintains the metadata of all the blocks.
If the size of the file is less than the HDFS block size, then the file does not occupy the complete block storage.
As the file is chunked into blocks, it is easy to store a file that is larger than the disk size as the data blocks are distributed and stored on multiple nodes in a hadoop cluster.
Blocks are easy to replicate between the datanodes and thus provide fault tolerance and high availability. Hadoop framework replicates each block across multiple nodes (default replication factor is 3). In case of any node failure or block corruption, the same block can be read from another node.

