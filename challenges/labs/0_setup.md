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
