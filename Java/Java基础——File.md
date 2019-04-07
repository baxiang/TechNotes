File是IO包中唯一表示文件和磁盘目录的对象的路径.包含了创建,删除,重命名,获取文件信息等.
构造函数
```
File(String pathname)	根据路径得到File对象
File(String parent,String child)	根据目录和子文件/目录得到对象
File(File parent,String child)	根据父File对象和子文件/目录得到对象
```
获取文件路径
```
File getAbsoluteFile() :获取绝对路径
String getAbsolutePath():获取绝对路径
String getPath() :获取文件路径
String getName() :获取文件名称
File getParentFile():获取上级目录文件
String getParent() :获取上级目录路径
```
获取文件属性
```
boolean canExecute() :判断是否是可执行文件
boolean canRead() :判断该文件是否可读
boolean canWrite():判断该文件是否可写
boolean isHidden():判断该文件是否是隐藏文件
long lastModified():判断该文件的最后修改时间
```
文件操作:
```
boolean isFile() :是否是文件
boolean createNewFile() :创建新的文件
static File createTempFile(String prefix, String suffix) :创建临时文件
boolean delete() :删除文件
void deleteOnExit() :在JVM停止时删除文件
boolean exists():判断文件是否存在
boolean renameTo(File dest) :重新修改名称
```
目录操作
```
boolean isDirectory() :判断是否是目录
boolean mkdir()  :创建当前目录
boolean mkdirs() :创建当前目录和上级目录
String[] list() :列出所有的文件名
File[] listFiles() :列出所有文件对象
```
获取当前目录的所有文件
```
  File file1  = new File("/Users/baxiang/Documents/");
  String[] fileList = file1.list();
   for (String string : fileList) {
    	  System.out.println(string);
}
```
