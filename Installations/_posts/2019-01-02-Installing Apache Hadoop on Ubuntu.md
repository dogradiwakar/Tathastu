---
layout: post
title: Installing Hadoop on Ubuntu
description: >

author: author1
canonical_url:
categories: [Installations]
tags: [Installations]
---



Pre-requisites

Install Brew

Go to Brew Website
https://brew.sh/



Copy the url from Home page on mac os terminal to install Home brew



Java Installation

Check Java Version

Java -version




Check below url for supported versions
https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions


Run below command to install Java8 as it is supported by Hadoop 3.0

brew cask install java8




For Latest Java use

brew cask install java

Check Java Version



Install Hadoop using brew
Run below command

Brew install hadoop







Configuration Changes

Go to  /usr/local/Cellar/hadoop/3.2.1 and make some changes or create the following files

	1. hadoop-env.sh
	2. core-site.xml
	3. mapred-site.xml
	4. hdfs-site.xml

hadoop-env.sh
In  /usr/local/Cellar/hadoop/3.2.1/libexec/etc/hadoop/hadoop-env.sh  look for export JAVA_HOME and configure it to the Java home value

Run below command to get the Java home

/usr/libexec/java_home





core-site.xml

In core-site.xml, you will configure the HDFS address and port number.

<!-- Put site-specific property overrides in this file. -->
<configuration>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
    <description>A base for other temporary directories</description>             
  </property>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:8020</value>
  </property>
</configuration>





mapred-site.xml


Add the following into mapred-site.xml .
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>localhost:8021</value>
  </property>
</configuration>




hdfs-site.xml

In hdfs-site.xml ,add below

<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>



Configure SSH

Check if ssh is enabled using below command
ssh localhost

In case of below error configure ssh


Run below commands and Authorize SSH Keys i.e allow your system to accept login, we have to make it aware of the keys that will be used

 ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
 chmod 0600 ~/.ssh/authorized_keys

Enable Remote Login: “System Preferences” -> “Sharing”. Check “Remote Login”

Check ssh again using ssh localhost




Format HDFS

Finally, the last step before starting to launch the different services would be to format the HDFS.
cd /usr/local/opt/hadoop
hdfs namenode -format






Alias to start and stop Hadoop Daemons

Now, we need to go to /usr/local/Cellar/hadoop/3.2.1/sbin/ to start and stop hadoop services.

Edit ~/.bash_profile and add

nano ~/.profile

alias hstart="/usr/local/Cellar/hadoop/3.2.1/sbin/start-all.sh"
alias hstop="/usr/local/Cellar/hadoop/3.2.1/sbin/stop-all.sh"

Then run

source ~/.bash_profile


Manual starting

HDFS Services
Go to /usr/local/opt/hadoop/sbin , there you can use the following scripts
# To start HDFS service
$ ./start-dfs.sh# To stop HDFS service
$ ./stop-dfs.sh


# to start all services
$ ./start-all.sh# to stop all services
$ ./stop-all.sh



Start Hadoop using the hstart alias




Run JPS to check the services


Access Hadoop web interface by connecting to

Resource Manager: http://localhost:9870
JobTracker: http://localhost:8088/
Node Specific Info: http://localhost:8042/
Name Node