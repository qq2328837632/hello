---
title: 第五天 c语言
date: 2023-03-19 14:49:49
tags:
---

# C 共用体

## 共用体类型数据的特点

1. 系统采用覆盖技术，共用体变量中起作用的成员是最后一次存放的成员，在存入一个新的成员后原有的成员就失去作用
2. 共用体变量的地址和它的各成员的地址都是同一地址
3. 不能引用变量名来得到一个值，不能对共用体变量名赋值，不能在定义共用体变量时对它整体初始化
4. 共用体类型的变量可作为函数实参进行传递
5. 共用体类型可出现在结构体类型中，结构体类型也可存在于共用体类型中

## **结构体与共用体**

结构体变量所占内存长度是其中最大字段大小的整数倍。

共用体变量所占的内存长度等于最长的成员变量的长度。例如，教程中定义的共用体Data各占20个字节（因为char str[20]变量占20个字节）,而不是各占4+4+20=28个字节。

```
union Data
{
   int i;
   float f;
   char  str[20];
} data;  
```

## 共用体作用

节省内存，有两个很长的数据结构，不会同时使用，比如一个表示老师，一个表示学生，如果要统计教师和学生的情况用结构体的话就有点浪费了！用共用体的话，只占用最长的那个数据结构所占用的空间，就足够了！

## 共用体应用场景

通信中的数据包会用到共用体:因为不知道对方会发一个什么包过来，用共用体的话就很简单了，定义几种格式的包，收到包之后就可以直接根据包的格式取出数据。

编程时经常会需要判断机器是大端机还是小端机，此时使用union就非常方便：

```
union
{
    char str;
    int data;
};
data=0x01020304;//最后赋给变量的值占用了内存位置，str是低地址，机器是从低地址到高地址读取数据，无论大小端机读取数据的时候，肯定是全部读取完后再判断数值
if(str==0x01)//str读取一个字节数据，只有两位
{
    cout<< "此机器是大端！"<<endl;
}
else if(str==0x04){
    cout<<"此机器是小端！"<<endl;
}
else{
    cout <<" 暂无法判断此机器类型！"<<endl;
}
```

注：大端机高位存在低位，小端机反之

------

# 下面给出部分概念：

- **位：**"位(bit)"是电子计算机中最小的数据单位。每一位的状态只能是0或1。

- **字节：**8个二进制位构成1个"字节(Byte)"，它是存储空间的基本计量单位。1个字节可以储存1个英文字母或者半个汉字，换句话说，1个汉字占据2个字节的存储空间。

- **字：**"字"由若干个字节构成，字的位数叫做字长，不同档次的机器有不同的字长。例如一台8位机，它的1个字就等于1个字节，字长为8位。如果是一台16位机，那么，它的1个字就由2个字节构成，字长为16位。字是计算机进行数据处理和运算的单位。
- 一般的计算机都已经到了64位机 也就是说 一个基本单位就是64位，也就是8字节了。这样再综合上面的分析就不难看出，结构体，共用体，位域的定义中，按顺序分配内存，下一个字段所占大小如果超出了上一个字段占的内存单元剩余部分，那么它会重新申请下一个内存单元，而上一个多出部分将空着。

# typedef vs #define

**#define** 是 C 指令，用于为各种数据类型定义别名，与 **typedef** 类似，但是它们有以下几点不同：

- **typedef** 仅限于为类型定义符号名称，**#define** 不仅可以为类型定义别名，也能为数值定义别名，比如您可以定义 1 为 ONE。
- **typedef** 是由编译器执行解释的，**#define** 语句是由预编译器进行处理的。

## **typedef 与 #define 的区别**

（1）#define可以使用其他类型说明符对宏类型名进行扩展，但对 typedef 所定义的类型名却不能这样做。例如：

```
#define INTERGE int;
unsigned INTERGE n;  //没问题
typedef int INTERGE;
unsigned INTERGE n;  //错误，不能在 INTERGE 前面添加 unsigned
```

（2） 在连续定义几个变量的时候，typedef 能够保证定义的所有变量均为同一类型，而 #define 则无法保证。例如：

```
#define PTR_INT int *
PTR_INT p1, p2;        //p1、p2 类型不相同，宏展开后变为int *p1, p2;
typedef int * PTR_INT
PTR_INT p1, p2;        //p1、p2 类型相同，它们都是指向 int 类型的指针。
```

简而言之，**#define** 只是字面上的替换，由预处理器执行，**#define A B** 相当于打开编辑器的替换功能，把所有的 B 替换成 A。

与 #define 不同，typedef 具有以下三个特点：

-  1.typedef 给出的符号名称仅限于对类型，而不是对值。

