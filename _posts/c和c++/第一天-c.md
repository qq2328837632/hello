---
title: 第一天 c++
date: 2023-03-29 11:21:09
tags:
---

# 函数的分文件编写

1、创建后缀名为.h的头文件
2、创建后缀名为.cpp的源文件
3、在头文件中写函数声明
4、在源文件中写函数的定义

示例：（求最大值）
1、创建后缀名为.h的头文件

```c
//max.h
#include<iostream>
using namespace std;
//3、函数声明
int max(int a, int b);
```

2、创建后缀名为.cpp的源文件

```c
//max.cpp
#include"max.h"//" "表示是我们自己定义的头文件
//4、函数的定义
int max(int a, int b)
{
	return a > b ? a : b;//三目运算符，返回最大值
}
```

`max.cpp`中包含我们自己定义的头文件`"max.h"`说明其与`"max.h"`是配套的。

```c
#include"max.h"
int main()
{
	int a = 10;
	int b = 20;
	max(a, b);
 	system("pause");
 	return 0;
}

```

main函数中也只需要引用我们自己定义的`"max.h"`即可
就不用再写

```c
#include<iostream>
using namespace std;
```

因为其已在`"max.h"`头文件中包含了

最后在注意一点，一个项目中只能有一个main函数！！！

------

总结：

1. 创建后缀名为.h的头文件，在头文件中写函数的声明，也就是我们自己封装好的函数的各个函数名。
2. 创建后缀名为.cpp的源文件，在源文件中写函数的定义，也就是在源文件中封装代表各个功能的函数，方便调用。
3. 创建主函数（main)文件，调用我们想用的各个封装函数，实现功能。

在终端运行mingw(按tab补全)，得到结果，注意，千万不要用右上角的run code执行！

再用![image-20230329213057548](C:\Users\86191\AppData\Roaming\Typora\typora-user-images\image-20230329213057548.png)

如果运行后显示中文乱码可以试试下面的代码

```cpp
#include<iostream>
#include <windows.h>//用于函数SetConsoleOutputCP(65001);更改cmd编码为utf8
using namespace std;

int main()
{
	SetConsoleOutputCP(65001);
	cout<<"中文正常显示"<<endl;
	system("pause");
    return 0;
}
```

每次如此运行后需要更改代码需要重新此步骤
