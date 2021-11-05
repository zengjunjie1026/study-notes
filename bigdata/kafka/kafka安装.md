tar -zxvf kafka_2.11-0.11.0.0.tgz -C /opt/module/  

mv kafka_2.11-0.11.0.0/ kafka

cd kafka

mkdir logs

cd config/

vim server.properties

```bash
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# see kafka.server.KafkaConfig for additional details and defaults

############################# Server Basics #############################

# The id of the broker. This must be set to a unique integer for each broker.
broker.id=0

############################# Socket Server Settings #############################


#broker.id=0
#删除 topic 功能使能 
delete.topic.enable=true #处理网络请求的线程数量 
num.network.threads=3
#用来处理磁盘 IO 的现成数量 
num.io.threads=8 
#发送套接字的缓冲区大小 
socket.send.buffer.bytes=102400 
#接收套接字的缓冲区大小 
socket.receive.buffer.bytes=102400 
#请求套接字的缓冲区大小 socket.request.max.bytes=104857600 
#kafka 运行日志存放的路径 
log.dirs=/opt/module/kafka/logs 
#topic 在当前 broker 上的分区个数 
num.partitions=1
#用来恢复和清理 data 下数据的线程数量 
num.recovery.threads.per.data.dir=1

```