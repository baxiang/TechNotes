
##vector
vector是将元素置于一个动态数组中加以管理的容器。
vector可以随机存取元素（支持索引值直接存取， 用[]操作符或at()方法，这个等下会详讲）。
vector尾部添加或移除元素非常快速。但是在中部或头部插入元素或移除元素比较费时

```
#import <vector>
using namespace std;

int main() {
    vector<int> v;
    for (int i = 0; i <10 ; ++i) {
         v.push_back(1+i);
    }
    for (int j = 0; j <v.size() ; ++j) {
          cout<<v[j]<<" ";
    }
    cout <<endl;
    v.pop_back();
    for (int j = 0; j <v.size() ; ++j) {
        cout<<v[j]<<" ";
    }
    return 0;
}
```
## Deque
deque是“double-ended queue”的缩写，和vector一样都是STL的容器，deque是双端数组，而vector是单端的。
deque在接口上和vector非常相似，在许多操作的地方可以直接替换。
deque可以随机存取元素（支持索引值直接存取， 用[]操作符或at()方法，这个等下会详讲）。
deque头部和尾部添加或移除元素都非常快速。但是在中部安插元素或移除元素比较费时。
deque采用模板类实现，deque对象的默认构造形式：deque<T> deqT; 
``` 
deque <int> deqInt;            //一个存放int的deque容器。
deque <float> deq Float;     //一个存放float的deque容器。
deque <string> deq String;     //一个存放string的deque容器。
```

####deque末尾的添加移除操作
```
deque.push_back(elem);	//在容器尾部添加一个数据
deque.push_front(elem);	//在容器头部插入一个数据
deque.pop_back();    		//删除容器最后一个数据
deque.pop_front();		//删除容器第一个数据
```
####插入
```
deque.insert(pos,elem);   //在pos位置插入一个elem元素的拷贝，返回新数据的位置。
deque.insert(pos,n,elem);   //在pos位置插入n个elem数据，无返回值。
deque.insert(pos,beg,end);   //在pos位置插入[beg,end)区间的数据，无返回值。
```
##Stack
stack是堆栈容器，是一种“先进后出”的容器。
stack是简单地装饰deque容器而成为另外的一种容器。

##Queue容器 
queue是队列容器，是一种“先进先出”的容器。
queue是简单地装饰deque容器而成为另外的一种容器。
```
queue<int> queInt;            //一个存放int的queue容器。
queue<float> queFloat;     //一个存放float的queue容器。
queue<string> queString;     //一个存放string的queue容器。
```

##List 
List 是一个双向链表
```
list<int> lstInt;            //定义一个存放int的list容器。
list<float> lstFloat;     //定义一个存放float的list容器。
list<string> lstString;     //定义一个存放string的list容器。
```
#### 元素访问
```
it.front();// 返回第一个元素
it.back();// 返回最后一个元素
it.begin();//返回第一个元素的迭代器
it.end();//返回指向最后一个元素的下一个迭代器
```
####添加元素
```
it.push_back();// 在尾部插入数据
it.push_front();//在头部插入数据
it.insert(pos,elem);// 在pos位置插入元素
it.insert(pos,n,elem)//  在pos位置插入n个元素elem;
it.insert(pos,begin,end);//在pos位置插入[begin end]区间的值作为元素
```
####删除元素
```
it.pop_back();// 从尾部删除元素
it.pop_front();//在头部插入元素
it.erase(pos);// 从中间删除元素
it.remove(elem);// 在容器中删除所有与elem匹配的元素
```

```
using namespace std;
template <typename  T>
void printList(list<T> currList){
    auto index = currList.begin();
    for (index;index!=currList.end();index++) {
        cout<< *index<<" ";
    }
    cout<<endl;
};

int main() {
    list<int> it;
    for (int i = 0; i < 10; ++i) {
        it.push_back(i+1);
    }
    printList(it);
    it.pop_back();
    it.push_front(5);
    printList(it);
    it.remove(5);
    printList(it);
    return 0;
}
```
时间复杂度：

| container        | access       | insert or erase  |
| :-------------: |:-------------:| :-----:|
|vector	|O(1)	|O(n^2)|
|list	|O(n)	|O(n)|
|dequeue	|O(n)	|O(n)|
