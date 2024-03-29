---
title: 第八天 c语言
date: 2023-03-22 16:14:02
tags:
---

# C递归

递归是一个简洁的概念，同时也是一种很有用的手段。但是，使用递归是要付出代价的。与直接的语句(如while循环)相比，递归函数会**耗费更多的运行时间，并且要占用大量的栈空间**。递归函数每次调用自身时，都需要把它的状态存到栈中，以便在它调用完自身后，程序可以返回到它原来的状态。未经精心设计的递归函数总是会带来麻烦。

电脑空间大致分**Heap（堆）**和**Stack（栈）**两种。

**栈**是用于**函数**的空间。

电脑调用一个函数，就会使用一层栈；

相反，电脑中一个函数结束（return），就会释放这一层栈，**连同在这层栈（这个函数）中定义的所有东西**。

不在栈中的，应该就在堆中。**（这就是定义全区变量与局部变量的用处）**

如果调用太多层栈（太多个函数），电脑就会暴空间！

所以说，调用递归函数，就会**一层一层地压栈**，**电脑就会暴空间！**（**并不代表不建议用递归，只是作提示而已**）

**栈：**

在函数中定义的变量存放的内存区域。常见的int、float、char等变量均存放于栈区中，它的特点是由系统自动分配与释放，不需要程序员考虑资源回收的问题，方便简洁。ps：**栈区的地址分配是从内存的高地址开始向低地址分配；**

**堆：**

通过指令自主向系统申请的内存区域，大小由自己决定，它在使用完后同样需要自己通过指令去释放该区域内存，否则将有可能出现内存的浪费与溢出。

C语言中申请堆区指令为：

```c
int *p = (int *) malloc( N * sizeof(int) );  //分配N个int型(4字节)的内存，即 4 * N 个字节
```

ps：**但指针p存放于栈区，指向堆**。

C语言中释放堆区指令为：

```c
free( p ); //注意此处参数为指针
```

使用中应该注意，尽量不要去修改p指针对应的地址值，否则在内存释放时将出现错误。(编译可通过，运行出现问题)

**bss区：**

通常是指用来存放程序中未初始化的全局变量的一块内存区域，由static修饰，BSS是英文Block Started by Symbol的简称，BSS段属于静态内存分配。ps：静态变量仅在第一次创建时初始化一次，之后自动跳过初始化语句。全局变量与静态变量均由系统分配和释放内存，若未对它们进行初始化操作，系统将自动将其值设置为0。(堆区与栈区则不会)

**代码段：**

代码段(code segment/text segment)通常是指用来存放程序执行代码的一块内存区域。这部分区域的大小在程序运行前就已经确定，并且内存区域通常属于只读, 某些架构也允许代码段为可写，即允许修改程序。在代码段中，也有可能包含一些只读的常数变量，例如字符串常量等。常见的使用：

```c
char *s = "HelloWorld";//该字符串 HelloWorld 即存放于文字常量区，不可修改。
```

ps：但指针s存放于栈区。

pps：若在程序中尝试对其修改（例如尝试修改第一个字符 *s = 'h';），将出现编译可通过，运行报错的情况。

同时因注意它与const修饰的变量之间的区别：char aa = 'A';//aa存放于栈区；const char bb = 'B'; //bb同样存放于栈区，const修饰的变量仅仅用于告诉编译器bb是一个常量，如果后续的程序中有出现尝试修改bb的操作时，编译将报错。这种写法主要是为了防止程序员在后续的代码中误操作bb变量而添加的一个约束条件，并不会影响它存放的位置。

### 例子：

```c
#include <stdio.h>

static unsigned int val1 = 1;        //val1存放在.data段
unsigned int val2 = 1;               //初始化的全局变量存放在.data段
unsigned int val3 ;                  //未初始化的全局变量存放在.bss段
const unsigned int val4 = 1;         //val4存放在.rodata（只读数据段）

unsigned char Demo(unsigned int num) //num 存放在栈区
{
  char var = "123456";               //var存放在栈区，"123456"存放在常量区
  unsigned int num1 = 1 ;            //num1存放在栈区
  static unsigned int num2 = 0;      //num2存放在.data段
  const unsigned int num3 = 7;       //num3存放在栈区
  void *p;
  p = malloc(8);                     //p存放在堆区，但指针p存放于栈区，指向堆
  free(p);
  return 1;
}

void main()
{
  unsigned int num = 0 ;
  num = Demo(num);                   //Demo()函数的返回值存放在栈区。
}
```

