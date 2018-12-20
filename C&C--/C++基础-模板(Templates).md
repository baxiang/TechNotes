##泛型(Generic Programming)
&emsp;&emsp;泛型即是指具有在多种数据类型上皆可操作的含意。 泛型编程的代表作品：STL 是一种高效、泛型、可交互操作的软件组件。泛型编程最初诞生于 C++中, 目的是为了实现 C++的 STL(标准模板库)。其语言支持机制就是模板。模板的精神其实很简单:参数化类型。换句话说,把一个原本特定于某个类型的算法或类当中的类型信息抽掉,抽出来做成模板参数 T。
##函数模板
&emsp;&emsp;实际上是建立一个通用函数，其函数类型和形参类型不具体指定，用一个虚拟的类型来代表。这个通用函数就称为函数模板。在调用函数时系统会根据实参的类型来取代模板中的虚拟类型，从而实现了不同函数的功能。
#### 函数模板语法
```
Template <class或者也可以用typename T>//函数(类)模板的声明
返回类型 函数名（形参表）//函数模板的定义/实现
{
  //函数定义体
}
```
template是声明模板的关键字，typename是定义形式参数的关键字，他可以 是class代替，typename和class没有区别的，<>中的参数就是模板形参，模板形参和函数形参很像，但是模板形参不能为空的
####函数模板调用 
myswap<float>(a, b);	 //显示类型调用
myswap(a, b); 		//自动数据类型推导 
``` 
//template   < 类型形式参数表 >   
template <typename T>
void myswap(T &a,T &b)
{
    T temp = a;
    a = b;
    b = temp;
}
int main()
{
    int  x = 1;
    int	 y = 2;
    myswap<int>(x, y); //自动数据类型 推导的方式

    cout << x <<endl << y <<endl;
    char m = 'a';
    char n = 'b';
    myswap<char>(m,n);
    cout << m <<"-----" <<n <<endl;
    return 0;
}
```
####函数模板的重载
当模板类和重载函数一起使用时，会首先考虑重载函数，其次是模板类，再没有的话会考虑类型转换(可能会不精确)。
函数模板和普通函数在一起，调用规则:
1. 函数模板可以像普通函数一样被重载
2. C++编译器优先考虑普通函数
3. 如果函数模板可以产生一个更好的匹配，那么选择模板
4. 可以通过空模板实参列表的语法限定编译器只通过模板匹配

##类模板
允许用户为类定义一种模式，使得类中的某些数据成员、默认成员函数的参数、某些成员函数的返回值，能够取任意类型(包括系统预定义的和用户自定义的)。
 如果一个类中数据成员的数据类型不能确定，或者是某个成员函数的参数或返回值的类型不能确定，就必须将此类声明为模板，它的存在不是代表一个具体的、实际的类，而是代表着一类类。
####类模板的语法
```
template <class 类型参数名>//声明模板类
class 具体类型参数名 //定义具体类
{
   //...
}
```
比较两个整数的大小
```
class Compare_integer
{
public:
    Compare(int a,int b)
    {
        x = a;
        y = b;
    }
    int max()
    {
        return (x>y)?x:y;
    }
    int min()
    {
        return (x<y)?x:y;
    }
    private:
      int x,y;
};
```
如果此时我们需要double数据类型 我们此时还得创建一个新的类型实现
```
 class Compare_double
    {
    public :
       Compare(double a,double b)
       {
          x=a;
          y=b;
       }
       double max()
       {
          return (x>y)?x:y;
       }
       double min()
       {
          return (x<y)?x:y;
       }
    private :
       double x,y;
    };
```
有两个或多个类，其功能是相同的，仅仅是数据类型不同，如上面语句声明了两个类重复性代码太多，使用类模板就可以减小这些问题，通过声明一个通用的类模板。
```
  template <class T>
 class Compare
 {
 public:
    Compare(T a, T b){
        x= a;
        y = b;
    }
    T max()
    {
        return (x>y)?x:y;
    }
    T min()
     {
        return(x<y)?x:y;
    }

    private:
      T x,y;
 };
int main(int argc, char *argv[])
{
     Compare<int> com(1,3);
    cout<< com.max()<<endl;
    cout<< com.min()<<endl;
    return 0;
}
```



