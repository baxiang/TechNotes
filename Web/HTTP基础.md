##五层模型
（1）物理层
物理层主要作用是定义物理设备如何传输数据
（2）数据链路层
数据链路层在通信的实体间建立数据链路连接
（3）网络层
网络层为数据在结点之间传输创建逻辑链路
（4）传输层
向用户提供可靠的端到端（End-to-End）服务，传输层向高层屏蔽了下层数据通信的细节。
（5）应用层
为应用软件提供了很多服务。构建与TCP协议之上，屏蔽网络传输相关的细节
####HTTP发展历史
（1）Http/0.9 只有一个命令GET 没有HEADER等描述数据的信息，服务器发送完毕，就关闭TCP连接。
（2）HTTP/1.0 增加了很多命令 增加status code和header。
（3）HTTP/1.1 持久连接，pipeline，增加host和其他一些命令
（4） HTTP2 所有数据以二进制传输，同一个连接里面发送多个请求不在需要按照顺序来，头信息压缩以及推送等提高效率的功能
####HTTP三次握手
####HTTP客户端

