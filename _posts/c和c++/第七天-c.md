---
title: 第七天 c++
date: 2023-04-08 20:26:50
tags:
---

# 容器算法迭代器初识

## vector存放内置数据类型

容器:`vector`

算法:`for_each`

迭代器:`vector<int>::iterator`

```
#include <vector>
#include <algorilhm>
void MyPrint(int val){
	cout << val << endl;
}
void test01() {
	//创建vector容器对象，并且通过模板参数指定容器中存放的数据的类型
	vector<int> v;
	//向容器中放数据
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);
	//每一个容器都有自己的迭代器,迭代器是用来遍历容器中的元素
	//v.begin()返回迭代器，这个迭代器指向容器中第一个数据
	//v.end()返回迭代器，这个迭代器指向容器元素的最后一个元素的下一个位置
	//vector<int>::iterator拿到vector<int>这种容器的迭代器类型
	vector<int> ::iterator pBegin = v.begin();
	vector<int> ::iterator pEnd = v.end();
	//第—种遍历方式:
	while (pBegin ! = pEnd){
		cout <<*pBegin << endl;
		pBegin++;
	}
	//第二种遍历方式:
	for (vector<int> : :iterator it = v.begin(); it != v.end( ); it++) {
		cout <<*it << endl;
	}
	cout <<endl;
	//第三种遍历方式:
	//使用STL提供标准遍历算法头文件algorithm
	for_each(v.begin(), v.end( ), MyPrint);
}
```

##  Vector容器嵌套容器

学习目标:

容器中嵌套容器，我们将所有数据进行遍历输出

```
//容器嵌套容器
void test01() {
	vector< vector<int> >v ;
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	vector<int> v4;
	for (int i = ; i < 4; i++) {
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
		v4.push_back(i + 4);
	}
	//将容器元素插入到vector v中
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	v.push_back(v4);
	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++)
	{
		for (vector<int>: :iterator vit = (*it).begin(); vit != (*it).end(); vit++) 
		{
			cout <<*vit << " ";
		}
	cout<<endl;
	}
}
```

## string容器

### string赋值操作

功能描述:

- 给string字符串进行赋值

赋值的函数原型:

- string& operator=(const char* s );//char*类型字符串赋值给当前的字符串
- string& operator=(const string &s );//把字符串s赋给当前的字符串
- string& operator=( char c);//字符赋值给当前的字符串
- string& assign(const char *s ) ;//把字符串s赋给当前的字符串
- string& assign(const char *s, int n);//把字符串s的前n个字符赋给当前的字符串
- string& assign( const sting &s );//把字符串s赋给当前字符串
- string& assign(int n, char c);//用n个字符c赋给当前字符串

```
void test01(){
	str1 = "hello vorld";
	cout << "str1 - " << str1 << endl;
	string str2;
	str2 = str1;
	cout << "str2 = " << str2 << endl;
	string str3;
	str3 = 'a ';
	cout << "str3 = " << str3 << endl;
	string str4;
	str4.assign( "hello c++");
	cout << "str4 = " <<str4 << endl;
	string str5;
	str5.assign("hello c++",5);
	cout << "str5 =<< str5 << endl;
	string str6;
	str6.assign(str5);
	cout << "str6 = " << str6 << endl;
	string str7;
	str7 .assign(5，'x');
	cout << "str7 = " << str7 << endl;
}
```

总结:
string的赋值方式很多,operator=这种方式是比较实用的

### string字符串拼接

功能描述:

- 实现在字符串末尾拼接字符串

函数原型:

- string& operator+=(const char* str);//重载+=操作符
- string& operator+=( const char c);//重载+=操作符
- string& operator+=( const string& str);//重载+=操作符
- string& append(const char *s );//把字符串s连接到当前字符串结尾
- string& append(const char *s, int n);//把字符串s的前n个字符连接到当前字符串结尾
- string& append(const string &s );//同operator+=(const string& str)
- string& append(const string &s，int pos，int n);//字符串s中从pos开始的n个字符连接到字符串结尾

### string查找和替换

函数原型:

