## 数据库自增ID

最简单的实现方式是使用数据库的id自增策略，如 `MySQL` 的 `auto_increment`。如果两台数据库分别设置不同步长，可以生成不重复ID，从而实现高可用。

#### 优点

实现简单，容易理解，单调自增，绝对有序。

#### 缺点

1.  强依赖DB，当DB异常时整个系统不可用，属于致命问题。
2.  ID发号性能瓶颈限制在单台MySQL的读写性能。

## UUID系列

结合机器的网卡、当地时间、一个随记数来生成UUID。存在一些UUID的变种也是不错的实现。

#### 优点

本地生成，生成简单，性能非常好，高可用。

#### 缺点

1.  长度过长，不易存储，无序不可读，查询效率低。
2.  信息不安全，基于MAC地址生成UUID的算法可能会造成MAC地址泄露，这个漏洞曾被用于寻找梅丽莎病毒的制作者位置。
3.  UUID的无序性可能会引起数据位置频繁变动，严重影响性能。

## Redis实现ID

Redis的所有命令操作都是单线程的，本身提供像 `incr` 和 `increby` 这样的自增原子命令，所以能保证生成的 ID 肯定是唯一有序的。

举例，使用 Redis 来生成每天从0开始的流水号。比如订单号 = 日期 + 当日自增长号。可以每天在 Redis 中生成一个 Key ，使用 INCR 进行累加。

#### 优点

1.  灵活方便，且性能优于数据库。
2.  数字ID天然排序，通过合理设计可以得到更具有表达能力的ID。

### 缺点

1.  引入Redis，编码和配置的工作量比较大。
2.  如果ID是连续的，恶意用户的扒取工作就非常容易做了，直接按照顺序下载指定URL即可；如果是订单号就更危险了，竞对可以直接知道我们一天的单量。所以在一些应用场景下，会需要ID无规则、不规则。

## Twitter的snowflake算法生成ID

详细可以参照 [github.com/twitter/sno…](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Ftwitter%2Fsnowflake)

#### 优点

1.  时间有序，毫秒数在高位，自增序列在低位，整个ID都是趋势递增的。
2.  不依赖数据库等第三方系统，以服务的方式部署，稳定性更高，生成ID的性能也是非常高的。
3.  可以根据自身业务特性分配bit位，非常灵活。
4.  Long型。

#### 缺点

依赖于机器的时钟，如果机器上时钟回拨，会导致发号重复或者服务会处于不可用状态。

## 百度UidGenerator

百度基于`snowflake`的一种实现，参见 [github.com/baidu/uid-g…](https://link.juejin.im/?target=https%3A%2F%2Fgithub.com%2Fbaidu%2Fuid-generator)

#### 优点

同上 `Twitter的snowflake算法生成ID`

#### 缺点

需要MySQL(内置WorkerID分配器, 启动阶段通过DB进行分配; 如自定义实现, 则DB非必选依赖）

## 美团Leaf

参见 [tech.meituan.com/2017/04/21/…](https://link.juejin.im/?target=https%3A%2F%2Ftech.meituan.com%2F2017%2F04%2F21%2Fmt-leaf.html)

#### 优点

1.  高可用容灾。
2.  ID号码是趋势递增的8byte的64位数字，满足数据库存储的主键要求。

#### 缺点

1.  DB宕机会造成整个系统不可用。
2.  比较复杂。

## MongoDB的ObjectId

通过“时间+机器码+pid+inc”共12个字节，通过4+3+2+3的方式最终标识成一个24长度的十六进制字符。

#### 优点

1.  轻量型的，不同的机器都能用全局唯一的同种方法方便地生成它。
2.  本地生成，含时间戳，有序，成本低。
3.  安全性高。
4.  比较短，24位，比如掘金的ID，[juejin.im/editor/post…](https://juejin.im/editor/posts/5d42756ee51d4561bf46201a)

#### 缺点

1.  比较长，难于记忆。
2.  使用机器ID和进程ID，64位Long无法存储，只能生成特殊ObjectId对象。

## 自己编程实现雪花算法

参照 `Twitter的snowflake算法生成ID`，参考`MongoDB`的`ObjectId`的生成策略，使用类似机器ID，进程ID来保证唯一性。
