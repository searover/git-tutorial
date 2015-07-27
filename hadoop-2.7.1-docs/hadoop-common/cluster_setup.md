# Hadoop Cluster Setup

## Installation
Typically one machine in the cluster is designated as the NameNode and another machine the as ResourceManager, exclusively. These are the masters. Other services(such as Web App proxy server and MapReduce Job History server) are usually run either on dedicated hardware or on shared infrastructure, depending upon the load. The rest of the machines in the cluster act as both DataNode and NodeManager. These are the slaves.

## Configuring Hadoop in Non-Secure Mode
*Hadoop's java configuration is driven by two types of important configuration files:*
* Read-only default configuration: core-default.xml, hdfs-default.xml, yarn-default.xml and mapred-default.xml
* Site-specific configuration: etc/hadoop/core-site.xml, etc/hadoop/hdfs-site.xml, etc/hadoop/yarn-site.xml and etc/hadoop/mapred-site.xml

Additionally, you can control the Hadoop scripts found in the bin/ directory of the distribution, by setting site-specific values via the etc/hadoop/hadoop-evn.sh and etc/hadoop/yarn-env.sh

To configure the Hadoop cluster you will need to configure the environment in which the Hadoop daemons execute as well the configuration parameters for the Hadoop daemons.

HDFS daemons are NameNode, SecondaryNameNode, and DataNode. YARN daemons are ResourceManager, NodeManager, and WebAppProxy. If MapReduce is to be used, then the MapReduce Job History Server will also be running. For large installations, these are generally running on seperate hosts.

## Configuring Environment of Hadoop Daemons
Administrators should use the etc/hadoop/hadoop-env.sh and optionally the etc/hadoop/mapred-env.sh and etc/hadoop/yarn-env.sh scripts to do site-specific customization of the Hadoop daemon's process environment.

At the very least, you must specify the JAVA_HOME so that it is correctly defined on each remote node.

Administrators can configure individual daemons using the configuration options shown below in the table:

   *Daemon*				*Environment Variable* 

*   NameNode ==>			HADOOP_NAMENODE_OPTS

*   DataNode ==>			HADOOP_DATANODE_OPTS

*   Secondary NameNode ==>		HADOOP_SECONDARYNAMENODE_OPTS

*   ResourceManager ==>			YARN_RESOURCEMANAGER_OPTS

*   NodeManager	==>			YARN_NODEMANAGER_OPTS

*   WebAppProxy	==>			YARN_PROXYSERVER_OPTS

*   Map Reduce Job History Server ==>	HADOOP_JOB_HISTORYSERVER_OPTS

For Example, to configure NameNode to use parallelGC, the flowing statement should be added in hadoop-env.sh:
    
    export HADOOP_NAMENODE_OPTS="-XX:+UseParallelGC"

   Other useful configuration parameters that you can customize include:
    * HADOOP_PID_DIR - The directory where the daemon's process id files are stored.
    * HADOOP_LOG_DIR - The directory where the daemon's log files are storead. Log files are automatically created if they don't exist.
    * HADOOP_HEAPSIZE / YARN_HEAPSIZE - The maximum amount of heapsize to use, in MB e.g. if the variable is set to 1000 the heap will be set to 1000MB. This is used to configure the heap size for the daemon. By default, the value is 1000. If you want to configure the values separately for each daemon you can use.

  *In most cases, you should specify the HADOOP_PID_DIR and HADOOP_LOG_DIR directories such that they can only be written to by the users that are going to run the daemons.*
  *It is also traditional to configure HADOOP_PREFIX in the system-wide shell environment configuration. For example:*
 
     HADOOP_PREFIX = /path/to/hadoop
     export HADOOP_PREFIX
 
## Configuring the Hadoop Daemons
This section deals with important parameters to be specified in the given configuration files:
* etc/hadoop/core-site.xml

