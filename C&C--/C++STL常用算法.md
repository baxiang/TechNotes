##查找算法
####adjacent_find()
在iterator对标识元素范围内，查找一对相邻重复元素，找到则返回指向这对元素的第一个元素的迭代器。否则返回past-the-end。
```c++
    vector<int> vecInt;
    vecInt.push_back(1);
    vecInt.push_back(2);
    vecInt.push_back(3);
    vecInt.push_back(4);
    vecInt.push_back(5);
    vecInt.push_back(5);
    vector<int>::iterator it = adjacent_find(vecInt.begin(), vecInt.end());
```
####binary_search
在有序序列中查找value,找到则返回true。注意：在无序序列中，不可使用。
```c++
    set<int> setInt;
    setInt.insert(2);
    setInt.insert(1);
    setInt.insert(8);
    setInt.insert(5);
    setInt.insert(9);
    bool bFind = binary_search(setInt.begin(),setInt.end(),8);
```
####count() 
利用等于操作符，把标志范围内的元素与输入值比较，返回相等的个数。
```
   vector<int> vecInt;
    vecInt.push_back(8);
    vecInt.push_back(2);
    vecInt.push_back(1);
    vecInt.push_back(4);
    vecInt.push_back(8);
    vecInt.push_back(6);
    int iCount = count(vecInt.begin(),vecInt.end(),8);	//iCount==2
```
####count_if()
 count_if 算法计算中的元素范围 [first, last)，返回满足条件的元素的数量。
```
   vector<int> vecInt;
    vecInt.push_back(3);
    vecInt.push_back(3);
    vecInt.push_back(1);
    vecInt.push_back(4);
    vecInt.push_back(8);
    vecInt.push_back(9);
    int count = count_if(vecInt.begin(), vecInt.end(), evenNumber);// 偶数个数是 2;
```
####find
利用底层元素的等于操作符，对指定范围内的元素与输入值进行比较。当匹配时，结束搜索，返回该元素的迭代器。
	
```
    vector<int> vecInt;
    vecInt.push_back(3);
    vecInt.push_back(3);
    vecInt.push_back(2);
    vecInt.push_back(4);
    vecInt.push_back(8);
    vecInt.push_back(9);
    vector<int>::iterator it = find(vecInt.begin(), vecInt.end(), 2);
```
####equal_range:   
返回一对iterator，第一个表示lower_bound,第二个表示upper_bound。
```
    set<int> setInt;
    for (int i=0; i<10; i++)
    {
        setInt.insert(i+1);
    }
    pair<set<int>::iterator, set<int>::iterator>  mypair = setInt.equal_range(3);
    set<int>::iterator it1 = mypair.first;
    cout << "it1:" << *it1 << endl;  //3  //4
    set<int>::iterator it2 =  mypair.second;
    cout << "it2:" << *it2 << endl;  //4  //4
```
##查找算法
merge() 
以下是排序和通用算法：提供元素排序策略
merge:    合并两个有序序列，存放到另一个序列。
例如：vecIntA,vecIntB,vecIntC是用vector<int>声明的容器，vecIntA已包含1,3,5,7,9元素，vecIntB已包含2,4,6,8元素
vecIntC.resize(9);  //扩大容量
merge(vecIntA.begin(),vecIntA.end(),vecIntB.begin(),vecIntB.end(),vecIntC.begin());
此时vecIntC就存放了按顺序的1,2,3,4,5,6,7,8,9九个元素
####sort() 
sort:  以默认升序的方式重新排列指定范围内的元素。若要改排序规则，可以输入比较函数。
```
//学号比较函数
bool Compare(const Student &stuA,const Student &stuB)
{
    return (stuA.getID()<stuB.getID());
}
int main()
{
    vector<Student> vecStu;
    vecStu.push_back(Student(2,"赵彤"));
    vecStu.push_back(Student(1,"王伟"));
    vecStu.push_back(Student(4,"李明"));
    vecStu.push_back(Student(3,"张燕"));
    sort(vecStu.begin(),vecStu.end(),Compare);
    for (vector<Student>::iterator it =  vecStu.begin();it!= vecStu.end();it++) {
        cout<<it->getName() <<endl;
    }
}
```
random_shuffle() 
	random_shuffle:     对指定范围内的元素随机调整次序。
```
		srand(time(0));			//设置随机种子

		vector<int> vecInt;
		vecInt.push_back(1);
		vecInt.push_back(3);
		vecInt.push_back(5);
		vecInt.push_back(7);
		vecInt.push_back(9);

		string str("itcastitcast ");
	
		random_shuffle(vecInt.begin(), vecInt.end());   //随机排序，结果比如：9,7,1,5,3
		random_shuffle(str.begin(), str.end());		   //随机排序，结果比如：" itstcasticat "
```
reverse() 
```
		vector<int> vecInt;
		vecInt.push_back(1);
		vecInt.push_back(3);
		vecInt.push_back(5);
		vecInt.push_back(7);
		vecInt.push_back(9);
		reverse(vecInt.begin(), vecInt.end());		//{9,7,5,3,1}
```
