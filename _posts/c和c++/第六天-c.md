---
title: 第六天 c++
date: 2023-04-07 16:10:16
tags:
---

# 模板

## 函数模板语法

函数模板作用:
建立一个通用函数，其函数返回值类型和形参类型可以不具体制定，用一个虚拟的类型来代表。
语法:

```
template<typename T>

函数声明或定义
```

解释:

template ---声明创建模板Ⅰ

typename ---表面其后面的符号是—种数据类型，可以用class代替T---通用的数据类型，名称可以替换，通常为大写字母

```
//利用模板提供通用的交换函数
template<typename T>
void mySwap(T& a，T& b)
{
	T temp = a;
	a = b;
	b = temp;
}
void test01()
{
	int a = 10;
	int b = 20;
	// swapInt(a，b);
	//利用模板实现交换
	//1、自动类型推导
	mySwap(a，b);
	//2、显示指定类型
	mySwap<int>(a，b);
	cout << "a = " << a << endl;
	cout<< " b = "<< b << endl;
}
```

总结:

- 函数模板利用关键字template
- 使用函数模板有两种方式:自动类型推导、显示指定类型
- 模板的目的是为了提高复用性，将类型参数化
- 使用模板时必须确定出通用数据类型T，并且能够推导出一致的类型

普通函数与函数模板区别:

- 普通函数调用时可以发生自动类型转换(隐式类型转换)
- 函数模板调用时，如果利用自动类型推导，不会发生隐式类型转换
- 如果利用显示指定类型的方式，可以发生隐式类型转换

总结:建议使用显示指定类型的方式，调用函数模板，因为可以自己确定通用类型T。

## 普通函数与函数模板的调用规则

调用规则如下:

1.如果函数模板和普通函数都可以实现，优先调用普通函数

2.可以通过空模板参数列表来强制调用函数模板

3.函数模板也可以发生重载

4.如果函数模板可以产生更好的匹配,优先调用函数模板

```
//普通函数与函数模板调用规则
void myPrint(int a, int b){
	cout <<"调用的普通函数”"<<endl;
template<typename T>
void myPrint(T a， T b)
{
	cout <<“调用的模板"<< endl;
}

template<typename T>
void myPrint(T a， T b， T c)
{
	cout <<"调用重载的模板"<< endl;
}
void test01()
{
    //1、如果函数模板和普通函数都可以实现，优先调用普通函数
    //注意如果告诉编译器普通函数是有的，但只是声明没有实现，或者不在当前文件内实现，就会报错找不到
    int a = 10;
    int b = 20;
    myPrint(a，b);//调用普通函数
    //2、可以通过空模板参数列表<>来强制调用函数模板
    myPrint<>(a, b);//调用函数模板
    //3、函数模板也可以发生重载
    int c = 30;
    myprint(a,b, c);//调用重载的函数模板
    //4、如果函数模板可以产生更好的匹配,优先调用函数模板
    char c1 = 'a';
    char c2 = 'b';
    myPrint(c1，c2);//调用函数模板
}
```

总结:

既然提供了函数模板，最好就不要提供普通函数，否则容易出现二义性。

## 模板的局限性

局限性:

模板的通用性并不是万能的

```
template<class T>
void f(T a，T b)
{
    if(a > b) 
    { .. }
}
```

在上述代码中，如果T的数据类型传入的是像Person这样的自定义数据类型，也无法正常运行

因此C++为了解决这种问题，提供模板的重载，可以为这些特定的类型提供具体化的模板

```
class Person{
public:
	Person( string name，int age){
	this- >m_Name I name;
	this->m_Age - age;
	}
string m_Name;
int m_Age;
};
//普通函数模板
template<class T>
bool myCompare(T& a，T& b){
	if (a == b){
	return true;
	}
	else{
	return false;
	}
}

//具体化，显示具体化的原型和定意思以template<>开头，并通过名称来指出类型
//具体化优先于常规模板
template<> bool myCompare(Person &p1，Person &p2)
{
	if ( p1.m_Name == p2.m_Name && p1.m_Age == p2.m_Age){
		return true;
	}
	else{
	return false;
	}
} 	
void test01(){
	int a - 10;
	int b = 20;
	//内置数据类型可以直接使用通用的函数模板
	bool ret = myCompare(a， b);
	if (ret)
	{
		cout << "a == b " << endl;
	}
	else
	{
		cout << "a != b " << endl;
	}
}
```

