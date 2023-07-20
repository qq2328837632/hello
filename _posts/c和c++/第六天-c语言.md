---
title: 第六天 c语言
date: 2023-03-20 10:28:59
tags:
---

# C 文件读写

## 打开文件

您可以使用 **fopen( )** 函数来**创建一个新的文件或者打开一个已有的文件**，这个调用会初始化类型 **FILE** 的一个对象，类型 **FILE** 包含了所有用来控制流的必要的信息。下面是这个函数调用的原型：

```
FILE *fopen( const char *filename, const char *mode );
```

对 fopen()函数补充说明几点：

```
FILE *fopen( const char * filename, const char * mode );
```

- 该函数可能执行失败，返回值是NULL，安全起见必须对返回值进行合法性判断；
- 该函数有多种模式，其中r+和w+看似一样，都是读写其实还是有几点区别的；
-    1.模式r+找不到文件不会自动新建，而w+会；
-    2.模式r+打开文件后，不会清除文件原数据，若直接开始写入，只会从起始位置开始进行**覆盖**，而w+会直接**清零**后，再开始读写；
- 模式的合法性说明：不能用大写，只能是小写，且rb+和r+b都是合法的，但br+和+rb等都是非法的，w和a也是一样的处理；
- 模式w的自动新建文件是有条件的，只有对应的路径存在(即文件所在的文件夹存在)，文件不存在才会新建，否则是不会新建的，返回NULL

在这里，**filename** 是字符串，用来命名文件，访问模式 **mode** 的值可以是下列值中的一个：

| 模式 | 描述                                                         |
| :--- | :----------------------------------------------------------- |
| r    | 打开一个已有的文本文件，允许读取文件。                       |
| w    | 打开一个文本文件，允许写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会从文件的开头写入内容。如果文件存在，则该会被截断为零长度，重新写入。 |
| a    | 打开一个文本文件，以追加模式写入文件。如果文件不存在，则会创建一个新文件。在这里，您的程序会在已有的文件内容中追加内容。 |
| r+   | 打开一个文本文件，允许读写文件。                             |
| w+   | 打开一个文本文件，允许读写文件。如果文件已存在，则文件会被截断为零长度，如果文件不存在，则会创建一个新文件。 |
| a+   | 打开一个文本文件，允许读写文件。如果文件不存在，则会创建一个新文件。读取会从文件的开头开始，写入则只能是追加模式。 |

如果处理的是二进制文件，则需使用下面的访问模式来取代上面的访问模式：

```
"rb", "wb", "ab", "rb+", "r+b", "wb+", "w+b", "ab+", "a+b"
```

## 关闭文件

为了关闭文件，请使用 fclose( ) 函数。函数的原型如下：

```
 int fclose( FILE *fp );
```

如果成功关闭文件，**fclose( )** 函数**返回零**，如果关闭文件时发生错误，函数返回 **EOF**。这个函数实际上，会清空缓冲区中的数据，关闭文件，并释放用于该文件的所有内存。EOF 是一个定义在头文件 **stdio.h** 中的常量。

C 标准库提供了各种函数来按字符或者以固定长度字符串的形式读写文件。

## 写入文件

下面是把字符写入到流中的最简单的函数：

```
int fputc( int c, FILE *fp );
```

函数 **fputc()** 把参数 c 的字符值写入到 fp 所指向的输出流中。如果写入成功，它会返回写入的字符，如果发生错误，则会返回 **EOF**。您可以使用下面的函数来把一个以 null 结尾的字符串写入到流中：

```
int fputs( const char *s, FILE *fp );
```

函数 **fputs()** 把字符串 **s** 写入到 fp 所指向的输出流中。如果写入成功，它会返回一个非负值，如果发生错误，则会返回 **EOF**。您也可以使用 **int fprintf(FILE \*fp,const char \*format, ...)** 函数把一个字符串写入到文件中。尝试下面的实例：

> **注意：**请确保您**有可用的 tmp 目录**，如果不存在该目录，则需要在您的计算机上先创建该目录。
>
> **/tmp** 一般是 Linux 系统上的临时目录，如果你在 Windows 系统上运行，则需要修改为本地环境中已存在的目录，例如: **C:\tmp**、**D:\tmp**等。

