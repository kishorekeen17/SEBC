1) List the command and output

command => ls -lrt /etc/yum.repos.d
output => 
-rw-r--r--. 1 root root 1056 Nov  5  2012 epel-testing.repo
-rw-r--r--. 1 root root  957 Nov  5  2012 epel.repo
-rw-r--r--  1 root root 1060 Dec  2  2013 mysql-community-source.repo
-rw-r--r--  1 root root 1209 Dec  2  2013 mysql-community.repo
-rw-r--r--  1 root root 6259 May 18  2016 CentOS-Vault.repo
-rw-r--r--  1 root root  630 May 18  2016 CentOS-Media.repo
-rw-r--r--  1 root root  289 May 18  2016 CentOS-fasttrack.repo
-rw-r--r--  1 root root  647 May 18  2016 CentOS-Debuginfo.repo
-rw-r--r--  1 root root 1991 May 18  2016 CentOS-Base.repo
-rw-r--r--  1 root root  293 Mar 24 18:01 cloudera-manager.repo

2) sudo /usr/share/cmf/schema/scm_prepare_database.sh mysql kishore root kishore