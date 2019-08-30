####序列化
把内存中对象转换成字节序列（或其他数据传输协议）以便于存储到磁盘（持久化）和网络传输
####反序列化
接收到的字节序列或者其他传输协议或者是磁盘的持久化数据转换成内存的对象
####自定义bean对象实现序列化接口（Writable）
具体实现bean对象序列化步骤如下7步。
（1）必须实现Writable接口
（2）反序列化时，需要反射调用空参构造函数，所以必须有空参构造
（3）重写序列化方法
（4）重写反序列化方法
（5）注意反序列化的顺序和序列化的顺序完全一致
（6）要想把结果显示在文件中，需要重写toString()，可用”\t”分开，方便后续用。
（7）如果需要将自定义的bean放在key中传输，则还需要实现Comparable接口，因为MapReduce框中的Shuffle过程要求对key必须能排序。详见后面排序案例。
####hello word
测试数据
```
1	13726230503	24681	24681	200
2	13826544101	264	0	200
3	13926435656	132	1512	200
4	13926251106	240	0	200
5	18211575961	1527	2106	200
6	18211575961	4116	1432	200
7	13560439658	1116	954	200
8	15920133257	3156	2936	200
9	13719199419	240	0	200
10	13660577991	6960	690	200
11	15013685858	3659	3538	200
12	15989002119	1938	180	200
13	13560439658	918	4938	200
14	13480253104	80	180	200
15	13602846565	1938	2910	200
16	13922314466	3008	3720	200
17	13502468823	7335	110349	200
18	18320173382	9531	2412	200
19	13925057413	11058	48243	200
20	13760778710	120	120	200
21	13560436666	2481	24681	200
22	13560436666	1116	954	200
```
hadoop 序列化和反序列化对象
```
package com.bigdata.mapreduce.flowdata;

import org.apache.hadoop.io.Writable;

import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;

public class FlowBean implements Writable {
    private long upFlow;
    private long downFlow;
    private long sumFlow;

    public FlowBean() {
    }

    public String toString() {
        return upFlow + "\t" + downFlow + "\t" + sumFlow;
    }

    public void set(long upFlow, long downFlow) {
        this.upFlow = upFlow;
        this.downFlow = downFlow;
        this.sumFlow = upFlow + downFlow;
    }

    public long getUpFlow() {
        return upFlow;
    }

    public void setUpFlow(long upFlow) {
        this.upFlow = upFlow;
    }

    public long getDownFlow() {
        return downFlow;
    }

    public void setDownFlow(long downFlow) {
        this.downFlow = downFlow;
    }

    public long getSumFlow() {
        return sumFlow;
    }

    public void setSumFlow(long sumFlow) {
        this.sumFlow = sumFlow;
    }

    // 序列化
    public void write(DataOutput dataOutput) throws IOException {
        dataOutput.writeLong(upFlow);
        dataOutput.writeLong(downFlow);
        dataOutput.writeLong(sumFlow);
    }

    // 反序列 跟序列化顺序一致
    public void readFields(DataInput dataInput) throws IOException {
        upFlow = dataInput.readLong();
        downFlow = dataInput.readLong();
        sumFlow = dataInput.readLong();
    }
}

```
maper
```
package com.bigdata.mapreduce.flowdata;

import org.apache.hadoop.io.Writable;

import java.io.DataInput;
import java.io.DataOutput;
import java.io.IOException;

public class FlowBean implements Writable {
    private long upFlow;
    private long downFlow;
    private long sumFlow;

    public FlowBean() {
    }

    public String toString() {
        return upFlow + "\t" + downFlow + "\t" + sumFlow;
    }

    public void set(long upFlow, long downFlow) {
        this.upFlow = upFlow;
        this.downFlow = downFlow;
        this.sumFlow = upFlow + downFlow;
    }

    public long getUpFlow() {
        return upFlow;
    }

    public void setUpFlow(long upFlow) {
        this.upFlow = upFlow;
    }

    public long getDownFlow() {
        return downFlow;
    }

    public void setDownFlow(long downFlow) {
        this.downFlow = downFlow;
    }

    public long getSumFlow() {
        return sumFlow;
    }

    public void setSumFlow(long sumFlow) {
        this.sumFlow = sumFlow;
    }

    // 序列化
    public void write(DataOutput dataOutput) throws IOException {
        dataOutput.writeLong(upFlow);
        dataOutput.writeLong(downFlow);
        dataOutput.writeLong(sumFlow);
    }

    // 反序列 跟序列化顺序一致
    public void readFields(DataInput dataInput) throws IOException {
        upFlow = dataInput.readLong();
        downFlow = dataInput.readLong();
        sumFlow = dataInput.readLong();
    }
}

```
reducer
```
package com.bigdata.mapreduce.flowdata;

import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Reducer;

import java.io.IOException;

public class FlowReducer extends Reducer<Text, FlowBean, Text, FlowBean> {
    private FlowBean sumFlow = new FlowBean();

    @Override
    protected void reduce(Text key, Iterable<FlowBean> values, Context context) throws IOException, InterruptedException {
        long sumUpFlow = 0;
        long sumDownFlow = 0;
        for (FlowBean value : values) {
            sumUpFlow += value.getUpFlow();
            sumDownFlow += value.getDownFlow();
        }
        sumFlow.set(sumUpFlow, sumDownFlow);
        context.write(key, sumFlow);
    }
}

```
driver
```
package com.bigdata.mapreduce.flowdata;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.Path;
import org.apache.hadoop.io.Text;
import org.apache.hadoop.mapreduce.Job;
import org.apache.hadoop.mapreduce.lib.input.FileInputFormat;
import org.apache.hadoop.mapreduce.lib.output.FileOutputFormat;

import java.io.IOException;

public class FlowDriver {
    public static void main(String[] args) throws IOException, ClassNotFoundException, InterruptedException {
        Job job = Job.getInstance(new Configuration());

        //设置类路径
        job.setJarByClass(FlowDriver.class);
        job.setMapperClass(FlowMapper.class);
        job.setReducerClass(FlowReducer.class);

        job.setMapOutputKeyClass(Text.class);
        job.setMapOutputValueClass(FlowBean.class);

        job.setOutputKeyClass(Text.class);
        job.setOutputValueClass(FlowBean.class);

        FileInputFormat.setInputPaths(job, new Path(args[0]));
        FileOutputFormat.setOutputPath(job, new Path(args[1]));
        boolean b = job.waitForCompletion(true);
        System.exit(b ? 0 : 1);

    }
}

```
运行结果
```
13480253104	80	180	260
13502468823	7335	110349	117684
13560436666	3597	25635	29232
13560439658	2034	5892	7926
13602846565	1938	2910	4848
13660577991	6960	690	7650
13719199419	240	0	240
13726230503	24681	24681	49362
13760778710	120	120	240
13826544101	264	0	264
13922314466	3008	3720	6728
13925057413	11058	48243	59301
13926251106	240	0	240
13926435656	132	1512	1644
15013685858	3659	3538	7197
15920133257	3156	2936	6092
15989002119	1938	180	2118
18211575961	5643	3538	9181
18320173382	9531	2412	11943

```
