# Hadoop_Spark_Kafka_Installation_Windows
Tutorial for installing Hadoop, Spark, Kafka on Windows
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
JAVA_HOME: C:\java\jdk1.8.0_172\bin
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
        <value>C:\hadoop-2.8.0\data\namenode</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>C:\hadoop-2.8.0\data\datanode</value>
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
3. Open cmd and type command `hdfs namenode –format`.
4. Open cmd and enter into `C:\Hadoop-2.8.4\sbin` and type command `start-all.cmd` to start apache.
5. type `stop-all.cmd` to stop apache.
6. You can use http://localhost:8088 to manage YARN and use http://localhost:50070 to manage hdfs.
# Install Spark 2.3
## Prepare
1. Scala (Link: https://downloads.lightbend.com/scala/2.12.6/scala-2.12.6.msi)
2. SBT (Link: https://www.scala-sbt.org/download.html?_ga=2.24904318.53452594.1527175988-789449220.1524666135)
3. Spark (Link: http://spark.apache.org/downloads.html)
## Set up
1. Install Scala under `C:\scala` and set `SCALA_HOME: C:\scala` in Control Panel\System and Security\System goto "Adv System settings" and add `%SCALA_HOME%\bin` in PATH variable in environment variables.
2. Install SBT and set `SBT_HOME` as an environment variable with value as `C:\sbt`.
3. Extract `spark-2.3.0-bin-hadoop2.7.tgz` under `C:` and rename it to spark.
4. Set `SPARK_HOME: C:\spark` and add `%SPARK_HOME%\bin` in PATH variable in environment variables.
5. Run command: `spark-shell`
6. Open http://localhost:4040/ in a browser to see the SparkContext web UI.
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
import spark
```
