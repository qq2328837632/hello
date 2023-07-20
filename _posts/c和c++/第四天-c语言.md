---
title: 第四天 c语言
date: 2023-03-18 14:53:58
tags:
---

# 回调函数

## 函数指针作为某个函数的参数

函数指针变量可以作为某个函数的参数来使用的，回调函数就是一个通过函数指针调用的函数。

简单讲：回调函数是由别人的函数执行时调用你实现的函数。

> 以下是来自知乎作者常溪玲的解说：
>
> 你到一个商店买东西，刚好你要的东西没有货，于是你在店员那里留下了你的电话，过了几天店里有货了，店员就打了你的电话，然后你接到电话后就到店里去取了货。在这个例子里，你的电话号码就叫回调函数，你把电话留给店员就叫登记回调函数，店里后来有货了叫做触发了回调关联的事件，店员给你打电话叫做调用回调函数，你到店里去取货叫做响应回调事件。

### 实例

实例中 **populate_array()** 函数定义了三个参数，其中第三个参数是函数的指针，通过该函数来设置数组的值。

实例中我们定义了回调函数 **getNextRandomValue()**，它返回一个随机值，它作为一个函数指针传递给 **populate_array()** 函数。

**populate_array()** 将调用 **10** 次回调函数，并将回调函数的返回值赋值给数组。

**回调函数最大的意义在于解耦，降低了代码之间的耦合度。**

## 实例

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

size_t 类型在C语言标准库函数原型使用的很多，数值范围一般是要大于int和unsigned.

但凡不涉及负值范围的表示size取值的，都可以用size_t；比如array[size_t]。

size_t 在stddef.h头文件中定义。

在其他常见的宏定义以及函数中常用到有：

1，sizeof运算符返回的结果是size_t类型；

2，void *malloc(size_t size)...

------

# C字符串

字符串在以如下输入进行初始化的时候需要对 **\0** 特别注意：

```
char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'};
```

如果没有在字符数组最后增加 **\0** 的话输出结果有误：

```
// 初始化字符串
char greeting[5] = { 'H', 'e', 'l', 'l', 'o' };
printf("Greeting message: %s\n", greeting);
```

输出结果:

```
Greeting message: Hello烫烫烫?侵7(?╔?╚╔╔
```

在使用不定长数组初始化字符串时默认结尾为 **\0**

```
char greeting[] = "Hello";
printf("Greeting message: %s, greeting[] Length: %d\n", greeting, sizeof(greeting));
```

输出结果:

```
Greeting message: Hello, greeting[] Length: 6
```

**结论：**需在给定字符数组的大小时在原有的字符串的字符数上加 1。

------

**strlen 与 sizeof的区别：**

strlen 是函数，sizeof 是运算操作符，二者得到的结果类型为 size_t，即 unsigned int 类型。

sizeof 计算的是变量的大小，不受字符 **\0** 影响；

sizeof() 计算字符串的长度，包含末尾的 '\0';

##### 实例

```
char s[] = "Hello, world!";
sizeof(s) ;// 输出 14，即字符串 s 中有 14 个字符（包括结尾的空字符 '\0'）
```

而 strlen 计算的是字符串的长度，以 **\0** 作为长度判定依据。strlen() 函数只能用于计算以空字符 '\0' 结尾的字符串的长度，如果字符串中没有空字符，则 strlen() 函数的行为是未定义的.

其中 string 是一个以空字符 '\0' 结尾的字符串，但是计算字符串的长度，不包含末尾的 '\0'。例如：

##### 实例

```
char s[] = "Hello, world!";
strlen(s) ;// 输出 13，即字符串 s 中有 13 个字符（不包括结尾的空字符 '\0'）
```

sizeof 可以用类型做参数，**strlen** 只能用 **char\*** 做参数，且必须是以 **\0** 结尾的。

sizeof 还可以用函数做参数，比如：

```
short f();
printf("%d\n", sizeof(f()));
```

输出的结果是 sizeof(short)，即 2。

数组做 **sizeof** 的参数不退化，传递给 **strlen** 就退化为指针了。

大部分编译程序在编译的时候就把 **sizeof** 计算过了，是类型或是变量的长度，这就是 **sizeof(x)** 可以用来定义数组维数的原因。

```
char str[20]="0123456789";
int a=strlen(str); // a=10;
int b=sizeof(str); // 而 b=20;
```

strlen 的结果要在运行的时候才能计算出来，是用来计算字符串的长度，不是类型占内存的大小。

数组作为参数传给函数时传的是指针而不是数组，传递的是数组的首地址， 如：

```
fun(char [8])
fun(char [])
```

都等价于

```
fun(char *) 
```

在 C++ 里参数传递数组永远都是传递指向数组首元素的指针，编译器不知道数组的大小。

------

