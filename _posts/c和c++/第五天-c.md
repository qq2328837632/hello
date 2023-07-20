---
title: 第五天 c++
date: 2023-04-03 09:24:35
tags:
---

## 多态

### 多态的基本概念

多态是C++面向对象三大特性之一

多态分为两类:

- 静态多态:函数重载和运算符重载属于静态多态，复用函数名
- 动态多态:派生类和虚函数实现运行时多态

静态多态和动态多态区别:

- 静态多态的函数地址早绑定–编译阶段确定函数地址
- 动态多态的函数地址晚绑定–运行阶段确定函数地址

```
#include<iostream>
#include <windows.h>
using namespace std;
class Animal
{
public:
	// Speak函数就是虚函数
	//函数前面加上virtual关键字，变成虚函数，那么编译器在编译的时候就不能确定函数调用了。
	//vfptr-虚函教（表）指针
	//v- virtual
	//f - function
	//ptr - pointer
	virtual void speak(){
		cout << "动物在说话"<<endl;
	}
};
class Cat :public Animal{
public:
	void speak(){
		cout <<"小猫在说话"<<endl;
	}	

};
class Dog :public Animal{
public:
	void speak( ){
		cout <<"小狗在说话"<<endl;
	}
};
//我们希望传入什么对象，那么就调用什么对象的函数
//如果函数地址在编译阶段就能确定，那么静态联编
//如果函数地址在运行阶段才能确定，就是动态联编

void DoSpeak( Animal & animal){//Animal & animal=cat;引用指向子类对象,如果没有virtual，则编译阶段就确定函数调用Animal了，则不能实现动态调用了
	animal.speak( );
}

//多态满足条件:
//1、有继承关系
//2、子类重写父类中的虚函数，这里就是speak函数
//多态使用:
//父类指针或引用指向子类对象
void test01(){
	Cat cat;
	DoSpeak(cat);//用引用也可以。引用不用手动释放内存。指针不用实例化对象。各有优劣,即引用时不能写成	//DoSpeak(new Cat);
	Dog dog;
	DoSpeak( dog);
}
int main() {
    SetConsoleOutputCP(65001);
	test01();
	//test02();
	system( "pause" );
	return 0;
}
```

总结:

多态满足条件

- 有继承关系
- 子类**重写**父类中的虚函数

多态信用条件

- 父类指针或引用指向子类对象

重写:函数返回值类型	函数名	参数列表	完全—致称为重写

```
//重写	函数返回值类型	函数名参	数列表完全相同
virtual void speak ()
{
	cout<<”小猫在说话”<< endl:
}
当子类重写父类的虚函数
这里不是子类中的虚函数表内部会替换成子类的虚函数地址，而是添加，因为替换的话，子类的虚函数就会丢失父类的虚函数地址，但实际上子类的虚函数表中同时有父类的虚函数地址和它自己的虚函数地址，通过使用方式不同，如利用父类指针或引用指向子类这种方式，来区分到底是使用子类虚数表里两个虚数表中的哪一个
```

### 多态案例一-计算器类

案例描述:

分别利用普通写法和多态技术，设计实现两个操作数进行运算的计算器类

多态的优点:

- 代码组织结构清晰。可读性强
- 利于前期和后期的扩展以及维护

