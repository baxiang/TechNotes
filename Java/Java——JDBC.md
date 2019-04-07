####JDBC概述
Java DataBase Connectivity(java 数据库连接)
####JDBC 
• 加载数据库驱动
• 建立连接
• 创建用于向数据库发送SQL的Statement对象 • 从代表结果集的ResultSet中取出数据
• 断开与数据库的连接，并释放相关资源
pom配置文件
```
 <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.6</version>
        </dependency>
    </dependencies>
```
创建数据表user
```
CREATE TABLE `user` (
  `id` int(10) unsigned NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `gender` tinyint(1) DEFAULT NULL,
  PRIMARY KEY (`id`) USING BTREE
);

INSERT INTO `user` VALUES (1, 'xiaowang', 1);
INSERT INTO `user` VALUES (2, 'xiaoming', 2);
```
eg
```
    public static void main(String[] args) {
        Connection conn = null;
        Statement stmt = null;
        ResultSet ret = null;
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            conn = DriverManager.getConnection("jdbc:mysql://192.168.1.112:3306/test?useSSL=false", "root", "baxiang");
            stmt = conn.createStatement();
            ret = stmt.executeQuery("SELECT id,name FROM user ");
            while (ret.next()) {
                int uId = ret.getInt("id");
                String name = ret.getString("name");
                System.out.printf("%d:%s\n", uId, name);
            }

        } catch (Exception e) {
            e.printStackTrace();

        } finally {
            if (ret != null) {
                try {
                    ret.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                ret = null;
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                stmt = null;
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
                conn = null;
            }

        }
    }
```
####API
一、注册驱动
实际开发中注册驱动会使用如下的方式: Class.forName("com.mysql.jdbc.Driver"); 因为之前的方式会导致驱动注册两次。
二、获得连接
Connection getConnection(String url,String username,String password); url 写法:jdbc:mysql://localhost:3306/jdbc
jdbc :协议
mysql :子协议
localhost :主机名
3306 :端口号
url 简写:jdbc:mysql:///jdbc
Connection :连接对象 主要作用:
一、创建执行 SQL 语句的对象
Statement createStatement() :执行 SQL 语句，有 SQL 注入的漏洞存在。
PreparedStatement prepareStatement(String sql)  :预编译 SQL 语句，解决 SQL注入的漏洞。
CallableStatement prepareCall(String sql) :执行 SQL 中存储过程.
二、进行事务的管理
setAutoCommit(boolean autoCommit):设置事务是否自动提交。 commit():事务提交
rollback():事务回滚
Statement :执行 SQL 主要作用:
一、执行 SQL 语句
boolean execute(String sql):执行 SQL，执行 select 语句返回 true，否则返回 false ResultSet executeQuery(String sql):执行 SQL 中的 select 语句
int executeUpdate(String sql):执行 SQL 中的 insert/update/delete 语句
二、执行批处理操作
addBatch(String sql):添加到批处理 
executeBatch():执行批处理 clearBatch():清空批处理
ResultSet:结果集 
结果集:其实就是查询语句(select)语句查询的结果的封装。 主要作用:结果集获取查询到的结果的。
next():针对不同的类型的数据可以使用 getXXX()获取数据，通用的获取数据的方法: getObject();

####SQL注入漏洞解决
PreparedStatement是Statement的子接口，它的实例对象可以通过调用 Connection.preparedStatement(sql)方法获得，相对于Statement对象而 言:
– PreperedStatement可以避免SQL注入的问题。
– Statement会使数据库频繁编译SQL，可能造成数据库缓冲区溢出。
PreparedStatement 可对SQL进行预编译，从而提高数据库的执行效率。
– 并且PreperedStatement对于sql中的参数，允许使用占位符的形式进行 替换，简化sql语句的编写。
 
