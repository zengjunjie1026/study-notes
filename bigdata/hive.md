#### Hive安装 
##### 2.1 Hive 安装地址
1)Hive 官网地址 http://hive.apache.org/  
2)文档查看地址 https://cwiki.apache.org/confluence/display/Hive/GettingStarted  
3)下载地址 http://archive.apache.org/dist/hive/  
4)github 地址 https://github.com/apache/hive


##### 2.2Hive 安装部署 2.2.1 安装 Hive
1)把 apache-hive-3.1.2-bin.tar.gz 上传到 linux 的/opt/software 目录下   
2)解压 apache-hive-3.1.2-bin.tar.gz 到/opt/module/目录下面  
3)修改 apache-hive-3.1.2-bin.tar.gz 的名称为 hive   
4)修改/etc/profile.d/my_env.sh，添加环境变量  
    [andrw@hadoop101 software]$ sudo vim /etc/profile.d/my_env.sh  
5)添加内容  
6)解决日志 Jar 包冲突   
7)初始化元数据库  
    [andrw@hadoop101 hive]$ bin/schematool -dbType derby -initSchema  
2.2.2 启动并使用 Hive 1)启动 Hive  
    [andrw@hadoop101 hive]$ bin/hive  
2)使用 Hive  
3)在 CRT 窗口中开启另一个窗口开启 Hive，在/tmp/andrw 目录下监控 hive.log 文件  
    [andrw@hadoop101 software]$ tar -zxvf /opt/software/apache-hive-3.1.2-bin.tar.gz -C /opt/module/  
    [andrw@hadoop101 software]$ mv /opt/module/apache-hive-3.1.2-bin/ /opt/module/hive  

#HIVE_HOME
export HIVE_HOME=/opt/module/hive  
export PATH=$PATH:$HIVE_HOME/bin  
[andrw@hadoop101 software]$ mv $HIVE_HOME/lib/log4j-slf4j-impl-2.10.0.jar $HIVE_HOME/lib/log4j-slf4j-impl-2.10.0.bak  
hive> show databases;  
hive> show tables;  
hive> create table test(id int);  
hive> insert into test values(1);  
hive> select * from test;  
Caused by: ERROR XSDB6: Another instance of Derby may have already booted  
the database /opt/module/hive/metastore_db.at  
org.apache.derby.iapi.error.StandardException.newException(Unknown  
Source)  
       at  
org.apache.derby.iapi.error.StandardException.newException(Unknown  
Source) at  
org.apache.derby.impl.store.raw.data.BaseDataFileFactory.privGetJBMSLockO  
nDB(Unknown Source)  
       at  
org.apache.derby.impl.store.raw.data.BaseDataFileFactory.run(Unknown  
Source)  
...
原因在于 Hive 默认使用的元数据库为 derby，开启 Hive 之后就会占用元数据库，且不与 其他客户端共享数据，所以我们需要将 Hive 的元数据地址改为 MySQL。

2.4 Hive 元数据配置到 MySQL 2.4.1 拷贝驱动  
将 MySQL 的 JDBC 驱动拷贝到 Hive 的 lib 目录下  
2.4.2 配置 Metastore 到 MySQL   
1)在$HIVE_HOME/conf 目录下新建 hive-site.xml 文件  
    [andrew@hadoop101 software]$ vim $HIVE_HOME/conf/hive-site.xml  
添加如下内容  
   [andrew@hadoop101 software]$ cp /opt/software/mysql-connector-java-5.1.37.jar $HIVE_HOME/lib  
```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<!-- jdbc 连接的 URL --> 
    <property>
       <name>javax.jdo.option.ConnectionURL</name>
       <value>jdbc:mysql://hadoop101:3306/metastore?useSSL=false</value>
   </property>
<!-- jdbc 连接的 Driver--> 
    <property>
       <name>javax.jdo.option.ConnectionDriverName</name>
       <value>com.mysql.jdbc.Driver</value>
   </property>
<!-- jdbc 连接的 username--> 
    <property>
       <name>javax.jdo.option.ConnectionUserName</name>
       <value>root</value>
   </property>
<!-- jdbc 连接的 password --> 
    <property>
       <name>javax.jdo.option.ConnectionPassword</name>
       <value>000000</value>
    </property>
<!-- Hive 元数据存储版本的验证 --> 
    <property>
        <name>hive.metastore.schema.verification</name>
        <value>false</value>
    </property>
<!--元数据存储授权--> 
    <property>
        <name>hive.metastore.event.db.notification.api.auth</name>
        <value>false</value>
    </property>
<!-- Hive 默认在 HDFS 的工作目录 --> 
    <property>
        <name>hive.metastore.warehouse.dir</name>
        <value>/user/hive/warehouse</value>
   </property>
</configuration>
```
2)登陆 MySQL  
[andrew@hadoop102 software]$ mysql -uroot -p000000  
3)新建 Hive 元数据库
mysql> create database metastore chartset utf8mb4;
mysql> quit;

4) 初始化 Hive 元数据库
schematool -initSchema -dbType mysql -verbose

2.4.3 再次启动 Hive 

1)启动 Hive
hive> show databases;
hive> show tables;
hive> create table test (id int);
hive> insert into test values(1);
hive> select * from test;


2.5 使用元数据服务的方式访问 Hive 

1)在 hive-site.xml 文件中添加如下配置信息
<!-- 指定存储元数据要连接的地址 -->
```xml
 <property>
   <name>hive.metastore.uris</name>
   <value>thrift://hadoop102:9083</value>
</property>
```
2)启动 metastore
 [andrew@hadoop101 hive]$ hive --service metastore 
 2020-04-24 16:58:08: Starting Hive Metastore Server 
 注意: 启动后窗口不能再操作，需打开一个新的 shell 窗口做别的操作
 
 
3)启动 hive
[andrew@hadoop101 hive]$ bin/hive


2.6 使用 JDBC 方式访问 Hive
1)在 hive-site.xml 文件中添加如下配置信息
```xml
 <!-- 指定 hiveserver2 连接的 host --> 
<property>
   <name>hive.server2.thrift.bind.host</name>
    <value>hadoop102</value>
</property>
<!-- 指定 hiveserver2 连接的端口号 --> 
<property>
   <name>hive.server2.thrift.port</name>
   <value>10000</value>
</property>
```

2)启动 hiveserver2
bin/hive --service hiveserver2


3)启动 beeline 客户端(需要多等待一会)
beeline -u jdbc:hive2://hadoop101:10000 -n andrew

4)看到如下界面
Connecting to jdbc:hive2://hadoop101:10000
Connected to: Apache Hive (version 3.1.2)
Driver: Hive JDBC (version 3.1.2)
Transaction isolation: TRANSACTION_REPEATABLE_READ
Beeline version 3.1.2 by Apache Hive
0: jdbc:hive2://hadoop101:10000>


nohup hive --service metastore 2>&1 &
nohup hive --service hiveserver2 2>&1 &
 
 
打印当前数据库名和表头
```xml
<property>
   <name>hive.cli.print.header</name>
   <value>true</value>
</property>
<property>
   <name>hive.cli.print.current.db</name>
   <value>true</value>
</property>
```

查看当前设置
hive>set;

comment 中文乱码

①修改表字段注解和表注解
alter table COLUMNS_V2 modify column COMMENT varchar(256) character set utf8
alter table TABLE_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8
② 修改分区字段注解：
alter table PARTITION_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8 ;
alter table PARTITION_KEYS modify column PKEY_COMMENT varchar(4000) character set utf8;
③修改索引注解：
alter table INDEX_PARAMS modify column PARAM_VALUE varchar(4000) character set utf8;

