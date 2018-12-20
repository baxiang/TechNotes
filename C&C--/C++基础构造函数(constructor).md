
##构造函数定义
C++中的类可以定义与类名相同的特殊成员函数，这种与类名相同的成员函数叫做构造函数，构造函数在定义时可以有参数；但是是没有任何返回类型的声明。
## 构造函数语法
```
class 类名
{
public:
    类名(参数列表)
{
   函数体
}
private:
   数据成员
};
```
构造函数的特点：
1. 函数名称就是当前的类名称。
2. 构造函数没有返回值
##构造函数的调用 
####显示调用构造函数
```
Student studentOne = Student("st1",20);
```
####隐式调用构造函数
```
Student studentTwo("st2",21);
```
#### new创建指针
创建一个Student对象，并将该对象的地址赋给studentThree指针，对象没有名称，但是可以通过指针来管理对象。
```
Student *studentThree = new Student("St3",18);
```
##构造函数的作用
初始化对象的数据成员。

##构造函数的种类
####默认构造函数
默认构造函数实在未提供显示初始化值用来创建对象的，主要用于下面这种情况.
```
Student stu;
```
默认构造函数可能如下函数:
```
Student::Student(){}
```
注意：
1. 只有当且仅当类没有定义任何构造函数的情况下，编译器才会创建默认构造函数。
2.当类中定义了构造函数，但是没有提供默认构造函数，下面的声明是错误的：所以如果想使用默认构造函数必须重载来定义一个没有参数的构造函数。
```
Student()
    {
        name = "";
        age = 0;
    }
```


####无参构造函数
编译器本身会提供一个无参的构造函数，但是这个系统的无参构造函数实际意义没有太大，因为系统默认的无参构造函数没有给成员属性提供初始值，而是会随机分配初始值，因此通常自定义的无参构造函数会对类中的数据进行初始值。
```
class Student
{
public:
    Student()
    {
        s_Name = "default name";
    }
    string getName()
    {
        return s_Name;
    }
private:
   string s_Name;
};

int main(int argc, char *argv[])
{
    Student stu;
    cout<<"name: " << stu.getName() <<endl;
    return 0;
}
```
####自定义带参数构造函数
通过初始化列表实现构造参数，就是在参数列表的后面加冒号“：”
```
  Student(string s_Name):s_Name(s_Name){}
```
默认参数得构造函数
默认参数的构造函数 需要防止调用的二义性，构造函数的第n个参数有默认值，则其后的所有参数都有默认值
```
 Student(int id ,string name = "default baxiang",int age = 0){
          s_ID = id;
          s_Name = name;
          s_age = age;
    }
```
####包含成员对象的构造函数
个类的对象时，应先调用其构造函数。但是如果这个类有成员对象，则要先执行成员对象自己所属类的构造函数，当全部成员对象都执行了自身类的构造函数后，再执行当前类的构造函数
```
<类名>::<类名>(<总参数表>):<成员对象1>(<形参表1>), <成员对象2>(<形参表2>)，……
{ //函数体 }
```
```
class SDate
{
public:
    SDate(int y,int m,int d);
    void show();


private:
    int year,month,day;
};
SDate::SDate(int y,int m,int d)
{
    year = y;
    month = m;
    day = d;
}
void SDate::show()
{
        cout<<year<<month<<day<<endl;
 }

class Student
{
public:
   Student(string name,int y,int m,int d);
   void printInfo();
private:
    string s_Name;
    SDate birthday;
};

Student:: Student(string name,int y,int m,int d):birthday(y,m,d){
    s_Name = name;
}
```
#### 拷贝构造函数（复制构造函数）
```
class 类名
{
public:
          类名:(类名 & 变量名)
         {
           函数体
         }
};
```
使用一个对象初始化另外一个新的对象
```
class Student
{
public:
    Student(string n,int a){
        name =n;
        age = a;
    }
    Student(Student &s){
        name = s.name;
        age = s.age;
    }
   void printInfo()
   {
       cout<<"name " <<name << " age "<<age <<endl;
   }
private:
    string name;
    int age;
};

int main(int argc, char *argv[])
{
    Student student("baxiang",18);
    student.printInfo();
    Student copyS(student);
    copyS.printInfo();
    return 0;
}
```
对象作为函数参数
当参数为对象是 会调用拷贝构造函数
```
class Student
{
public:
    Student(string n,int a){
        name =n;
        age = a;
    }
    Student(Student &s){
        cout<<"copy func"<<endl;
        name = s.name;
        age = s.age;
    }
    void printInfo()
    {
        cout<<"name " <<name << " age "<<age <<endl;
    }
private:
    string name;
    int age;
};

void printStudent(Student s)
{
    s.printInfo();
}
int main(int argc, char *argv[])
{
    Student student("baxiang",18);
    printStudent(student);
    return 0;
}
```

## 浅拷贝与深拷贝
浅拷贝是只拷贝指针地址，意思是浅拷贝指针都指向同一个内存空间，当原指针地址所指空间被释放，那么浅拷贝的指针全部失效。
深拷贝是先申请一块跟被拷贝数据一样大的内存空间，把数据复制过去。这样拷贝多少次，就有多少个不同的内存空间，数据之间相互独立互不影响。
