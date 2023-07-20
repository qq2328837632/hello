---
title: c++ 函数
date: 2023-04-13 16:38:09
tags:
---

# 函数

## 参数传递

### 指针或引用形参与const

形参的初始化方式和变量的初始化方式是一样的,所以回顾通用的初始化规则有助于理解本节知识。我们可以使用非常量初始化一个底层const对象，但是反过来不行;同时一个普通的引用必须用同类型的对象初始化。

```
int i = 42;
const int *cp = &i;//正确:但是cp不能改变i
const int &r = i;//正确:但是r不能改变i
const int &r2 = 42;//正确:
int *p=cp;//错误:p的类型和cp的类型不匹配
int &r3 = r;//错误:r3的类型和r的类型不匹配
int &r4=42;//错误:不能用字面值初始化一个非常量引用


```

```
&arr两端的括号必不可少
	f(int &arr[10])//错误:将arr声明成了引用的数组
	f(int (&arr)[10])//正确:arr是具有10个整数的整型数组的引用
```

### main:处理命令行选项

main函数是演示C++程序如何向函数传递数组的好例子。到目前为止，我们定义的main函数都只有空形参列表:
int main()[ ...}

然而，有时我们确实需要给main传递实参，一种常见的情况是用户通过设置一组选项来确定函数所要执行的操作。例如，假定 main函数位于可执行文件prog之内，我们可以向程序传递下面的选项:
`prog -d -o ofile data0`
这些命令行选项通过两个（可选的）形参传递给main函数:

int main(int argc,char  * argv[]){...}
第二个形参argv是一个数组，它的元素是指向C风格字符串的指针:第一个形参arge表示数组中字符串的数量。因为第二个形参是数组，所以main函数也可以定义成:
int main(int arge,char  * * argv) { ...}
其中argv指向char*。

当实参传给main函数之后，argv的第一个元素指向程序的名字或者一个空字符串，接下来的元素依次传递命令行提供的实参。最后一个指针之后的元素值保证为0。
以上面提供的**命令行**为例，argc应该等于5，argv应该包含如下的C风格字符串:

```
argv[0] = "prog" ;//或者argv [0]也可以指向一个空字符串
argv[1] = "-d";
argv[2] = "-o" ;
argv[3] = "ofile" ;
argv[4]= "data0";
argv [5] = 0;
```

当使用argv中的实参时,一定要记得可选的实参从argv[1]开始; 

argv[0]保存程序的名字，而非用户输入。

### 含有可变形参的函数

为了编写能处理不同数量实参的函数，C++11新标准提供了两种主要的方法:如果所有的实参类型相同,可以传递一个名为initializer_list的标准库类型;如果实参的类型不同，我们可以编写一种特殊的函数，也就是所谓的可变参数模板

和vector一样，initializer_list也是一种模板类型(参见3.3节,第86页)。定义initializer_list对象时，必须说明列表中所含元素的类型:

```
initializer_list<string> ls;// initializer_list的元素类型是stringinitializer_list<int> li;// initializer_list的元素类型是int
```

和 vector不一样的是，initializer_list对象中的元素永远是常量值，我们无法改变initializer_list对象中元素的值。

编写一个函数，它的参数是initializer_list类型的对象，函数的功能是计算列表中所有元素的和。

```
#include <iostream>
#include <initializer_list>

int counter_int(std::initializer_list<int> il)
{
	int cnt_i = 0;
	for(auto e : il)
		cnt_i += e;
	return cnt_i;
}

int main(int argc, char const *argv[])
{
	std::cout << counter_int({1,2,3,4,5}) << std::endl;

	return 0;

}
```



### 省略符形参

省略符形参是为了便于C++程序访问某些特殊的C代码而设置的,这些代码使用了名为varargs的C标准库功能。通常，省略符形参不应用于其他目的。你的C编译器文档会描述如何使用varargs。

省略符形参应该仅仅用于C和C++通用的类型。特别应该注意的是，大多数类类型的对象在传递给省略符形参时都无法正确拷贝。

省略符形参只能出现在形参列表的最后一个位置，它的形式无外乎以下两种:

```
void foo (parm_list, ...);
void foo(.. .);
```

第一种形式指定了 foo函数的部分形参的类型，对应于这些形参的实参将会执行正常的类型检查。**省略符形参所对应的实参无须类型检查**。在第一种形式中，形参声明后面的逗号是可选的。

### 引用返回左值

