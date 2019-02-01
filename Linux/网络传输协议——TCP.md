####TCP头
![image.png](https://upload-images.jianshu.io/upload_images/143845-d2e78c5164b51de3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####状态位
在TCP层，有个FLAGS字段，这个字段有以下几个标识：SYN, FIN, ACK, PSH, RST, URG.
它们的含义是：
URG:Urget pointer is valid (紧急指针字段值有效)
SYN: 表示建立连接
FIN: 表示关闭连接
ACK: 表示响应
PSH: 表示有 DATA数据传输
RST: 表示连接重置。
####TCP三次握手
####TCP四次挥手
