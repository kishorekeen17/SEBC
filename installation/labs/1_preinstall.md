********************************************************************************************
					System Configuration Checks Started !
********************************************************************************************

ssh -i my-aws-key.pem root@ec2-52-54-105-135.compute-1.amazonaws.com
ssh -i my-aws-key.pem root@ec2-54-88-53-71.compute-1.amazonaws.com
ssh -i my-aws-key.pem root@ec2-54-165-70-52.compute-1.amazonaws.com
ssh -i my-aws-key.pem root@ec2-54-225-62-61.compute-1.amazonaws.com
ssh -i my-aws-key.pem root@ec2-107-21-64-76.compute-1.amazonaws.com


scp -i my-aws-key.pem jdk-8u112-linux-i586.tar.gz centos@ec2-54-173-170-100.compute-1.amazonaws.com:/home/centos/

1) cat /proc/sys/vm/swappiness
2) sysctl vm.swappiness=10
3) cat /proc/sys/vm/swappiness
4) mount
5) df -h
6) ext-based volumes
7) sudo tune2fs -l /dev/xfs (sudo tune2fs -l /dev/xvda1)
8) Disable transparent hugepage support
9) ifconfig -a
10) sudo yum install bind-utils
11) nslookup 172.31.25.56
12) nscd running
sudo yum install nscd
service nscd start
13) Show the ntpd service is running

install steps : 

yum install ntp ntpdate ntp-doc
chkconfig ntpd on
systemctl start ntpd
				

********************************************************************************************
					System Configuration Checks Completed !
********************************************************************************************


********************************************************************************************
					MySQL/MariaDB Installation Lab Started !
********************************************************************************************

sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm (Linux 7)
sudo rpm -Uvh http://repo.mysql.com/mysql-community-release-el6-5.noarch.rpm (Linux 6)
sudo yum install -y mysql-server
sudo yum update -y
sudo reboot
service mysqld status
sudo service mysqld stop
service mysqld status
sudo mkdir /var/lib/mysql/installbackups
sudo mv /var/lib/mysql/ib* /var/lib/mysql/installbackups
sudo ls /var/lib/mysql
sudo cp /etc/my.cnf /etc/my.cnf.backup  
sudo truncate -s0 /etc/my.cnf
ls -l /etc/my.cnf
sudo vi /etc/my.cnf
chown mysql:mysql /etc/my.cnf
sudo service mysqld start
sudo service mysqld status
sudo /sbin/chkconfig mysqld on
sudo /usr/bin/mysql_secure_installation

copy below content into my.cnf

******************************************
[mysqld]
transaction-isolation = READ-COMMITTED
# Disabling symbolic-links is recommended to prevent assorted security risks;
# to do so, uncomment this line:
# symbolic-links = 0

key_buffer_size = 32M
max_allowed_packet = 32M
thread_stack = 256K
thread_cache_size = 64
query_cache_limit = 8M
query_cache_size = 64M
query_cache_type = 1

max_connections = 550
#expire_logs_days = 10
#max_binlog_size = 100M

#log_bin should be on a disk with enough free space. Replace '/var/lib/mysql/mysql_binary_log' with an appropriate path for your system
#and chown the specified folder to the mysql user.
log_bin=/var/lib/mysql/mysql_binary_log

# For MySQL version 5.1.8 or later. For older versions, reference MySQL documentation for configuration help.
binlog_format = mixed

read_buffer_size = 2M
read_rnd_buffer_size = 16M
sort_buffer_size = 8M
join_buffer_size = 8M

# InnoDB settings
innodb_file_per_table = 1
innodb_flush_log_at_trx_commit  = 2
innodb_log_buffer_size = 64M
innodb_buffer_pool_size = 4G
innodb_thread_concurrency = 8
innodb_flush_method = O_DIRECT
innodb_log_file_size = 512M

[mysqld_safe]
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid

