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
2.4.2 配置 Metastore 到 MySQL 1)在$HIVE_HOME/conf 目录下新建 hive-site.xml 文件  
    [andrew@hadoop101 software]$ vim $HIVE_HOME/conf/hive-site.xml  
添加如下内容  
   [andrew@hadoop101 software]$ cp /opt/software/mysql-connector-java-5.1.37.jar $HIVE_HOME/lib  
```xml
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
<!-- jdbc 连接的 URL --> <property>
       <name>javax.jdo.option.ConnectionURL</name>
       <value>jdbc:mysql://hadoop101:3306/metastore?useSSL=false</value>
   </property>
<!-- jdbc 连接的 Driver--> <property>
       <name>javax.jdo.option.ConnectionDriverName</name>
       <value>com.mysql.jdbc.Driver</value>
   </property>
<!-- jdbc 连接的 username--> <property>
       <name>javax.jdo.option.ConnectionUserName</name>
       <value>root</value>
   </property>
<!-- jdbc 连接的 password --> <property>
       <name>javax.jdo.option.ConnectionPassword</name>
<value>000000</value>
</property>
<!-- Hive 元数据存储版本的验证 --> <property>
   <name>hive.metastore.schema.verification</name>
   <value>false</value>
</property>
<!--元数据存储授权--> <property>
   <name>hive.metastore.event.db.notification.api.auth</name>
   <value>false</value>
</property>
<!-- Hive 默认在 HDFS 的工作目录 --> <property>
        <name>hive.metastore.warehouse.dir</name>
       <value>/user/hive/warehouse</value>
   </property>
</configuration>
```
2)登陆 MySQL  
[atguigu@hadoop102 software]$ mysql -uroot -p000000  
3)新建 Hive 元数据库


2)登陆 MySQL 
[andrew@hadoop101 software]$ mysql -uroot -p000000  
3)新建 Hive 元数据库  

