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