```
//普通实现
class calculator {
public:
	int getResult( string oper){
		if (oper == "+") {
			return m_Num1 + m_Num2;
		}
		else if (oper == "-") {
			return m_Num1 - m_Num2;
		}
		else if (oper ==“*"){
			return m_Num1 *m_Num2;
		}
	//如果要提供新的运算，需要修改源码
	}
public:
	int m_Num1;
	int m_Num2;
};
void teste1(){
	//普通实现测试
	Calculator c;
	c.m_Num1 = 10;
	c.m_Num2 = 10;
	cout << c.m_Num1 << " + " << c.m_Num2<< " - " << c.getResult("+") << endl;
	cout < c.m_Num1 << " . " << c.m_Num2<< " = " << c.getResult("-") << endl;
	cout < c.m_Num1 <<”*" << c.m_Num2 << " = " << c.getResult("*") << endl;
}
//多态实现
//抽象计算器类
//多态优点:代码组织结构清晰，可读性强，利于前期和后期的扩展以及维护
class Abstractcalculator
{
public :
	virtual int getResult(){
		return 0;
	}
		int m_Num1;
		int m_Num2;
};

//加法计算器
class Addcalculator :public Abstractcalculator{
public:
	int getResult(){
		return m_Num1 + m_Num2;
	}
};
//减法计算器
class Subcalculator :public Abstractcalculator{
public:
	int getResult(){
		return m_Num1 - m_Num2;
	}
};
//乘法计算器
class Mulcalculator :public Abstractcalculator{
public:
	int getResult(){
		return m_Num1 * m_Num2;
	}
};
void teste2(){
	//创建加法计算器
	Abstractcalculator *abc = new Addcalculator;//多态使用:
	//父类指针或引用指向子类对象,这里是父类指针指向子类对象
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout < abc->m_Num1 << " + " << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;//用完了记得销毁
	//创建减法计算器
	abc = new Subcalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 << " -" << abc->m_Num2 << " = " << abc->getResult() << endl;
	delete abc;
	//创建乘法计算器
	abc = new MulCalculator;
	abc->m_Num1 = 10;
	abc->m_Num2 = 10;
	cout << abc->m_Num1 <<" * " << abc->m_Num2 〈< " - " << abc->getResult() << endl;
	delete abc;
	} 
```

总结:C++开发提倡利用多森设计程序架构，因为多态优点很多

### 纯虚函数和抽象类

在多态中，通常父类中虚函数的实现是毫无意义的，主要都是调用子类重写的内容

因此可以将虚函数改为纯虚函数

纯虚函数语法:virtual 	返回值类型	函数名	(参数列表)= 0;

当类中有了纯虚函数，这个类也称为**抽象类**

抽象类特点:

- 无法实例化对象
- 子类必须重写抽象类中的纯虚函数，否则也属于抽象类

```
class Base{
public:
	//纯虚函数
	//类中只要有一个纯虚函数就称为抽象类
	//抽象类无法实例化对象,但是能定义一个指向该类的指针
	//子类必须重写父类中的纯虚函数，否则也属于抽象类
	virtual void func() = 0;
};
class Son :public Base{
public:
	virtual void func(){
		cout << "func调用"<< endl;
	};
};
void teste1(){
	Base * base = NULL;
	// base = new Base; 
	//错误，抽象类无法实例化对象
	base = new Son;
	base->func( );
	delete base; //记得销毁
}
```

### 虚析构和纯虚析构

多态使用时，如果子类中有属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方式:将父类中的析构函数改为**虚析构**或者纯虚析构

虚析构和纯虚析构共性:

- 可以解决父类指针释放子类对象
- 都需要有具体的函数实现

虚析构和纯虚析构区别:

- 如果是纯虚析构，该类属于抽象类，无法实例化对象

在基类析构函数声明为virtual的时候，delete基类指针，会先调用派生类的析构函数，再调用基类的析构函数。在基类析构函数没有声明为virtual的时候，delete基类指针，只会调用基类的析构函数，而不会调用派生类的析构函数，这样会造成销毁对象的不完全。

虚析构语法:

`virtual ~类名( ){}`

纯虚析构语法:

`virtual	~类名()= 0;`

`类名::~类名(){}`

