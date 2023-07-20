---
title: 第四天 c++
date: 2023-04-02 10:18:27
tags:
---

## C++对象模型和this指针

### 成员变量和成员函数分开存储

```
Class Person{
	int m_A; //非静态成员变量 属于类的对象上
	static int m_B;//静态成员变量 不属于类对象上
	void func(){}//非静态成员函数 不属于类对象上,即test02测试空间没有包含  不属于类对象上
	static void func2(){}//静态成员函数 不属于类对象上

};
int Person: :m_B = 0;
void test01()
{
Person p ;
//空对象占用内存空间为:1
//C++编译器会给每个空对象也分配一个字节空间，是为了区分空对象占内存的位置
//每个空对象也应该有一个独一无二的内存地址
cout << "size of p = " << sizeof(p) << endl;
}
void test02(){
	Person p;
	cout << "size of p = " << sizeof(p) << endl;
}
```

每一个非静态成员函数只会诞生一份函数实例，也就是说多个同类型的对象会共用一块代码，如果有p1,p2,p3...

每一个调用都是调用同一块内存空间

### this指针概念

接上那么问题是:这—块代码是如何区分那个对象调用自己的呢?

C++通i过提供特殊的对象指针， this指针，解决上述问题。

this指针指向被调用的成员函数所属的对象。

this指针是隐含每一个非静态成员函数内的一种指针。

this指针不需要定义，直接使用即可。

静态函数没有this指针。因为静态函数不属于某个对象。

this指针指向的就是对象,通过*this解引用可以访问该对象本身

**this指针的用途**:

当形参和成员变量同名时，可用this指针来区分。

在类的非静态成员函数中返回对象本身，可使用return *this。

------

```
class Person{
public:
	Person (int age){
	this->age = age;//谁调用Person，this就指向谁，比如Person p1;则this指向p1
	}
	Person& PersonAddAge (Person &p){//引用指向本身内存。不用引用就是拷贝了,而拷贝指向另一个内存
	//不引用就会直接使用复制构造函数。因为*this这个对象会在函数结束后销毁
	//引用可以让p2使用同一个内存，而*this指向的就是内存地址，所以不加&后面的
	//p2.PersonAddAge(p1).PersonAddAge(p1).PersonAddAge(pl);就和第一个调用的p2无关了
	this->age += p.age;
	//this指向p2的指针，而*this指向的就是p2这个对象本体
	return *this;
	}
	int age;
} ;

void test02 (){
	Person p1(10) ;
	Person p2(10);
	//链式编程思想
	p2.PersonAddAge(p1).PersonAddAge(p1). PersonAddAge(pl);//上面如果不加引用，则后面每次创建都是一个新的对象，则输出总是20
	cout << "p2的年龄为:"<< p2.age << endl;
}
```

### 空指针访问成员函数

C++中空指针也是可以调用成员函数的，但是也要注意有没有用到this指针

如果用到this指针，需要加以判断保证代码的健壮性

this指针的本质是指针常量,指针的指向是不可以修改的

```
class Person{
public:
	void showPersonAge(){
	//报错原因是因为传入的指针是为NULL
		if (this == NULL)//保证代码健壮性
		{
			return;
		}
		cout << "age = " << this->m_Age << endl;//当为null时，this空对象，报错
		}
int age;
} ;
void test(){
	Person *p=NULL;
	p->showPersonAge();
}
```

### const修饰成员函数

常函数:

成员函数后加const后我们称为这个函数为常函数

常函数内不可以修改成员属性

成员属性声明时加关键字mutable后，在常函数中依然可以修改

常对象:

声明对象前加const称该对象为常对象

常对象只能调用常函数

