---
title: 第九天 c语言
date: 2023-03-23 14:43:06
tags:
---

# C 内存管理

C 语言为内存的分配和管理提供了几个函数。这些函数可以在 **<stdlib.h>** 头文件中找到。

1.stdlib.h中的几个函数操作内存；calloc(）分配指定个数指定大小的连续内存块，返回值是这些连续内存块组成的大内存块地址；malloc()分配指定大小的一块内存，返回值是内存的地址；realloc()通过已分配的内存块的地址扩展或者减小内存的大小；free()释放指定地址对应的内存块，无返回值；alloc是allocate 分配的缩写；malloc mess+allocate 整块的分配

2.stdlib中几个内存管理的函数返回值是 void *,表示任意类型的指针，或者说它可以转化成任意类型；

3.内存管理的意义：数组、基本数据类型、结构体、共用体都是固定的为数据分配内存空间，而内存管理却可以直接申请一块内存，然后给其指定存储的数据类型，之后就可以存储数据了，且还可以根据数据的大小来扩展内存空间；

------

malloc与calloc没有本质区别，malloc之后的未初始化内存可以使用memset进行初始化。

主要的不同是malloc不初始化分配的内存，calloc初始化已分配的内存为0。

次要的不同是calloc返回的是一个数组，而malloc返回的是一个对象。

calloc等于malloc后再memset，所以malloc比calloc更高效。

分配内存空间函数malloc 调用形式: (类型说明符*) malloc (size) 。

分配内存空间函数 calloc calloc 也用于分配内存空间。

为什么多用malloc而很少用calloc？

因为calloc虽然对内存进行了初始化（全部初始化为0），

calloc相当于

p = malloc();

memset(p, 0,size);(C 库函数 **void \*memset(void \*str, int c, size_t n)** 复制字符 **c**（一个无符号字符）到参数 **str** 所指向的字符串的前 **n** 个字符。)

相对于malloc多了对内存的写零操作，而写零这个操作我们有时候需要，而大部分时间不需要。

------



| 序号 | 函数和描述                                                   |
| :--- | :----------------------------------------------------------- |
| 1    | **void \*calloc(int num, int size);** 在内存中动态地分配 num 个长度为 size 的连续空间，并将每一个字节都初始化为 0。所以它的结果是分配了 num*size 个字节长度的内存空间，并且每个字节的值都是 0。 |
| 2    | **void free(void \*address);** 该函数释放 address 所指向的内存块,释放的是动态分配的内存空间。 |
| 3    | **void \*malloc(int num);** 在堆区分配一块指定大小的内存空间，用来存放数据。这块内存空间在函数执行完成后不会被初始化，它们的值是未知的。 |
| 4    | **void \*realloc(void \*address, int newsize);** 该函数重新分配内存，把内存扩展到 **newsize**。 |

直接使用原来的指针变量接收 **realloc** 的返回值是可能存在内存泄漏的。例如以下语句：

```
description = (char *) realloc( description, 100 * sizeof(char) );
```

若 realloc 函数执行失败，description 原先所指向的空间不变，realloc 函数返回 NULL。

此时 description 的值被赋为 NULL, 但原先指向的空间未被释放，造成了内存泄漏。（内存泄漏 （Memory leak）是在计算机科学中，由于疏忽或错误造成程序未能释放已经不再使用的内存 。 内存泄漏并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，导致在释放该段内存之前就失去了对该段内存的控制，从而造成了内存的浪费。 内存泄漏通常情况下只能由获得程序源代码的程序员才能分析出来。）

------

**注意：**void * 类型表示未确定类型的指针。C、C++ 规定 void * 类型可以通过类型转换强制转换为任何其它类型的指针。

**注：**void 指针可以任意类型的数据，可以在程序中给我们带来一些好处，函数中形为指针类型时，我们可以将其定义为 void 指针，这样函数就可以接受任意类型的指针。

void 指针可以指向任意类型的数据，就是说可以用任意类型的指针对 void 指针对 void 指针赋值。例如：

```
int *a；
void *p；
p=a；
```

如果要将 void 指针 p 赋给其他类型的指针，则需要强制类型转换，就本例而言：**a=（int \*）p**。

对于 void 指针，GNU 认为 **void \*** 和 **char \*** 一样，所以以下写法是正确的:

```
description = malloc( 200 * sizeof(char) );
```

但按照 ANSI(American National Standards Institute) 标准，需要对 void 指针进行强制转换，如下:

```
description = (char *)malloc( 200 * sizeof(char) );
```

同时，按照 ANSI(American National Standards Institute) 标准，不能对 void 指针进行算法操作:

```
void * pvoid;
pvoid++; //ANSI：错误
pvoid += 1; //ANSI：错误
// ANSI标准之所以这样认定，是因为它坚持：进行算法操作的指针必须是确定知道其指向数据类型大小的。

int *pint;
pint++; //ANSI：正确
```

在实际的程序设计中，为迎合 ANSI 标准，并提高程序的可移植性，我们可以这样编写实现同样功能的代码：

```
void * pvoid;
((char *)pvoid)++; //ANSI：错误；GNU：正确
(char *)pvoid += 1; //ANSI：错误；GNU：正确
```

## 动态分配内存

预先不知道需要存储的文本长度，例如您想存储有关一个主题的详细描述。在这里，我们需要定义一个指针，该指针指向未定义所需内存大小的字符，后续再根据需求来分配内存，如下所示：

## 实例

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
int main()
{
   char name[100];
   char *description;
 
   strcpy(name, "Zara Ali");
 
   /* 动态分配内存 */
   description = (char *)malloc( 200 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcpy( description, "Zara ali a DPS student in class 10th");
   }
   printf("Name = %s\n", name );
   printf("Description: %s\n", description );
}
```

上面的程序也可以使用 **calloc()** 来编写，只需要把 malloc 替换为 calloc 即可，如下所示：

```
calloc(200, sizeof(char));
```

当动态分配内存时，您有完全控制权，可以传递任何大小的值。而那些预先定义了大小的数组，一旦定义则无法改变大小。

## 重新调整内存的大小和释放内存

当程序退出时，操作系统会自动释放所有分配给程序的内存，但是，建议您在不需要内存时，都应该调用函数 **free()** 来释放内存。

或者，您可以通过调用函数 **realloc()** 来增加或减少已分配的内存块的大小。让我们使用 realloc() 和 free() 函数，再次查看上面的实例：

```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
 
int main()
{
   char name[100];
   char *description;
 
   strcpy(name, "Zara Ali");
 
   /* 动态分配内存 */
   description = (char *)malloc( 30 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcpy( description, "Zara ali a DPS student.");
   }
   /* 假设您想要存储更大的描述信息 */
   description = (char *) realloc( description, 100 * sizeof(char) );
   if( description == NULL )
   {
      fprintf(stderr, "Error - unable to allocate required memory\n");
   }
   else
   {
      strcat( description, "She is in class 10th");
   }
   
   printf("Name = %s\n", name );
   printf("Description: %s\n", description );
 
   /* 使用 free() 函数释放内存 */
   free(description);
}
```

尝试一下不重新分配额外的内存，strcat() 函数会生成一个错误，因为存储 description 时可用的内存不足。

------

动态可变长的结构体：

```
typedef struct
{
  int id;
  char name[0];
}stu_t;
```

定义该结构体，只占用4字节的内存，name不占用内存。

```
stu_t *s = NULL;    //定义一个结构体指针
s = malloc(sizeof(*s) + 100);//sizeof(*s)获取的是4，但加上了100，4字节给id成员使用，100字节是属于name成员的
s->id = 1010;
strcpy(s->name,"hello");
```

注意：一个结构体中只能有一个可变长的成员，并且该成员必须是最后一个成员。

### C 语言中常用的内存管理函数和运算符

- malloc() 函数：用于动态分配内存。它接受一个参数，即需要分配的内存大小（以字节为单位），并返回一个指向分配内存的指针。
- free() 函数：用于释放先前分配的内存。它接受一个指向要释放内存的指针作为参数，并将该内存标记为未使用状态。
- calloc() 函数：用于动态分配内存，并将其初始化为零。它接受两个参数，即需要分配的内存块数和每个内存块的大小（以字节为单位），并返回一个指向分配内存的指针。
- realloc() 函数：用于重新分配内存。它接受两个参数，即一个先前分配的指针和一个新的内存大小，然后尝试重新调整先前分配的内存块的大小。如果调整成功，它将返回一个指向重新分配内存的指针，否则返回一个空指针。
- sizeof 运算符：用于获取数据类型或变量的大小（以字节为单位）。
- 指针运算符：用于获取指针所指向的内存地址或变量的值。
- & 运算符：用于获取变量的内存地址。
- ***** 运算符：用于获取指针所指向的变量的值。
- **->** 运算符：用于指针访问结构体成员，语法为 **pointer->member**，等价于 **(\*pointer).member**。
- memcpy() 函数：用于从源内存区域复制数据到目标内存区域。它接受三个参数，即目标内存区域的指针、源内存区域的指针和要复制的数据大小（以字节为单位）。**void \*memcpy(void \*str1, const void \*str2, size_t n)** 从存储区 **str2** 复制 **n** 个字节到存储区 **str1**。该函数返回一个指向目标存储区 str1 的指针。
- memmove() 函数：类似于 memcpy() 函数，但它可以处理重叠的内存区域。它接受三个参数，即目标内存区域的指针、源内存区域的指针和要复制的数据大小（以字节为单位）。**void \*memmove(void \*str1, const void \*str2, size_t n)** 从 **str2** 复制 **n** 个字符到 **str1**，但是在重叠内存块这方面，memmove() 是比 memcpy() 更安全的方法。如果目标区域和源区域有重叠的话，memmove() 能够保证源串在被覆盖之前将重叠区域的字节拷贝到目标区域中，复制后源区域的内容会被更改。如果目标区域与源区域没有重叠，则和 memcpy() 函数功能相同。

任何类型的指针都可以传入 memcpy 和 memset 中

------

对于我们手动分配的内存，在 C 语言中是不用强制转换类型的。

```
description = malloc( 200 * sizeof(char) ); // C 语言正确。
description = malloc( 200 * sizeof(char) ); // C++ 错误
```

但是 C++ 是强制要求的，不然会报错。

------

