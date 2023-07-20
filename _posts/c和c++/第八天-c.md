---
title: 第八天 c++
date: 2023-04-10 16:38:50
tags:
---

## stack容器

### stack基本概念

概念: stack是一种先进后出(First In Last Out,FILO)的数据结构，它只有一个出口

栈中只有顶端的元素才可以被外界使用，因此栈不允许有遍历行为

### stack常用接口

功能描述:栈容器常用的对外接口

构造函数:

- stack<T> stk;//stack采用模板类实现, stack对象的默认构造形式
- stack( const stack &stk);//拷贝构造函数

赋值操作:

- stack& operator=(const stack &stk);//重载等号操作符

数据存取:

- push(elem);//向栈顶添加元素
- pop();//从栈顶移除第一个元素
- top();//返回栈顶元素

大小操作:

- empty();//判断堆栈是否为空
- size();//返回栈的大小

## queue容器

### queue基本概念

概念:Queud是一种先进先出(First ln First Out,FIFO)的数据结构，它有两个出口

- 队列容器允许从一端新增元素，从另一端移除元素
- 队列中只有队头和队尾才可以被外界使用，因此队列不允许有遍历行为
- 队列中进数据称为---入队push
- 队列中出数据称为---出队pop

### queue 常用接口

功能描述:栈容器常用的对外接口

构造函数:

- queue<T> que;//queue采用模板类实现，queue对象的默认构造形式
- queue(const queue &que) ;//拷贝构造函数

赋值操作:

- queue& operator=(const queue &que) ;//重载等号操作符

数据存取:

- push(elem) ;//往队尾添加元素
- pop();//从队头移除第一个元素
- back();//返回最后一个元素
- front();//返回第一个元素

大小操作:

- empty();//判断堆栈是否为空
- size();//返回栈的大小

## list容器

### list基本概念

功能:将数据进行链式存储

链表(list)是一种物理存储单元上非连续的存储结构，数据元素的逻辑顺序是通过链表中的指针链接实现的

链表的组成:链表由一系列结点组成

结点的组成:一个是存储数据元素的数据域，另一个是存储下一个结点地址的指针域

STL中的链表是一个双向循环链表

由于链表的存储方式并不是连续的内存空间，因此链表list中的迭代器只支持前移和后移，属于双向迭代器

list的优点:

- 采用动态存储分配，不会造成内存浪费和溢出
- 链表执行插入和删除操作十分方便，修改指针即可，不需要移动大量元素

list的缺点:

