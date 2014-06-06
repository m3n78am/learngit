###### 安装hadoop2.2.0

+ 安装jdk@3台机器
	- yum search java | grep 'java-'
	- yum install java-1.7.0-openjdk.x86_64

+ 生成ssh pub key@bj-hadoop1
	- ssh-keygen
	
+ 免密码登录@bj-hadoop1
	- ssh-copy-id -i .ssh/id_rsa.pub humingqiang@bj-hadoop2
	- ssh-copy-id -i .ssh/id_rsa.pub humingqiang@bj-hadoop3

+ 设置$JAVA_HOME@3台机器
	- export JAVA_HOME='/usr/lib/jvm/jre-1.7.0-openjdk.x86_64'

+ 增加目录@3台机器
	- sudo mkdir /data/user/hadoop/namenode,datanode,namenodesecondary (用来存放持久化的数据,设置在hdfs-site.xml文件中)
	- sudo chown humingqiang /data/user/hadoop/ (需要有写权限,否则无法启动dfs，需要拥有者才能启动dfs，否则报错  WARN org.apache.hadoop.hdfs.server.datanode.DataNode: Invalid dfs.datanode.data.dir /data/users/hadoop/datanode :
EPERM: Operation not permitted)

+ 本地库需要自己编译，编译的过程中需要修复一个补丁
	1. cd hadoop-2.2.0-src
	1. wget https://issues.apache.org/jira/secure/attachment/12614482、HADOOP-10110.patch
	1. patch -p0 < HADOOP-10110.patch


#### 遇到的问题

1. 不能连接通常是因为host文件将启动的ip绑成了127.0.0.1  telnet master 10001，查看是否连接的通
2. 如果不能连接通，查看namenode上的netstat -ano|gerp 10001是否只是监听的127.0.0.1的10001端口，如果是的话，修改/etc/hosts,将127.0.0.1 bj-hadoop 这行注释之，所有机器都是要注释的
3. 关闭safemode  hadoop dfsadmin -safemode leave
4. 自己编译之后，将.bashrc中变量HADOOP_OPTS="-Djava.library.path=$HADOOP_PREFIX/lib/Native64"
5. 重新格式化
	<pre> 
	rm * /tmp
	rm * $HADOOP_PREFIX/logs
	rm * ${dfs.namenode.name.dir}
	rm * ${dfs.datanode.data.dir}
	rm * ${dfs.namenode.checkpoint.dir}
	
	hadoop namenode -format
	start-all.sh
	</pre>
6.  跑mapreduce程序的时候出现启动无法执行，然后bj-hadoop1:8088 下unhealthy node为所有，可能就是因为没有权限写 ${hadoop.tmp.dir}
7.  


##### hdfs-site.xml下得参数

1. dfs.namenode.checkpoint.dir 是存放namenodesecondary的地方，current文件下存放有edits文件和fsimage文件
2. dfs.namenode.name.dir 是namenode持久化的地方
7. dfs.datanode.data.dir 是datanode持久化的地方
8. hadoop.tmp.dir 是临时文件存放的地方
9. 