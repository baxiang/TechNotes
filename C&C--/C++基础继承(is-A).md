##继承特点
子类拥有父类的所有属性和方法（除了构造函数和析构函数）。
子类可以拥有父类没有的属性和方法。
## 继承语法
```
class 派生类名：继承方式 基类名1， 继承方式 基类名2，...，继承方式 基类名n
{
    派生类成员声明；
};
```
## 继承的访问控制
public继承：父类成员在子类中保持原有的访问级别（子类可以访问public和protected）。
private继承：父类成员在子类中变为private成员（虽然此时父类的成员在子类中体现为private修饰，但是父类的public和protected是允许访问的，因为是继承后改为private）。
protected继承
父类中的public成员会变为protected级别。
父类中的protected成员依然为protected级别。
父类中的private成员依然为private级别。
注意：父类中的private成员依然存在于子类中，但是却无法访问到。不论何种方式继承父类，子类都无法直接使用父类中的private成员。
## 继承中的构造函数
```
派生类名::派生类名（参数总表）：基类名1（参数表1），基类名（参数名2）....基类名n（参数名n），内嵌子对象1（参数表1），内嵌子对象2（参数表2）....内嵌子对象n（参数表n）
{
    派生类新增成员的初始化语句；
}

```
&emsp;&emsp;构造函数是为了初始类中的数据，对于派生类而言，不会继承基类的构造函数，因此为了完成派生类数据的初始化需要在派生类中自己定义构造函数，派生类的构造函数除了需要初始化派生类中新增的数据成员 还需要初始化基类中的数据成员
```
class student
{
  public:
    void display()
    {
        cout<<"ID: "<<s_ID<<" name : "<<name <<" age: "<<age <<endl;
    }
    student(int s_id,string s_name,int age)
    {
        s_ID = s_id;
        name = s_name;
        age = age;
    }
private:
    int s_ID;
    string name;
    int age;

};
class midStudent :public student
{
public:
    midStudent(int s_ID,string s_name,int age,int score):student(s_ID,s_name,age)
    {
        collegeScrore = score;
    }
    void  getCollegeScore()
    {
        cout<<"score :"<<collegeScrore<<endl;
    }
 private:
    int collegeScrore;

};


int main(int argc, char *argv[])
{
    midStudent stu(1,"baxiang",18,100);
    stu.display();
    stu.getCollegeScore();
    return 0;
}
```
## 覆写基类同名函数
&emsp;&emsp;派生类中重新定义基类的同名函数的方法，成为对基类的函数的覆写，覆写后基类的同名函数在派生类中被隐藏，定义派生类对象调用该函数，调用的是自身的函数，基类的同名函数不会被调用。
1. 若想调用基类的同名函数，可在函数前面加上基类的名称和作用域符号“::”
```
using  namespace std;
class Animal{
public:
    void speak(){
        cout<<"annimal lange!"<<endl;
    }
};

class Dog:public  Animal
{
public:
    void speak()
    {
        cout<<"dog language:anmial"<<endl;
    }
};

int main() {
    Dog dog;
    dog.speak();
    dog.Animal::speak();// 调用基类同名函数
    return 0;
}
```
##多重继承构造函数
派生类的构造函数后面的参数包含了各干基类的构造函数需要的所有参数，多重继承派生类的构造函数需要调用该派生类的所有构造函数
```
类名：类名构造函数（参数列表）：基类1构造函数（参数表1），基类2构造函数（参数表2）
{
  构造函数具体实现
}
```
多重继承调用顺序
1. 调用基类构造函数，按照派生类中定义的先后顺序，依次调用
2. 调用对象成员的构造函数
3. 调用派生类的构造函数
```
 Bird(int fh){
        cout<<"Bird constructor"<<endl;
        m_flightAltitude = fh;
    }
 Fish(int speed)
    {
        cout<<"Fish constructor"<<endl;
        swim_speed = speed;
    }
 WaterBird(int fh, int speed):Bird(fh),Fish(speed)
    {
        cout<<"warerbird constructor"<<endl;
    }
```


