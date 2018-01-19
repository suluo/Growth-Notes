### Map-Reduce {#map-reduce}

```
-- hadoop string
-- pig
-- spark shell
-- spark submit
```

#### Hadoop string {#hadoop-string}

优点：简单易用，可以用python，迅速构建map-reduce

缺点：一个map一个reduce

Hadoop Streaming适合进行数据统计等简单的纯粹的MR job

[hadoop-streaming使用简介](http://zhazha.me/%E4%BD%BF%E7%94%A8Python%E5%92%8CHadoop-Streaming%E7%BC%96%E5%86%99MapReduce/)

#### spark shell {#spark-shell}

优点： 不用submit，不用编译打包，就和在python命令行里写代码一样

缺点：如上，在命令行里写代码；申请的集群资源一直占用

注：因为会一直占用申请的lanucher和集群资源，如果不是很短时间完成的任务，极力不推荐。如果要使用，建议先在其他地方写好代码，复制过来。

学习曲线：

* 需要学习scala语法
* shell命令参数 spark-shell（申请资源的参数同 spark-submit）

```
spark-shell --master yarn-client 
--executor-cores 2
--executor-memory 2g
--num-executors 10
--jars dump_offline-1.0-SNAPSHOT-jar-with-dependencies.jar
```

#### spark-submit {#spark-submit}

申请集群资源提交到spark集群运行。

* 配置本地编译环境： 注意线上版本
* scala 语法
* sbt 编译打包
* spark-submit 命令重点参数解释（为了更有效的利用集群资源，一定要看

##### 配置本地编译环境 {#配置本地编译环境}

配置在自己目录下

* Java 8:
  [java 8 配置环境](http://www.jianshu.com/p/014e775a8b8c)

下载时wget --no-check-certificate --no-cookies --header "Cookie: oraclelicense=accept-securebackup-cookie" + url

* Sbt ：
  [Linux安装sbt过程详解](http://blog.csdn.net/ZCF1002797280/article/details/49677881)

注：其中网络代理可不需要，[version已弃用](https://stackoverflow.com/questions/33234229/error-in-verifying-sbt-installation)

    java $SBT_OPTS -jar `dirname $0`/sbt-launch.jar "$@"

sbt下载url 设置了重定向，curl-o总是会丢失文件，最后本地下载上传服务器 不需要网络代理；version已弃用

* scala

##### scala 语法 {#scala-语法}

重点关注函数

```
# (一对多)针对迭代器的序列中的每个元素应用函数f，并返回指向结果序列的迭代器。
def flatMap[B](f: (A) => GenTraversableOnce[B]): Iterator[B] 

# 返回一个新迭代器，指向迭代器元素中所有满足条件p的元素。
def filter(p: (A) => Boolean): Iterator[A]

# （一对一）将 it 中的每个元素传入函数 f 后的结果生成新的迭代器
def map[B](f: (A) => B): Iterator[B]

# （相同key去同一个2桶）避免使用groupByKey
def reduceByKey(partitioner: Partitioner, func: (V, V) => V): RDD[(K, V)]

# 上述业务需求
sentence.flatMap(x = > x.split()).filter(_.length < 30).map(x => (x, 1)).reduceByKey(_ + _)

## 慎用var可变数据
```

[深入理解groupByKey、reduceByKey](http://www.jianshu.com/p/0c6705724cff)

##### Sbt 打包 {#sbt-打包}

依赖sbt1.0--scala2.x.2+--sbt-assembly0.14.5[sbt assembly时碰到的错误](https://huajianmao.github.io/notes/2017-03-22-spark-sbt-assembly/)

目录：要使用sbt，目录必须符合一定要求;建议使用sbt new scala/hello-world.g8会创建一个简单模板

* sbt打包本身不能打包第三方库依赖，使用sbt的assembly pulgin: 把所有依赖的jar包打成一个fat jar ---
  [sbt的assembly插件使用\(打包所有依赖\)](http://www.cnblogs.com/zhangqingping/p/4997324.html)

build.sbt：版本及库依赖；[build.sbt](http://blog.stanzhai.site/shi-yong-sbtgou-jian-scalaxiang-mu/)

sbt 常用命令[sbt官方文档](http://www.scala-sbt.org/0.13/docs/zh-cn/Running.html#%E5%B8%B8%E7%94%A8%E5%91%BD%E4%BB%A4)

sbt 控制台：[sbt 控制台](http://www.importnew.com/4311.html)

sbt更复杂的依赖：[sbt复杂依赖.scala](https://github.com/CSUG/real_world_scala/blob/master/02_sbt.markdown)

```
 nohup spark-submit \
     --master yarn-master \
     --num-executors 100 \
     --executor-memory 6g \
     --executor-cores 4 \
     --driver-memory 1G \
     --conf spark.default.parallelism=1000 \
     --conf spark.storage.memoryFraction=0.5 \
     --conf spark.shuffle.memoryFraction=0.3 \
     --class cp.WordCount \
     --files ...\
     --dirver-class-path ...(driver依赖包，多个包:分隔) \
     --jars /data1/html_extractor-1.0-SNAPSHOT-jar-with-dependencies.jar（driver和executor都需要的包，多个包,分隔） \
     input（文件or文件夹） \
     output（文件夹） &
```

[spark-submit启动参数说明](http://www.jianshu.com/p/9d5234185d68)

##### 更多实例 {#更多实例}

[案例分析与编程实现](https://www.ibm.com/developerworks/cn/opensource/os-cn-spark-practice1/)

#### spark参考 {#spark参考}

[spark使用总结](http://smallx.me/2016/06/07/spark%E4%BD%BF%E7%94%A8%E6%80%BB%E7%BB%93/)