-  2.typedef 的解释由编译器，而不是预处理器执行。并不是简单的文本替换。

-  3.虽然范围有限，但是在其受限范围内 typedef 比 #define 灵活。

  ------

  

用 **typedef** 为数组去别名：

```
typedef int A[6];
```

表示用 **A** 代替 **int [6]**。

即：**A a;** 等于 **int a[6];**

------

**typedef 还有一个作用，就是为复杂的声明定义一个新的简单的别名。用在回调函数中特别好用：**

1. 原声明：**int \*(\*a[5])(int, char\*);**

在这里，变量名为 **a**，直接用一个新别名 **pFun** 替换 **a** 就可以了：

```
typedef int *(*pFun)(int, char*);
```

于是，原声明的最简化版：

```
pFun a[5];
```

2. 原声明：**void (\*b[10]) (void (\*)());**

这里，变量名为 b，先替换右边部分括号里的，pFunParam 为别名一：

```
typedef void (*pFunParam)();
```

再替换左边的变量 **b**，**pFunx** 为别名二：

```
typedef void (*pFunx)(pFunParam);
```

于是，原声明的最简化版：

```
pFunx b[10];
```

其实，可以这样理解:

```
typedef int *(*pFun)(int, char*); 
```

由 **typedef** 定义的函数 **pFun**，为一个新的类型，所以这个新的类型可以像 **int** 一样定义变量，于是，**pFun a[5];** 就定义了 **int \*(\*a[5])(int, char\*);**

所以我们可以用来定义回调函数，特别好用。

另外，也要注意，typedef 在语法上是一个存储类的关键字（如 auto、extern、mutable、static、register 等一样），虽然它并不真正影响对象的存储特性，如：

```
typedef static int INT2; // 不可行
```

编译将失败，会提示“指定了一个以上的存储类”。

关于回调函数的例子，沿用前面回调函数的代码改动：

**原代码**

```
#include <stdlib.h>  
#include <stdio.h>
 
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{  /*有关于 size_t:size_t 是一种数据类型，近似于无符号整型，但容量范围一般大于 int 和 unsigned。这里使用 size_t 是为了保证 arraysize 变量能够有足够大的容量来储存可能大的数组。*/
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}
 
// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}
 
int main(void)
{
    int myarray[10];
    /* getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针*/
    populate_array(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}
```

**改动之后**

```
#include <stdlib.h>  
#include <stdio.h>
int getNextRandomValue(void);
void populate_array(int *array, size_t arraySize, int (*getNextValue)(void));
typedef int parm(void);
typedef void p(int *array, size_t arraySize,parm);

void populate_array(int *array, size_t arraySize, int (*getNextValue)(void))
{  /*有关于 size_t:size_t 是一种数据类型，近似于无符号整型，但容量范围一般大于 int 和 unsigned。这里使用 size_t 是为了保证 arraysize 变量能够有足够大的容量来储存可能大的数组。*/
    for (size_t i=0; i<arraySize; i++)
        array[i] = getNextValue();
}
 
// 获取随机值
int getNextRandomValue(void)
{
    return rand();
}
 
int main(void)
{
    int myarray[10];
    
    /* getNextRandomValue 不能加括号，否则无法编译，因为加上括号之后相当于传入此参数时传入了 int , 而不是函数指针*/
    p *p1;//注意typedef void（.）p...的时候如果没加*，则添加新的p1需要加*（不然会报错p1不是左值，lvaues本质“就是指一些对象、或者是表达式。这些对象、表达式必须代表一块内存区域”）
    p1=populate_array;
    p1(myarray, 10, getNextRandomValue);
    for(int i = 0; i < 10; i++) {
        printf("%d ", myarray[i]);
    }
    printf("\n");
    return 0;
}
```

# c输入输出

## getchar() & putchar() 函数

**int getchar(void)** 函数从屏幕读取下一个可用的字符，并把它返回为一个整数。这个函数在同一个时间内只会读取一个单一的字符。您可以在循环内使用这个方法，以便从屏幕上读取多个字符。

**int putchar(int c)** 函数把字符输出到屏幕上，并返回相同的字符。这个函数在同一个时间内只会输出一个单一的字符。您可以在循环内使用这个方法，以便在屏幕上输出多个字符。

## gets()与fgets()

### gets()

gets函数原型：char*gets(char*buffer);//读取字符到数组：gets(str);str为数组名。

gets函数功能：从键盘上输入字符，直至接受到换行符或EOF时停止，并将读取的结果存放在buffer指针所指向的字符数组中。

读取的换行符被转换为null值，做为字符数组的最后一个字符，来结束字符串。