函数的返回类型决定函数调用是否是左值。调用一个返回引用的函数得到左值，其他返回类型得到右值。可以像使用其他左值那样来使用返回引用的函数的调用，特别是，我们能为返回类型是非常量引用的函数的结果赋值:

```
char &get_val(string &str, string::size_type ix)
{
	return str[ix];//get_val假定索引值是有效的
}
int main(){
	string s( "a value");
	cout <<s< endl;//输出a value
	get_val(s,0）='A';//将s[0]的值改为A
	cout c<s<c endl ;//输出A value
	return 0;
}
```

把函数调用放在赋值语句的左侧可能看起来有点奇怪，但其实这没什么特别的。返回值是引用，因此调用是个左值，和其他左值一样它也能出现在赋值运算符的左侧。

如果返回类型是常量引用，我们不能给调用的结果赋值，这一点和我们熟悉的情况是一样的。

### 列表初始化返回值

C++11新标准规定，函数可以返回花括号包围的值的列表。类似于其他返回结果，此处的列表也用来对表示函数返回的临时量进行初始化。如果列表为空，临时量执行值初始化；否则，返回的值由函数的返回类型决定。

```
vector<strings process()
{
	// ...
	//expected和actual是string对象
	if (expected.empty())
		return {};//返回一个空vector对象
	else if (expected -= actual)
		return {"functionx"，"okay"};//返回列表初始化的vector对象
	else
		return { "functionx",expected,actual};
}
```

### 主函数main的返回值

任何返回类型不是void的函数都必须返回一个值，但此规则的一个例外情况：允许main没有返回值就可结束。如果程序控制执行到主函数main的最后一个语句都还没有返回，那么编译器会隐式地插入返回0的语句。
关于main返回的另一个特别之处在于如何处理它的返回值。可将main返回值视为状态指示器。返回0表示程序运行成功，其他大部分返回值则失败。非0返回值的意义因机器不同而不同，为使返回值独立于机器，头文件

```
#define EXIT_SUCCESS    0
#define EXIT_FAILURE    1
```

## 返回数组指针

```
int( *func (int i))[10];
```

可以按照以下的顺序来逐层理解该声明的含义:

```
func(int i)表示调用func函数时需要一个int类型的实参。
(*func(int i))意味着我们可以对函数调用的结果执行解引用操作。
(*func(int i))[10]表示解引用func 的调用将得到一个大小是10的数组。
int (*func(int i))[10]表示数组中的元素是int类型。
```

### 使用尾置返回类型

在C++11新标准中还有一种可以**简化上述func声明**的方法,就是使用尾置返回类型(trailing return type)。任何函数的定义都能使用尾置返回，但是这种形式对于返回类型比较复杂的函数最有效，比如返回类型是数组的指针或者数组的引用。尾置返回类型跟在形参列表后面并以一个->符号开头。为了表示函数真正的返回类型跟在形参列表之后，我们在本应该出现返回类型的地方放置一个auto:

```
// func接受一个int类型的实参，返回一个指针，该指针指向含有10个整数的数组
auto func(int i)-> int(*)[10];
```

因为我们把函数的返回类型放在了形参列表之后，所以可以清楚地看到 func函数返回的是一个指针,并且该指针指向了含有10个整数的数组。

### 使用decltype

还有一种情况，如果我们知道函数返回的指针将指向哪个数组，就可以使用decltype关键字声明返回类型。例如，下面的函数返回一个指针，该指针根据参数i的不同指向两个已知数组中的某一个:

```
int odd[] ={ 1,3,5, 7,9};
int even[] ={0,2,4,6,8};
//返回一个指针,该指针指向含有5个整数的数组
decltype(odd) *arrPtr(int i)
{
	return (i%2) ? &odd : &even;//返回一个指向数组的指针
}
```

有一个地方需要注意:decltype并不负责把数组类型转换成对应的指针，所以decltype 的结果是个数组，要想表示 arrPtr返回指针还必须在函数声明时加一个*符号。

## 重载

### 重载和const形参

顶层const不影响传入函数的对象。一个拥有顶层const的形参无法和另一个没有顶层const的形参区分开来:

```
Record lookup (Phone) ;
Record lookup (const Phone);//重复声明了Record lookup (Phone)
Record lookup (Phone*);
Record lookup (Phone* const);//重复声明了Record lookup(Phone* )
```

在这两组函数声明中，每一组的第二个声明和第一个声明是等价的。

另一方面，如果形参是某种类型的指针或引用，则通过区分其指向的是常量对象还是非常量对象可以实现函数重载，此时的const是底层的:

```
//对于接受引用或指针的函数来说，对象是常量还是非常量对应的形参不同
//定义了4个独立的重载函数
Record lookup(Account&) ;//函数作用于Account的引用
Record lookup(const Account&);//新函数，作用于常量引用
Record lookup(Account*);//新函数，作用于指向Account的指针
Record lookup(const Account*);//新函数，作用于指向常量的指针
```

### const_cast和重载

const_cast在重载函数的情景中最有用。举个例子，shorterString函数:

```
//比较两个string对象的长度，返回较短的那个引用
const string &shorterString(const string &sl,const string &s2){
	return sl.size(<=s2.size() ? s1: s2;
}
```

这个函数的参数和返回类型都是 const string的引用。我们可以对两个非常量的string 实参调用这个函数，但返回的结果仍然是const string的引用。因此我们需要一种新的shorterstring函数，当它的实参不是常量时，得到的结果是一个普通的引用，使用const_cast可以做到这一点:

```
string &shorterString(string &sl,string &s2)
{
	auto &r = shorterString(const_cast<const string6>(s1)，
						const cast<const string &>(s2));
	return constcast<string&>(r);
}
```

在这个版本的函数中，首先将它的实参强制转换成对const的引用，然后调用了shorterString函数的const版本。const版本返回对const string 的引用，这个引用事实上绑定在了某个初始的非常量实参上。因此，我们可以再将其转换回一个普通的string&，这显然是安全的。

## 特殊用途语言特性

### 内联函数和 constexpr函数

使用shorterstring 函数也存在一个潜在的缺点:调用函数一般比求等价表达式的值要慢一些。在大多数机器上，一次函数调用其实包含着一系列工作:调用前要先保存寄存器，并在返回时恢复:可能需要拷贝实参;程序转向一个新的位置继续执行。

#### 内联函数可避免函数调用的开销

将函数指定为内联函数（inline)，通常就是将它在每个调用点上“内联地”展开。假设我们把shorterString函数定义成内联函数，则如下调用

```
cout << shorterstring (s1, s2)<<endl;
//将在编译过程中展开成类似于下面的形式
cout<<(s1.size()< s2.size()?sl : s2)<< endl;
//从而消除了shorterString函数的运行时开销。
```

在shorterstring函数的返回类型前面加上关键字inline,这样就可以将它声明成内联函数了:

```
//内联版本:寻找两个string对象中较短的那个
inline const string &
shorterString(const string &s1,const string &s2)
{
	return s1.size() <= s2.size() ?sl: s2;
}
```

一般来说，内联机制用于优化规模较小、流程直接、频繁调用的函数。很多编译器都不支持内联递归函数，而且一个75行的函数也不大可能在调用点内联地展开。

#### constexpr函数

constexpr函数(constexpr function)是指能用于常量表达式(的函数。定义constexpr函数的方法与其他函数类似，不过要遵循几项约定:**函数的返回类型及所有形参的类型都得是字面值类型(**到目前为止接触过的数据类型中，算术类型、引用和指针都属于字面值类型。自定义类sales item、IO库、string 类型则不属于字面值类型，也就不能被定义成constexpr。其他一些字面值类型将在后面介绍)，而且函数体中**必须有且只有一条return语句**（constexpr函数不一定返回常量表达式）:

```
constexpr int new_sz() { return 42;}
constexpr int foo = new_sz();//正确:foo是一个常量表达式
```

我们把new _sz定义成无参数的 constexpr函数。因为编译器能在程序编译时验证new_sz 函数返回的是常量表达式，所以可以用new_sz函数初始化constexpr类型的变量foo.

执行该初始化任务时，编译器把对constexpr函数的调用替换成其结果值。为了能在编译过程中随时展开，**constexpr函数被隐式地指定为内联函数**。

constexpr函数体内也可以包含其他语句，只要这些语句在运行时不执行任何操作就行。例如，constexpr函数中可以有空语句、类型别名以及using声明。

### 把内联函数和constexpr函数放在头文件内

和其他函数不一样，内联函数和 constexpr函数可以在程序中**多次定义**。毕竟，编译器要想展开函数仅有函数声明是不够的，还需要函数的定义。不过，对于某个给定的内联函数或者constexpr函数来说，它的多个定义必须完全一致。基于这个原因，内联函数和 constexpr函数通常定义在头文件中。

## 调试帮助

C+程序员有时会用到一种类似于头文件保护的技术，以便有选择地执行调试代码。基本思想是，程序可以包含一些用于调试的代码，但是这些代码只在开发程序时使用。当应用程序编写完成准备发布时，要先屏蔽掉调试代码。这种方法用到两项预处理功能:assert和 NDEBUG。

### assert预处理宏

assert是一种预处理宏( preprocessor marco)。所谓预处理宏其实是一个预处理变量,它的行为有点类似于内联函数。assert宏使用一个表达式作为它的条件:

```
assert (expr) ;
```

首先对expr求值，如果表达式为假(即0)，assert输出信息并终止程序的执行。如果表达式为真（即非0)，assert什么也不做。