- int find(const string& str, int pos = 0) const;//查找str第一次出现位置,从pos开始查找
- int find( cqhst char* s , int pos =0) const;//查找s第一次出现位置,从pos开始查找
- int find(const char* s, int pos， int n) const;//从pos位置查找s的前n个字符第一次位置
- int find(const char c, int pos =0) const;//查找字符c第一次出现位置
- int rfind(const string& str, int pos = npos) const;//查找str最后一次位置,从pos开始查找
- int rfind(const char* s, int pos = npos) const;//查找s最后一次出现位置,从pos开始查找
- int rfind(const char* s, int pos, int n) const;//从pos查找s的前n个字符最后一次位置
- int rfind(const char c, int pos = npos) const;//查找字符c最后一次出现位置
- string& replace(int pos, int n, const string& str);//替换从pos开始n个字符为字符串str
- string& replace(int pos, int n,const char*s );//替换从pos开始的n个字符为字符串s

总结:

- . find查找是从左往后，rfind从右往左

- . find找到字符串后返回查找的第一个字符位置，找不到返回-1

- . replace在替换时，要指定从哪个位置起，多少个字符，替换成什么样的字符串

  ------

  

总结:字符串对比主要是用于比较两个字符串是否相等，判断谁大谁小的意义并不是很大

------

string中单个字符存取方式有两种

- char& operator[](int n);//通过[]方式取字符

- char& at(int n);//通过at方法获取字符

  ------

  

函数原型:

- string& insert(int pos, const char* s );//插入字符串

- string& insert(int pos, const string& str);//插入字符串

- string& insert(int pos, int n, char c);//在指定位置插入n个字符c

- string& erase(int pos, int n = npos);//删除从Pos开始的n个字符

  ------

  

函数原型:

- string substr(int pos = 0,int n = npos) const;//返回由pos开始的n个字符组成的字符串

## vector容器

### vector构造函数

功能描述:

- 创建vector容器

函数原型:

- vector<T> v;//采用模板实现类实现，默认构造函数
- vector(v.begin(), v.end());//将v[begin(), end())区间中的元素拷贝给本身。
- vector(n, elem );//构造函数将n个elem拷贝给本身。
- vector( const vector &vec);//拷贝构造函数。

### vector赋值操作

函数原型:

- vector& operator=(const vector &vec);//重载等号操作符
- assign(beg, end);//将[beg, end)区间中的数据拷贝赋值给本身。
- assign(n, elem);//将n个elem拷贝赋值给本身。

### vector容量和大小

函数原型:

- empty();//判断容器是否为空
- capacity();//容器的容量
- size();//返回容器中元素的个数
- resize(int num);//重新指定容器的长度为num，若容器变长，则以默认值填充新位置。//如果容器变短，则末尾超出容器长度的元素被删除。
- resize(int num，elem);//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。
  //如果容器变短，则末尾超出容器长度的元素被删除

### vector插入和删除

函数原型:

- push_back(ele);//尾部插入元素ele
- pop_back();//删除最后一个元素
- insert(const_iterator pos,ele);//迭代器指向位置pos插入元素ele
- insert(const_iterator pos, int cunt,ele);//迭代器指向位置pos插入count个元素ele
- erase(const_iterator pos);//删除迭代器指向的元素
- erase(const_iterator start， const_iterator end);//删除迭代器从start到end之间的元素
- clear();//删除容器中所有元素

### vector互换容器

函数原型;

- swap(vec);//将vec与本身的元素互换

```
void teste2()
{
	vector<int> v;
	for (int i - e; i <100000; i++) {
		v.push_back(i);
	}
	cout <<"v的容量为:"<< v.capacity() << endl;
	cout <<"v的大小为:"<< v.size() << endl;
	v.resize(3);
	cout <<“v的容量为:"<< v.capacity() << endl;
	cout <<“v的大小为: " << v.size()<< endl;
	//收缩内存
	vector<int>(v).swap(v); //匿名对象vector<int>(v),读完这行自动被系统删除，匿名对象创建与v.size相同大小的cap和size
	cout <<“v的容量为:"<< v.capacity() << endl;
	cout << "v的大小为:" << v.size() << endl;
}
```

总结: swap可以使两个容器互换，可以达到实用的收缩内存效果.

### vector预留空间

功能描述:

- 减少vector在动态扩展容量时的扩展次数