DATA 段（全局初始化区）存放初始化的全局变量和静态变量；BSS 段（全局未初始化区）存放未初始化的全局变量和静态变量。

静态局部变量时在编译时被赋值的，即自始至终**只赋值一次**，在程序运行时它已经有初值。以后每次调用函数时不再重新赋初值而只是保留上次函数调用结束时的值。而自动变量赋初值，不是在编译时进行的，而是在**运行时进行**，所以**每调用一次函数就赋一次初值**。

# C 可变参数

有时，您可能会碰到这样的情况，您希望函数带有可变数量的参数，而不是预定义数量的参数。

C 语言为这种情况提供了一个解决方案，它允许您定义一个函数，能根据具体的需求接受可变数量的参数。

声明方式为：

```
int func_name(int arg1, ...);
```

其中，省略号 **...** 表示可变参数列表。

下面的实例演示了这种函数的使用：

int func(int, ... )  {   .   .   . }  int main() {   func(2, 2, 3);   func(3, 2, 3, 4); }

请注意，函数 **func()** 最后一个参数写成省略号，即三个点号（**...**），省略号之前的那个参数是 **int**，代表了要传递的可变参数的总数。为了使用这个功能，您需要使用 **stdarg.h** 头文件，该文件提供了实现可变参数功能的函数和宏。具体步骤如下：

- 定义一个函数，最后一个参数为省略号，省略号前面可以设置自定义参数。
- 在函数定义中创建一个 **va_list** 类型变量，该类型是在 stdarg.h 头文件中定义的。
- 使用 **int** 参数和 **va_start()** 宏来初始化 **va_list** 变量为一个参数列表。宏 **va_start()** 是在 stdarg.h 头文件中定义的。
- 使用 **va_arg()** 宏和 **va_list** 变量来访问参数列表中的每个项。
- 使用宏 **va_end()** 来清理赋予 **va_list** 变量的内存。

常用的宏有：

- `**va_start(ap, last_arg)**`：初始化可变参数列表。`ap` 是一个 `va_list` 类型的变量，`last_arg` 是最后一个固定参数的名称（也就是可变参数列表之前的参数）。该宏将 `ap` 指向可变参数列表中的第一个参数。
- `**va_arg(ap, type)**`：获取可变参数列表中的下一个参数。`ap` 是一个 `va_list` 类型的变量，`type` 是下一个参数的类型。该宏返回类型为 `type` 的值，并将 `ap` 指向下一个参数。
- va_copy(va_list dest, va_list src) 宏：用于将src指向的参数复制到dest中。
- `**va_end(ap)**`：结束可变参数列表的访问。`ap` 是一个 `va_list` 类型的变量。该宏将 `ap` 置为 `NULL`。

现在让我们按照上面的步骤，来编写一个带有可变数量参数的函数，并返回它们的平均值：

## 实例

```
#include <stdio.h>
#include <stdarg.h>
 
double average(int num,...)
{
 
    va_list valist;
    double sum = 0.0;
    int i;
 
    /* 为 num 个参数初始化 valist */
    va_start(valist, num);
 
    /* 访问所有赋给 valist 的参数 */
    for (i = 0; i < num; i++)
    {
       sum += va_arg(valist, int); // 获取当前参数，并将指针移动到下一个参数
    }
    /* 清理为 valist 保留的内存 */
    va_end(valist);
 
    return sum/num;
}
 
int main()
{
   printf("Average of 2, 3, 4, 5 = %f\n", average(4,2,3,4,5));
   printf("Average of 5, 10, 15 = %f\n", average(3,5,10,15));
}
```

在上面的例子中，average() 函数接受一个整数 num 和任意数量的整数参数。函数内部使用 va_list 类型的变量 va_list 来访问可变参数列表。在循环中，每次使用 va_arg() 宏获取下一个整数参数，并输出。最后，在函数结束时使用 va_end() 宏结束可变参数列表的访问。

当上面的代码被编译和执行时，它会产生下列结果。应该指出的是，函数 **average()** 被调用两次，每次第一个参数都是表示被传的可变参数的总数。省略号被用来传递可变数量的参数。

```
Average of 2, 3, 4, 5 = 3.500000
Average of 5, 10, 15 = 10.000000
```

```
// 本质上，va_list 类型，就是「char指针」类型，指向 1B 大小的内存地址
typedef char* va_list;
```