总结:

- 利用具体化的模板，可以解决由定义类型的通用化
- 学习模板并不是为了写模板，而是在STL能够运用系统提供的模板

## 类模板语法

类模板作用:
	建立一个通用类，类中的成员数据类型可以不具体制定，用一个虚拟的类型来代表。
语法:

```
template<typename T>
类
```

解释:

template ---声明创建模板

typename ---表面其后面的符号是一种数据类型，可以用class代替

T ---通用的数据类型，名称可以替换，通常为大写字母

```
template<class NameType,class AgeType>
class Person
{
public:
	Person(NameType name，AgeType age)
	{
	this- >mName = name;
	this->mAge = age;
	}
	void showPerson(){
	cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};
void test01()
{
	//指定NameType 为string类型，AgeType 为int类型
	Person<string, int>P1("孙悟空",999);
	p1.showPerson( );
}
```

## 类模板与函数模板区别

类模板与函数模板区别主要有两点:

1.类模板没有自动类型推导的使用方式

```
//1、类模板没有自动类型推导的使用方式
void test01()
{
	// Person p("孙悟空",1000); //错误类模板使用时候，不可以用自动类型推导
	Person <string ,int>p("孙悟空"，1000);//必须使用显示指定类型的方式，使用类模板
	p.showPerson();
}
```

⒉类模板在模板参数列表中可以有默认参数

```
//2、类模板在模板参数列表中可以有默认参数
void test02(){
    Person <string> p("猪八戒", 999);//类模板中的模板参数列表可以指定默认参数template<class //NameType,class AgeType=int>
    p.showPerson( ) ;
}
```

总结:

- 类模板使用只能用显示指定类型方式
- 类模板中的模板参数列表可以有默认参数

## 类模板对象做函数参数

学习目标:

类模板实例化出的对象，向函数传参的方式

—共有三种传入方式:

​	1.指定传入的类型	---直接显示对象的数据类型

​	2.参数模板化	---将对象中的参数变为模板进行传递

​	3.整个类模板化	---将这个对象类型模板化进行传递

```
template<class NameType,class AgeType=int>
class Person
{
public:
	Person(NameType name，AgeType age)
	{
	this- >mName = name;
	this->mAge = age;
	}
	void showPerson(){
	cout << "name: " << this->mName << " age: " << this->mAge << endl;
	}
public:
	NameType mName;
	AgeType mAge;
};
//1、指定传入的类型
void PrintPerson1( Person<string， int> &p)
{
	p.showPerson();
}
void test01()
{
	person <string， int >p("孙悟空"，100);
	printPerson1(p);
}
//2、参数模板化
template <class T1，class T2>
void printPerson2(Person<T1，T2>&p)
{
	p.showPerson();
	cout <<"T1的类型为:“ << typeid(T1).name() << endl;
	cout <<"T2的类型为:“ << typeid(T2).name() << endl;
}
void test02()
{
	Person <string, int >p("猪八戒"，90);
	printPerson2(p);
}
//3、整个类模板化
template<class T>
void printPerson3(T & p)(
	cout <<"T的类型为:“ << typeid(T).name() << endl;
	p.showPerson();
}
void test03(){
	Person <string, int >p(“唐借"，30);
	printPerson3(p);
}
```

总结:

- 通过类模板创建的对象，可以有三种方式向函数中进行传参
- 使用比较广泛是第一种:指定传入的类型

## 类模板与继承

当类模板碰到继承时，需要注意一下几点:

- 当子类继承的父类是一个类模板时，子类在声明的时候，要指定出父类中T的类型
- 如果不指定，编译器无法给子类分配内存
- 如果想灵活指定出父类中T的类型，子类也需变为类模板

```
template<class T>
class Base
{
	T m;
};
//class Son: public Base//错误，c++编译需要给子类分配内存，必须知道父类中T的类型才可以向下继承
class Son :public Base<int>//必须指定一个类型
{
};
void test01(){
	son c;
}
//类模板继承类模板,可以用T2指定父类中的T类型
template<class T1, class T2>
class Son2 :public Base<T2>
{
public:
	Son2(){
	cout << typeid(T1).name() << endl;
	cout << typeid(T2).name() << endl;
	}
};
void test02(){
	Son2<int, char> child1;
}
```

