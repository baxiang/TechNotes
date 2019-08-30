####矩阵运算
![image.png](https://upload-images.jianshu.io/upload_images/143845-15d2ea65e067ecef.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
矩阵转置
![image.png](https://upload-images.jianshu.io/upload_images/143845-cf7aa058f8b949e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/143845-3b74f924db45a544.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
####向量
![image.png](https://upload-images.jianshu.io/upload_images/143845-bb06df97d30484a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Spark 向量是以对象形式存储的
[http://spark.apache.org/docs/latest/mllib-data-types.html](http://spark.apache.org/docs/latest/mllib-data-types.html)
####Vector
```
scala> import org.apache.spark.mllib.linalg.{Vectors,Vector}
import org.apache.spark.mllib.linalg.{Vectors, Vector}

scala> Vectors.dense(1,2,3,4)
res0: org.apache.spark.mllib.linalg.Vector = [1.0,2.0,3.0,4.0]

scala> breeze.linalg.DenseVector(1,2,3,4)
res1: breeze.linalg.DenseVector[Int] = DenseVector(1, 2, 3, 4)

scala> res1.t
res2: breeze.linalg.Transpose[breeze.linalg.DenseVector[Int]] = Transpose(DenseVector(1, 2, 3, 4))

scala> res1+res1
res3: breeze.linalg.DenseVector[Int] = DenseVector(2, 4, 6, 8)

scala> res1*res1.t
res4: breeze.linalg.DenseMatrix[Int] =
1  2  3   4
2  4  6   8
3  6  9   12
4  8  12  16
```