**'a'** 表示是一个字符，**"a"** 表示一个字符串相当于 **'a'+'\0'**;

**''** 里面只能放一个字符;

**""** 里面表示是字符串系统自动会在串末尾补一个 0。

------

# C结构体

结构体定义由关键字 **struct** 和结构体名组成，结构体名可以根据需要自行定义。

struct 语句定义了一个包含多个成员的新的数据类型，struct 语句的格式如下：

```
struct tag { 
    member-list
    member-list 
    member-list  
    ...
} variable-list ;
```

在一般情况下，**tag、member-list、variable-list** 这 3 部分至少要出现 2 个。以下为实例：

```
在一般情况下，tag、member-list、variable-list 这 3 部分至少要出现 2 个。以下为实例：

//此声明声明了拥有3个成员的结构体，分别为整型的a，字符型的b和双精度的c
//同时又声明了结构体变量s1
//这个结构体并没有标明其标签
struct 
{
    int a;
    char b;
    double c;
} s1;
 
//此声明声明了拥有3个成员的结构体，分别为整型的a，字符型的b和双精度的c
//结构体的标签被命名为SIMPLE,没有声明变量
struct SIMPLE
{
    int a;
    char b;
    double c;
};
//用SIMPLE标签的结构体，另外声明了变量t1、t2、t3
struct SIMPLE t1, t2[20], *t3;
 
//也可以用typedef创建新类型
typedef struct
{
    int a;
    char b;
    double c; 
} Simple2;
//现在可以用Simple2作为类型声明新的结构体变量
Simple2 u1, u2[20], *u3;
在上面的声明中，第一个和第二声明被编译器当作两个完全不同的类型，即使他们的成员列表是一样的，如果令 t3=&s1，则是非法的。

结构体的成员可以包含其他结构体，也可以包含指向自己结构体类型的指针，而通常这种指针的应用是为了实现一些更高级的数据结构如链表和树等。

//此结构体的声明包含了其他的结构体
struct COMPLEX
{
    char string[100];
    struct SIMPLE a;
};
 
//此结构体的声明包含了指向自己类型的指针
struct NODE
{
    char string[100];
    struct NODE *next_node;
};
如果两个结构体互相包含，则需要对其中一个结构体进行不完整声明，如下所示：

struct B;    //对结构体B进行不完整声明
 
//结构体A中包含指向结构体B的指针
struct A
{
    struct B *partner;
    //other members;
};
 
//结构体B中包含指向结构体A的指针，在A声明完后，B也随之进行声明
struct B
{
    struct A *partner;
    //other members;
};

```

在上面的声明中，第一个和第二声明被编译器当作两个完全不同的类型，即使他们的成员列表是一样的，如果令 t3=&s1，则是非法的。

结构体的成员可以包含其他结构体，也可以包含指向自己结构体类型的指针，而通常这种指针的应用是为了实现一些更高级的数据结构如链表和树等。

```
//此结构体的声明包含了其他的结构体
struct COMPLEX
{
    char string[100];
    struct SIMPLE a;
};
 
//此结构体的声明包含了指向自己类型的指针
struct NODE
{
    char string[100];
    struct NODE *next_node;
};
```

如果两个结构体互相包含，则需要对其中一个结构体进行不完整声明，如下所示：

```
struct B;    //对结构体B进行不完整声明
 
//结构体A中包含指向结构体B的指针
struct A
{
    struct B *partner;
    //other members;
};
 
//结构体B中包含指向结构体A的指针，在A声明完后，B也随之进行声明
struct B
{
    struct A *partner;
    //other members;
};
```

## 指向结构的指针

您可以定义指向结构的指针，方式与定义指向其他类型变量的指针相似，如下所示：

```
struct Books *struct_pointer;
```

现在，您可以在上述定义的指针变量中存储结构变量的地址。为了查找结构变量的地址，请把 & 运算符放在结构名称的前面，如下所示：

```
struct_pointer = &Book1;
```

为了使用指向该结构的指针访问结构的成员，您必须使用 -> 运算符，如下所示：

```
struct_pointer->title;
```

让我们使用结构指针来重写上面的实例，这将有助于您理解结构指针的概念：

### 实例

