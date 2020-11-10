# Hadoop-CheatSheet 🐘

A cheatsheet to get you started with Hadoop

But the question is why should we learn Hadoop? How will it make our life easier?

Read till the end to know more. 

Happy learning 👩‍🎓


## Index Of Contents
1. [Introduction](#Introduction)
2. [Installation](#Installation)
3. [Configuration](#Configuration)<br>
    i) [NameNode](#NameNode)<br>
    ii) [DataNode](#DataNode)<br>
    iii) [ClientNode](#ClientNode)
4. [GUI](#GUI)
5. [Frequently Asked Questions](#FAQs)
6. [Testing](#Testing)
7. [Contributing](#Contributions)<br>
    i)[Contribution Practices](#Contribution-Practices)<br>
    ii)[Pull Request Process](#Pull-Request-Process)<br>
    iii)[Branch Policy](#Branch-Policy)
8. [Cool Links to Check out](#Cool-Links-To-Checkout)
7. [License](#License)
8. [Contact](#Contact)
<!--9. [Download Free PDF](#Download-free-PDF)-->

## Introduction

Simple answer to the the above question is to store data. Again the question, when there is Database as well as Drive storage why should we use Hadoop?

<b>TO STORE BIG DATA</b><br>

Now the question, What is Big Data?
An example of big data might be petabytes (1,024 terabytes) or exabytes (1,024 petabytes) of data consisting of billions to trillions of records of millions of people—all from different sources (e.g. Web, sales, customer contact center, social media, mobile data and so on).

To store so much of data we use the concept of DISTRIBUTED STORAGE CLUSTER. To implement these concepts we use Apache Hadoop.

<p align="center">
<img src="https://miro.medium.com/max/1050/1*H4_yv5YskknPaJ4lWJpzXA.png">
</p>

## Installation 
(For 1 master and multi slave and multi client nodes)
**For Master,Slave and Client Nodes** 
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

![Preview Image](./assets/installing.PNG)


## Configuration
## NameNode
(NameNode is also called Master Node)
```
    mkdir /nn
    vim /etc/hadoop/core-site.xml
        <configuration>
            <property>
                <name>fs.default.name</name>
                <value>hdfs://MasterIP:PortNo</value>
            </property>
        </configuration>

    vim /etc/hadoop/hdfs-site.xml
        <configuration>
            <property>
                <name>dfs.name.dir</name>
                <value>/nn</value>
            </property>
        </configuration>

```

The configured files:
<img src="assets/master files.PNG" alt="Logo">
#Check if the port number you assigned is free, if not then change the port number in the core-site.xml

Then we will have to format the /nn folder of the namenode.
``` hadoop namenode -format ```
<br>
<img src="assets/master format.PNG" alt="Logo">
```
    jps 
    netstat -tnlp 
```
We see that the process has not yet started and the assigned port is free 



<img src="assets/master before.png" alt="Logo">

<b> Then we will have to start the service:</b>
```
hadoop-daemon.sh start namenode
jps
netstat -tnlp
```
We see that the process has started and the port is assigned
<img src="assets/master successful.PNG" alt="Logo">

To view the no of slave nodes connected
```hadoop dfsadmin -report``` 

<img src="assets/dfsadmin master.PNG" alt="Logo">

### DataNode
(DataNode is also called Slave Node)

```
    vim /etc/hadoop/core-site.xml
        <configuration>
            <property>
                <name>fs.default.name</name>
                <value>hdfs://MasterIP:PortNo</value>
            </property>
        </configuration>
    mkdir /dn1
    vim /etc/hadoop/hdfs-site.xml
        <configuration>
            <property>
                <name>dfs.name.dir</name>
                <value>/dn1</value>
            </property>
        </configuration>
```
The Configured files:
<img src="assets/slave files.PNG" alt="Logo">

<b>Then we will have to start the service</b>
Make sure that if you doing the setup locally using VM's , then the firewall should be stopped in the master node.
To check so:
```
    systemctl status firewalld
   - If it is active then stop or disable(if you don't want to start after system reboot)
        systemctl stop firewalld
        systemctl disable firewalld
```
<img src="assets/firewalld master.PNG" alt="Logo">

```
hadoop-daemon.sh start datanode
jps
```
We see that the process has started.
<img src="assets/slave node done.PNG" alt="Logo">

To view the no of slave nodes connected

```hadoop dfsadmin -report```
<img src="assets/slave connected" alt="Logo">

### ClientNode 

```
    vim /etc/hadoop/core-site.xml
        <configuration>
            <property>
                <name>fs.default.name</name>
                <value>hdfs://MasterIP:PortNo</value>
            </property>
        </configuration>

    - To see how many files we have in their storage
        hadoop fs -ls /
    - To add a file
        cat > /file1.txt
        Hi I am the first file
        Ctrl+C
        hadoop fs - put /file1.txt /
    - To read the contents of the file
        hadoop fs -cat /file1.txt
    - To check the size of the file
        hadoop fs -count /file1.txt
    - To create a directory
        hadoop fs -mkdir /textfiles
    -To upload a blank file on the fly
        hadoop fs -touchz /my.txt
    -To move a file (source➡destination)
        hadoop fs -mv /lw.txt /textfiles
    - To copy a file (source➡destination)
        hadoop fs -cp /file1.txt /textfiles
    - To remove a file
        hadoop fs -rm  /file1.txt
    - To checkout and explore all the available options
        hadoop fs 
```
The attached screenshots of the above mentioned commands are :
<img src="assets/cmds client1.PNG" alt="Logo">
<img src="assets/cmds client2.PNG" alt="Logo">
<img src="assets/cmds client3.png" alt="Logo">

## GUI
We can also visualize using GUI
    Namenode : MasterIP:50070
    Datanode : SlaveIP:50075
<img src="assets/gui.PNG" alt="Logo">
We can visualize the uploaded files
<img src="assets/gui1.PNG" alt="Logo">

We see that if the file is small it is broken in only 1 block
<img src="assets/gui filetxt.PNG" alt="Logo">
We can check the size of the name.txt file like:
```
    -To see the permissions as well as the size of the block in bytes
        ls -l name.txt
    -To see the permissions as well as the size of the block 
        ls -l -h name.txt
```
<img src="assets/size of nametxt.PNG" alt="Logo">
The default DFS block size is 32768 , and therefore it is divided into blocks before storing. 
<img src="assets/gui nametxt.PNG" alt="Logo">

## FAQs
Will come up soon, stay tuned :)

## Testing 
These commands are even checked in AWS cloud.

## Contributions
Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

### Contribution Guidelines

When contributing to this repository, please first discuss the change you wish to make via issue,
email, or any other method with the owners of this repository before making a change. 


### Contribution Practices
* Write clear and meaningful commit messages.
* If you report a bug please provide steps to reproduce the bug.
* In case of changing the backend routes please submit an updated routes documentation for the same.
* If there is an UI related change it would be great if you could attach a screenshot 
 with the resultant changes so it is easier to review for the maintainers 

### Pull Request Process
1. Ensure any install or build dependencies are removed before the end of the layer when doing a 
   build.
2. Update the README.md with details of changes to the interface, this includes new environment 
   variables, exposed ports, useful file locations and container parameters.
3. Only send your pull requests to the development branch where once we reach a stable point 
    it will be merged with the   master branch 
4. Associate each Pull Request with the required issue number 

### Branch Policy 
* development: If you are making a contribution make sure to send your Pull Request to this branch . All
            developments goes in this branch.
* master: After significant features/bug-fixes are accumulated in development branch we merge it with the master branch.


## Cool Links to Checkout 

- [How Facebook stores so much data and its statistics](https://shirshadatta2000.medium.com/how-facebook-stores-so-much-data-and-its-statistics-bd0911ad39a1)

- [Facebook and Hadoop](https://www.facebook.com/notes/facebook-engineering/hadoop/16121578919/)

- [How Google stores massive amounts of data](https://medium.com/@avantikadasgupta/how-google-stores-massive-amounts-of-data-bigtable-d67f49bfc40e)

- [Apache Hadoop Ecosystem](https://www.cloudera.com/products/open-source/apache-hadoop.html)

## License

Distributed under the MIT License. See `LICENSE` for more information.

## Contact

- My Name - Shirsha Datta
 
- You can contact me at shirshadatta2000@gmail.com

- Connect with me on [LinkedIn](https://www.linkedin.com/in/shirsha-datta-30335a178/)

<!--
## Download free PDF


[Claim your free PDF here]()


** 
Add the line in Contribution : if you are facing in any issue in configuration contact, Open ISSUE
![before](./assets/master before.png)
Dynamically increase volume
💯21-10 topics
💯Add that these are tested in AWS too
💯Some intro to big data nd apache
💯heart beat and stuffs
💯How to change block size
** Make it more readable
** safemode and tcpdump
💯** Who is the one uploading the file?
✴️ Answer: Client gets the IP from Master and uploads the file to DataNode.

💯**Does client go to master and then read the file on slave via Master or Does Client go to slave directly and read the data?
✴️ Answer: Client goes to slave directly and reads the data stored on slave.
-->

<!-- ADD IF NECESSARY💯
# Contributing
Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

1. Fork the Project
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the Branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request
-->