总结:如果父类是类模板，子类需要指定出父类中T的数据类型

## 类模板成员函数类外实现

```
//类模板中成员函数类外实现
template<class T1，class T2>
class Person {
public:
	//成员函数类内声明
	Person(T1 name，T2 age);
	void showPerson();
public:
	T1 m_Name;
	T2 m_Age;
};
//构造函数类外实现
template<class T1,class T2>
Person<T1，T2> : : Person(T1 name，T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}
//成员函数类外实现
template<class T1，class T2>
void Person<T1，T2> : : showPerson( ) {
	cout <<“姓名:"<< this->m_Name <<”年龄:" << this->m_Age << endl;
}
void test01(){
	Person<string, int> p("Tom",20);
	p.showPerson();
}
```

## 类模板分文件编写

问题:

- 类模板中成员函数创建时机是在调用阶段，导致分文件编写时链接不到

解决:

- 解决方式1:直接包含.cpp源文件
- 解决方式2∶将声明和实现写到同一个文件中，并更改后缀名为.hpp，hpp是约定的名称，并不是强制

person.hpp中代码

```
#pragma once
#include <iostream>
using namespace std;
#include <string>
template<class T1,class T2>
class Person{
public:
	Person(T1 name，T2 age);
	void showPerson();
public:
	T1 m_Name;
	T2 m_Age;
};
//构造函数类外实现
template<class T1, class T2>
Person<T1，T2> : :Person(T1 name，T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}
//成员函数类外实现
template<class T1,class T2>
void Person<T1，T2> : : showPerson() {
	cout <<“姓名;"<< this->m_Name <<”年龄:"<< this->m_Age << endl;
}
```

类模板分文件编写.cpp中代码

```
#include<iosiream>
using namespace std;
//#include "person.h"
#include "person.cpp”//解决方式1，包含cpp源文件
//解决方式2，将声明和实现写到一起，文件后缀名改为.hpp
#include "person.hpp"
void teste1()
{
	Person<string, int> p( Tom"，10);
	p.showPerson( );
}
```

总结:主流的解决方式是第二种，将类模板成员函数写到一起，并将后缀名改为.hpp

## 类模板与友元

学习目标:

- 掌握类模板配合友元函数的类内和类外实现
- 全局函数类内实现-直接在类内声明友元即可
- 全局函数类外实现–需要提前让编译器知道全局函数的存在

成员函数加friend变为全局函数,可在全局作用于该类对象.

```
//2、全局函数配合友元类外实现–先做函数模板声明，下方在做函数模板定义，在做友元
template<class T1,class T2> 
class Person;
//如果声明了函数模板，可以将实现写到后面，否则需要将实现体写到类的前面让编译器提前看到
//template<class T1，class T2> void printPerson2(Person<T1，T2>& p);
template<class T1, class T2>
void printPerson2( Person<T1，T2>& p){
	cout <<"类外实现----姓名: " << p.m_Name <<”年龄:"<< p.m_Age << endl;
}
template<class T1, class T2>
class person
{
	//1、全局函数配合友元类内实现
	friend void printPerson(Person<T1，T2> &p)
		cout <<"姓名:"<< p.m_Name <<”年龄:”<< p.m_Age << endl;
}
	//全局函数配合友元类外实现
	friend void printPerson2<>(Person<T1，T2> & p);
public:
	Person( T1 name，T2 age){
		this->m_Name = name;
		this->m_Age -l age;
	}
private:
	T1 m_Name;
	T2 m_Age;
};
//1、全局函数在类内实现
void test01()
{
	Person <string， int >p( "Tom"，20);
	printPerson(p);
}
//2、全局函数在类外实现
void test02()
{
	Person <string, int >p("Jerry" , 30);
	printPerson2(p);
}
```

总结:建议全局函数做类内实现，用法简单，而且编译器可以直接识别

## 类模板案例

案例描述:实现一个通用的数组类，要求如下:

- 可以对内置数据类型以及自定义数据类型的数据进行存储
- 将数组中的数据存储到堆区
- 构造函数中可以传入数组的容量
- 提供对应的拷贝构造函数以及operator=防止浅拷贝问题
- 提供尾插法和尾删法对数组中的数据进行增加和删除
- 可以通过下标的方式访问数组中的元素
- 可以获取数组中当前元素个数和数组的容量

