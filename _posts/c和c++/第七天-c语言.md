---
title: 第七天 c语言
date: 2023-03-21 16:25:11
tags:
---

# C 强制类型转换

强制类型转换是把变量从一种类型转换为另一种数据类型。例如，如果您想存储一个 long 类型的值到一个简单的整型中，您需要把 long 类型强制转换为 int 类型。您可以使用**强制类型转换运算符**来把值显式地从一种类型转换为另一种类型，如下所示：

```
(type_name) expression
```

请看下面的实例，使用强制类型转换运算符把一个整数变量除以另一个整数变量，得到一个浮点数：

## 实例

```
#include <stdio.h>
 
int main()
{
   int sum = 17, count = 5;
   double mean;
 
   mean = (double) sum / count;
   printf("Value of mean : %f\n", mean );
 
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
Value of mean : 3.400000
```

这里要注意的是强制类型转换运算符的**优先级大于除法**，因此 **sum** 的值首先被转换为 **double** 型，然后除以 count，得到一个类型为 double 的值。

## 整数提升

整数提升是指把小于 **int** 或 **unsigned int** 的整数类型转换为 **int** 或 **unsigned int** 的过程：比如将char类型的字符转化成ascii码再与int型计算得到int型。

## 常用的算术转换

**常用的算术转换**是**隐式地**把值强制转换为相同的类型。编译器首先执行**整数提升**，如果操作数类型不同，则它们会被转换为下列层次中出现的最高层次的类型：

char,short-->int-->unsigned int-->long-->unsigned long-->long long-->unsigned long long-->float-->double-->long double

float、double、long double 类型赋值给整数类型：直接截断小数.

------

为了提高效率，C 语言对于不同的两个类型将直接转换成较高类型计算。

举个例子：对于已经分别被定义且被赋值，类型为 **double** 和 **int** 的 **a** 和 **b**：

假设如果进行 **a + b** 运算，那么b将直接被隐式转换为 **double** 类型，然后再进行运算，不能理解为逐层转换（即不能理解为b先转换为unsigned int类型，再转换为 **long => unsigned long => long long => unsigned long long => float => double**类型，最后再进行运算）。

**p.s:** 说到运算，对于 **char** 和 **short** 类型，进行运算时将会被隐式转换为 **int**

------

常用的算术转换不适用于赋值运算符、逻辑运算符 && 和 ||。

### 注意

C 语言中 printf 输出 double 和 float 都可以用 **%f** 占位符 可以混用，而 double 可以额外用 **%lf**。

而 scanf 输入情况下 double 必须用 **%lf**，float 必须用 **%f** 不能混用。

------

**强制类型转换只是临时类型转换，并不影响变量本身储存的值**，看如下代码：

```
#include <stdio.h>
int main() {
  float a = 6.9;
  printf("%.3f", a);
  putchar('\n');
  ((int)a);
  printf("%.3f", a);
  return 0;
}
```

输出结果：

```
6.900
6.900
```

------

### 存储长度较短的类型赋值给存储长度较长的类型：补足有效位，其它位补 0

假设有如下定义：

```
char c = 56;//1个字节,8位二进制
short num = 67;//2个字节,16位二进制
int m;//4个字节,32位二进制
long long int n;//8个字节,64位二进制
```

如果执行以下操作：

```
m = ((int)c);
n = ((long long)num);
```

那么它们在内存以 2 进制格式分别存储为：

```
                                                               00111000  //Binary of 'c'
                                                      00000000 01000011  //Binary of'num'
                                    00000000 00000000 00000000 00111000  //Binary of 'm'
00000000 00000000 00000000 00000000 00000000 00000000 00000000 01000011  //Binary of 'n'
```

### 存储长度较长的类型赋值给存储长度较短的类型：舍弃高位（但保留符号），截断低字节给存储长度较短的类型

假设定义：

```
long long int l = 223372036854775807;
```

进行赋值：

```
int i = (int)l;
```

它们在内存以2进制格式分别存储为：

```
00000011 00011001 10010011 10101111 00011101 01111011 11111111 11111111  //Binary of 'l'
                                    00011101 01111011 11111111 11111111  //Binary of 'v'
```

