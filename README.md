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
rpm -i -v -h hadoop-1.2.1-1.x86_64.rpm
- Verify if it is correctly installed with
java -version
hadoop version
```

## Configuration
**For Master Node** 
Configure Hadoop ```core-site.xml``` file
```
mkdir /nn
cd /etc/hadoop
ls 
vim core-site.xml


```
Configure Hadoop ```hdfs-site.xml``` file