sql_mode=STRICT_ALL_TABLES


******************************************


mysql --user=root --password=kishore

Step-8)

execute queries into mysql 

***************************************************************

create database amon DEFAULT CHARACTER SET utf8;
grant all on amon.* TO 'amon'@'%' IDENTIFIED BY 'amon_password';
create database rman DEFAULT CHARACTER SET utf8;
grant all on rman.* TO 'rman'@'%' IDENTIFIED BY 'rman_password';
create database smon DEFAULT CHARACTER SET utf8;
grant all on smon.* TO 'smon'@'%' IDENTIFIED BY 'smon_password';
create database hmon DEFAULT CHARACTER SET utf8;
grant all on hmon.* TO 'hmon'@'%' IDENTIFIED BY 'hmon_password';
create database kishore DEFAULT CHARACTER SET utf8;
grant all on kishore.* TO 'root'@'%' IDENTIFIED BY 'kishore';
grant all on kishore.* TO 'centos'@'%' IDENTIFIED BY 'kishore';
create database hive DEFAULT CHARACTER SET utf8;
grant all on hive.* TO 'hive'@'%' IDENTIFIED BY 'hive_password';
create database nav DEFAULT CHARACTER SET utf8;
grant all on nav.* TO 'nav'@'%' IDENTIFIED BY 'nav_password';
create database sentry DEFAULT CHARACTER SET utf8;
grant all on sentry.* TO 'sentry'@'%' IDENTIFIED BY 'sentry_password';
create database navms DEFAULT CHARACTER SET utf8;
grant all on navms.* TO 'navms'@'%' IDENTIFIED BY 'navms_password';
create database hue DEFAULT CHARACTER SET utf8;
grant all on hue.* to 'hue'@'%' identified by 'hue_password';
create database oozie DEFAULT CHARACTER SET utf8;
grant all privileges on oozie.* to 'oozie'@'%' identified by 'oozie';
create database scm DEFAULT CHARACTER SET utf8;
grant all privileges on scm.* to 'scm'@'%' identified by 'scm';


***************************************************************

Step-9)

JDBC Connectors copy process :
==============================

scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz centos@ec2-34-203-200-66.compute-1.amazonaws.com:/home/root/
sudo mkdir /usr/share/java
cd /home
tar zxvf mysql-connector-java-5.1.41.tar.gz
sudo cp /home/mysql-connector-java-5.1.41/mysql-connector-java-5.1.41-bin.jar /usr/share/java/mysql-connector-java.jar

5. On the master MySQL node, grant replication privileges for your replica node:

MySql Installed Nodes : 
1)	ip-172-31-23-92 (i.e. Master)
2)	ip-172-31-19-192 (i.e. Slave)

a. Log in with mysql -u ... -p

mysql --host=localhost --user=root --password=1234

b. Note the FQDN of your replica host.
c. GRANT REPLICATION SLAVE ON *.* TO 'root'@'ip-172-31-19-192' IDENTIFIED BY '1234';
d. SET GLOBAL binlog_format = 'ROW'; 
e. FLUSH TABLES WITH READ LOCK;


slave process

GRANT REPLICATION SLAVE ON *.* TO 'root'@'172.31.33.88' IDENTIFIED BY 'kishore';
CREATE USER 'root'@'%' IDENTIFIED BY 'kishore';

CHANGE MASTER TO MASTER_HOST='172.31.45.217', MASTER_USER='root', MASTER_PASSWORD='kishore', MASTER_LOG_FILE='mysql_binary_log.000001', MASTER_LOG_POS=4919;

ec2-54-92-192-40.compute-1.amazonaws.com (172.31.45.217)
ec2-54-173-170-100.compute-1.amazonaws.com (172.31.33.88)

==================================