## 实例

```
#include <stdio.h>
 
int main()
{
   FILE *fp = NULL;
 
   fp = fopen("/tmp/test.txt", "w+");
   fprintf(fp, "This is testing for fprintf...\n");
   fputs("This is testing for fputs...\n", fp);
   fclose(fp);
}
```

当上面的代码被编译和执行时，它会在 /tmp 目录中创建一个新的文件 **test.txt**，并使用两个不同的函数写入两行。接下来让我们来读取这个文件。

## 读取文件

下面是从文件读取单个字符的最简单的函数：

```
int fgetc( FILE * fp );
```

**fgetc()** 函数从 fp 所指向的输入文件中读取一个字符。返回值是读取的字符，如果发生错误则返回 **EOF**。下面的函数允许您从流中读取一个字符串：

```
char *fgets( char *buf, int n, FILE *fp );
```

函数 **fgets()** 从 fp 所指向的输入流中读取 n - 1 个字符。它会把读取的字符串复制到缓冲区 **buf**，并在最后追加一个 **null** 字符来终止字符串。

如果这个函数在读取最后一个字符之前就遇到一个**换行符 '\n'** 或文件的末尾 EOF，则只会返回读取到的字符，**包括换行符**。您也可以使用 **int fscanf(FILE \*fp, const char \*format, ...)** 函数来从文件中读取字符串，但是在遇到**第一个空格和换行符**时，它会停止读取。

## 实例

```
#include <stdio.h>
 
int main()
{
   FILE *fp = NULL;
   char buff[255];
 
   fp = fopen("/tmp/test.txt", "r");
   fscanf(fp, "%s", buff);
   printf("1: %s\n", buff );
 
   fgets(buff, 255, (FILE*)fp);
   printf("2: %s\n", buff );
   
   fgets(buff, 255, (FILE*)fp);
   printf("3: %s\n", buff );
   fclose(fp);
 
}
```

当上面的代码被编译和执行时，它会读取上一部分创建的文件，产生下列结果：

```
1: This
2: is testing for fprintf...

3: This is testing for fputs...
```

首先，**fscanf()** 方法只读取了 **This**，因为它在后边遇到了一个空格。其次，调用 **fgets()** 读取剩余的部分，直到行尾。最后，调用 **fgets()** 完整地读取第二行。**依次读取**

## fseek 可以移动文件指针到指定位置读,或插入写:

```
int fseek(FILE *stream, long offset, int whence);
```

fseek 设置当前读写点到 offset 处, whence 可以是 SEEK_SET,SEEK_CUR,SEEK_END 这些值决定是从文件头、当前点和文件尾计算偏移量 offset。

你可以定义一个文件指针 **FILE \*fp**,当你打开一个文件时，文件指针指向开头，你要指到多少个字节，只要控制偏移量就好，例如, 相对当前位置往后移动一个字节：**fseek(fp,1,SEEK_CUR);** 中间的值就是偏移量。 如果你要往前移动一个字节，直接改为负值就可以：**fseek(fp,-1,SEEK_CUR)**。

执行以下实例前，确保当前目录下 **test.txt** 文件已创建：

```
#include <stdio.h>

int main(){   
    FILE *fp = NULL;
    fp = fopen("test.txt", "r+");  // 确保 test.txt 文件已创建
    fprintf(fp, "This is testing for fprintf...\n");   
    fseek(fp, 10, SEEK_SET);
    if (fputc(65,fp) == EOF) {//将65对应字符值A替换掉当前指针指向的s，函数 fputc() 把参数 c 的字符值写入到 fp 所指向的输出流中。如果写入成功，它会返回写入的字符，如果发生错误，则会返回 EOF
        printf("fputc fail");   
    }   
    fclose(fp);
}
```

执行结束后，打开 test.txt 文件：

```
This is teAting for fprintf...
```

**注意：** 只有用 **r+** 模式打开文件才能插入内容，**w** 或 **w+** 模式都会清空掉原来文件的内容再来写，**a** 或 **a+** 模式即总会在文件最尾添加内容，哪怕用 fseek() 移动了文件指针位置。

# C 预处理器

