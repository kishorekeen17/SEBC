1) complete first line from your server log

2017-03-24 18:23:12,226 INFO main:com.cloudera.server.cmf.Main: Starting SCM Server. JVM Args: [-Dlog4j.configuration=file:/etc/cloudera-scm-server/log4j.properties, -Dfile.encoding=UTF-8, -Dcmf.root.logger=INFO,LOGFILE, -Dcmf.log.dir=/var/log/cloudera-scm-server, -Dcmf.log.file=cloudera-scm-server.log, -Dcmf.jetty.threshhold=WARN, -Dcmf.schema.dir=/usr/share/cmf/schema, -Djava.awt.headless=true, -Djava.net.preferIPv4Stack=true, -Dpython.home=/usr/share/cmf/python, -XX:+UseConcMarkSweepGC, -XX:+UseParNewGC, -XX:+HeapDumpOnOutOfMemoryError, -Xmx2G, -XX:MaxPermSize=256m, -XX:+HeapDumpOnOutOfMemoryError, -XX:HeapDumpPath=/tmp, -XX:OnOutOfMemoryError=kill -9 %p], Args: [], Version: 5.9.0 (#249 built by jenkins on 20161006-1801 git: 898a5e032c080e18833dfc58180761f6ef2ea351)

2) complete line that contains the phrase "Started Jetty server"

2017-03-24 18:24:01,597 INFO WebServerImpl:com.cloudera.server.cmf.WebServerImpl: Started Jetty server.

3) db.properties fil econtent 

content => 

com.cloudera.cmf.db.type=mysql
com.cloudera.cmf.db.host=172.31.28.12
com.cloudera.cmf.db.name=kishore
com.cloudera.cmf.db.user=root
com.cloudera.cmf.db.password=kishore
com.cloudera.cmf.db.setupType=EXTERNAL