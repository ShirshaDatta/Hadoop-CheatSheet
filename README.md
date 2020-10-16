# Hadoop-CheatSheet

A cheatsheet to get you started with Hadoop
<p align="center">
<img src="https://miro.medium.com/max/1050/1*H4_yv5YskknPaJ4lWJpzXA.png">
</p>
  
## Installation 
**For Master and Slave Nodes** 
```
This is for RedHat
- Install Java JDK as Hadoop depends on it
wget https://www.oracle.com/webapps/redirect/signon?nexturl=https://download.oracle.com/otn/java/jdk/8u171-b11/512cd62ec5174c3487ac17c61aaa89e8/jdk-8u171-linux-x64.rpm
rpm -i -v -h jdk-8u171-linux-x64.rpm
- Install apache hadoop
wget https://archive.apache.org/dist/hadoop/core/hadoop-1.2.1/hadoop-1.2.1-1.x86_64.rpm
rpm -i -v -h hadoop-1.2.1-1.x86_64.rpm --force
- Verify if it is correctly installed with
java -version
hadoop version
```
<img src="assets/installing.PNG" alt="Logo">
## Configuration
<b>For Master Node also called NameNode</b>
Configure Hadoop ```core-site.xml``` file
```
mkdir /nn
vim /etc/hadoop/core-site.xml
<configuration>
<property>
<name>dfs.default.name</name>
<value>hdfs://MasterIP:PortNo</value>
</property>
</configuration>

```
Configure Hadoop ```hdfs-site.xml``` file

```
vim /etc/hadoop/hdfs-site.xml
<configuration>
<property>
<name>dfs.name.dir</name>
<value>/nn</value>
</property>
</configuration>
```

Then we will have to format the /nn folder of the namenode.
```hadoop namenode -format```
📸
```jps``` // we see that the process has not yet started
``` netstat -tnlp ``` // we see that the assigned port is free
Then we will have to start the service
```
hadoop-daemon.sh start namenode
jps
netstat -tnlp
```
We see that the process has started and the port is assigned


To view the no of slave nodes connected
```hadoop dfsadmin -report``` 

<b>For Slave Node also called DataNode</b>
Configure Hadoop ```core-site.xml``` file
```
vim /etc/hadoop/core-site.xml
<configuration>
<property>
<name>fs.default.name</name>
<value>hdfs://MasterIP:PortNo</value>
</property>
</configuration>
```

Configure Hadoop ```hdfs-site.xml``` file

```
mkdir /dn1
vim /etc/hadoop/hdfs-site.xml
<configuration>
<property>
<name>dfs.name.dir</name>
<value>/dn1</value>
</property>
</configuration>
```

Then we will have to start the service
```
hadoop-daemon.sh start datanode
jps
```
We see that the process has started.

To view the no of slave nodes connected
```hadoop dfsadmin -report``` 