```
Class Person{
public:
	//this指针的本质是指针常量指针的指向是不可以修改的
	// const Person * const this;
	//在成员函数后面加const，修饰的是this指向，让指针指向的值也不可以修改
	void showPerson() const
	{
		this->m_B=100; 
		//this->m_A = 100;
		//this = NULL; //this指针不可以修改指针的指向的
	}
	int m_A;
	mutable int m_B;//特殊变量，即使在常函数中，也可以修改这个值
};
//常对象
void test02 ()
{
	const Person p;//在对象前加const，变为常对象
	//p.m_A = 100;
	p.m_B = 100; //m_B是特殊值，在常对象下也可以修改
	//常对象只能调用常函数
	p.showPerson () ;
	//P.func ();//如果能调用，则可能会出现m_A被修改，其实是不能修改的。常对象不可以调用普通成员函数，因为普通成员函数可以修改属性,常对象可以修改静态变量的值
}
```

## 友元

### 全局函数做友元

在程序里，有些私有属性也想让类外特殊的一些函数或者类进行访问，就需要用到友元的技术

友元的目的就是让—个函数或者类访问另一个类中私有成员

友元的关键字为friend 

友元的三种实现

- 全局函数做友元
- 类做友元
- 成员函数做友元

```
class Building{
	//goodGay全局函数是 Building好朋友，可以访问Building中私有成员
	//告诉编译器goodGay全局函数是Building类的好朋友，可以访问类中的私有内容
	friend void goodGay(Building *building);
public:

private:

};
//全局函数
void goodGay(Building *building)
{
	cout<<"好基友全局函数正在访问:"<< building->m_SittingRoom << endl;
	cout<<"好基友全局函数正在访问: "<< building->m_BedRoom << endl;
void test01()
{
	Building building;
	goodGay(&building);
}
```

### 类做友元

```
class Building;
class goodGay{
public:
	goodGay();
	void visit();
private:
	Building *building;
};
class Building{
	//告诉编译器goodGay类是Building类的好朋友，可以访问到Building类中私有内容
	friend class goodGay;
public :
	Building();
public:
	string m_sittingRoom;//客厅
private:
	string m_BedRoom; //卧室
};
Building : : Building(){//类的定义可以在类外进行，所以以后文件太大时，声明在头文件中，定义在源文件中
	this->m_sittingRoom ="客厅";
	this->m_BedRoom ="卧室";
}
goodGay : : goodGay (){
	building = new Buildng;//创建一个堆区 
}
void goodGay : : visit(){
	cout <<“好基友正在访问”<< building->m_sittingRoom << endl;
	cout <<“好基友正在访问”<< building->m_BedRoom << endl;
}
void teste1(){
	goodGay gg;
	gg.visit();
}
```

### 成员函数做友元

```
class Building;
class goodGay{
public:
	goodGay();
	void visit();//只让visit函数作为Building的好朋友，可以发访问Building中私有内容
	void visit2();
private:
	Building *building;
};
class Building{
	//告诉编译器goodGay类中的visit成员函数是Building类的好朋友，可以访问到Building类中私有内容
	friend void goodGay::visit();
public :
	Building();
public:
	string m_sittingRoom;//客厅
private:
	string m_BedRoom; //卧室
};
Building : : Building(){//类的定义可以在类外进行，所以以后文件太大时，声明在头文件中，定义在源文件中
	this->m_sittingRoom ="客厅";
	this->m_BedRoom ="卧室";
}
goodGay : : goodGay (){
	building = new Buildng;//创建一个堆区 
}
void goodGay : : visit(){
	cout <<“好基友正在访问”<< building->m_sittingRoom << endl;
	cout <<“好基友正在访问”<< building->m_BedRoom << endl;
}
void goodGay : : visit2(){
	cout <<“好基友正在访问”<< building->m_sittingRoom << endl;
	//cout <<“好基友正在访问”<< building->m_BedRoom << endl;
}
void teste1(){
	goodGay gg;
	gg.visit();
}
```

## 运算符重载

运算符重载概念:对已有的运算符重新进行定义，赋予其另一种功能，以适应不同的数据类型

### 加号运算符重载

作用:实现两个自定义数据类型相加的运算