函数原型:

- reserve(int len);//容器预留len个元素长度，预留位置不初始化，元素不可访问。

```
void teste1()
{
	vector<int> v;
	//预留空间
	v.reserve ( 100000);
	int num = 0;
	int* p = NULL;
	for (int i = 0; i < 100080; i++) {
		v.push_back(i);
	if (p != &v[0]){
		p = &v[0];//用来计算开辟了多少次内存,如果没有预留内存，则每次vector容量不够会重新分配，即&v[0]会改变
		num++;
	}
	cout<<"num="<<num<<endl;
}
```

总结:如果数据量较大，可以一开始利用reserve预留空间

## C++中resize和reserve的区别

reserve是设置了capacity的值，比如reserve(20)，表示该容器最大容量为20，但此时容器内还没有任何对象，也**不能通过下标访问**。会发生问题。
resize既分配了空间，也创建了对象，可以通过下标访问。
reserve只修改capacity大小，不修改size大小，resize既修改capacity大小，也修改size大小。

所以以后访问string容器的值不够用什么方法都需要resize来初始化默认值防止出现错误。

## deque容器

### deque容器基本概念

功能:

- 双端数组，可以对头端进行插入删除操作

deque与vector区别:

- vector对于头部的插入删除效率低，数据量越大，效率越低.
- deque相对而言，对头部的插入删除速度比vector快
- vector访问元素时的速度会比deque快,这和两者内部实现有关

deque内部工作原理:

deque内部有个中控器，维护每段缓冲区中的内容，缓冲区中存放真实数据

中控器维护的是每个缓冲区的地址，使得使用deque时像—片连续的内存空间

```
void printDeque(const deque<int>& d){//限制不允许修改，只读，则 const_iterator
	for (deque<int> :: const_iterator it = d.begin(); it != d.end(); it++){
		cout <<*it<<"";
	}
	cout <<endl;
}
// deque构造
void teste1(){
	deque<int> d1;//无参构造函数
	for (int i = e; i < 10; i++){
		d1.push_back(i);
	}
	printDeque(d1);
}
```

总结: deque容器和vector容器的构造方式几乎一致，灵活使用即可

### deque赋值操作

函数原型:

- deque& operator=(const deque &deq);//重载等号操作符
- assign(beg, end) ;//将[beg, end)区间中的数据拷贝赋值给本身。
- assign(n, elem);//将n个elem拷贝赋值给本身。

函数原型:

- deque.empty();//判断容器是否为空

- deque.size();//返回容器中元素的个数,deque没有容量概念（cap）

- deque.resize(num );//重新指定容器的长度为num,若容器变长，则以默认值0填充新位置。//如果容器变短，则末尾超出容器长度的元素被删除。

- deque.resize(num，elem);//重新指定容器的长度为num,若容器变长，则以elem值填充新位置

  //如果容器变短，则末尾超出容器长度的元素被删除。

总结:

- deque没有容量的概念
- 判断是否为空--- empty
- 返回元素个数--- size
- 重新指定个数--- resize

### 3.3.5 deque插入和删除

功能描述:

- 向deque容器中插入和删除数据

函数原型:

​	两端插入操作:

- push_back(elem);//在容器尾部添加一个数据
- push_front(elem);//在容器头部插入一个数据
- pop_back();//删除容器最后一个数据
- pop_front( );//删除容器第一个数据

指定位置操作:

- insert(pos,elem) ;//在pos位置插入一个elem元素的拷贝，返回新数据的位置。
- insert( pos,n,elem);//在pos位置插入n个elem数据，无返回值。
- insert(pos ,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
- clear();清空容器的所有数据
- erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
- erase(pos);//删除pos位置的数据，返回下一个数据的位置。

总结:

插入和删除提供的位置是迭代器!(如d.begin()),还可以

```
deque<int>::iterator it = d1.begin() ;
it++;
d1.erase(it);
```

尾插--- push_back

尾删--- pop_back

头插--- push_front

头删--- pop_front

------

函数原型:

- at(int idx);//返回索引idx所指的数据
- operator[]; //返回索引idx所指的数据
- front( );//返回容器中第一个数据元素
- back();//返回容器中最后一个数据元素
