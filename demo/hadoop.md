# Hadoop MapReduce Demo (Java)

- 创建Maven项目 (推荐在Vscode里用Maven管理Java项目) 

   - pom.xml片段，添加相关依赖包（**版本仅供参考**）

```xml
    <!-- https://mvnrepository.com/artifact/commonslogging/commons-logging -->
    <dependency>
      <groupId>commons-logging</groupId>
      <artifactId>commons-logging</artifactId>
      <version>1.1.3</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common -->
    <dependency>
      <groupId>org.apache.hadoop</groupId>
      <artifactId>hadoop-common</artifactId>
      <version>2.7.4</version>
    </dependency>
    <dependency>
      <groupId>org.apache.hadoop</groupId>
      <artifactId>hadoop-hdfs</artifactId>
      <version>2.7.4</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-client -->
    <dependency>
      <groupId>org.apache.hadoop</groupId>
      <artifactId>hadoop-client</artifactId>
      <version>2.7.4</version>
    </dependency>

```

- 编写Word Count代码 

```java
import java.io.IOException;
import java.util.StringTokenizer;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.IntWritable;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.Mapper;
import org.apache.hadoop.mapreduce.Reducer;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;
import org.apache.hadoop.util.GenericOptionsParser;

public class WordCount {

  public static class TokenizerMapper 
       extends Mapper<Object, Text, Text, IntWritable>{
    
    private final static IntWritable one = new IntWritable(1);
    private Text word = new Text();
      
    public void map(Object key, Text value, Context context
                    ) throws IOException, InterruptedException {
      StringTokenizer itr = new StringTokenizer(value.toString());
      while (itr.hasMoreTokens()) {
        word.set(itr.nextToken());
        context.write(word, one);
      }
    }
  }
  
  public static class IntSumReducer 
       extends Reducer<Text,IntWritable,Text,IntWritable> {
    private IntWritable result = new IntWritable();

    public void reduce(Text key, Iterable<IntWritable> values, 
                       Context context
                       ) throws IOException, InterruptedException {
      int sum = 0;
      for (IntWritable val : values) {
        sum += val.get();
      }
      result.set(sum);
      context.write(key, result);
    }
  }

  public static void main(String[] args) throws Exception {
    Configuration conf = new Configuration();
    String[] otherArgs = new GenericOptionsParser(conf, args).getRemainingArgs();
    if (otherArgs.length < 2) {
      System.err.println("Usage: wordcount <in> [<in>...] <out>");
      System.exit(2);
    }
    Job job = Job.getInstance(conf, "word count");
    job.setJarByClass(WordCount.class);
    job.setMapperClass(TokenizerMapper.class);
    job.setCombinerClass(IntSumReducer.class);
    job.setReducerClass(IntSumReducer.class);
    job.setOutputKeyClass(Text.class);
    job.setOutputValueClass(IntWritable.class);
    for (int i = 0; i < otherArgs.length - 1; ++i) {
      FileInputFormat.addInputPath(job, new Path(otherArgs[i]));
    }
    FileOutputFormat.setOutputPath(job,
      new Path(otherArgs[otherArgs.length - 1]));
    System.exit(job.waitForCompletion(true) ? 0 : 1);
  }
}
```

- 执行mvn package，编译并打包

```
$ mvn package
```

- 创建本地文件用于测试

```
$ mkdir input
$ echo "Hello Docker" >input/file2.txt
$ echo "Hello Hadoop" >input/file1.txt

```

- 上传本地文件到HDFS

```
$ hadoop fs -mkdir -p input
$ hdfs dfs -put ./input/* input
```

- 运行Word Count程序

```
$ hadoop jar target/wordcount-1.0.jar WordCount input output
```

- 查看输出

```
$ hadoop fs -cat output/part-r-00000
Docker  1
Hadoop  1
Hello   2
```
![MapReduce Output](../images/vscode-mr-java4.png "查看输出")


# Hadoop MapReduce Demo (Python)

- 原理：Hadoop Streaming提供了一个便于进行MapReduce编程的工具包，使用它可以基于一些可执行命令、脚本语言或其他编程语言来实现Mapper和 Reducer，从而充分利用Hadoop并行计算框架的优势和能力，来处理大数据。Hadoop Streaming负责从标准输入依次读取文件的每一行，执行函数，把标准输出转化成key-value对或者key-null对。

  

- 编写Word Count代码

### mapper.py

```python
#!/usr/bin/env python
 
import sys
 
# input comes from STDIN (standard input)
for line in sys.stdin:
    # remove leading and trailing whitespace
    line = line.strip()
    # split the line into words
    words = line.split()
    # increase counters
    for word in words:
        # write the results to STDOUT (standard output);
        # what we output here will be the input for the
        # Reduce step, i.e. the input for reducer.py
        #
        # tab-delimited; the trivial word count is 1
        print '%s\t%s' % (word, 1)
```

###  reducer.py

```python
#!/usr/bin/env python
 
from operator import itemgetter
import sys
 
current_word = None
current_count = 0
word = None
 
# input comes from STDIN
for line in sys.stdin:
    # remove leading and trailing whitespace
    line = line.strip()
 
    # parse the input we got from mapper.py
    word, count = line.split('\t', 1)
 
    # convert count (currently a string) to int
    try:
        count = int(count)
    except ValueError:
        # count was not a number, so silently
        # ignore/discard this line
        continue
 
    # this IF-switch only works because Hadoop sorts map output
    # by key (here: word) before it is passed to the reducer
    if current_word == word:
        current_count += count
    else:
        if current_word:
            # write result to STDOUT
            print '%s\t%s' % (current_word, current_count)
        current_count = count
        current_word = word
 
# do not forget to output the last word if needed!
if current_word == word:
    print '%s\t%s' % (current_word, current_count)
```

- 上传本地文件到HDFS

```
$ hadoop fs -mkdir -p input
$ hdfs dfs -put ./input/wordcount_data.txt input
```


- 运行Word Count程序

```
$ hadoop jar /usr/local/hadoop/share/hadoop/tools/lib/hadoop-streaming-2.7.7.jar \
> -D stream.non.zero.exit.is.failure=false \
> -D mapreduce.job.name=python_wc \
> -files  mapper.py,reducer.py \
> -mapper  mapper.py \
> -reducer reducer.py \
> -input input/wordcount_data.txt \
> -output output
```

- 查看输出

```
$ hadoop fs -cat output/part-r-00000
Bye     1
Goodbye 1
Hadoop! 1
Hadoop, 1
Hbase,  1
Hello   4
Hive    1
Java    1
Python  1
Scala   1
Spark   1
World!  1
World,  1
hello   1
to      1
```
![MapReduce Python](../images/vscode-mr-py.png "运行截图")