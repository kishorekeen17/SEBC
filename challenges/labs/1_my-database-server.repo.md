1) The hostname of your db server node 
Ans : 172.31.19.230

2) The command and output showing your database server's version

Query  : SHOW VARIABLES LIKE "%version%";
Result : 
+-------------------------+------------------------------+
| Variable_name           | Value                        |
+-------------------------+------------------------------+
| innodb_version          | 5.6.35                       |
| protocol_version        | 10                           |
| slave_type_conversions  |                              |
| version                 | 5.6.35-log                   |
| version_comment         | MySQL Community Server (GPL) |
| version_compile_machine | x86_64                       |
| version_compile_os      | Linux                        |
+-------------------------+------------------------------+

3) The command and output for listing your databases
Query : show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| hive               |
| installbackups     |
| mysql              |
| oozie              |
| performance_schema |
| rman               |
| scm                |
| sentry             |
+--------------------+

4) 