**p.s:** 此时的 v=494665727。

顺带说明一下，如果变量 “l” 的后 32 个字节都为 1，那么 v 将等于 -1。因为负数在计算机都以补码形式存在，即

后 32 个字节都为 1的是补码变为原码就为-1.

### unsigned类型赋值给非unsigned类型：直接传递数值

**注意：**如果 unsigned 类型储存的量太大，强制类型转换后可能会出现非 unsigned 类型的值的绝对值不等于 unsigned 类型的值的绝对值的情况。

说到 “unsigned 类型储存的量太大”，顺带说一下，虽然 printf 输出 int 和 unsigned int 时可以混用 %d（或%i）和 %u（或%ui），但还是建议输出 int 类型的时候用 %d（或%i），输出 unsigned int 类型时用 %u（或%ui）（其它类型同理<如%ul等>）

### 非 unsigned 类型赋值给 unsigned 类型：直接传递数值

给个小技巧：如果你想“临时”给一个不知道正负的非 unsigned 类型的变量加上绝对值，可以使用abs函数，但利用(unsigned)(非unsigned类型变量名)可以节省一点内存开销

但是也有弊端：可能会出现 unsigned-unsigned 永远大于 0 的情况（不确定）

# C 错误处理

C 语言不提供对错误处理的直接支持，但是作为一种系统编程语言，它以返回值的形式允许您访问底层数据。在发生错误时，大多数的 C 或 UNIX 函数调用返回 1 或 NULL，同时会设置一个错误代码 **errno**，该错误代码是全局变量，表示在函数调用期间发生了错误。您可以在 errno.h 头文件中找到各种各样的错误代码。

所以，C 程序员可以通过检查返回值，然后根据返回值决定采取哪种适当的动作。开发人员应该在程序初始化时，把 errno 设置为 0，这是一种良好的编程习惯。0 值表示程序中没有错误。

## errno、perror() 和 strerror()

C 语言提供了 **perror()** 和 **strerror()** 函数来显示与 **errno** 相关的文本消息。

- **perror()** 函数显示您传给它的字符串，后跟一个冒号、一个空格和当前 errno 值的文本表示形式。
- **strerror()** 函数，返回一个指针，指针指向当前 errno 值的文本表示形式。

让我们来模拟一种错误情况，尝试打开一个不存在的文件。您可以使用多种方式来输出错误消息，在这里我们使用函数来演示用法。另外有一点需要注意，您应该使用 **stderr 文件流**来输出所有的错误。

## 实例

```
#include <stdio.h>
#include <errno.h>
#include <string.h>
 
extern int errno ;
 
int main ()
{
   FILE * pf;
   int errnum;
   pf = fopen ("unexist.txt", "rb");
   if (pf == NULL)
   {
      errnum = errno;
      fprintf(stderr, "错误号: %d\n", errno);
      perror("通过 perror 输出错误");
      fprintf(stderr, "打开文件错误: %s\n", strerror( errnum ));
   }
   else
   {
      fclose (pf);
   }
   return 0;
}
```

fprintf是C/C++中的一个格式化库函数，位于头文件中，其作用是格式化输出到一个流文件中；

函数原型为int fprintf( FILE *stream, const char *format, [ argument ]…)，fprintf()函数根据指定的格式(format)，向输出流(stream)写入数据(argument)。将内容输出到指定.txt中FILE* fp = NULL;
fp = fopen("e:\\gz.txt","w+");fprintf(fp,"%d,%x,%o",10,10,10);将内容输出到屏幕,使用stdout或者stderr:fprintf(stdout,"%d,%x,%o",10,10,10);stdout和stderr，stdout是标准的输出流，而stderr是标准的错误输出流。stdout和stderr的类型都是FILE*，在stdio.h中定义。默认情况下，stdout和stderr中的数据都会被打印到屏幕上。

当上面的代码被编译和执行时，它会产生下列结果：

```
错误号: 2
通过 perror 输出错误: No such file or directory
打开文件错误: No such file or directory
```