**C 预处理器**不是编译器的组成部分，但是它是编译过程中一个单独的步骤。简言之，C 预处理器只不过是一个文本替换工具而已，它们会指示编译器在实际编译之前完成所需的预处理。我们将把 C 预处理器（C Preprocessor）简写为 CPP。

所有的预处理器命令都是以井号（#）开头。它必须是第一个非空字符，为了增强可读性，预处理器指令应从第一列开始。下面列出了所有重要的预处理器指令：

| 指令     | 描述                                                        |
| :------- | :---------------------------------------------------------- |
| #define  | 定义宏                                                      |
| #include | 包含一个源代码文件                                          |
| #undef   | 取消已定义的宏                                              |
| #ifdef   | 如果宏已经定义，则返回真                                    |
| #ifndef  | 如果宏没有定义，则返回真                                    |
| #if      | 如果给定条件为真，则编译下面代码                            |
| #else    | #if 的替代方案                                              |
| #elif    | 如果前面的 #if 给定条件不为真，当前条件为真，则编译下面代码 |
| #endif   | 结束一个 #if……#else 条件编译块                              |
| #error   | 当遇到标准错误时，输出错误消息                              |
| #pragma  | 使用标准化方法，向编译器发布特殊的命令到编译器中            |

------

```
#include <stdio.h>
#include "myheader.h"
```

这些指令告诉 CPP 从**系统库**中获取 stdio.h，并添加文本到当前的源文件中。下一行告诉 CPP 从本地目录中获取 **myheader.h**，并添加内容到当前的源文件中。

------

这些指令告诉 CPP 从**系统库**中获取 stdio.h，并添加文本到当前的源文件中。下一行告诉 CPP 从本地目录中获取 **myheader.h**，并添加内容到当前的源文件中。

```
#undef  FILE_SIZE
#define FILE_SIZE 42
```

这个指令告诉 CPP 取消已定义的 FILE_SIZE，并定义它为 42。

```
#ifndef MESSAGE
   #define MESSAGE "You wish!"
#endif
```

这个指令告诉 CPP 只有当 MESSAGE 未定义时，才定义 MESSAGE。

```
#ifdef DEBUG
   /* Your debugging statements here */
#endif
```

这个指令告诉 CPP 如果**定义了 DEBUG，则执行处理语句**。在编译时，如果您向 gcc 编译器传递了 *-DDEBUG* 开关量，这个指令就非常有用。它定义了 DEBUG，您可以在编译期间**随时开启或关闭调试**。

------

## 预定义宏

ANSI C 定义了许多宏。在编程中您可以使用这些宏，但是不能直接修改这些预定义的宏。

| 宏       | 描述                                                |
| :------- | :-------------------------------------------------- |
| __DATE__ | 当前日期，一个以 "MMM DD YYYY" 格式表示的字符常量。 |
| __TIME__ | 当前时间，一个以 "HH:MM:SS" 格式表示的字符常量。    |
| __FILE__ | 这会包含当前文件名，一个字符串常量。                |
| __LINE__ | 这会包含当前行号，一个十进制常量。                  |
| __STDC__ | 当编译器以 ANSI 标准编译时，则定义为 1。            |

## 预处理器运算符

C 预处理器提供了下列的运算符来帮助您创建宏：

### 宏延续运算符（\）

一个宏通常写在一个单行上。但是如果宏太长，一个单行容纳不下，则使用宏延续运算符（\）。例如：

```
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")
```

### 字符串常量化运算符（#）

在宏定义中，当需要把一个宏的参数转换为字符串常量时，则使用字符串常量化运算符（#）。在宏中使用的该运算符有一个特定的参数或参数列表。例如：

#### 实例

```
#include <stdio.h>
 
#define  message_for(a, b)  \
    printf(#a " and " #b ": We love you!\n")
 
int main(void)
{
   message_for(Carole, Debra);
   return 0;
}
```

### 标记粘贴运算符（##）

宏定义内的标记粘贴运算符（##）会合并两个参数。它允许在宏定义中两个独立的标记被合并为一个标记。例如：

#### 实例