- 链表灵活，但是空间(指针域)和时间(遍历额外耗费较大

List有一个重要的性质，插入操作和删除操作都不会造成原有list迭代器的失效，这在vector是不成立的。

总结:STL中List和vector是两个最常被使用的容器，各有优缺点

### list构造函数

函数原型:

- list<T> lst;//list采用采用模板类实现,对象的默认构造形式:
- list(beg,end) ;//构造函数将[beg, end)区间中的元素拷贝给本身。
- list(n,elem) ;//构造函数将n个elem拷贝给本身。
- list(const list &lst);//拷贝构造函数。

总结:list构造方式同其他几个STL常用容器，熟练掌握即可

### list赋值和交换

函数原型:

- assign(beg,_end) ;//将[beg, end)区间中的数据拷贝赋值给本身。
- assign(n, eiem) ;//将n个elem拷贝赋值给本身。
- list& operator=(const list &lst);//重载等号操作符
- swap(lst);//将lst与本身的元素互换。

### list大小操作

函数原型:

- size();//返回容器中元素的个数
- empty();//判断容器是否为空
- resize(num ) ;//重新指定容器的长度为num，若容器变长，则以默认值填充新位置。//如果容器变短，则末尾超出容器长度的元素被删除。
- resize(num，elem);//重新指定容器的长度为num，若容器变长，则以elem值填充新位置。//如果容器变短，则末尾超出容器长度的元素被删除。

### list插入和删除

函数原型:

- push_back(elem);//在容器尾部加入一个元素.
- pop_back();//删除容器中最后一个元素
- push_front(elem);//在容器开头插入一个元素.
- pop_front();//从容器开头移除第一个元素
- insert(pos,elem);//在pos位置插elem元素的拷贝，返回新数据的位置。
- insert(pos,n,elem);//在pos位置插入n个elem数据，无返回值。
- insert(pos,beg,end);//在pos位置插入[beg,end)区间的数据，无返回值。
- clear();//移除容器的所有数据
- erase(beg,end);//删除[beg,end)区间的数据，返回下一个数据的位置。
- erase(pos);//删除pos位置的数据，返回下一个数据的位置。
- remove(elem);//删除容器中所有与elem值匹配的元素。

### list数据存取

```
//L1[0]不可以用[]访问list容器中的元素
//L1.at(O)不可以用at方式访问list容器中的元素
//原因是list本质链表，不是用连续线性空间存储数据，迭代器也是不支持随机访问的
cout <<"第一个元素为:"<< L1.front() << endl;
cout<<"最后一个元素为:"<< L1. back() << endl ;
//验证迭代器是不支持随机访问的
list<int> : :iterator it = L1. begin();
it++;//支持双向,可以用来判断其它容器是否支持双向访问，随机访问也一样
it--;
//it = it + 1;//不支持随机访问
```

总结:

- list容器中不可以通过[]或者at方式访问数据
- 返回第一个元素--- front
- 返回最后一个元素--- back

### list反转和排序

函数原型:

- reverse();//反转链表
- sort(); //链表排序

```
//指定排序规则
bool mycompare( int val1 , int val2){
	return val1 > val2;
}
//所有不支持随机访问迭代器的容器，不可以用标准算法
//不支持随机访问迭代器的容器，内部会提供对应一些算法
//sort(L1.begin()，L1.end() ) ;
L1.sort();//默认排序规则从小到大升序
L.sort( myCompare); //指定规则，从大到小
```

总结:

- 对于自定义数据类型，必须要指定排序规则(

  ```
  bool mycompare( int val1 , int val2){
  	return val1 > val2;
  }//val可以是任何类型，注意引用
  ```

  )，否则编译器不知道如何进行排序

- 高级排序只是在排序规则上再进行一次逻辑规则制定，并不复杂

## set/ multiset容器

### set基本概念

简介:

- 所有元素都会在插入时自动被排序

本质:

- set/multiset属于关联式容器，底层结构是用二叉树实现。

set和multiset区别:

- set不允许容器中有重复的元素（无resize）
- multiset允许容器中有重复的元素

总结:

- set容器插入数据时用insert
- set容器插入数据的数据会自动排序

总结:

- 统计大小--- size
- 判断是否为空--- empty
- 交换容器--- swap

函数原型:

- insert(elem);//在容器中插入元素。
- clear();//清除所有元素
- erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
- erase(beg, end);//删除区间[beg,end)的所有元素，返回下一个元素的迭代器。
- erase(elem);//删除容器中值为elem的元素。

- find(key ) ;//查找key是否存在,若存在，返回该键的元素的迭代器;若不存在，返回set.end();
- count(key);//统计key的元素个数,对于set而言统计结果要么是0要么是1

### set和multiset区别

区别:

- set不可以插入重复数据，而multiset可以
- set插入数据的同时会返回插入结果，表示插入是否成功. multiset不会检测数据，因此可以插入重复数据

总结:

- 如果不允许插入重复数据可以利用set
- 如果需要插入重复数据利用multiset

### pair对组创建

功能描述:

- 成对出现的数据，利用对组可以返回两个数据

两种创建方式:

- pair<type, tyie> p ( value1, value2 );
- pair<type, type> p = make_pair( value1，value2 );

### set容器排序

```
class MyCompare{
public:
	bool operator()(int v1,int v2)//重载（），//自定义数据类型都会指定排序规则
	{
		return v1 > v2;
	}
};
void test(){
	set<int，MyCompare>s2;//要放在插入之前，int与上面传入的数据类型一致,MyCompare是仿函数
	s2.insert(10) ;
}
```

总结:利用仿函数可以指定set容器的排序规则

总结:
对于自定义数据类型，set必须指定排序规则才可以插入数据

## map/multimap容器

### map基本概念

简介:

- map中所有元素都是pair
- pair中第一个元素为key(键值)，起到索引作期，第二个元素为value(实值)·所有元素都会  

本质:

- map/multimap属于关联式容器，底层结构是用二叉树实现。

优点:

- 可以根据key值快速找到value值

map和multimap区别:

- map不允许容器中有重复key值元素
- multimap允许容器中有重复key值元素

### map构造和赋值

构造:

- map<T1，T2> mp;//map默认构造函数:
- map(const ma &mp );//拷贝构造函数

赋值:

- map& operator=(const map &mp);//重载等号操作符

总结: map中所有元素都是成对出现，插入数据时候要使用对组

```
map<int,int>m;//默认构造
m.insert(pair<int, int>(1，10));
```

- 统计大小--- size
- 判断是否为空--- empty
- 交换容器--- swap

- insert(elem);//在容器中插入元素。
- clear();//清除所有元素
- erase(pos);//删除pos迭代器所指的元素，返回下一个元素的迭代器。
- erase(beg, end);//删除区间[beg,end)的所有元素，返回下一个元素的迭代器。
- erase(key);//删除容器中值为elem的元素。

```
m[4] = 40;
//[]不建议插入，用途	可以利用key访问到value
cout << m[4]<< endl;
```

- find(key ) ;//查找key是否存在,若存在，返回该键的元素的迭代器;若不存在，返回set.end();返回迭代器，需要迭代器接受map<int,int>: :iterator pos = m. find(3) ;
- count(key);//统计key的元素个数,对于set而言统计结果要么是0要么是1

map不允许插入重复key元素，count统计而言结果要么是0要么是1

multimap的count统计可能大于1

### map容器排序

学习目标:

- map容器默认排序规则为按照key值进行从小到大排序，掌握如何改变排序规则

主要技术点:

- 利用仿函数，可以改变排序规则

总结:

- 利用仿函数可以指定map容器的排序规则
- 对于自定义数据类型，map必须要指定排序规则，同set容器
