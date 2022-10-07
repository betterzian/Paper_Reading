# 1、修改host文件
`vim /etc/hosts`  
```
<MASTER-IP> master
<SLAVE01-IP> slave01
<SLAVE02-IP> slave02
<SLAVE03-IP> slave03
<SLAVE04-IP> slave04
```

# 2、centos7安装JAVA 11
`yum search java-11`
其中，java-11-openjdk-devel为jdk版本（即开发环境），java-11-openjdk为jre版本（即运行环境）  
具体细节参考：[centos7如何安装java11](https://wxnacy.com/2018/12/27/centos7-install-java11/)

# 3、配置无密码SSH
主节点生成密钥：  
`ssh-keygen -t rsa -P ""`  
然后将密钥发送到奴隶节点：  
`scp -P port id_rsa.pub root@IP:~/.ssh/id_rsa.pub`  
再在每个奴隶节点上运行：  
`cat .ssh/id_rsa.pub > .ssh/authorized_keys`   
(A节点将公钥发给B节点，B节点加入授权后，A可以直接访问B)  

# 4、主节点安装配置spark
`wget https://dlcdn.apache.org/spark/spark-3.2.2/spark-3.2.2-bin-hadoop3.2.tgz`  
`tar zxvf spark-3.2.2-bin-hadoop3.2.tgz`  
在spark目录下的conf文件夹下面：  
`cp spark-env.sh.template spark-env.sh`  
`vim spark-env.sh`  
新增以下环境变量：（对于yum安装的则不需要）
```
export JAVA_HOME=<path-of-Java-installation> (eg: /usr/lib/jvm/java-7-oracle/)
export SPARK_WORKER_CORES=8
```
然后配置slave文件（现在改名为worker）：  
`cp workers.template workers`  
`vim workers`  
添加在host中存在的slave节点的名字。  

# 5、配置slave节点
在每个slave节点安装java，然后下载spark即可。