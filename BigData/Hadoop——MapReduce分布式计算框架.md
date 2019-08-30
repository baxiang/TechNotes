##概述
源自2014年12月的Google发表的MapReduce论文，它是一个编程模型，用于大数据量的计算，MapReduce是分布式计算框架。具有海量数据离线处理。对于大数据量的计算，通常采用的处理方式就是并行计算，MapReduce就是一种`分布式计算的编程框架`，它使得并没有并行计算经验的开发人员也可以计算并行应用程序。
MapReduce核心功能是将用户编写的业务逻辑代码和自带默认组件整合成一个完整的分布式运算程序，并发运行在一个Hadoop集群上。
####设计目标
MapReduce采用的是分而治之的思想，即把大规模数据集的操作，分发给一个主节点管理下的各个子节点共同完成，然后整合各个子节点的中间结果，从而得到最终的计算结果。就是分散任务，汇总结果
####编程模型
MapReduce分成Map阶段和Reduce阶段。用户只需要编写map()和reduce两个函数，即可完成简单的分布式程序的设计
map()函数以key/value对作为输入，产生另外一系列key/value对作为中间输出写入本地磁盘，MapReduc框架会自动将这些中间数据按照key值进行聚集，且key值相同（用户可设定聚集策略，默认情况下是对key值进行哈希取模）的数据被统一交给reduce()函数处理。
reduce()函数以key及对应的value列表作为输入，经合并key相同的value值后，产生另外一系列key/value作为最终输出写入HDFS.
####优点
1MapReduce易于编程，它简单的实现一些接口，就可以完成一个分布式程序
2 易于拓展
3 高容错性
4 适合PB级别以上海量数据的离线处理
####缺点
计算慢，通过分布式解决速度计算速度问题
##常用数据序列化类型
无法使用Java的类型变量
![image.png](https://upload-images.jianshu.io/upload_images/143845-81190e4338137c21.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####wordcount
pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.bigdata</groupId>
    <artifactId>MapReduceNote</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <hadoop.version>2.7.2</hadoop.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-common</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-hdfs</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-mapreduce-client-core</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-mapreduce-client-jobclient</artifactId>
            <version>${hadoop.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.3.2</version>
                <executions>
                    <execution>
                        <id>default-compile</id>
                        <phase>compile</phase>
                        <goals>
                            <goal>compile</goal>
                        </goals>
                        <configuration>
                            <encoding>UTF-8</encoding>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <artifactId>maven-assembly-plugin </artifactId>
                <configuration>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <archive>
                        <manifest>
                            <mainClass>com.bigdata.mapreduce.wordcount.WordCountDriver</mainClass>
                        </manifest>
                    </archive>
                </configuration>
                <executions>
                    <execution>
                        <id>make-assembly</id>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```
Mapper
```
package com.bigdata.mapreduce.wordcount;

import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.LongWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Mapper;
import java.io.IOException;

// 输入 偏移量 输出
public class WordCountMapper extends Mapper<LongWritable, Text,Text, IntWritable> {
    private Text word = new Text();
    private IntWritable one = new IntWritable(1);
    @Override
    protected void map(LongWritable key, Text value, Context context) throws IOException, InterruptedException {
        // 获取字符串内容
        String line = value.toString();
        // 安装空格切分数据
        String[] words = line.split(" ");
        // 遍历数组，把单词变成(word,1)
        for (String word : words) {
            this.word.set(word);
            context.write(this.word,one);
        }
    }
}

```
Reducer
```
package com.bigdata.mapreduce.wordcount;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class WordCountReduce extends Reducer<Text, IntWritable,Text,IntWritable> {
    private IntWritable total = new IntWritable();
    @Override
    protected void reduce(Text key, Iterable<IntWritable> values, Context context) throws IOException, InterruptedException {
       int sum = 0;
        for (IntWritable value : values) {
            sum +=value.get();
        }
        total.set(sum);
        context.write(key,total);
    }
}

```
Driver
```
package com.bigdata.mapreduce.wordcount;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class WordCountDriver {
    public static void main(String[] args) throws IOException ,InterruptedException,ClassNotFoundException{
        // 申请一个job实例
        Job job = Job.getInstance(new Configuration());
        // 设置类路径
        job.setJarByClass(WordCountDriver.class);
        // 设置mapper类
        job.setMapperClass(WordCountMapper.class);
        // 设置reduce类
        job.setReducerClass(WordCountReduce.class);
        //设置map和reduce 的输出类型
        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(IntWritable.class);

        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(IntWritable.class);
        //设置输入输出

        FileInputFormat.setInputPaths(job,new Path(args[0]));
        FileOutputFormat.setOutputPath(job,new Path(args[1]));

        boolean b = job.waitForCompletion(true);
        System.exit(b?0:1);
    }
}

```
创建src/main/resources/log4j.properties
```
log4j.rootLogger=INFO, stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d %p [%c] - %m%n
log4j.appender.logfile=org.apache.log4j.FileAppender
log4j.appender.logfile.File=target/spring.log
log4j.appender.logfile.layout=org.apache.log4j.PatternLayout
log4j.appender.logfile.layout.ConversionPattern=%d %p [%c] - %m%n
```

没有依赖库需要配置com.bigdata.mapreduce.wordcount.WordCountDriver 
```
hadoop jar MapReduceNote-1.0-SNAPSHOT.jar com.bigdata.mapreduce.wordcount.WordCountDriver  /user/baxiang/input/word.txt /user/baxiang/output
```
