 - [x] Solr Commands https://bhagadepravin.github.io/commands/solr
 - [x] Ambari Commands https://bhagadepravin.github.io/commands/ambari
 - [x] Ranger Commands https://bhagadepravin.github.io/commands/ranger
 - [x] Haproxy Commands https://bhagadepravin.github.io/commands/haproxy
 - [X] SSL Commands https://bhagadepravin.github.io/commands/ssl
 - [X] Kafka Commands https://bhagadepravin.github.io/commands/kafka
 - [X] Useful Commands https://bhagadepravin.github.io/commands
 - [X] Setup_ssl_certs Commands https://bhagadepravin.github.io/commands/setup_ssl_certs
 - [X] Atlas Commands https://bhagadepravin.github.io/commands/atlas
 - [X] Kerberos Commands https://bhagadepravin.github.io/commands/kerberos
 - [X] Reset_Ranger_admin_password https://bhagadepravin.github.io/commands/Reset_Ranger_admin_password_to_default
 - [X] Hive LDAP  https://github.com/bhagadepravin/commands/blob/master/hive-ldap
 - [X] NiFi https://bhagadepravin.github.io/commands/nifi

```bash
env GZIP=-9 tar cvzf ambari-log.tar.gz <log file>
tail -f /var/log/cloudera-scm-server/cloudera-scm-server.log  | grep "Started Jetty server"
tshark -r /var/tmp/ldap.pcap -R frame.number==7 -V
/usr/bin/ambari-python-wrap -c 'import sys;print (sys.version)'
python -c 'import socket;print socket.getfqdn()'
find / -executable -name java
echo stat |nc localhost 2181 
openssl s_client -showcerts -connect hostname:port
echo -n | openssl s_client -connect ${knoxserver}:8443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > /tmp/knoxcert.crt
watch -n 1 'netstat -anp | grep `cat /var/run/knox/gateway.pid` | grep ESTABLISHED | wc -l' 
{GATEWAY_HOME}/bin/knoxcli.sh create-alias ldcSystemPassword --cluster hdp --value hadoop
ldapsearch -h <ldap-hostname> -p <port> -D <bind-dn> -w <bind_DN_password> -b <base_search> "(cn=<username>)"
strace -o /var/tmp/strace.keytool /usr/java/latest/bin/keytool -list -keystore keystore1.p12 -storepass PASSWORD

hadoop daemonlog -getlevel `hostname -f`:50070  org.apache.commons.httpclient.auth


hadoop daemonlog -setlevel `hostname -f`:50470  org.apache.commons.httpclient.auth DEBUG -protocol https

tshark -r /tmp/hdfs.pcap -O kerberos > /tmp/hdfs.out
```

```
sudo -u $USER bash
ex:
sudo -u impala bash
su -s /bin/bash impala
```

```
# useradd origin
# passwd origin
# echo -e 'Defaults:origin !requiretty\norigin ALL = (root) NOPASSWD:ALL' | tee /etc/sudoers.d/openshift
# chmod 440 /etc/sudoers.d/openshift
```
```
env GZIP=-9 tar czhvf ./knox_all_conf_$(hostname)_$(date +"%Y%m%d%H%M%S").tgz /usr/hdp/current/knox-server/conf/ /etc/ranger/*/policycache /usr/hdp/current/knox-server/data/deployments/ /var/log/knox/gateway.log /var/log/knox/gateway-audit.log 2>/dev/null
# keytool -import -file /tmp/adcert.crt -keystore $JAVA_HOME/jre/lib/security/cacerts -alias AD-Cert -storepass changeit
# curl -i -k -u Username:Password -X GET  'https://<KNOX-HOSTNAME>:8443/gateway/default/webhdfs/v1/?op=LISTSTATUS'

```
```
Login into Atlas node
export ATLAS_PROCESS_DIR=$(ls -1dtr /var/run/cloudera-scm-agent/process/*ATLAS_SERVER | tail -1)
ps auxwwf | grep atlas-ATLAS_SERVER > /tmp/atlas-ps.txt
env GZIP=-9  tar -cvzf atlas.tar.gz $ATLAS_PROCESS_DIR /var/log/atlas/application.log /tmp/atlas-ps.txt

 Login into the Zookeeper node.

export ZK_PROCESS_DIR=$(ls -1dtr /var/run/cloudera-scm-agent/process/*zookeeper-server | tail -1)
env GZIP=-9  tar -cvzf zookeeper.tar.gz $ZK_PROCESS_DIR /var/log/zookeeper/zookeeper-cmf-ZOOKEEPER-1-SERVER-`hostname -f`.log
```
```java
JAVA_HOME=/usr/jdk64/jdk1.8.0_112/
export PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME=/usr/jdk64/jdk1.8.0_112/
```