**注意：**gets函数由于没有指定输入字符大小，所以会无限读取，一旦输入的字符大于数组长度，就会发生内存越界，

从而造成程序崩溃或其他数据的错误。

### fgets()

fgets函数原型：char *fgets(char *s, int n, FILE *stream);//我们平时可以这么使用：fgets(str, sizeof(str), stdin);

其中str为数组首地址，sizeof(str)为数组大小，stdin表示我们从键盘输入数据。

fgets函数功能：从文件指针stream中读取字符，存到以s为起始地址的空间里，直到读完N-1个字符，或者读完一行。

**注意：**调用fgets函数时，最多只能读入n-1个字符。读入结束后，系统将自动在最后加'\0'，并以str作为函数值返回。

借用教程实例，我把char str[100] 改为 char str[5]



```
#include <stdio.h>

int main( )
{
    char str[5];

    printf( "Enter a value :");
    gets( str );

    printf( "\nYou entered: ");
    puts( str );
    return 0;
}
```

如果输入123(长度小于5)结果为：

```
Enter a value :123

You entered: 123
```

如果输入123456789(长度大于5)结果为：

```
Enter a value :123456789

You entered: 123456789
```

虽然正常显示了，但是系统提示程序崩溃了

如果不能正确使用gets()函数，带来的危害是很大的，就如上面我们看到的，输入字符串的长度大于缓冲区长度时，并没有截断，原样输出了读入的字符串，造成程序崩溃。



考虑到程序安全性和健壮性，建议用fgets()来代替gets()。如：



```
#include <stdio.h>

int main( )
{
    char str[5];

    printf( "Enter a value :");
    fgets( str,5,stdin );      //fgets()函数;

    printf( "\nYou entered: ");
    puts( str );
    return 0;
}

```

## 其他：

在进行输出时，若要用到用来输出实数的 **f** 格式符（以小数形式输出），有以下几种用法：

**1、基本型，用 \**%f\****

不指定输出类型的长度，用系统根据情况决定，一般是实数中的整数部分全部输出，小数部分输出六位。例：

```
#include<stdio.h>
int main()
{
    double a=1.0;
    printf("%f\n",a/3);
    return 0;
}
```

运行结果：**0.333333**

**2、指定数据宽度和小数位数，用 \**%m.nf\****

例：将上个程序的双精度变量 a 输出 15 位小数，用 **%20.15f** 的格式声明，指定输出的数据占 20 列，其中包括 15 位小数。改动上面程序如下：

```
#include<stdio.h>
int main()
{
    double a=1.0;
    printf("%20.15f\n",a/3);
    return 0;
}
```

运行结果：  **0.333333333333333**

注意在 0 的前面有 3 个空格，且双精度数只保证 15 位有效数字的准确性。

**3、输出的数据相左对齐，用 \**%-m.nf\****

在 **m.n** 前加一个负号，其作用与 **%m.nf** 形式作用基本相同，但当数据长度不长过 **m** 时，数据向左靠，右端补空格。

------

putchar()将一个字符赋给字符变量和将字符的 ASCII 代码(int型)赋给字符变量作用是完全相同的，但要注意其值必须在字符的 ASCII 代码范围内。

------

- 1、scanf() 函数有返回值且类型 int 型，当发生错误时立刻返回 EOF。
-  2、scanf() 函数返回的值为：正确按指定格式输入变量的个数；也即能正确接收到值的变量个数。

------

c 语言中每种数据类型的输出都有各自的占位符，下面是各种数据类型的输出占位符：

**short（2字节）/int（4字节） :** **%d**

```
int a = 1;
printf("这个整数是：%d", a);
```

**long:** **%ld** (long 是 int 得修饰，不能算是一种单独的数据类型，对长整型的定义也是long int应该至少和int一样长，而不是long int 一定要比int占用存储字节长。所以，正确的关系应该是——*long*≥*int*≥*short*)

**long long（8字节） :** **%lld**

**char :** **%c**

**float/double :** **%f** float 默认是 6 位小数输出；可以在 %f 中控制；例如：%.2f：输出两位小数。

**char \*s(字符串) ：****%s**

**unsigned:** **%u** (signed:有符号类型， unsigned：无符号类型；默认都是有符号的)

八进制：**%o** 以 **0** 开头

十六进制：**%x** 以 **0x** 开头

```
int a = 10;
printf("a的八进制输出是：%o \n", a);//输出是12
printf("a的十六进制输出：%x \n", a);//输出是a
```

地址值/指针值：**%p**，***** 取指针里地址指向的地方的值，**&** 取改值存储位置的地址值。

二进制的输出没有占位符，只能通过其他方法。
