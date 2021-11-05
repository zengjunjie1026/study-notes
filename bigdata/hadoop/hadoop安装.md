Hadoop 下载地址:https://archive.apache.org/dist/hadoop/common/hadoop-3.1.3/

cd /opt/software/

解压安装文件到/opt/module 下面
[andrew@hadoop101 software]$ tar -zxvf hadoop-3.1.3.tar.gz -C /opt/module/

将 Hadoop 添加到环境变量
vim ~/.bashrc

#HADOOP_HOME  
export HADOOP_HOME=/opt/module/hadoop-3.1.3   
export PATH=$PATH:$HADOOP_HOME/bin  
export PATH=$PATH:$HADOOP_HOME/sbin   

source ~/.bashrc


修改以下文件
vim mapred-site.xml

```xml
<configuration>
    <property>
	<name>mapreduce.framework.name</name>
	<value>yarn</value>
    </property>

     <property>
	 <name>mapreduce.jobhistory.address</name>
         <value>localhost:10020</value>
     </property>
     <property>
	 <name>mapreduce.jobhistory.webapp.address</name>
         <value>localhost:19888</value>
     </property>

</configuration>

```

vim yarn-site.xml

```xml
<configuration>

<!-- Site specific YARN configuration properties -->
    <!--MR shuffle -->
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>1</value>
    </property>
    <property>
        <name>yarn.scheduler.maximum-allocation-mb</name>
        <value>4096</value>
    </property>

    <property>
        <name>yarn.nodemanager.resource.memory-mb</name>
        <value>4096</value>
    </property>

    <!-- 关闭yarn对物理内存和虚拟内存的限制检查 -->
    <property>
        <name>yarn.nodemanager.pmen-check-enabled</name>
        <value>false</value>
    </property>

    <property>
        <name>yarn.nodemanager.vmen-check-enabled</name>
        <value>false</value>
    </property>

</configuration>

```


vim hdfs-site.xml

```xml
<configuration>
    <!--nn web访问 -->
    <property>
        <name>dfs.namenode.http-address</name>
        <value>localhost:9870</value>
    </property>

     <!--2nn web访问 -->
    <property>
        <name>dfs.namenode.secondary.http-address</name>
        <value>localhost:9868</value>
    </property>

     <!-- 指定hdfs副本数量 -->
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>

    <property>
        <name>dfs.datanode.data.dir</name>
	<value>file:///opt/module/hadoop-3.1.3/data/dfs/data</value>
    </property>
</configuration>
```

vim workers
localhost


vim core-site.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!--
  Licensed under the Apache License, Version 2.0 (the "License");
  you may not use this file except in compliance with the License.
  You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License. See accompanying LICENSE file.
-->

<!-- Put site-specific property overrides in this file. -->

<configuration>

	<!--指定NameNode的地址 -->
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:8020</value>
	</property>

        <!-- hadoop数据存放目录 -->
         <property>
         	<name>hadoop.tmp.dir</name>
                <value>/opt/module/hadoop-3.1.3/data</value>
         </property>

        <!--HDFS网页登入使用的的静态用户为andrew -->
        <property>
        	<name>hadoop.http.staticuser.user</name>
                <value>andrew</value>
        </property>

        <!-- 配置andrew允通过代理访问主机节点 -->
        <property>
         	<name>hadoop.proxyuser.andrew.hosts</name>
                <value>*</value>
        </property>

        <!-- 配置andrew允通过代理访问主机节点 -->
        <property>
                <name>hadoop.proxyuser.andrew.groups</name>
                <value>*</value>
	</property>

        <property>
		<name>io.compression.codecs</name>
		<value>
			org.apache.hadoop.io.compress.GzipCodec,
			org.apache.hadoop.io.compress.DefaultCodec,
			org.apache.hadoop.io.compress.BZip2Codec,
			org.apache.hadoop.io.compress.SnappyCodec,
			com.hadoop.compression.lzo.LzoCodec,
			com.hadoop.compression.lzo.LzopCodec
		</value>
	</property>
	<property>
		<name>io.compression.codec.lzo.class</name>
		<value>com.hadoop.compression.lzo.LzoCodec</value>
	</property>
</configuration>
```

vim hadoop-env.sh

```shell script
export HDFS_NAMENODE_USER=andrew
export HDFS_DATANODE_USER=andrew
export HDFS_SECONDARYNAMENODE_USER=andrew
export YARN_RESOURCEMANAGER_USER=andrew
export YARN_NODEMANAGER_USER=andrew
export HDFS_JOURNALNODE_USER=andrew
export HDFS_ZKFC_USER=andrew
export HADOOP_SHELL_EXECNAME=andrew

```