## 官网两个map-reduce程序解读

官网教程：  
https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html

中文版老教程参考理解：  
http://hadoop.apache.org/docs/r1.0.4/cn/mapred_tutorial.html  

## 环境

NameNode: localhost:9870 
ResourceManager: localhost:8088   

## 测试步骤

1、下载hadoop安装包，注意不是源码包  
2、按伪分布式模式配置好  
https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-common/SingleCluster.html  
3、启动hadoop  
sbin/start-dfs.sh  
sbin/start-yarn.sh  
4、export HADOOP_CLASSPATH=${JAVA_HOME}/lib/tools.jar  
5、打包  
在安装包下建WordCount.java文件  
bin/hadoop com.sun.tools.javac.Main WordCount.java  
jar cf wc.jar WordCount*.class  
6、准备需要统计单词的文件  
建目录：  
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/joe  
bin/hdfs dfs -mkdir /user/joe/wordcount  
bin/hdfs dfs -mkdir /user/joe/wordcount/input  
bin/hdfs dfs -mkdir /user/joe/wordcount/output  

准备文件：  
echo "Hello World Bye World" > file01  
echo "Hello Hadoop Goodbye Hadoop" > file02  

上传文件：  
bin/hdfs dfs -put file01 /user/joe/wordcount/input  
bin/hdfs dfs -put file02 /user/joe/wordcount/input  

检查：
bin/hadoop fs -cat /user/joe/wordcount/input/file01  
bin/hadoop fs -cat /user/joe/wordcount/input/file02  
7、运行程序  
bin/hadoop jar wc.jar WordCount /user/joe/wordcount/input /user/joe/wordcount/output  
8、ResourceManager中出现wordcount任务  
9、命令行查看结果  
bin/hadoop fs -cat /user/joe/wordcount/output/part-r-00000

## 碰到问题

【ok】浏览器端无法删除目录  
bin/hadoop fs -rm -r -skipTrash /user/joe/wordcount/output