## SSL Poke Test
```java
wget https://confluence.atlassian.com/download/attachments/117455/SSLPoke.java
javac SSLPoke.java
java SSLPoke -Djavax.net.debug=ssl SSLPoke sme-2012-ad.support.com 636
java SSLPoke hostname port
```

## LDAP test
```java
https://github.com/hajimeo/samples/blob/master/java/LDAPTest.java
java -Djavax.net.debug=ssl,keymanager -Djavax.net.ssl.trustStore=/path/to/truststore.jks LDAPTest "ldap://ad.your-server.com:389" "dc=ad,dc=my-domain,dc=com" myLdapUsername myLdapPassword
Please enable certpath debugging (java -Djava.security.debug=certpath) 
```

## MyISAM to InnoDB using command:
alter table ranger.table_name Engine = InnoDB;

```sql
[ERROR] /usr/libexec/mysqld: Table './ranger/x_auth_sess' is marked as crashed and should be repaired 
mysql> repair table x_auth_sess; 
```

## tcpdump commands for ldap troubleshooting
```
tcpdump -i any -s 10000 -A 'host <ranger host> and port 9292'
tcpdump -ni any -nvvvXSs 4096 host localhost and port 389 -w tcpdump.pcap
tcpdump -ni any -nvvvXSs 4096 host localhost and port 8443 -w knox.pcap
tcpdump -ni any -nvvvXSs 4096 host sme-2012-ad.support.com and port 636 -w tcpdump.pcap
tcpdump -ni any -nvvvXSs 4096 host <host> and port 636 -w tcpdump.pcap

tcpdump -i eth0 -w /var/tmp/ldap3.pcap port 389 &
tcpdump -i any -tttt -w /var/tmp/ldap.pcap port 389 &

tshark -r /var/tmp/ldap.pcap
tshark -r /var/tmp/ldap.pcap -V
tshark -r /var/tmp/ldap.pcap  -frame == 7

tshark -i any -d tcp.port==88,kerberos -R kerberos -nVXs0

-i interface (an interface argument of ??????any??????)
-n Don???t convert host addresses to names.
-vvv Even more verbose output
-X When parsing and printing, in addition to printing the headers of each packet, print the data of each packet (minus its link level header) in hex and ASCII. This is very
-S Print absolute, rather than relative, TCP sequence numbers.
-s Snarf snaplen bytes of data from each packet rather than the default of 65535 bytes.
```


## Knox Hive beeline command
```
beeline -u "jdbc:hive2://KnoxserverInternalHostName:8443/;ssl=true;transportMode=http;httpPath=gateway/default/hive" -n <username> -p <password>
```

## OS cmds

```shell
ps auxwww > /tmp/ps_$(hostname)_$(date +'%Y%m%d%H%M%S').out 2>&1 
free -m > /tmp/free_$(hostname)_$(date +'%Y%m%d%H%M%S').out 2>&1 
top -b -c -n 1 > /tmp/top_$(hostname)_$(date +'%Y%m%d%H%M%S').out 2>&1 

( date;set -x;hostname -A;uname -a;top -b -n 1 -c;ps auxwwwf;netstat -aopen;ifconfig;iptables -t nat -nL;cat /proc/meminfo;df -h;mount;vmstat -d; ls -l /etc/security/keytabs/ ) &> /tmp/os_cmds.out; tar czhvf ./hdp_all_conf_$(hostname)_$(date +"%Y%m%d%H%M%S").tgz /usr/hdp/current/*/conf /etc/{ams,ambari}-* /etc/ranger/*/policycache /etc/hosts /etc/krb5.conf /tmp/os_cmds.out 2>/dev/null

( date;set -x;hostname -A;uname -a;top -b -n 1 -c;ps auxwwwf;netstat -aopen;ifconfig;iptables -t nat -nL;cat /proc/meminfo;df -h;mount;vmstat -d;vmstat 1 5 ) &> /tmp/os_cmds.out

env GZIP=-9 tar cvzf /tmp/ranger_admin.tar.gz /etc/ranger/admin/conf/* /var/log/ranger/admin/xa_portal.log /var/log/ranger/admin/catalina.out /tmp/os_cmds.out

( date;set -x;hostname -A;uptime;last reboot;df -h; mount;free -g ) &> /tmp/os_cmds.out  ; env GZIP=-9 tar czhvf ./hdp_$(hostname)_$(date +"%Y%m%d%H%M%S").tgz /var/log/messages

```

