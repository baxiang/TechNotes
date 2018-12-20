![image.png](http://upload-images.jianshu.io/upload_images/143845-aac04d4d268e41b4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

核心动画位于UIKit的下一层，相比UIView动画，它可以实现更复杂的动画效果。
核心动画作用在CALayer（Core animation layer）上，CALayer从概念上类似UIView，我们可以将UIView看成是一种特殊的CALayer（可以响应事件）。
实际上，每一个view都有其对应的layer，这个layer是root layer：
```
 @property(nonatomic,readonly,strong)                 CALayer  *layer;
```
　核心动画和UIView动画的对比：UIView动画可以看成是对核心动画的封装，和UIView动画不同的是，通过核心动画改变layer的状态（比如position），动画执行完毕后实际上是没有改变的（表面上看起来已改变）。

##核心动画类的常用属性
```
//CATransform3D Key Paths : (example)transform.rotation.z
     //rotation.x
     //rotation.y
     //rotation.z
     //rotation 旋轉
     //scale.x
     //scale.y
     //scale.z
     //scale 缩放
     //translation.x
     //translation.y
     //translation.z
     //translation 平移

     //CGPoint Key Paths : (example)position.x
     //x
     //y

     //CGRect Key Paths : (example)bounds.size.width
     //origin.x
     //origin.y
     //origin
     //size.width
     //size.height
     //size

     //opacity
     //backgroundColor
     //cornerRadius 
     //borderWidth
     //contents 

     //Shadow Key Path:
     //shadowColor 
     //shadowOffset 
     //shadowOpacity 
     //shadowRadius
```
####timingFunction
```
     kCAMediaTimingFunctionLinear 匀速
     kCAMediaTimingFunctionEaseIn 慢进
     kCAMediaTimingFunctionEaseOut 慢出
     kCAMediaTimingFunctionEaseInEaseOut 慢进慢出
     kCAMediaTimingFunctionDefault 默认值（慢进慢出）
```
##CABasicAnimation
```
- (void)position {
    CABasicAnimation * ani = [CABasicAnimation animationWithKeyPath:@"position"];
    ani.toValue = [NSValue valueWithCGPoint:self.centerShow.center];
    ani.removedOnCompletion = NO;
    ani.fillMode = kCAFillModeForwards;
    ani.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
    [self.cartCenter.layer addAnimation:ani forKey:@"PostionAni"];
}
```
##CAKeyframeAnimation
```
- (void)valueKeyframeAni {
    CAKeyframeAnimation * ani = [CAKeyframeAnimation animationWithKeyPath:@"position"];
    ani.duration = 4.0;
    ani.removedOnCompletion = NO;
    ani.fillMode = kCAFillModeForwards;
    ani.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
    NSValue * value1 = [NSValue valueWithCGPoint:CGPointMake(150, 200)];
    NSValue *value2=[NSValue valueWithCGPoint:CGPointMake(250, 200)];
    NSValue *value3=[NSValue valueWithCGPoint:CGPointMake(250, 300)];
    NSValue *value4=[NSValue valueWithCGPoint:CGPointMake(150, 300)];
    NSValue *value5=[NSValue valueWithCGPoint:CGPointMake(150, 200)];
    ani.values = @[value1, value2, value3, value4, value5];
    [self.centerShow.layer addAnimation:ani forKey:@"PostionKeyframeValueAni"];
}
```
设置path使其绕圆圈运动
```
- (void)pathKeyframeAni {
    CAKeyframeAnimation * ani = [CAKeyframeAnimation animationWithKeyPath:@"position"];
    CGMutablePathRef path = CGPathCreateMutable();
    CGPathAddEllipseInRect(path, NULL, CGRectMake(130, 200, 100, 100));
    ani.path = path;
    CGPathRelease(path);
    ani.duration = 4.0;
    ani.removedOnCompletion = NO;
    ani.fillMode = kCAFillModeForwards;
    [self.centerShow.layer addAnimation:ani forKey:@"PostionKeyframePathAni"];
}
```
##CATransition

##CAAnimationGroup
##CASpringAnimation
```
- (void)springAni {
    CASpringAnimation * ani = [CASpringAnimation animationWithKeyPath:@"bounds"];
    ani.mass = 10.0; //质量，影响图层运动时的弹簧惯性，质量越大，弹簧拉伸和压缩的幅度越大
    ani.stiffness = 5000; //刚度系数(劲度系数/弹性系数)，刚度系数越大，形变产生的力就越大，运动越快
    ani.damping = 100.0;//阻尼系数，阻止弹簧伸缩的系数，阻尼系数越大，停止越快
    ani.initialVelocity = 5.f;//初始速率，动画视图的初始速度大小;速率为正数时，速度方向与运动方向一致，速率为负数时，速度方向与运动方向相反
    ani.duration = ani.settlingDuration;
    ani.toValue = [NSValue valueWithCGRect:self.centerShow.bounds];
    ani.removedOnCompletion = NO;
    ani.fillMode = kCAFillModeForwards;
    ani.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
    [self.cartCenter.layer addAnimation:ani forKey:@"boundsAni"];
}
```