```
class Person {
public:
	person(){};
	person(int a, int b)
	{
		this->m_A = a;
		this->m_B = b;
	}
	//成员函数实现＋号运算符重载
	Person operator+( const Person& p){
		Person temp;
		temp.m_A = this->m_A + p.m_A;
		temp.m_B = this->m_B + p.m_B;
		return temp;
	}
public:
	int m_A;
	int m_B;
}
//全局函数实现+号运算符重载 
//Person operator+( const Person &p1, const Person &p2){
//	Person temp(0，0);
//	temp.m_A = p1.m_A + p2.m_A;
//	temp.m_B = p1.m_B +p2.m_B;
//}
//运算符重载可以发生函数重载
Person operator+(const Person& p2， int val)
{
	Person temp;
	temp.m_A = p2.m_A + val;
	temp.m_B = p2.m_B + val;
	return temp;
}
void test()
{
	Person p1(10，10);
	Person p2(20，20);
	//成员函数方式
	Person p3 = p2 +p1;//相当于p2.operaor+(p1)
	cout << "mA: " <<p3.m_A << " mB: " << p3.m_B << endl;
	Person p4 = p3 +10;//相当于operator+(p3,10)
	cout <<“mA: " <<p4.m_A << " mB : "<< p4.m_B << endl;
}
```

总结1:对于内置的数据类型的表达式的的运算符是不可能改变的

总结2:不要滥用运算符重载

### 左移运算符重载

作用:可以输出自定义数据类型

```
class Person {
	friend ostream& operator<<(ostream& out，Person& p);
public:
	Person(int a, int b)
	{
		this->m_A = a;this->m_B = b;
	}
	//成员函数实现不了p <<cout不是我们想要的效果 
	//void operator<<(Person& p){
	//}
private :
	int m_A;
	int m_B;
};
//全局函数实现左移重载
//ostream对象只能有一个
ostream& operator<<(ostream& cout，Person& p){
	out << "a: " << p.m_A << " b:" << p.m_B;
	return cout;
}
void test() {
	Person p1(10，20);
	cout <<p1 << "hello world" <<endl;//链式编程,需要返回ostream&,才可以接着后面运算
}
```

总结:重载左移运算符配合友元可以实现输出自定义数据类型

### 递增运算符重载

```
#include<iostream>
using namespace std;
class MyInteger {
	friend ostream& operator<<(ostream& out,MyInteger myint);
public:
	MyInteger() {
		m_Num = 0;
	}
	//前置++
	MyInteger& operator++()
	{
		//先++
		m_Num++;
		//再返回自身
		return *this;
	}
	//后置++
	MyInteger operator++(int) {
		//先返回
		MyInteger temp = *this;//记录当前本身的值，然后让本身的值加1，但是返回的是以前的值，达到先返回后++;
		m_Num++;
		return temp;//temp是临时变量，完了会释放，所以不会返回引用
	}
private:
	int m_Num;
};
ostream& operator<<(ostream& out,MyInteger myint) {
	out << myint.m_Num;
	return out;
}
//前置++ 先++ 再返回
void test01() {
	MyInteger myInt;
	cout<<(++myInt)++ << endl;
	cout <<myInt << endl;
}

//后置++ 先返回 再++
void test02() {
	MyInteger myInt;
	cout << myInt++ << endl;
	cout << myInt << endl;
}
int main() {
	test01();
	//test02();
	system( "pause" );
	return 0;
}
```

总结:前置递增返回引用，后置递增返回值

### 赋值运算符重载

C++编译器至少给一个类添加4个函数

- 1.默认构造函数(无参，函数体为空)
- ⒉.默认析构函数(无参，函数体为空)
- 3.默认拷贝构造函数，对属性进行值拷贝
- 4.赋值运算符operator=,对属性进行值拷贝

**如果类中有属性指向堆区，做赋值操作时也会出现深浅拷贝问题**

