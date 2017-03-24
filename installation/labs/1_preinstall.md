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
7) sudo tune2fs -l /dev/xfs
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

sudo rpm -Uvh http://dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm
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
sudo /sbin/chkconfig mysqld on
sudo service mysqld start
sudo service mysqld status


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


step-6)

[root@ip-172-31-25-56 mysql]# sudo /usr/bin/mysql_secure_installation

NOTE: RUNNING ALL PARTS OF THIS SCRIPT IS RECOMMENDED FOR ALL MariaDB
      SERVERS IN PRODUCTION USE!  PLEASE READ EACH STEP CAREFULLY!

In order to log into MariaDB to secure it, we'll need the current
password for the root user.  If you've just installed MariaDB, and
you haven't set the root password yet, the password will be blank,
so you should just press enter here.

Enter current password for root (enter for none):
OK, successfully used password, moving on...

Setting the root password ensures that nobody can log into the MariaDB
root user without the proper authorisation.

Set root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Reloading privilege tables..
 ... Success!


By default, a MariaDB installation has an anonymous user, allowing anyone
to log into MariaDB without having to have a user account created for
them.  This is intended only for testing, and to make the installation
go a bit smoother.  You should remove them before moving into a
production environment.

Remove anonymous users? [Y/n] y
 ... Success!

Normally, root should only be allowed to connect from 'localhost'.  This
ensures that someone cannot guess at the root password from the network.

Disallow root login remotely? [Y/n] n
 ... skipping.

By default, MariaDB comes with a database named 'test' that anyone can
access.  This is also intended only for testing, and should be removed
before moving into a production environment.

Remove test database and access to it? [Y/n] y
 - Dropping test database...
 ... Success!
 - Removing privileges on test database...
 ... Success!

Reloading the privilege tables will ensure that all changes made so far
will take effect immediately.

Reload privilege tables now? [Y/n] y
 ... Success!

Cleaning up...

All done!  If you've completed all of the above steps, your MariaDB
installation should now be secure.

Thanks for using MariaDB!

mysql --user=root --password=kishore

select table status