## Kerberos JCE verification

```bash
grep -i java_home /etc/hadoop/conf/hadoop-env.sh
JAVA_HOME=/usr/jdk64/jdk1.8.0_77
export JAVA_HOME=/usr/jdk64/jdk1.8.0_77
source /etc/hadoop/conf/hadoop-env.sh && zipgrep CryptoAllPermission $JAVA_HOME/jre/lib/security/local_policy.jar
source /etc/hadoop/conf/hadoop-env.sh && zipgrep CryptoAllPermission $JAVA_HOME/jre/lib/security/US_export_policy.jar
```

## delete alias from keystore
`keytool -delete -noprompt -alias ${cert.alias} -keystore ${keystore.file} -storepass ${keystore.pass}`

## Hadoop debug
```sh
export HADOOP_ROOT_LOGGER=DEBUG,console
export HADOOP_OPTS="-Dsun.security.krb5.debug=true ${HADOOP_OPTS}"
```

```
You can add the below parameters in the KMS service from the Cloudera manager.
Debug JVM
-Dsun.security.krb5.debug=true -Dsun.security.jgss.debug=true -Dsun.security.spnego.debug=true
export HADOOP_ROOT_LOGGER=TRACE,console;
export HADOOP_JAAS_DEBUG=true; export HADOOP_OPTS="-Dsun.security.krb5.debug=true"
```

## network tunning ####
```
lease tune below parameters and then test the Alteryx job. 
10 Gbits - Socket tunings. 

1. /etc/sysctl.conf 
net.core.rmem_max = 134217728 
net.core.wmem_max = 134217728 
net.ipv4.tcp_rmem = 4096 65536 134217728 
net.ipv4.tcp_wmem = 4096 65536 134217728 
net.core.netdev_max_backlog = 250000 
net.core.somaxconn = 4096 
net.ipv4.tcp_sack = 1 
net.ipv4.tcp_max_syn_backlog = 8192 
net.ipv4.tcp_syncookies = 1 

2. Reflect the above settings. 
#sysctl -p 

3. Increase the MTU value. 
/etc/sysconfig/network-scripts/<interface> 
MTU=9000 

4. Increase the txqueuelen. 
# ifconfig <interface> txqueuelen 10000 
```

## nfs gateway
```
hostname:/ on /HDFS_ROOT type nfs (rw,relatime,sync,vers=3,rsize=1048576,wsize=1048576,namlen=255,hard,nolock,proto=tcp,timeo=600,retrans=2,sec=sys,mountaddr=ip-address,mountvers=3,mountport=4242,mountproto=tcp,local_lock=all,addr=ip-address)
```

## Zepplein

Zepplein corrupt notebooks
```sh
grep ERROR zeppelin-zeppelin-xx.internal.log | awk '{print $8}' | awk -F'.' '{print $NF}' | grep -v for | grep -v 2019* | grep -v but | grep -v handle | sort | uniq
```
## Druid

```sh
su druid
kinit -kt /etc/security/keytabs/druid.headless.keytab $(klist -kt /etc/security/keytabs/druid.headless.keytab |sed -n "4p"|cut -d ' ' -f7)
curl -ik --negotiate -u :  'http://$(hostname -f):8081/druid/coordinator/v1/loadqueue?simple'
curl -ik --negotiate -u :  'http://$(hostname -f):8081/druid/indexer/v1/workers'
curl -ik --negotiate -u :  'http://$(hostname -f):8081/druid/coordinator/v1/loadstatus'
```

## clean up linux host
```
find /var -name "*.log" \( \( -size +50M -mtime +7 \) -o -mtime +30 \) -exec truncate {} --size 0 \;
yum clean all
rm -rf /var/cache/yum
rm -rf /var/tmp/yum-*
rm -rf /hadoop/yarn/local/usercache/*
rm -rf /var/lib/ambari-agent/tmp/*

hdfs dfs -rm -R -skipTrash /ranger/audit

du -sch *  /home
du -xh / |grep '^\S*[0-9\.]\+G'|sort -rn
```

## Zookeeper Debug