[root@ip-172-31-23-92 cloudera-scm-server]# sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql mysql root kishore
JAVA_HOME=/usr/java/jdk1.7.0_67-cloudera
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/java/jdk1.7.0_67-cloudera/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
2017-03-21 19:29:18,436 [main] INFO  com.cloudera.enterprise.dbutil.DbCommandExecutor  - Successfully connected to database.
All done, your SCM database is configured correctly!
[root@ip-172-31-23-92 cloudera-scm-server]# ls -lrt
total 12
-rw-r--r--. 1 root         root         1521 Oct  7 01:34 log4j.properties
-rw-------. 1 cloudera-scm cloudera-scm  714 Oct  7 01:34 db.properties.~1~
-rw-------. 1 cloudera-scm cloudera-scm  431 Mar 21 19:29 db.properties
[root@ip-172-31-23-92 cloudera-scm-server]# vi db.properties
[root@ip-172-31-23-92 cloudera-scm-server]# ls -lrt
total 12
-rw-r--r--. 1 root         root         1521 Oct  7 01:34 log4j.properties
-rw-------. 1 cloudera-scm cloudera-scm  714 Oct  7 01:34 db.properties.~1~
-rw-------. 1 cloudera-scm cloudera-scm  431 Mar 21 19:29 db.properties



=================================================================================

********************************************************************************************
					MySQL/MariaDB Installation Lab Completed !
********************************************************************************************

********************************************************************************************
					JDK Installation Started !
********************************************************************************************

download Java tar and unzip
===========================

sudo yum install oracle-j2sdk1.7

JDK classpath : 

scp -i my-aws-key.pem jdk-8u121-linux-x64.tar.gz root@ec2-107-21-64-76.compute-1.amazonaws.com:/home/

sudo mkdir /usr/java			 
				 
export JAVA_HOME=/usr/java/jdk1.8.0_121
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/jre/lib/ext:$JAVA_HOME/lib/tools.jar

or

sudo yum install oracle-j2sdk1.7.x86_64


********************************************************************************************
					JDK Installation Completed !
********************************************************************************************

********************************************************************************************
					Cloudera Installation Started !
********************************************************************************************


CM/CDH Install Lab

Step-1) 

file need to create : /etc/yum.repos.d/cloudera-manager.repo

Save the appropriate Cloudera Manager repo file (cloudera-manager.repo) for your system: 

for 5.9.0 repo 
==============

[cloudera-manager]
# Packages for Cloudera Manager, Version 5, on RedHat or CentOS 7 x86_64           	  
name=Cloudera Manager
baseurl=https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/5.9.0/
gpgkey =https://archive.cloudera.com/cm5/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera    
gpgcheck = 1

Step-2)

In master node : 
sudo yum install cloudera-manager-daemons cloudera-manager-server

Step-3)

sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql kishore centos kishore

[centos@ip-172-31-25-56 cloudera-scm-server]$ sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql kishore root kishore
JAVA_HOME=/usr/lib/jvm/jre-openjdk
Verifying that we can write to /etc/cloudera-scm-server
Creating SCM configuration file in /etc/cloudera-scm-server
Executing:  /usr/lib/jvm/jre-openjdk/bin/java -cp /usr/share/java/mysql-connector-java.jar:/usr/share/java/oracle-connector-java.jar:/usr/share/cmf/schema/../lib/* com.cloudera.enterprise.dbutil.DbCommandExecutor /etc/cloudera-scm-server/db.properties com.cloudera.cmf.db.
[                          main] DbCommandExecutor              INFO  Successfully connected to database.
All done, your SCM database is configured correctly!

Step-4)

sudo service cloudera-scm-server start

http://ec2-107-21-64-76.compute-1.amazonaws.com:7180
http://172.31.45.217:7180/

http://ec2-34-204-43-28.compute-1.amazonaws.com:7180

CDH Installation Document : https://www.cloudera.com/documentation/enterprise/latest/topics/cm_ig_host_allocations.html

********************************************************************************************
					Cloudera Installation Completed !
********************************************************************************************
