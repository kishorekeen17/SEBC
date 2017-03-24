1)  cloud provider : AWS

2) IP address and private DNS

52.54.105.135 (ip-172-31-27-88.ec2.internal)
54.88.53.71 (ip-172-31-29-76.ec2.internal)
54.165.70.52 (ip-172-31-31-32.ec2.internal)
54.225.62.61 (ip-172-31-19-230.ec2.internal)
107.21.64.76 (ip-172-31-28-12.ec2.internal)

3) Command and output that lists the Linux release
command : cat /etc/centos-release
Result =>  CentOS release 6.8 (Final)

4) Command and output for the contents of /etc/yum.repos.d/
 command : ls -lrt /etc/yum.repos.d/
 Result => 
 
 -rw-r--r--. 1 root root 1056 Nov  5  2012 epel-testing.repo
-rw-r--r--. 1 root root  957 Nov  5  2012 epel.repo
-rw-r--r--  1 root root 1060 Dec  2  2013 mysql-community-source.repo
-rw-r--r--  1 root root 1209 Dec  2  2013 mysql-community.repo
-rw-r--r--  1 root root 6259 May 18  2016 CentOS-Vault.repo
-rw-r--r--  1 root root  630 May 18  2016 CentOS-Media.repo
-rw-r--r--  1 root root  289 May 18  2016 CentOS-fasttrack.repo
-rw-r--r--  1 root root  647 May 18  2016 CentOS-Debuginfo.repo
-rw-r--r--  1 root root 1991 May 18  2016 CentOS-Base.repo

5) Add the following Linux accounts

user Commands => 
adduser -u 2040 berkeley
adduser -u 2420 stanford

Result =>
[root@ip-172-31-28-12 ~]# id -u berkeley
2040
[root@ip-172-31-28-12 ~]# id -u stanford
2420

group commands =>
groupadd cardinal
groupadd bruins

result =>

[root@ip-172-31-28-12 ~]# grep cardinal /etc/group
cardinal:x:2421:
[root@ip-172-31-28-12 ~]# grep bruins /etc/group
bruins:x:2422:

user add to group commands =>
usermod -g bruins berkeley
usermod -g cardinal stanford

reult => 

[root@ip-172-31-28-12 ~]# grep berkeley /etc/group
berkeley:x:2040:
[root@ip-172-31-28-12 ~]# grep stanford /etc/group
stanford:x:2420:

5) JDBC Connector

scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz root@ec2-54-225-62-61.compute-1.amazonaws.com:/home/
scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz root@ec2-54-88-53-71.compute-1.amazonaws.com:/home/
scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz root@ec2-54-165-70-52.compute-1.amazonaws.com:/home/
scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz root@ec2-54-225-62-61.compute-1.amazonaws.com:/home/
scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz root@ec2-107-21-64-76.compute-1.amazonaws.com:/home/

scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz centos@ec2-34-203-200-66.compute-1.amazonaws.com:/home/root/
sudo mkdir /usr/share/java
cd /home
tar zxvf mysql-connector-java-5.1.41.tar.gz
sudo cp /home/mysql-connector-java-5.1.41/mysql-connector-java-5.1.41-bin.jar /usr/share/java/mysql-connector-java.jar

6) JDK Installation

scp -i my-aws-key.pem jdk-8u121-linux-x64.tar.gz root@ec2-107-21-64-76.compute-1.amazonaws.com:/home/
cd /home
tar zxvf jdk-8u121-linux-x64.tar.gz
sudo mkdir /usr/java
mv jdk1.8.0_121 /usr/java/

export JAVA_HOME=/usr/java/jdk1.8.0_121
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

7) Cloudera Manager Installation
created file /etc/yum.repos.d/cloudera-manager.repo (i.e. cloudera manager repo set)

content of file => for download cm 5.9.0

[cloudera-manager]
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64           	  
name=Cloudera Manager
baseurl=https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.9.0/
gpgkey =https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera    
gpgcheck = 1

sudo yum install cloudera-manager-daemons cloudera-manager-server
sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql kishore root kishore
sudo service cloudera-scm-server start

8) CDH Installation 

Cloudera Reference => https://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_host_allocations.html