```
class Person{
public:
	Person( int age){
	//将年龄数据开辟到堆区
	m_Age = new int(age);
	}
	//重载赋值运算符
	person& operator=(Person &p){
		if (m_Age != NULL){
			delete m_Age;
			m_Age = NULL;
		}
		//编译器提供的代码是浅拷贝
		// m_Age = p.m_Age;
		//提供深拷贝解决浅拷贝的问题
		m_Age = new int(*p.m_Age);
		//返回自身,保证了下面能连等
		return *this;
	}
	~Person(){
		if (m_Age != NULL){
			delete m_Age;
			m_Age = NULL;
		}
	}
	//年龄的指针
	int *m_Age;
};
void test01(){
	Person p1 (18);
	Person p2(20);
	Person p3( 30);
 	p3=p2= p1;//赋值操作
	cout <<"p1的年龄为:"<<*p1.m_Age << endl;
	cout << "p2的年龄为:" <<*p2.m_Age << endl;
	cout << "p3的年龄为:" <<*p3.m_Age << endl;
}
```

### 关系运算符重载

作用:重载关系运算符，可以让两个**自定义类型对象**进行对比操作

```
class Person{
public:
	Person( string name，int age){
		this->m_Name = name;
		this->m_Age = age;
	};
	bool operator==(Person & p){
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age){
			return true;
		}
		else{
			return false;
		}
	}
	bool operator ! =( Person & p){
		if (this->m_Name == p.m_Name && this->m_Age == p.m_Age){
			return false;
		}
		else{
			return true;
		}
	}
	string m_Name;
	int m_Age;
};
void test01()
{
	Person a("孙悟空"，18);
	Person b("孙悟空"，18);
	if ( a == b)
	{
    	cout << "a和b相等”"<< endl;
    }
    else{
    cout << "a和b不等”"<< endl;
    }
    if (a !=b){
		cout <<"a和b不相等"<<endl;
	}
	else{
		cout << "a和b相等”<<endl;
	}
}
```

### 函数调用运算符重载

函数调用运算符()也可以重载

由于重载后使用的方式非常像函数的调用，因此称为仿函数

仿函数没有固定写法，非常灵活

```
class MyPrint{
public:
	void operator()(string text){
		cout << text <<endl;
	}
};
void test01(){
	//重载的()操作符也称为仿函数
	MyPrint myFunc;
	myFunc( "hello world" ) ;
}
class MyAdd{
public:
	int operator()(int v1,int v2){
		return v1 +v2;
	}
};
void test02(){
	MyAdd add;
	int ret = add(10，10);
	cout << "ret - " << ret << endl;
	//匿名对象调用
	cout <<"MyAdd()(100,100) = " << MyAdd() (100，100) << endl;
}
```

## 继承

### 继承的基本语法

继承的好处:减少重复代码

语法:class 	子类:	继承方式	父类

子类也称为派生类

父类也称为基类

### 继承方式

继承的语法:class子类∶继承方式父类

继承方式一共有三种:

- 公共继承
- 保护继承
- 私有继承

![image-20230402164555514](C:\Users\86191\AppData\Roaming\Typora\typora-user-images\image-20230402164555514.png)

保护权限和私有权限类外都访问不到。

### 继承中的对象模型

```
class Base{
public:
	int m_A;
protected:
	int m_B;
private:
	int m_c; //私有成员只是被隐藏了，但是还是会继承下去
};
//公共继承
class Son :public Base{
public:
	int m_D;
};
void teste1(){
	cout << "sizeof Son = " << sizeof(Son)<< endl;
}
```

结论:父类中私有成员也是被子类继承下去了，只是由编译器给隐藏后访问不到

### 继承中构造和析构顺序

子拳继承父类后，当创建子类对象，也会调用父类的构造函数

问题:父类和子类的构造和析构顺序是谁先谁后?

总结:继承中先调用父类构造函数，再调用子类构造函数，析构顺序与构造相反

### 4.6.5继承同名成员处理方式

问题:当子类与父类出现同名的成员，如何通过子类对象，访问到子类或父类中同名的数据呢?

访问子类同名成员直接访问即可

访问父类同名成员需要加作用域

