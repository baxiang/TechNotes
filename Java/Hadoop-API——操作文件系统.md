mvn 配置
```
 <dependencies>

        <dependency>
            <groupId>org.apache.hadoop</groupId>
            <artifactId>hadoop-client</artifactId>
            <version>2.6.0</version>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.10</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
```
创建文件夹
```
Configuration config = new Configuration();
FileSystem fileSystem = FileSystem.get(new URI("hdfs://192.168.1.114:8020"),config,"hadoop");
boolean result = fileSystem.mkdirs(new Path("/hdfsapi/test"));
```
读取文件
```
 FSDataInputStream in = fileSystem.open(new Path("/hadoop.txt"));
 IOUtils.copyBytes(in,System.out,1024);
```
创建文件
```
FSDataOutputStream out = fileSystem.create(new Path("/hello.txt"));
out.writeUTF("hello world");
out.flush();
out.close();
```
重命名文件
```
fileSystem.rename(new Path("/hello.txt"),new Path("/new.txt"));
```
拷贝本地文件到hdfs
```
 fileSystem.copyFromLocalFile(new Path("./pom.xml"),new Path("/pom.xml"));
```
下载hdfs文件到本地
```
fileSystem.copyToLocalFile(new Path("/hadoop.txt"),new Path("hadoop.txt"));
```
获取文件列表
```
FileStatus[] fileStatus = fileSystem.listStatus(new Path("/"));
for(FileStatus s: fileStatus){
      System.out.println(s.getPath().toString());
}
```
递归获取文件
```
RemoteIterator<LocatedFileStatus> fileStatus = fileSystem.listFiles(new Path("/"),true);
        while (fileStatus.hasNext()){
            System.out.println(fileStatus.next().getPath().toString());
        }
```
删除文件
```
fileSystem.delete(new Path("/new.txt"),true);
```