assert宏定义在cassert头文件中。如我们所知，预处理名字由预处理器而非编译器管理,因此我们可以直接使用预处理名字而无须提供using声明。也就是说，我们应该使用assert而不是std::assert，也不需要为assert提供using声明。

assert宏常用于检查“不能发生”的条件。例如，一个对输入文本进行操作的程序可能要求所有给定单词的长度都大于某个阈值。此时,程序可以包含一条如下所示的语句:
`assert(word.size(>threshold);`

### NDEBUG 预处理变量

assert 的行为依赖于一个名为NDEBUG 的预处理变量的状态。如果定义了NDEBUG,则assert什么也不做。默认状态下没有定义NDEBUG,此时assert将执行运行时检查。

我们可以使用一个#define语句定义 NDEBUG，从而关闭调试状态。同时，很多编译器都提供了一个命令行选项使我们可以定义预处理变量:
`$ cc-D NDEBUG main.C # use/D with the Microsoft compiler`

这条命令的作用等价于在main.c文件的一开始写#define NDEBUG。

定义NDEBUG能避免检查各种条件所需的运行时开销,当然此时根本就不会执行运行时检查。因此，assert应该仅用于验证那些确实不可能发生的事情。我们可以把assert当成调试程序的一种辅助手段，但是不能用它替代真正的运行时逻辑检查，也不能替代程序本身应该包含的错误检查。

```
_ _ func _ _是编译器定义的一个局部静态变量，用于存放函数的名字

_ _ FILE  _ _ 存放文件名的字符串字面值。

_ _ LINE _ _ 存放当前行号的整型字面值。

_ _ TIME _ _ 存放文件编译时间的字符串字面值。

_ _ DATE_ _ 存放文件编译日期的字符串字面值。
```

可以使用这些常量在错误消息中提供更多信息

练习6.48:说明下面这个循环的含义，它对assert的使用合理吗?

```
string s;
while (cin >>S&&S!=sought){]//空函数体
assert (cin);
```

```
这不是一个好的assert用法。assert宏通常用于检查“不能发生”的条件。但是assert总是在用户直接输入EOF时发生，所以检查是没有意义的。使用assert(!cin || s == sought)更好
【出题思路】
assert是一种预处理宏，当assert的条件为真时什么也不做，当它的条件为假时输出信息并终止程序。
【解答】
该程序对assert 的使用有不合理之处。在调试器打开的情况下，当用户输入字符串 s并且s 的内容与 sought不相等时，执行循环体，否则继续执行assert(cin);语句。换句话说，程序执行到assert的原因可能有两个，一是用户终止了输入，二是用户输入的内容正好与sought 的内容一样。如果用户尝试终止输入（事实上用户总有停止输入结束程序的时候)，则assert的条件为假，输出错误信息，这与程序的原意是不相符的。
当调试器关闭时，assert 什么也不做。
```

## 函数指针

函数指针指向的是函数而非对象。和其他指针一样，函数指针指向某种特定类型。函数的类型由它的返回类型和形参类型共同决定，与函数名无关。例如:

```
//比较两个string对象的长度
bool lengthCompare(const string &,const string &);
```

该函数的类型是bool(const string&,const string&)。要想声明一个可以指向该函数的指针，只需要用指针替换函数名即可:

```
// pf指向一个函数，该函数的参数是两个const string的引用，返回值是bool类型
bool (*pf) (const string &,const string &);//未初始化
```

从我们声明的名字开始观察，pf前面有个*，因此pf是指针;右侧是形参列表，表示pf指向的是函数;再观察左侧，发现函数的返回类型是布尔值。因此，pf 就是一个指向函数的指针，其中该函数的参数是两个const string 的引用，返回值是bool类型。

```
*pf两端的括号必不可少。如果不写这对括号，则pf是一个返回值为bool指针的函数:
//声明一个名为pf的函数，该函数返回bool*
bool *pf(const string &, const string &);
```

### 使用函数指针

