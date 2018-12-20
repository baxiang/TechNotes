
##函数的定义
```
[修饰符] <返回类型> <函数名>（<形式参数列表>）[override] [const] [final]
```
&emsp;&emsp;返回类型是必须的，当没有返回类型的时候就用void代替，如果参数个数超过1，则用逗号分隔参数列表，参数列表可以为空。

##默认参数
C++中可以在函数声明时为参数提供一个默认值，当函数调用时没有指定这个参数的值，编译器会自动用默认值代替一旦在一个函数调用中开始使用默认参数值，那么这个参数后的所有参数都必须使用默认参数
```c++
void myPrint(int x = 3)
{
	printf("x:%d", x);
}
```
##函数重载(Overroad）
&emsp;&emsp;函数重载(Function Overload)用同一个函数名定义不同的函数,当函数名和不同的参数搭配时函数的含义不同。 函数重载至少满足下面的一个条件：1.参数个数不同。 2.参数类型不同。3.参数顺序不同。

```c++
class foo
{
public:
    int add(int a,int b)
    {
        return  a+b;
    }
    float add(float a, float b)
    {
        return  a+b;
    }
};
```
函数的返回值不作为区分重载函数的的条件。
```
float add(int a, int b)
    {
        return  a+b;
    } //编译错误
```
####override
&emsp;&emsp;如果不使用override，当你手一抖，将**foo()**写成了**f00()**会怎么样呢？结果是编译器并不会报错，因为它并不知道你的目的是重写虚函数，而是把它当成了新的函数。如果这个虚函数很重要的话，那就会对整个程序不利。所以，override的作用就出来了，它指定了子类的这个虚函数是重写的父类的，如果你名字不小心打错了的话，编译器是不会编译通过的.
```
class A
{
    virtual void foo();
};
class B :public A
{
     //void fo0();
     void fo0() override; //会编译报错
};
```

## virtual 虚函数
&emsp;&emsp;被virtual关键字修饰的成员函数，就是虚函数。虚函数的作用，用专业术语来解释就是实现多态性 （Polymorphism），多态性是将接口与实现进行分离，虚函数是C++ 的多态性的主要体现，指向基类的指针在操作它的多态类对象时，会根据不同的类对象，调用其相应的函数。
```
class Parent {
public:
    void func_one(){
        cout<<"parent:func_one"<<endl;
    }
    virtual void func_two(){
        cout<<"parent:func_two"<<endl;
    }
};

class Child : public Parent{
public:
   void func_one(){
       cout<<"Child:func_one"<<endl;
   }
    virtual void func_two(){
        cout<<"Child:func_two"<<endl;
    }
};


int main() {
    Child child;
    Parent *c= &child;//指向子类的指针
    c->func_one();
    c->func_two();
    Parent &p = child;//子类的引用
    p.func_one();
    p.func_two();
    return 0;
}
打印结果：
parent:func_one
Child:func_two
parent:func_one
Child:func_two
```
&emsp;&emsp;简单总结就是：基类中将某方法定义为虚函数，则在派生类中，该方法仍为虚方法。在使用时，定义基类类型的指针，使其指向派生类的对象，使用该指针调用某个方法，若该方法未被声明为虚函数，则调用的是指针类中的方法，若该方法是虚函数，则调用的是指针指向对象类中的该方法。这也即是动态联编。
虚函数使用原则:
1)当类不会用作基类时，成员函数不要声明为virtual
2)当成员函数不重新定义基类的方法，成员函数不要声明为virtual

##inline内联函数
&emsp;&emsp;内联函数由 编译器处理，直接将编译后的函数体插入调用的地方。宏代码片段 由预处理器处理， 进行简单的文本替换，没有任何编译过程
```c++
#include "iostream"
using namespace std;
#define MYFUNC(a, b) ((a) < (b) ? (a) : (b))  

inline int myfunc(int a, int b) 
{
	return a < b ? a : b;
}

int main()
{
	int a = 1;
	int b = 3;
	//int c = myfunc(++a, b);  //头疼系统
	int c = MYFUNC(++a, b);  

	printf("a = %d\n", a); 
	printf("b = %d\n", b);
	printf("c = %d\n", c);

system("pause");
	return 0;
}
```
&emsp;&emsp;编译器对于内联函数的限制并不是绝对的，内联函数相对于普通函数的优势只是省去了函数调用时压栈，跳转和返回的开销。因此，当函数体的执行开销远大于压栈，跳转和返回所用的开销时，那么内联将无意义。C++中内联编译的限制：
1.不能存在任何形式的循环语句    2.不能存在过多的条件判断语句 3.函数体不能过于庞大 4.不能对函数进行取址操作 5.函数内联声明必须在调用语句之前。

##静态成员函数
&emsp;&emsp;静态成员函数数添加关键字static，类的静态成员(变量和方法)属于类本身，在类加载的时候就会分配内存，可以通过类名直接去访问；非静态成员（变量和方法）属于类的对象，所以只有在类的对象产生（创建类的实例）时才会分配内存，然后通过类的对象（实例）去访问。调用静态成员函数如下：
```
<类名>::<静态成员名>
```
&emsp;&emsp;因为静态成员函数属于整个类，在类实例化对象之前就已经分配空间了，而类的非静态成员必须在类实例化对象后才有内存空间，所以静态成员函数中，不能使用普通变量和成员函数，静态成员函数与非静态成员函数的根本区别是：非静态成员函数有 this 指针，而静态成员函数没有 this 指针。

```
private:
    int x;
public:
    static void output()
        {
        cout<<x<<endl;
    }
};
//error: invalid use of member 'x' in static member function
```

##友元函数
```
friend <返回类型> <函数名> (<参数列表>);
```
&emsp;&emsp;友元函数是可以直接访问类的私有成员的非成员函数。它是定义在类外的普通函数，它不属于任何类，但需要在类的定义中加以声明，声明时只需在友元的名称前加上关键字friend。
&emsp;&emsp;需要注意的是友元函数不是成员函数，却可以访问类中的私有成员。友元的作用在于提高程序的运行效率（即减少了类型检查和安全性检查等都需要的时间开销），同时它破坏了类的封装性和隐藏性，使得非成员函数可以访问类的私有成员。
```
Point::Point(int currX, int currY)
{
    x = currX;
    y = currY;
}

double  distance(const Point &a,const Point &b)
{
    double length;
    length=sqrt((a.x-b.x)*(a.x-b.x)+(a.y-b.y)*(a.y-b.y));     //它可以引用类中的私有成员
    return length;
}

int main()
{
    Point p1(0,3), p2(4,0);
    cout<<distance(p1,p2)<<endl;
}
```
