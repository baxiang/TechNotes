##字符串的构造
```c++
string();	   //构造一个空的字符串string 。
string(const string &str);	//构造一个与str一样的string。如string s1(s2)。
string(const char *s);    //用字符串s初始化
string(int n,char c);    //用n个字符c初始化
```
##字符串特性描述
```
int capacity()const;    //返回当前容量（即string中不必增加内存即可存放的元素个数）
int max_size()const;    //返回string对象中可存放的最大字符串的长度
int size()const;        //返回当前字符串的大小
int length()const;       //返回当前字符串的长度
bool empty()const;        //当前字符串是否为空
void resize(int len,char c);//把字符串当前大小置为len，并用字符c填充不足的部分
string类的输入输出操作:
string类重载运算符operator>>用于输入，同样重载运算符operator<<用于输出操作。
函数getline(istream &in,string &s);用于从输入流in中读取字符串到s中，以换行符'\n'分开。
```
##字符操作
```
const char &operator[](int n)const;
const char &at(int n)const;
char &operator[](int n);
char &at(int n);
operator[]和at()均返回当前字符串中第n个字符的位置，但at函数提供范围检查，当越界时会抛出out_of_range异常，下标运算符[]不提供检查访问。
const char *data()const;//返回一个非null终止的c字符数组
const char *c_str()const;//返回一个以null终止的c字符串
int copy(char *s, int n, int pos = 0) const;//把当前串中以pos开始的n个字符拷贝到以s为起始位置的字符数组中，返回实际拷贝的数目
```
##字符串赋值
```
string &operator=(const string &s);//把字符串s赋给当前的字符串
string &assign(const char *s); //把字符串s赋给当前的字符串
string &assign(const char *s, int n); //把字符串s的前n个字符赋给当前的字符串
string &assign(const string &s);  //把字符串s赋给当前字符串
string &assign(int n,char c);  //用n个字符c赋给当前字符串
string &assign(const string &s,int start, int n);  //把字符串s中从start开始的n个字符赋给当前字符串
```
##字符串连接
```
string &operator+=(const string &s);  //把字符串s连接到当前字符串结尾
string &operator+=(const char *s);//把字符串s连接到当前字符串结尾
string &append(const char *s);    //把字符串s连接到当前字符串结尾
string &append(const char *s,int n);  //把字符串s的前n个字符连接到当前字符串结尾
string &append(const string &s);   //同operator+=()
string &append(const string &s,int pos, int n);//把字符串s中从pos开始的n个字符连接到当前字符串结尾
string &append(int n, char c);   //在当前字符串结尾添加n个字符c
```
##字符串比较
```
nt compare(const string &s) const;  //与字符串s比较
int compare(const char *s) const;   //与字符串s比较
compare函数在>时返回 1，<时返回 -1，==时返回 0。比较区分大小写，比较时参考字典顺序，排越前面的越小。大写的A比小写的a小。

```
##字符串查找
```
int find(char c,int pos=0) const;  //从pos开始查找字符c在当前字符串的位置 
int find(const char *s, int pos=0) const;  //从pos开始查找字符串s在当前字符串的位置
int find(const string &s, int pos=0) const;  //从pos开始查找字符串s在当前字符串中的位置
find函数如果查找不到，就返回-1
int rfind(char c, int pos=npos) const;   //从pos开始从后向前查找字符c在当前字符串中的位置 
int rfind(const char *s, int pos=npos) const;
int rfind(const string &s, int pos=npos) const;
//rfind是反向查找的意思，如果查找不到， 返回-1

```
##字符串替换
```
string &replace(int pos, int n, const char *s);//删除从pos开始的n个字符，然后在pos处插入串s
string &replace(int pos, int n, const string &s);  //删除从pos开始的n个字符，然后在pos处插入串s
void swap(string &s2);    //交换当前字符串与s2的值

```
##字符串删除
```
iterator erase(iterator first, iterator last);//删除[first，last）之间的所有字符，返回删除后迭代器的位置
iterator erase(iterator it);//删除it指向的字符，返回删除后迭代器的位置
string &erase(int pos = 0, int n = npos);//删除pos开始的n个字符，返回修改后的字符串
```
##字符串迭代器 
string类提供了向前和向后遍历的迭代器iterator，迭代器提供了访问各个字符的语法，类似于指针操作，迭代器不检查范围。用string::iterator或string::const_iterator声明迭代器变量，const_iterator不允许改变迭代的内容。常用迭代器函数有：
```c++
const_iterator begin()const;
iterator begin();                //返回string的起始位置
const_iterator end()const;
iterator end();                    //返回string的最后一个字符后面的位置
const_iterator rbegin()const;
iterator rbegin();                //返回string的最后一个字符的位置
const_iterator rend()const;
iterator rend();                    //返回string第一个字符位置的前面
```