```
#include <stdio.h>
#include <string.h>
 
struct Books
{
   char  title[50];
   char  author[50];
   char  subject[100];
   int   book_id;
};
 
/* 函数声明 */
void printBook( struct Books *book );
int main( )
{
   struct Books Book1;        /* 声明 Book1，类型为 Books */
   struct Books Book2;        /* 声明 Book2，类型为 Books */
 
   /* Book1 详述 */
   strcpy( Book1.title, "C Programming");
   strcpy( Book1.author, "Nuha Ali"); 
   strcpy( Book1.subject, "C Programming Tutorial");
   Book1.book_id = 6495407;
 
   /* Book2 详述 */
   strcpy( Book2.title, "Telecom Billing");
   strcpy( Book2.author, "Zara Ali");
   strcpy( Book2.subject, "Telecom Billing Tutorial");
   Book2.book_id = 6495700;
 
   /* 通过传 Book1 的地址来输出 Book1 信息 */
   printBook( &Book1 );
 
   /* 通过传 Book2 的地址来输出 Book2 信息 */
   printBook( &Book2 );
 
   return 0;
}
void printBook( struct Books *book )
{
   printf( "Book title : %s\n", book->title);
   printf( "Book author : %s\n", book->author);
   printf( "Book subject : %s\n", book->subject);
   printf( "Book book_id : %d\n", book->book_id);
}
```

------

**struct内存原则：** 从上至下进行内存分配，对齐方式以**当前分配到的**内部成员类型**最宽字节数**为基准；整体以结构体成员最宽类型字节为基准，且整个结构体的总大小为最宽基本类型成员大小的整数倍。

1. struct A 内存分配

来看一下结构体AA的内存分配：编译器首先将变量 char a 分配一个字节，并以1个字节为对齐基准；
接着对 short b 进行内存分配，此时发现b为两个字节，则以b为基准对齐，此时 a 需要补充一个字节内存，a、b 总分配内存为4个字节；
接着对 int c 进行内存分配，发现c占4个字节，则以4个字节为基准对齐，因为之前 a、b 内存共计为4个字节刚好与“4个字节的基准”一致，则此时 struct A 共计8个字节内存。
struct A { char a; short b; int c; };

结构体中成员变量分配的空间是按照成员变量中占用空间最大的来作为分配单位,同样成员变量的存储空间也是不能跨分配单位的,如果当前的空间不足,则会存储到下一个分配单位中。

```
#include <stdio.h>

typedef struct
{
    unsigned char a;
    unsigned int  b;
    unsigned char c;
} debug_size1_t;
typedef struct
{
    unsigned char a;
    unsigned char b;
    unsigned int  c;
} debug_size2_t;

int main(void)
{
    printf("debug_size1_t size=%lu,debug_size2_t size=%lu\r\n", sizeof(debug_size1_t), sizeof(debug_size2_t));
    return 0;
}
```

编译执行输出结果：

```
debug_size1_t size=12,debug_size2_t size=8
```

结构体占用存储空间,以32位机为例

-  1.debug_size1_t 存储空间分布为a(1byte)+空闲(3byte)+b(4byte)+c(1byte)+空闲(3byte)=12(byte)。

-  1.debug_size2_t 存储空间分布为a(1byte)+b(1byte)+空闲(2byte)+c(4byte)=8(byte)。

  ------

  ```原版
  #include <stdio.h>
  
  int main(void)
  {
      struct Student {
          char name[50];
          int gender;
          int age;
      } student2 = {"张三",0,30};
      struct Student student1;
      printf("name:\n");
      scanf("%s",student1.name);
      printf("gender:\n");
      scanf("%d",&student1.gender);
      printf("age:\n");
      scanf("%d",&student1.age);
      printf("student1 >>name = %s, gender = %d, age = %d\n", student1.name, student1.gender, student1.age);
      printf("student2 >>name = %s, gender = %d, age = %d\n", student2.name, student2.gender, student2.age);
  }
  ```

  

```改良
#include <stdio.h>

int main(void)
{
    struct Student {
        char name[100];
        int gender;
        int age;
    } student2 = {"0",0,1};//char数组初始化如果是'0'，age会无法初始化。中文占两个字节，而char类型是一个字节长度的；因此，它的长度与char类型数据并不符合。这也就意味着我们不能像char类型一样简单操作中文字符
   
    struct Student student1;
    printf("name:\n");
    scanf("%s",&student1.name);
    printf("gender:\n");
    scanf("%d",&student1.gender);
    printf("age:\n");
    scanf("%d",&student1.age);
    printf("student2 >>name:\n");
    scanf("%s",&student2.name);//阻止了汉字乱码
    printf("student1 >>name = %s, gender = %d, age = %d\n", student1.name, student1.gender, student1.age);
    printf("student2 >>name = %s, gender = %d, age = %d\n", student2.name, student2.gender, student2.age);
}
```

------

## 结构体数组

一个结构体变量中可以存放一组数据（如一个学生的学号，姓名，成绩等数据）。如果有10个学生的数据需要参加运算，显然应该用数组，这就是结构体数组。结构体数组与以前介绍过的数据值型数组不同之处在于每个数组元素都一个结构体类型的数据，它们分别包括各个成员（分量）项。

### 定义结构体数组

和定义结构体变量的方法相仿，只需说明其为数组即可。

```
struct student
{
    int num;
    char name[20];
    char sex;
    int age;
    float score;
    char addr[30];
};
struct student stu[3];
```

------