windows上述代码里 errno 的值从而得出不同的错误类型，从 43 之后就一直是 Unknown error 错误类型。

```
Value of errno: 0
Error opening file: No error

Value of errno: 1
Error opening file: Operation not permitted

Value of errno: 2
Error opening file: No such file or directory

Value of errno: 3
Error opening file: No such process

Value of errno: 4
Error opening file: Interrupted function call

Value of errno: 5
Error opening file: Input/output error

Value of errno: 6
Error opening file: No such device or address

Value of errno: 7
Error opening file: Arg list too long

Value of errno: 8
Error opening file: Exec format error

Value of errno: 9
Error opening file: Bad file descriptor

Value of errno: 10
Error opening file: No child processes

Value of errno: 11
Error opening file: Resource temporarily unavailable

Value of errno: 12
Error opening file: Not enough space

Value of errno: 13
Error opening file: Permission denied

Value of errno: 14
Error opening file: Bad address

Value of errno: 15
Error opening file: Unknown error

Value of errno: 16
Error opening file: Resource device

Value of errno: 17
Error opening file: File exists

Value of errno: 18
Error opening file: Improper link

Value of errno: 19
Error opening file: No such device

Value of errno: 20
Error opening file: Not a directory

Value of errno: 21
Error opening file: Is a directory

Value of errno: 22
Error opening file: Invalid argument

Value of errno: 23
Error opening file: Too many open files in system

Value of errno: 24
Error opening file: Too many open files

Value of errno: 25
Error opening file: Inappropriate I/O control operation

Value of errno: 26
Error opening file: Unknown error

Value of errno: 27
Error opening file: File too large

Value of errno: 28
Error opening file: No space left on device

Value of errno: 29
Error opening file: Invalid seek

Value of errno: 30
Error opening file: Read-only file system

Value of errno: 31
Error opening file: Too many links

Value of errno: 32
Error opening file: Broken pipe

Value of errno: 33
Error opening file: Domain error

Value of errno: 34
Error opening file: Result too large

Value of errno: 35
Error opening file: Unknown error

Value of errno: 36
Error opening file: Resource deadlock avoided

Value of errno: 37
Error opening file: Unknown error

Value of errno: 38
Error opening file: Filename too long

Value of errno: 39
Error opening file: No locks available

Value of errno: 40
Error opening file: Function not implemented

Value of errno: 41
Error opening file: Directory not empty

Value of errno: 42
Error opening file: Illegal byte sequence

Value of errno: 43
Error opening file: Unknown error

Value of errno: 44
Error opening file: Unknown error

Value of errno: 45
Error opening file: Unknown error

Value of errno: 46
Error opening file: Unknown error
```

## 程序退出状态

通常情况下，程序成功执行完一个操作正常退出的时候会带有值 EXIT_SUCCESS。在这里，EXIT_SUCCESS 是宏，它被定义为 0。

如果程序中存在一种错误情况，当您退出程序时，会带有状态值 EXIT_FAILURE，被定义为 -1。

exit（）函数关闭了所有打开的文件并终止程序，exit()函数的参数会被传递给一些操作系统，通常的约定是正常终止的程序传递值0,非正常终止的程序传递非0值。

exit（!0）； //表示异常退出
exit（0）； //表示正常退出

return 0；
exit（0）；
这两者之间表达的效果是相同的。
但是：不同的是，main（） 函数在一个递归程序中，exit（）会终止程序，但是return将控制权交给递归上一级直至到最初的一级。

exit() 的参数，是给自己的父进程使用的。通常一个程序的父进程可能是任何进程，因此我们无法预期我们的父进程是否规定必须要有这个**返回值**，那么我们应当提供这个返回值，以保证不同的父进程的需求得到满足。

虽然现在大多数平台下，直接在 main() 函数里面 return 可以退出程序。但是在某些平台下，在 main() 函数里面 return 会导致程序永远不退出（因为代码已经执行完毕，程序却还没有收到要退出的指令）。换句话说，为了兼容性考虑，在特定的平台下，程序最后一行必须使用 exit() 才能正常退出，这是 exit() 存在的重要价值。

