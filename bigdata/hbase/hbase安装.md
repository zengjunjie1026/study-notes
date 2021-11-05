### 2.1 HBase 安装部署
##### 2.1.1 Zookeeper 正常部署

首先保证 Zookeeper 集群的正常部署，并启动之:

[atguigu@hadoop102 zookeeper-3.4.10]$ bin/zkServer.sh start 
[atguigu@hadoop103 zookeeper-3.4.10]$ bin/zkServer.sh start 
[atguigu@hadoop104 zookeeper-3.4.10]$ bin/zkServer.sh start


##### 2.1.2 Hadoop 正常部署 Hadoop 集群的正常部署并启动:
[atguigu@hadoop102 hadoop-2.7.2]$ sbin/start-dfs.sh 
[atguigu@hadoop103 hadoop-2.7.2]$ sbin/start-yarn.sh

##### 2.1.3 HBase 的解压 解压 Hbase 到指定目录:
[atguigu@hadoop102 software]$ tar -zxvf hbase-1.3.1-bin.tar.gz -C /opt/module





2.1.4 HBase 的配置文件 
修改 HBase 对应的配置文件。

 1)hbase-env.sh 修改内容: 2)hbase-site.xml 修改内容:
vim ~/.bashrc  
export JAVA_HOME=/opt/module/jdk1.8.0_144   
export HBASE_MANAGES_ZK=false 


hbase-site.xml 修改内容
```xml

<configuration>
    <property>
        <name>hbase.rootdir</name>
	<value>hdfs://localhost:8020/hbase</value>
    </property>

    <property>
	<name>hbase.cluster.distributed</name>
	<value>true</value>
    </property>
    <!-- 0.98 后的新变动，之前版本没有.port,默认端口为 60000 -->
    <property>
	<name>hbase.master.port</name>
	<value>16000</value>
    </property>
    <!--  -->
    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>localhost:2181</value>
    </property>
    <!-- -->
    <property>
	<name>hbase.zookeeper.property.dataDir</name>
	<value>/opt/module/zookeeper-3.5.7/zkData</value>
    </property>

    <property>
        <name>hbase.unsafe.stream.capability.enforce</name>
        <value>false</value>
    </property>
   <!-- <property>
      <name>phoenix.schema.isNamespaceMappingEnabled</name>
      <value>true</value>
    </property>
    <property>
    <name>phoenix.schema.mapSystemTablesToNamespace</name>
      <value>true</value>
   </property> -->
</configuration>

```
 
vim regionservers

localhost

启动hbase

bin/hbase-daemon.sh start 