+---------------------------+--------+---------+------------+------+----------------+-------------+--------------------+--------------+-----------+----------------+---------------------+---------------------+---------------------+-------------------+----------+----------------+---------------------------------------------------+
| Name                      | Engine | Version | Row_format | Rows | Avg_row_length | Data_length | Max_data_length    | Index_length | Data_free | Auto_increment | Create_time         | Update_time         | Check_time          | Collation         | Checksum | Create_options | Comment                                           |
+---------------------------+--------+---------+------------+------+----------------+-------------+--------------------+--------------+-----------+----------------+---------------------+---------------------+---------------------+-------------------+----------+----------------+---------------------------------------------------+
| columns_priv              | MyISAM |      10 | Fixed      |    0 |              0 |           0 | 227994731135631359 |         4096 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_bin          |     NULL |                | Column privileges                                 |
| db                        | MyISAM |      10 | Fixed      |    0 |              0 |         880 | 123848989752688639 |         5120 |       880 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:55:21 | 2017-03-22 17:53:07 | utf8_bin          |     NULL |                | Database privileges                               |
| event                     | MyISAM |      10 | Dynamic    |    0 |              0 |           0 |    281474976710655 |         2048 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Events                                            |
| func                      | MyISAM |      10 | Fixed      |    0 |              0 |           0 | 162974011515469823 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_bin          |     NULL |                | User defined functions                            |
| general_log               | CSV    |      10 | Dynamic    |    2 |              0 |           0 |                  0 |            0 |         0 |           NULL | NULL                | NULL                | NULL                | utf8_general_ci   |     NULL |                | General log                                       |
| help_category             | MyISAM |      10 | Dynamic    |   39 |             28 |        1092 |    281474976710655 |         3072 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | help categories                                   |
| help_keyword              | MyISAM |      10 | Fixed      |  464 |            197 |       91408 |  55450570411999231 |        16384 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | help keywords                                     |
| help_relation             | MyISAM |      10 | Fixed      | 1028 |              9 |        9252 |   2533274790395903 |        19456 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | keyword-topic relation                            |
| help_topic                | MyISAM |      10 | Dynamic    |  508 |            886 |      450388 |    281474976710655 |        20480 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | help topics                                       |
| host                      | MyISAM |      10 | Fixed      |    0 |              0 |           0 | 110056715893866495 |         2048 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_bin          |     NULL |                | Host privileges;  Merged with database privileges |
| ndb_binlog_index          | MyISAM |      10 | Dynamic    |    0 |              0 |           0 |    281474976710655 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | latin1_swedish_ci |     NULL |                |                                                   |
| plugin                    | MyISAM |      10 | Dynamic    |    0 |              0 |           0 |    281474976710655 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | MySQL plugins                                     |
| proc                      | MyISAM |      10 | Dynamic    |    0 |              0 |         292 |    281474976710655 |         4096 |       292 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Stored Procedures                                 |
| procs_priv                | MyISAM |      10 | Fixed      |    0 |              0 |           0 | 239253730204057599 |         4096 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_bin          |     NULL |                | Procedure privileges                              |
| proxies_priv              | MyISAM |      10 | Fixed      |    2 |            693 |        1386 | 195062158860484607 |         5120 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | utf8_bin          |     NULL |                | User proxy privileges                             |
| servers                   | MyISAM |      10 | Fixed      |    0 |              0 |           0 | 433752939111120895 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | MySQL Foreign Servers table                       |
| slow_log                  | CSV    |      10 | Dynamic    |    2 |              0 |           0 |                  0 |            0 |         0 |           NULL | NULL                | NULL                | NULL                | utf8_general_ci   |     NULL |                | Slow log                                          |
| tables_priv               | MyISAM |      10 | Fixed      |    0 |              0 |           0 | 239535205180768255 |         4096 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_bin          |     NULL |                | Table privileges                                  |
| time_zone                 | MyISAM |      10 | Fixed      |    0 |              0 |           0 |   1970324836974591 |         1024 |         0 |              1 | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Time zones                                        |
| time_zone_leap_second     | MyISAM |      10 | Fixed      |    0 |              0 |           0 |   3659174697238527 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Leap seconds information for time zones           |
| time_zone_name            | MyISAM |      10 | Fixed      |    0 |              0 |           0 |  55450570411999231 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Time zone names                                   |
| time_zone_transition      | MyISAM |      10 | Fixed      |    0 |              0 |           0 |   4785074604081151 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Time zone transitions                             |
| time_zone_transition_type | MyISAM |      10 | Fixed      |    0 |              0 |           0 |  10696049115004927 |         1024 |         0 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:53:07 | NULL                | utf8_general_ci   |     NULL |                | Time zone transition types                        |
| user                      | MyISAM |      10 | Dynamic    |    4 |            107 |         532 |    281474976710655 |         2048 |       104 |           NULL | 2017-03-22 17:53:07 | 2017-03-22 17:55:11 | NULL                | utf8_bin          |     NULL |                | Users and global privileges                       |
+---------------------------+--------+---------+------------+------+----------------+-------------+--------------------+--------------+-----------+----------------+---------------------+---------------------+---------------------+-------------------+----------+----------------+---------------------------------------------------+
24 rows in set (0.00 sec)

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
create database metastore DEFAULT CHARACTER SET utf8;
grant all on metastore.* TO 'hive'@'%' IDENTIFIED BY 'hive_password';
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

***************************************************************

Step-9)

JDBC Connectors copy process :
==============================

scp -i my-aws-key.pem mysql-connector-java-5.1.41.tar.gz centos@ec2-34-203-200-66.compute-1.amazonaws.com:/home/centos/
sudo mkdir /usr/share/java
sudo cp mysql-connector-java-5.1.41-bin.jar /usr/share/java/mysql-connector-java.jar

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

scp -i my-aws-key.pem jdk-8u112-linux-i586.tar.gz centos@ec2-34-204-43-28.compute-1.amazonaws.com:/home/centos/
				 
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
baseurl=https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/5.9.0/
gpgkey =https://archive.cloudera.com/cm5/redhat/7/x86_64/cm/RPM-GPG-KEY-cloudera    
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

http://ec2-34-203-200-66.compute-1.amazonaws.com:7180
http://172.31.45.217:7180/

http://ec2-34-204-43-28.compute-1.amazonaws.com:7180

********************************************************************************************
					Cloudera Installation Completed !
********************************************************************************************
