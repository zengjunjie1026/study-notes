tar -zxvf jdk-8u144-linux-x64.tar.gz -C /opt/module/

vim  /etc/profile
#JAVA_HOME  
export JAVA_HOME=/opt/module/jdk1.8.0_144   
export PATH=$PATH:$JAVA_HOME/bin  

source /etc/profile