Java只能单继承，无法多继承，但是实际开发中可能存在继承的问题，Java中的接口可以变相实现多重继承。
## 接口定义：interface
```
[修饰符]interface <接口名> [extends 父类接口名称列表]{
   [public][static][final] 变量
  [public][abstract] 方法
}
```
```
interface TestOne{
}
interface TestTwo{
}
interface TestThree  extends TestOne,TestTwo{
}
```
## 接口的实现
```
[修饰符] class <类名> [extends 父类类] [implements 接口列表]{
```
## 接口和和抽象类的对比
相同点：
（1）主要都是被其他类继承，不能创建对象 不能实例化
（2）都可以定义抽象方法 子类都必须覆写抽象方法
不同点：
（1）接口没有构造方法
（2）Java8之前接口只能有抽象方法
（3）子类只能继承一个抽象类 但是可以支持多个接口。
## 接口的继承
（1）接口只能继承接口 不能继承类，而且是单继承，不能继承多个接口