```
class Animal {
public:
	Animal(){
	cout << "Animal构造函数调用!" << endl;
	}
	virtual void Speak() = 0;
	//析构函数加上virtual关键字，变成虚析构函数
	//virtual ~Animal()
	//{
	//cout << "Animal虚析构函数调用! "<<endl;
    //}
	virtual ~Animal() = 0;
};
Animal : :~Animal(){
	cout <<"Animal纯虚析构函数调用!" << endl;
}
//和包含普通纯虚函数的类一样，包含了纯虚析构函数的类也是一个抽象类。不能够被实例化。
class Cat : public Animal {
public:
	Cat(string name)
	{
		cout << "cat构造函数调用!" <<endl;
		m_Name = new string( name);
	}
	virtual void Speak(){
		cout << *m_Name <<"小猫在说话!"<< endl;
	}
	~Cat()
	{
		cout << "cat析构函数调用!"<< endl;
		if (this->m_Name != NULL){
		delete m_Name;
		m_Name = NULL;//置空是为了防止出现野指针
		}
	}
public:
	string *m_Name;
};
void test01()
{
	Animal *animal = new Cat( "Tom" ) ;
	animal->Speak();
	//通过父类指针去释放，会导致子类对象可能清理不干净，造成内存泄漏
	//怎么解决?给基类增加一个虚析构函数
	//虚析构函数就是用来解决通过父类指针释放子类对象
	delete animal;
}
```

总结：

1.虚析构或纯虚析构就是用来解决通过父类指针释放子类对象

2.如果子类中没有堆区数据，可以不写为虚析构或纯虚析构

3.拥有纯虚析构函数的类也属于抽象类

抽象类不能实例化,但是能定义一个指向该类的指针

# 文件操作

程序运行时产生的数据都属于临时数据，程序—旦运行结束都会被释放通过文件可以将数据持久化

C++中对文件操作需要包含头文件< fstream >

文件类型分为两种:

1.文本文件-文件以文本的ASCII码形式存储在计算机中

2.二进制文件-文件以文本的二进制形式存储在计算机中，用户一般不能直接读懂它们

操作文件的三大类:

- ofstream:写操作
- ifstream:读操作
- fstream :读写操作

### 写文件步骤如下:

1.包含头文件

#include <fstream>

2.创建流对象
ofstream ofs;//读文件则为ifstream ofs

3.打开文件
ofs.open("文件路径",打开方式);//读文件则为ifs.open("文件路径",打开方式)

4.写数据

ofs <<"写入的数据";

5.关闭文件

ofs.close();//读文件则为ifs.close()



文件打开方式:

| 打开方式   | 解释                       |
| ---------- | -------------------------- |
| ios:in     | 为读文件而打开文件         |
| ios::out   | 为写文件而打开文件         |
| ios:.ate   | 初始位置:文件尾            |
| ios:.app   | 追加方式写文件             |
| ios:trunc  | 如果文件存在先删除，再创建 |
| ios:binary | 二进制方式                 |

注意:文件打开方式可以配合使用，利用|操作符

例如:用二进制方式写文件`ios ::binary | ios:: out`

### 读文件步骤如下:

```
ifstream ifs;
ifs.open("test.txt", ios ::in);
if ( !ifs.is_open())
{
	cout <<"文件打开失败”<< endl;
	return;
}
//第—种方式
//char buf[1024] ={ 0 };
// while (ifs >> buf)
//{
	//cout << buf << endl;
	//}
//第二种
//char buf[ 1024] - { 0 };
// while (ifs.getline(buf,sizeof( buf) )); 
//{
	//cout << buf << endl;	
//}
//第三种
//string buf;
// while (getline(ifs, buf )) 
//{
	//cout << buf << endl;
// }
char c;
while ((c = ifs.get()) !=EOF){//EOF end of file
	cout << c;
}
ifs.close();
```

在 C++ 中，ifstream 是用于从文件中读取数据的输入流对象。它通过空格字符（空格符、制表符、换行符等）来分隔输入数据。在默认情况下，每次读取一定数量的字符（通常是一个单词或一行），直到遇到空格字符或换行为止。如果需要，可以使用 getline 函数来读取整行数据，而不是只读取到第一个空格字符。
