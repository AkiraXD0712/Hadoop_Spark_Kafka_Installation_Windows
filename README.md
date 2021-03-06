# Hadoop_Spark_Kafka_Installation_Windows
Tutorial for installing Hadoop 2.8.4, Spark 2.3, Pyspark and Kafka on Windows 10

Reference: 
1. https://github.com/MuhammadBilalYar/Hadoop-On-Window/wiki/Step-by-step-Hadoop-2.8.0-installation-on-Window-10
2. http://blog.51cto.com/balich/2058194
3. https://stackoverflow.com/questions/25481325/how-to-set-up-spark-on-windows?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa
4. https://blog.sicara.com/get-started-pyspark-jupyter-guide-tutorial-ae2fe84f594f
5. https://j5technology.net/zookeeper-install
6. https://blog.csdn.net/u012050154/article/details/76270655
7. https://www.jianshu.com/p/b631c3899558
# Install Hadoop 2.8.4
## Prepare
These softwares should be prepared to install Hadoop 2.8.0 on window 10 64bit
1. Hadoop 2.8.4 (Link: http://mirrors.ircam.fr/pub/apache/hadoop/common/hadoop-2.8.4/hadoop-2.8.4.tar.gz)
2. Java JDK 1.8.0 (Link: http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
## Set up
1. Install java under `C:\java` if java is not installed on your system. Use `Javac -version` to check if you have successfully installed java. **(do not install java under C:\Programs Files which contains spaces because it will cause an issue when you set up the environment varible)**
2. Extract file `Hadoop 2.8.4.tar.gz` and place under `C:\Hadoop-2.8.4`. 
3. Set some Environment variables on windows 10.
```
HADOOP_HOME: C:\hadoop-2.8.4
HADOOP_CONF_DIR: %HADOOP_HOME%\etc\hadoop
YARN_CONF_DIR: %HADOOP_CONF_DIR%
JAVA_HOME: C:\java\jdk1.8.0_172
CLASSPATH: .;%JAVA_HOME%\lib;%JAVA_HOME%\jre\lib
PATH: %PATH%;%JAVA_HOME%\bin;%JAVA_HOME%\lib;%JAVA_HOME%\jre\bin;%HADOOP_HOME%\bin
```
## Configuration
1. Edit file `C:/Hadoop-2.8.4/etc/hadoop/core-site.xml`, paste below xml paragraph and save this file.
```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
</configuration>
```
2. Rename `mapred-site.xml.template` to `mapred-site.xml` and edit this file `C:/Hadoop-2.8.4/etc/hadoop/mapred-site.xml`, paste below xml paragraph and save this file.
```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```
3. Create folder `data` under `C:\Hadoop-2.8.4` and than create sub-folder `datanode` and `datanode`.
4. Edit file `C:\Hadoop-2.8.4/etc/hadoop/hdfs-site.xml`, paste below xml paragraph and save this file.
```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>C:\hadoop-2.8.4\data\namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>C:\hadoop-2.8.4\data\datanode</value>
    </property>
</configuration>
```
5. Edit file `C:/Hadoop-2.8.4/etc/hadoop/yarn-site.xml`, paste below xml paragraph and save this file.
```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.auxservices.mapreduce.shuffle.class</name>  
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>
```
6. Edit file `C:/Hadoop-2.8.4/etc/hadoop/hadoop-env.cmd` by closing the command line `JAVA_HOME=%JAVA_HOME%` instead of set `JAVA_HOME=C:\java\jdk1.8.0_172` and adding the command lines below at the end of the file.
```
set HADOOP_PREFIX=C:\hadoop-2.8.4
set HADOOP_CONF_DIR=%HADOOP_PREFIX%\etc\hadoop
set YARN_CONF_DIR=%HADOOP_CONF_DIR%
set PATH=%PATH%;%HADOOP_PREFIX%\bin
```
## Hadoop configuration
1. Download Windows binaries for Hadoop versions. (Link: https://github.com/steveloughran/winutils/tree/master/hadoop-2.8.3/bin)
2. Delete file bin on `C:\Hadoop-2.8.4\bin`, replaced by file bin on file just downloaded.
## Run
1. Open cmd and type command `hdfs namenode –format`.
2. Open cmd and enter into `C:\Hadoop-2.8.4\sbin` and type command `start-dfs.cmd` to start apache.
3. Type `stop-dfs.cmd` to stop apache.
4. You can use http://localhost:8088 to manage YARN and use http://localhost:50070 to manage hdfs.
# Install Spark 2.3
## Prepare
1. Scala (Link: https://downloads.lightbend.com/scala/2.11.12/scala-2.11.12.msi)
2. SBT (Link: https://www.scala-sbt.org/download.html?_ga=2.24904318.53452594.1527175988-789449220.1524666135)
3. Spark (Link: http://spark.apache.org/downloads.html)
## Set up
1. Install Scala under `C:\scala` and set `SCALA_HOME: C:\scala` in Control Panel\System and Security\System goto "Adv System settings" and add `%SCALA_HOME%\bin` in PATH variable in environment variables.
2. Install SBT and set `SBT_HOME` as an environment variable with value as `C:\sbt`.
3. Extract `spark-2.3.0-bin-hadoop2.7.tgz` under `C:` and rename it to `spark`.
4. Set `SPARK_HOME: C:\spark` and add `%SPARK_HOME%\bin` in PATH variable in environment variables.
## Run
1. Run command: `spark-shell`
2. Open http://localhost:4040/ in a browser to see the SparkContext web UI.
# Install Pyspark in Anaconda
## Prepare
1. Install anaconda
## Set up
1. Use `conda install -c conda-forge pyspark` to install pyspark.
2. Set PYTHON_PATH an environment variable with value as `%SPARK_HOME%\python;%SPARK_HOME%\python\lib\py4j-0.10.6-src.zip`.
3. Use `conda install -c conda-forge findspark` to install findspark.
## Run
1. Open cmd and type command `jupyter notebook` to launch jupyter.
2. Use the code below to import pyspark.
```python
import findspark
findspark.init()
import pyspark
```
# Install Kafka
## Prepare
1. Zookeeper (Link: http://apache.mediamirrors.org/zookeeper/)
2. Kafka (Link: http://apache.crihan.fr/dist/kafka/1.1.0/kafka_2.11-1.1.0.tgz)
## Set up
### Zookeeper
1. Unzip `zookeeper-3.4.12.tar.gz` under `C:\Zookeeper`.
2. Navigate to `C:\Zookeeper\zookeeper-3.4.12\conf` and rename `zoo_sample.cfg` to `zoo.cfg`.
3. Open `zoo.cfg` and set the directory for all the Zookeeper Data to be stored. Make sure that this directory exists before starting Zookeeper.
```
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
dataDir=C:\Zookeeper\zkData
```
4. Edit your environment variables and add `ZOOKEEPER_HOME` as the variable name and the path where you installed.
5. Add `%ZOOKEEPER_HOME%\bin` to the PATH environment variables.
### Kafka
1. Unzip `kafka_2.11-1.1.0.tgz` under `C:\`.
2. Navigate to `C:\kafka_2.11-1.1.0\config`.
3. Open `server.properties` and set the `log.dirs` for the log file to be stored. Make sure that this directory exists.
```
# A comma separated list of directories under which to store log files
log.dirs=C:\kafka_2.11-1.1.0\kafka-logs
```
## Run
1. Run command `zkserver` to launch zookeeper. It should be running on `<ip address>:2181` by default.
2. Navigate to `C:\kafka_2.11-1.1.0` and run command `.\bin\windows\kafka-server-start.bat .\config\server.properties` to launch Broker.
3. Navigate to `C:\kafka_2.11-1.1.0\bin\windows` and run command `kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic testDemo` to create topic.
4. Open another cmd and navigate to `C:\kafka_2.11-1.1.0\bin\windows`. Run command `kafka-console-producer.bat --broker-list localhost:9092 --topic testDemo` to launch Producer.
5. Open another cmd and navigate to `C:\kafka_2.11-1.1.0\bin\windows`. Run command `kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic testDemo` to launch Consumer.
6. Now you can send your messages.