myArray.hpp中代码

```
pragma once
#include <iostream>
using namespace std;
template<class T>
class MyArray{
public:
	//构造函数
	MyArray(int capacity){
		this->m_Capacity = capacity;
		this->m_size = 0;
		pAddress = new T[this->m_Capacity];
	}
	//拷贝构造
	MyArray(const MyArray & arr){
		this->m_Capacity = arr.m_capacity;
		this->m_Size = arr.m_Size;
		this->pAddress = new T[this->m_capacity];
		for (int i =0; i < this->m_size; i++)
		{
			//如果T为对象，而且还包含指针，必须需要重载=操作符，因为这个等号不是构造而是赋值
			//普通类型可以直接=但是指针类型需要深拷贝
			this->pAddress[i] = arr. pAddress[i];
		}
	}
	//重载=操作符  防止浅拷贝问题
	MyArray& operator=( const MyArray& myarray) {
		if(this->pAddress != NULL) {
			delete[] this->pAddress;
			this->m_Capacity = 0;
			this->m_size = 0;
		}
		this->m_Capacity = myarray.m_capacity;
		this->m_Size = myarray.m_size;
		this->pAddress = new T[this->m_capacity];
		for (int i = 0; i < this->m_size; i++) {
				this->pAddress[i] = myarray[i];
		}
		return *this;
	}
	//重载[]操作符 arr[0]
	T& operator [](int index)
	{
		return this->pAddress[index]; //不考虑越界，用户自己去处理
	}
	//尾插法
	void Push_back( const T & val)
	{
		if (this->m_Capacity == this->m_size){
			return;
		}
		this->pAddress[this->m_size] = val;
		this->m_size++;
	}
	//尾删法
	void Pop_back(){
		if (this->m_size == e){
			return;
		}
		this->m_size--;
	}
	//获取数组容量
	int getCapacity(){
		return this->m_capacity;
	}
	//获取数组大小
	int getsize()
	{
		return this->m_size;
	}
	//析构
	~MyArray(){
		if (this->pAddress != NULL){
			delete[] this->pAddress;
			this->pAddress = NULL;
			this->m_Capacity = 0;
			this->m_Size = 0;
		}
	}
private:
	T *pAddress;//指向一个堆空间，这个空间存储真正的数据
	int m_Capacity;//容量
	int m_Size;//大小
};
```

类模板案例―数组类封装.cpp中

```
#include "myArray.hpp"
include <string>
void printIntArray(MyArray<int>& arr) {
	for (int i =0; i < arr.getsize(); i++) {
	cout << arr[i] <<" ";
	}
	cout <<endl;
}
//测试内置数据类型
void test01(){
	MyArray<int> artay1(10);
	for (int i = 0; i < 10; i++){
		array1.Push_back(i);
	}
	cout << "array1打印输出:” << endl;
	printIntArray( array1);
	cout << "array1的大小:" <<array1.getsize() << endl;
	cout << "array1的容量:"<< array1.getCapacity() << endl;
	cout << "-------------------"<<endl;
	MyArray<int> array2( array1);//拷贝
	array2.pop_back( );
	cout << "array2打印输出:” << endl;printIntArray( array2) ;
	cout << "array2的大小:" <<array2.getsize() << endl;
	cout << "array2的容量:"<< array2.getCapacity() << endl;
}
//测试自定义数据类型
class Person {
public:
	Person(){}
	Person(string name,int age) 
	{
		this->m_Name = name;
		this->m_Age = age;
	}
public:
	string m_Name;
	int m_Age;
};
void printPersonArray(MyArray<Person>& personArr)
{
	for (int i = 0; i < personArr.getsize(); i++) {
		cout <<"姓名:" <<personArr[i].m_Name <<”年龄: "<< personArr[i].m_Age << endl;
	}
}
void test02()
	{
	//创建数组
		MyArray<Person> pArray (10);
		Person p1(”孙悟空"，30);
		person p2(“韩信"，20);
		Person p3("妲己"，18) ;
		Person p4(“王昭君"，15);
		person p5("赵云"，24);
		//插入数据
		pArray. push_back(p1);
		pArray. Push_back(p2);
		printPersonArray(pArray);
		cout << "pArray的大小:" <<pArray.getsize() << endl;
		cout << "pArray的容量:"<<pArray.getcapacity() << endl;
	}
```