```bash

CLIENT_JVMFLAGS="-Djava.security.auth.login.config=./jaas.conf -Dsun.security.krb5.debug=true zookeeper-client -server <zkHost>:2181"


+++++++++

You can also enabled trace in log4j
# vi /etc/zookeeper/conf/log4j.properties
Uncomment the line which has log4j.rootLogger=TRACE
# zookeeper-client -server <zkHost>:2181
This will write file with name /etc/zookeeper/conf/zookeeper_trace.log
That will have DEBUG messages related to kerberos and zookeeper client log

```

## Zookeeper digest
```
# Using superDigest to become a Zookeeper superuser

export ZK_CLASSPATH=/etc/zookeeper/conf/*:/usr/hdp/current/zookeeper-server/lib/*:/usr/hdp/current/zookeeper-server/* 
java -cp $ZK_CLASSPATH org.apache.zookeeper.server.auth.DigestAuthenticationProvider super:hadoop

# From the output we can just add the following to SERVER_JVMFLAGS and restart Zookeeper:

SERVER_JVMFLAGS=-Dzookeeper.DigestAuthenticationProvider.superDigest=super:QczWs9XWUeidfNqiyCcD6Dy2ORw=


Then, in zkCli do:

[zk: sandbox.hortonworks.com:2181(CONNECTED) 1] addauth digest super:hadoop
```

## Snapshot

```

login into Zookeeper node:

cd /hadoop/zookeeper/version-2/   #or where they have snapshots

java -cp /usr/hdp/current/zookeeper-server/zookeeper.jar:/usr/hdp/current/zookeeper-server/lib/* org.apache.zookeeper.server.SnapshotFormatter snapshot.c0088bd4e9 >  snapshot.1b000385da.txt

$ cat snapshot.1b000385da.txt| grep -A2 /zkdtsm/ZKDTSMRoot/ZKDTSMTokensRoot/DT | grep ctime | awk '{print $8", "$4" "$5}'|sort| uniq -c >> DT_list.txt

less > DT_list.txt

same goes for latest the transaction logs 


java -cp /usr/hdp/current/zookeeper-server/zookeeper.jar:/usr/hdp/current/zookeeper-server/lib/* org.apache.zookeeper.server.SnapshotFormatter snapshot.c0088bd4e9 | grep -i '/zkdtsm/ZKDTSMRoot/ZKDTSMTokensRoot/DT' | wc -l

```


## kerberos debug renewal

```
a) What JDK version you are using in the cluster? 
b) Below command outputs:

    # ps -ef | grep ranger
    # JAVA_HOME/bin/jrunscript -e 'print (javax.crypto.Cipher.getMaxAllowedKeyLength("AES") >= 256);'         
        // use the correct path if JAVA_HOME undefined eg: /usr/java/
c) To test the connectivity:

    # kdestroy
    # cd /var/run/cloudera-scm-agent/process/<nnnn>-ranger-RANGER_ADMIN      // use the latest directory 
    # kinit -kt ranger.keytab rangeradmin/_HOST
    # klist -ef 
    # kinit -R  ==> this will confirm if the tickets are renewable

If kinit -R fails, share the following output:
    # KRB5_TRACE=/dev/stdout kinit -R  
```


### CM
```
vim /etc/default/cloudera-scm-server

For CM add the following to /etc/default/cloudera-scm-server, e.g

export CMF_JAVA_OPTS="-Xmx2G -XX:MaxPermSize=256m -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp -Dcom.sun.jndi.ldap.object.disableEndpointIdentification=true"
```


## ldaps
```
1. Add the following AD server to your /etc/hosts file on each host :    
2. Install openldap services:
sudo yum -y install openldap-clients ca-certificates

3. Add your AD certificate to your hosts:
 openssl s_client -connect <AD-hostname>:636 <<<'' | openssl x509 -out /etc/pki/ca-trust/source/anchors/ad01.crt
  openssl s_client -connect <AD-hostname>:636 <<<'' | openssl x509 -out /etc/pki/tls/cert.pem

4.Update catrust certificates:
sudo update-ca-trust force-enable
sudo update-ca-trust extract
sudo update-ca-trust check

5) Add your ad server to be trusted:

sudo tee -a /etc/openldap/ldap.conf > /dev/null << EOF
TLS_CACERT /etc/pki/tls/cert.pem
URI ldaps://<AD-hostname> ldap://<AD-hostname>
BASE <searchbase >
EOF

6) Test connection to AD using openssl client:
openssl s_client -connect <AD-hostname>:636 </dev/null

```
