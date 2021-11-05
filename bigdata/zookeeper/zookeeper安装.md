本地模式安装部署  
1.安装前准备   
(1)安装 Jdk  
(2)拷贝 Zookeeper 安装包到 Linux 系统下   
(3)解压到指定目录
[andrew@hadoop101 software]$ tar -zxvf zookeeper-3.4.10.tar.gz -C /opt/module/  

2.配置修改
(1)将/opt/module/zookeeper-3.4.10/conf 这个路径下的 zoo_sample.cfg 修改为 zoo.cfg;  
[andrew@hadoop101 conf]$ mv zoo_sample.cfg zoo.cfg


(2)打开 zoo.cfg 文件，修改 dataDir 路径: 
[andrew@hadoop101 zookeeper-3.4.10]$ vim zoo.cfg  
修改如下内容:
dataDir=/opt/module/zookeeper-3.4.10/zkData   
(3)在/opt/module/zookeeper-3.4.10/这个目录上创建 zkData 文件夹  


[andrew@hadoop101 zookeeper-3.4.10]$ mkdir zkData  
3.操作 Zookeeper   
(1)启动 Zookeeper  

[andrew@hadoop101 zookeeper-3.4.10]$ bin/zkServer.sh start  
(2)查看进程是否启动  
[andrew@hadoop101 zookeeper-3.4.10]$ jps
4020 Jps
4001 QuorumPeerMain

(3)查看状态:  
[andrew@hadoop101 zookeeper-3.4.10]$ bin/zkServer.sh status
ZooKeeper JMX enabled by default
Using config: /opt/module/zookeeper-3.4.10/bin/../conf/zoo.cfg
Mode: standalone

(4)启动客户端:  
[andrew@hadoop101 zookeeper-3.4.10]$ bin/zkCli.sh  

(5)退出客户端:    
[zk: localhost:2181(CONNECTED) 0] quit    

(6)停止 Zookeeper    
[andrew@hadoop101 zookeeper-3.4.10]$ bin/zkServer.sh stop  



Zookeeper中的配置文件zoo.cfg中参数含义解读如下:  
1.tickTime =2000:通信心跳数，Zookeeper 服务器与客户端心跳时间，单位毫秒  
Zookeeper使用的基本时间，服务器之间或客户端与服务器之间维持心跳的时间间隔， 
也就是每个tickTime时间就会发送一个心跳，时间单位为毫秒。  
它用于心跳机制，并且设置最小的session超时时间为两倍心跳时间。(session的最小超 时时间是2*tickTime)  
2.initLimit =10:LF 初始通信时限 集群中的Follower跟随者服务器与Leader领导者服务器之间初始连接时能容忍
的最多心  
跳数(tickTime的数量)，用它来限定集群中的Zookeeper服务器连接到Leader的时限。 
3.syncLimit =5:LF 同步通信时限  
集群中Leader与Follower之间的最大响应时间单位，假如响应超过syncLimit * tickTime， 
Leader认为Follwer死掉，从服务器列表中删除Follwer。
4.dataDir:数据文件目录+数据持久化路径 主要用于保存 Zookeeper 中的数据。  
5.clientPort =2181:客户端连接端口 监听客户端连接的端口。  