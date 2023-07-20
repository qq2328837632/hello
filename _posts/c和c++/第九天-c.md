---
title: 第九天 c++
date: 2023-04-11 16:39:12
tags:
---

# STL-函数对象

## 函数对象

### 函数对象概念

概念:

- 重载函数调用操作符的类，其对象常称为函数对象
- 函数对象使用重载的()时，行为类似函数调用，也叫仿函数

本质:

- 函数对象(仿函数)是一个类，不是一个函数

### 函数对象使用

特点:

- 函数对象在使用时，可以像普通函数那样调用,可以有参数，可以有返回值
- 函数对象超出普通函数的概念，函数对象可以有自己的状态
- 函数对象可以作为参数传递

## 谓词

### 谓词概念

概念:

- 返回bool类型的仿函数称为谓词
- 如果operator()接受一个参数，那么叫做一元谓词
- 如果operator()接受两个参数，那么叫做二元谓词

```
#include <vector>
#include <algorithm>
//1.一元谓词
class GreaterFive{
public:
	bool operator()(int val) {
		return val > 5;
	}
};
void test01() {
	vector<int> v;
	for (int i =0; i < 10; i++){
		v.push_back(i);
	}
	vector<int> : :iterator it = find_if(v.begin(), v.end()，GreaterFive());//匿名形式
	//GreaterFive如果括号里有常量，则这个与仿函数内部定义的量相对应，而重载的val与容器值对应，无需传入
	if (it == v.end()) {
		cout <<"没找到!”<< endl;
	}
	else {
		cout <<“找到:” <<*it << endl;
	}
}
```

```
#include <vector>
#include <algorithm>
//二元谓词
class MyCompare{
public:
	bool operator( )(int num1,int num2){
		return num1 > num2;
	}
};
void test01(){
	vector<int> v;
	v.push_back(10);
	v.push_back(40);
	v.push_back(20);
	v.push_back(30);
	v.push_back(50) ;
	//默认从小到大
	sort(v.begin(), v.end() );
	for (vector<int> : :iterator it = v.begin(); it != v.end(); it++){
		cout <<*it <<” ";
	}
	cout << endl;
	cout << "------------" <<endl;
	//使用函数对象改变算法策略，排序从大到小
	sort(v.begin(), v.end( ), MyCompare( ) );
	for (vector<int> ::iterator it = v.begin(); it != v.end(); it++){
		cout <<*it <<"";
	}
	cout <<endl;
}
```

总结:参数只有两个的谓词，称为二元谓词

# 内建函数对象

## 内建函数对象意义

概念:

- STL内建了一些函数对象

分类:

- 算术仿函数
- 关系仿函数
- 逻辑仿函数

用法:

- 这些仿函数所产生的对象，用法和一般函数完全相同
- 使用内建函数对象，需要引入头文件#include<functional>

## 算术仿函数

功能描述:

- 实现四则运算

其中negate是一元运算，其他都是二元运算仿函数原型:

- template<class T> T plus<T>//加法仿函数

  ```
  plus<int> p;
  cout << p( 10，20) << endl;
  ```

  

- template<class T> T minus<T>//减法仿函数

- template<class T> T multiplies<T>//乘法仿函数

- template<class T> T divides<T>//除法仿函数

- template<class T> T modulus<T>//取模仿函数

- template<class T> T negate<T>//取反仿函数

## 关系仿函数

功能描述:

- 实现关系对比

仿函数原型:

- template<class T> bool equal_to<T>//等于

- template<class T> bool not_equal_to<T>//不等于

- template<class T> bool greater<T>//大于

  ```
  //降序
  // sort(v.begin(), v.end()，MyCompare() ) ;
  sort(v.begin()， v.end(),greater<int>());
  ```

  

- template<class T> bool greater_equal<T>//大于等于

- template<class T> bool less<T>//小于

- template<class T> bool less_equal<T>//小于等于

总结:关系仿函数中最常用的就是greater<>大于,因为默认是less小于

## 逻辑仿函数

功能描述:

- 实现逻辑运算

函数原型:

- template<class T> bool logical_and<T>//逻辑与
- template<class T> bool logical_or<T>//逻辑或
- template<class T> bool logical_not<T>//逻辑非

# STL-常用算法

概述:

- 算法主要是由头文件<algorithm><functional><numeric>组成。
- <algorithm>是所有STL头文件中最大的一个，范围涉及到比较、交换、查找、遍历操作、复制、修改等等<numeric>体积很小，只包括几个在序列上面进行简单数学运算的模板函数
- <functional>定义了一些模板类,用以声明函数对象。

## 常用遍历算法

算法简介:

- for_each//遍历容器
- transform//搬运容器到另一个容器中

## for_each

函数原型:

- for_each(iterator beg, iterator end，_func);

​	//遍历算法遍历容器元素
​	//beg开始迭代器
​	//end结束迭代器
​	// _func函数或者函数对象

```
#include <algorithm>
#include <vector>
//普通函数
void print01(int val){
	cout <<val << "";
}
//函数对象
class print02{
public :
	void operator()(int val){
		cout << val <<" ";
	}
};
//for_each算法基本用法
void test01(){
	vector<int> v;
	for (int i = 0; i < 10; i++){
		v.push_back(i);
	}
	//遍历算法
	for_each(v.begin(), v.end( ), print01);//相当于传入了一个函数指针
	cout << endl;
	for_each(v.begin(), v.end(), print02());
	cout << endl;
}
```

## transform

函数原型:

- transform(iterator beg1, iterator end1， iterator beg2，_func);

​	//beg1源容器开始迭代器

​	//end1源容器结束迭代器

​	//beg2目标容器开始迭代器

​	//_func函数或者函数对象

总结:搬运的目标容器必须要提前开辟空间，否则无法正常搬运

## 常用查找算法

算法简介:

- find//查找元素
- find_if//按条件查找元素
- adjacent_find//查找相邻重复元素
- binary_search//二分查找法
- count//统计元素个数
- count_if//按条件统计元素个数

函数原型:
find(iterator beg, iterator end,value ) ;

- //按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
- //beg开始迭代器
- //end结束迭代器
- //value查找的元素

```
void test01() {
	vector<int> v;
	for (int i =0; i < 10; i++){
		v.push_back(i);
	}
	vector<int> : :iterator it = find(v.begin(), v.end()，5);//查找容器中是否有5这个元素
	if (it == v.end()) {
		cout <<"没找到!”<< endl;
	}
	else {
		cout <<“找到:” <<*it << endl;
	}
}
class Person {
public:
	Person( string name，int age){
		this->m_Name = name;
		this->m_Age = age;
	}
	//重载==,是find底层的==
	bool operato==(const Person& p){
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age){
			return true;
		}
		return false;
	}
public:
	string m_Name ;
	int m_Age;
};
void test02(){
	vector<Person> v;
	//创建数据
	Person p1( "aaa", 10);
	Person p2("bbb",20);
	Person p3("cce", 30);
	Person p4( "ddd",40);
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	Person pp=("ads",11);
	vector<Person> : :iterator it = find(v.begin(), v.end(), pp);
	if (it == v.end())
	{
		cout <<"没有找到!"<< endl;
	}
	else{
		cout <<"找到姓名:" << it->m_Name <<”年龄: " << it->m_Age << endl;
	}
}
```

总结:利用find可以在容器中找指定的元素，返回值是迭代器
