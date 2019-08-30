pom.xml配置zookeeper
```
        <!-- https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper -->
        <dependency>
            <groupId>org.apache.zookeeper</groupId>
            <artifactId>zookeeper</artifactId>
            <version>3.4.14</version>
        </dependency>
```
####创建create
```
String result = zk.create("/test_node",
"test".getBytes(), 
ZooDefs.Ids.OPEN_ACL_UNSAFE, 
CreateMode.PERSISTENT);
```
####异步
```
class CreateCallBack implements AsyncCallback.StringCallback{
    public void processResult(int rc, String path, Object ctx, String name) {
        System.out.println("创建节点: " + path);
        System.out.println((String)ctx);
    }
}
```
```
 String ctx = "{'create':'success'}";
        zk.create("/asyn_node",
                "test".getBytes(),
                ZooDefs.Ids.OPEN_ACL_UNSAFE,
                CreateMode.PERSISTENT,
                new CreateCallBack(),
                ctx
                );
```
####修改节点数据
```
zk.setData("/test_node","update_data".getBytes(),0);
```
####删除
```
zk.delete("/test_node",1);
```
####ACL
```
        String writerId = DigestAuthenticationProvider.generateDigest("writer:123456");
        String readerId = DigestAuthenticationProvider.generateDigest("reader:654321");
        Id wId = new Id("digest", writerId);
        Id rId = new Id("digest", readerId);
        List<ACL> aclList = new ArrayList<ACL>();
        aclList.add(new ACL(ZooDefs.Perms.ALL, wId));
        aclList.add(new ACL(ZooDefs.Perms.READ, rId));
        zk.create("/acl_node", "node".getBytes(), aclList, CreateMode.PERSISTENT);
```
```
zk.addAuthInfo("digest","writer:123456".getBytes());
        zk.create("/acl_node/digest_node","digest_node".getBytes(), ZooDefs.Ids.OPEN_ACL_UNSAFE,CreateMode.PERSISTENT);
```
