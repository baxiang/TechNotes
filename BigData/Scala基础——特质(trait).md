##概述
java通过接口实现多重继承，scala没有接口通过trait关键字实现多重继承。
特质类似于抽象类的定义，trait可以定义抽象方法，也可以定义具体实现的方法，不需要使用abstract关键字。特质可以使用extends继承其他特质
![image.png](https://upload-images.jianshu.io/upload_images/143845-b943bfa0935ca1ce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####extend
![image.png](https://upload-images.jianshu.io/upload_images/143845-9d6ccdc800e65c73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####with
如果要混入多个特质，可以使用多个with