当我们把函数名作为一个值使用时，该函数自动地转换成指针。例如，按照如下形式我们可以将lengthCompare的地址赋给pf:

```
pf = lengthCompare;//pf指向名为lengthCompare的函数
pf=&lengthCompare;//等价的赋值语句:取地址符是可选的
```

此外，我们还能直接使用指向函数的指针调用该函数，无须提前解引用指针(*..):

```
bool bl = pf( "hello", "goodbye" );//调用lengthCompare函数
bool b2= (*pf) ( "hello", "goodbye" );//一个等价的调用
bool b3= lengthCompare ("hello","goodbye");//另一个等价的调用
```

### 函数指针形参

和数组类似，虽然不能定义函数类型的形参，但是形参可以是指向函数的指针。此时，形参看起来是函数类型，实际上却是当成指针使用:

```
//第三个形参是函数类型，它会自动地转换成指向函数的指针
void useBigger(const string &s1,const string &s2,
	bool pf(const string &,const string &));
//等价的声明;显式地将形参定义成指向函数的指针
void useBigger(const string &s1,const string &s2，
	bool (*pf) (const string &,const string &));
```

我们可以直接把函数作为实参使用，此时它会**自动转换成指针**:

```
//自动将函数lengthCompare转换成指向该函数的指针
useBigger(sl, s2, lengthCompare) ;
```

正如 useBigger的声明语句所示，直接使用函数指针类型显得冗长而烦琐。类型别名和decltype能让我们简化使用了函数指针的代码:

```
//Func和Func2是函数类型
typedef bool Func(const string&,const string&) ;
typedef decltype(lengthcompare) Func2;//等价的类型
// FuncP和FuncP2是指向函数的指针
typedef bool(*FuncP) (const string&,const string&);
typedef decltype(lengthCompare) *FuncP2;
//等价的类型
```

我们使用typedef定义自己的类型。Func和 Func2是函数类型，而FuncP和 FuncP2是指针类型。需要注意的是，decltype返回函数类型，此时不会将函数类型自动转换成指针类型。因为decltype的结果是函数类型,所以只有在结果前面加上*才能得到指针。

### 返回指向函数的指针

和数组类似，虽然不能返回一个函数，但是能返回指向函数类型的指针。然而，我们必须把返回类型写成指针形式，编译器不会自动地将函数返回类型当成对应的指针类型处理。与往常一样，要想声明一个返回函数指针的函数，最简单的办法是使用类型别名:

```
using F= int(int*,int);//F是函数类型，不是指针
using PF = int(*)(int*, int);//PF是指针类型
其中我们使用类型别名将F定义成函数类型，将PF定义成指向函数类型的指针。必须时刻注意的是，和函数类型的形参不一样，返回类型不会自动地转换成指针。我们必须显式地将返回类型指定为指针:
PF f1 (int);//正确:PF是指向函数的指针,f1返回指向函数的指针
F f1(int);//错误:F是函数类型，f1不能返回一个函数
F*f1(int) ;//正确:显式地指定返回类型是指向函数的指针
当然，我们也能用下面的形式直接声明f1:
int(*f1 (int))(int*, int);
```

按照由内向外的顺序阅读这条声明语句:我们看到f1有形参列表，所以f1是个函数;f1前面有*，所以f1返回一个指针;进一步观察发现，指针的类型本身也包含形参列表,因此指针指向函数，该函数的返回类型是int。

出于完整性的考虑,有必要提醒读者我们还可以使用尾置返回类型的方式声明一个返回函数指针的函数:

```
auto f1(int) -> int (*)(int*,int);
```

### 将auto和decltype用于函数指针类型

如果我们明确知道返回的函数是哪一个，就能使用decltype简化书写函数指针返回类型的过程。例如假定有两个函数，它们的返回类型都是 string ::size_type，并且各有两个 const strings类型的形参，此时我们可以编写第三个函数，它接受一个string类型的参数，返回一个指针，该指针指向**前两个函数中的一个**:

```
string::size_type sumLength(const string&,const string&);
string::size_type largerLength (const string&,const string&);
//根据其形参的取值，getFcn函数返回指向sumLength或者largerLength 的指针decltype (sumLength) *getFcn(const string &);
//getFcn有形参列表，所以getFcn是个函数

//声明 getFcn 唯一需要注意的地方是，牢记当我们将decltype作用于某个函数时，它返回函数类型而非指针类型。因此，我们显式地加上*以表明我们需要返回指针，而非函数本身。
```

```
decltype(*p)`得到的类型是`int &
```
