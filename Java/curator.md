zookeeper原生api的不足之处
超时重连，不支持自动，需要手动重连
Watch注册一次会失效
不支持递归创建节点
```
<dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-recipes</artifactId>
            <version>4.2.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.curator</groupId>
            <artifactId>curator-framework</artifactId>
            <version>4.2.0</version>
        </dependency>
```
```
CuratorFramework client = CuratorFrameworkFactory.builder().connectString("127.0.0.1:2181")
                .sessionTimeoutMs(15000)
                .retryPolicy(new RetryNTimes(3,5000)).
                namespace("namespace").build();
        client.start();
        try {
            client.create().creatingParentsIfNeeded().
                    withMode(CreateMode.PERSISTENT).
                    withACL(ZooDefs.Ids.OPEN_ACL_UNSAFE).forPath("/super/test","testdata".getBytes());
        }catch (Exception e){
            e.printStackTrace();
        }
```