```
class Base {
public:
	Base(){
		m_A = 100;
	}
	void func(){
		cout <<"Base - func()调用”<<endl;
	}
	void func(int a){
		cout << "Base - func ( int a)调用"<< endl;
	}
public:
	int m_A;
};
class son : public Base {
public:
	son()
	{
		m_A= 200;
	}
//当子类与父类拥有同名的成员函数，子类会隐藏父类中所有版本的同名成员函数
//如果想访问父类中被隐藏的同名成员函数，需要加父类的作用域
	void func()
	{
		cout << "Son - func()调用"<<endl;
	}
public:
	int m_A;
};
void test01(){
	Son s;
	cout << "Son下的m_A = " << s.m_A << endl;
	cout << "Base下的m_A = " << s.Base::m_A<< endl;
	s.func();
	s.Base::func();
	s.Base::func(10);
}
```

总结:
1.子类对象可以直接访问到子类中同名成员

2.子类对象加作用域可以访问到父类同名成员

3.当子类与父类拥有同名的成员函数，子类会隐藏父类中同名成员函数，加作用域可以访问到父类中同名函数

### 继承同名静态成员处理方式

问题:继承中同名的静态成员在子类对象上如何进行访问?

静态成员和非静态成员出现同名，处理方式—致

·访问子类同名成员直接访问即可

·访问父类同名成员需要加作用域

```
class Base {
public:
	static void func(){
		cout << "Base - static void func()" << endl;
	}

	static void func(int a){
		cout <<"Base - static void func( int a)" << endl;
	}
	static int m_A;
};
int Base::m_A = 100;
class Son : public Base{
public:
	static void func(){
		cout << "Son - static void func()" << endl;
	}
	static int m_A;
};
int Son::m_A = 200;
//同名成员属性
void test01(){
	//通过对象访问
	cout <<“通过对象访问:"<<endl;
	Son s;
	cout << "Son 下m_A =" << s.m_A << endl;
	cout << "Base 下m_A = " << s.Base::m_A << endl;
	//通过类名访问
	cout <<“通过类名访问:“ << endl;
	cout << "Son下m_A = " << Son::m_A << endl;
	cout<< "Base 下m_A = " <<Son::Base::m_A << endl;
}
//同名成员函数
void test02(){}
	//通过对象访问
	cout <<“通过对象访问:“ <<endl;
	Son s;
	s.func();
	s.Base::func( );
	cout <<“通过类名访问:" << endl ;
	Son::func();
	son::Base::func();
	//出现同名。子类会隐藏掉父类中所有同名成员函数，需要加作用域访问
	Son::Base::func(100);
}
```

总结:同名静态成员处理方式和非静态处理方式一样，只不过有两种访问的方式(通过对象和通过类名）

### 多继承语法

C++允许一个类继承多个类

语法:	class	子类	∶	继承方式	父类1 ，继承方式	父类2...{};

多继承可能会引发父类中有同名成员出现，需要加作用域区分

C++实际开发中不建议用多继承

总结:多继承中如果父类中出现了同名情况，子类使用时候要加作用域

### 菱形继承

```
class Animal{
public:
	int m_Age;
};
//继承前加virtual关键字后，变为虚继承//此时公共的父类Animal称为虚基类
class sheep : virtual public Animal{};
class Tuo: virtual public Animal {};
class SheepTuo : public Sheep，public Tuo {};
void test01()
{
	sheepTuo st;
	st.Sheep::m_Age = 100;
	st.Tuo::m_Age = 200;//指向的是一样的地址，第二个是后改变的，用后面的数据
	cout << "st.Sheep::m_Age = "<< st.sheep:: m_Age << endl;
	cout << "st.Tuo : :m_Age = "<<st.Tuo::m_Age << endl;
	cout << "st.m_Age =" << st.m_Age << endl;
}
```

总结:
菱形继承带来的主要问题是子类继承两份相同的数据，导致资源浪费以及毫无意义

利用虚继承可以解决菱形继承问题

虚继承底层原理：

![image-20230402175305513](C:\Users\86191\AppData\Roaming\Typora\typora-user-images\image-20230402175305513.png)

vbptr相当于指针，指向一个虚基类表，表中记录了偏移量，指针加上偏移量，指向了唯一的数据。