```
#include <stdio.h>
 
#define tokenpaster(n) printf ("token" #n " = %d", token##n)
 
int main(void)
{
   int token34 = 40;
   
   tokenpaster(34);
   return 0;
}
```

当上面的代码被编译和执行时，它会产生下列结果：

```
token34 = 40
```

这是怎么发生的，因为这个实例会从编译器产生下列的实际输出：

```
printf ("token34 = %d", token34);
```

这个实例演示了 token##n 会连接到 token34 中，在这里，我们使用了**字符串常量化运算符（#）**和**标记粘贴运算符（##）**。

### defined() 运算符

预处理器 **defined** 运算符是用在常量表达式中的，用来确定一个标识符是否已经使用 #define 定义过。如果指定的标识符已定义，则值为真（非零）。如果指定的标识符未定义，则值为假（零）。下面的实例演示了 defined() 运算符的用法：

#### 实例

```
#include <stdio.h>
 
#if !defined (MESSAGE)
   #define MESSAGE "You wish!"
#endif
 
int main(void)
{
   printf("Here is the message: %s\n", MESSAGE);  
   return 0;
}
```

使用#define含参时，参数括号很重要，如上例中省略括号会导致运算错误：

```
#include <stdio.h>

#define square(x) ((x) * (x))

#define square_1(x) (x * x)

int main(void)
{
   printf("square 5+4 is %d\n", square(5+4));  
   printf("square_1 5+4 is %d\n", square_1(5+4)); 
   return 0;
}
```

输出结果为：

```
square 5+4 is 81
square_1 5+4 is 29
```

原因:

```
square   等价于   （5+4）*（5+4）=81
square_1 等价于   5+4*5+4=29
```

------

# 关于两数值的交换

异或运算可以达到交换两数的目的，代码如下:

```
void swap(int &a, int &b)
{
    a = a^b;
    b = a^b;
    a = a^b;
}
```

**但不推荐使用这种方式**，附上常用的临时变量方法对比说明。

临时变量方法:

```
void swap(int &a, int &b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```

对于临时变量法，每次赋值只要读取一个变量的值到寄存器，然后再从寄存器写回到另一个变量中即可，前后涉及两次内存写入操作； 但是对于异或运算操作，每次都需要读取两个数据到寄存器中，再进行运算操作，之后把结果写回到变量中，前后共需要三次内存写入操作。 另外一点，异或操作的代码可读性差。

如果使用C语言实现上述两种方法，并用gcc编译器编译，可以使用命令 **gcc -S swap.c** 查看相应的汇编代码，临时变量法代码行数更少，另外使用 **gcc** 编译器时，用异或运算交换数组会出错。

在不引入临时变量的基础上，交换两数的值还可以使用三次加减法，代码如下:

```
void swap(int &a, int &b)
{
    a = a + b;
    b = a - b;
    a = a - b;
}
```

这种方式同样需要三次内存写入操作，同时代码可读性也较差。

# C 头文件

**C 语言中 include <> 与include "" 的区别?**

**#include < >** 引用的是编译器的类库路径里面的头文件。

**#include " "** 引用的是你程序目录的相对路径中的头文件，如果在程序目录没有找到引用的头文件则到编译器的类库路径的目录下找该头文件。

## 只引用一次头文件

如果一个头文件被引用两次，编译器会处理两次头文件的内容，这将产生错误。为了防止这种情况，标准的做法是把文件的整个内容放在条件编译语句中，如下：

```
#ifndef HEADER_FILE
#define HEADER_FILE

the entire header file file

#endif
```

这种结构就是通常所说的包装器 **#ifndef**。当再次引用头文件时，条件为假，因为 HEADER_FILE 已定义。此时，预处理器会跳过文件的整个内容，编译器会忽略它。

在有多个 **.h** 文件和多个 **.c** 文件的时候，往往我们会用一个 **global.h** 的头文件来包括所有的 **.h** 文件，然后在除 **global.h** 文件外的头文件中 包含 **global.h** 就可以实现所有头文件的包含，同时不会乱。方便在各个文件里面调用其他文件的函数或者变量。

```
#ifndef _GLOBAL_H
#define _GLOBAL_H
#include <fstream>
#include <iostream>
#include <math.h>
#include <Config.h>
```

