---
title: 第十天 c++
date: 2023-04-12 14:36:10
tags:
---

### find_if

函数原型:
find_if(iterator beg，iterator end，_Pred);

- //按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
- // beg 开始迭代器
- // end结束迭代器
- //_Pred 函数或者谓词(返回bool类型的仿函数)

### adjacent_find

功能描述:

- 查找相邻重复元素

函数原型:
adjacent_find(iterator beg， iterator end) ;

- //查找相邻重复元素,返回相邻元素的第一个位置的迭代器
- // beg开始迭代器
- // end结束迭代器

总结:面试题中如果出现查找相邻重复元素，记得用STL中的adjacent_find算法

### binary_search

功能描述:

- 查找指定元素是否存在,二分法

函数原型:
bool binary_search(iterator beg, iterator end，value);

- //查找指定的元素，查到返回true否则false
- //注意:在无序序列中不可用
- // beg开始迭代器
- // end结束迭代器
- // value查找的元素

### count

功能描述:

- 统计元素个数

函数原型:
count(iterator beg, iterator end，value) ;

- //统计元素出现次数
- //beg 开始迭代器
- //end结束迭代器
- //value统计的元素

自定义数据类型与find方法一致

### count_if 

函数原型:
count_if(iterator beg, iterator end，_Pred ) ;

- //按条件统计元素出现次数
- // beg开始迭代器
- //end结束迭代器
- // _Pred谓词

## 常用排序算法

- sort	//对容器内元素进行排序
- random_shuffle	//洗牌指定范围内的元素随机调整次序
- merge	//容器元素合并，并存储到另一容器中
- reverse	//反转指定范围的元素

函数原型:
sort(iterator beg, iterator end，_Pred) ;

- //按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
- //beg开始迭代器
- // end结束迭代器
- // _Pred 谓词

```
//从大到小排序,没有pred默认是从小到大
sort(v.begin(), v.end (), greater<int>());// greater<int>()头文件在functional
```

函数原型:

random_shuffle(iterator beg, iterator end) ;

- //指定范围内的元素随机调整次序
- // beg 开始迭代器
- // end结束迭代器

总结:random_shuffle洗牌算法比较实用，使用时记得加随机数种子（srand((unsigned int)time(NULL)) ;）

- 函数原型:
  merge(iterator beg1，iterator end1， iterator beg2，iterator end2， iterator dest);
- - //容器元素合并，并存储到另一容器中
  - // 注意:两个容器必须是有序的
  - // beg1容器1开始迭代器
  - // end1容器1结束迭代器
  - // beg2容器2开始迭代器
  - // end2容器2结束迭代器
  - // dest目标容器开始迭代器

函数原型:
reverse(iterator beg, iterator end );

- //反转指定范围的元素
- // beg开始迭代器
- // end结束迭代器

## 常用拷贝和替换算法

算法简介:

- copy	//容器内指定范围的元素拷贝到另—容器中
- replace	//将容器内指定范围的旧元素修改为新元素
- replace_if	//容器内指定范围满足条件的元素替换为新元素
- swap	//互换两个容器的元素

函数原型:
copy(iterator beg, iterator end，iterator dest) ;

- //按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置
- //beg开始迭代器
- // end结束迭代器
- //dest目标容器开始迭代器

函数原型:
replace(iterator beg, iterator end，oldvalue,newvalue) ;

- //将区间内旧元素替换成新元素
- //beg开始迭代器
- // end结束迭代器
- //oldvalue旧元素
- //newvalue新元素

函数原型:
replace_if(iterator beg, iterator end，_pred,newvalue) ;

- //按条件替换元素，满足条件的替换成指定元素
- //beg开始迭代器
- // end结束迭代器
- //_pred谓词
- //newvalue替换的新元素

函数原型:
swap(container c1, container c2);

//互换两个相同类型容器的元素
// c1容器1
// c2容器2

## 常用算术生成算法

注意:
算术生成算法属于小型算法，使用时包含的头文件为#include <numeric>

算法简介:

- accumplate	//计算容器元素累计总和
- fill	//向容器中添加元素

函数原型:
accumplate(iterator beg, iterator end，value) ;

- //计算容器元素累计总和
- //beg开始迭代器
- //end结束迭代器
- //value起始值

函数原型:
fill(iterator beg, iterator end，value) ;

- //向容器中填充元素
- //beg开始迭代器
- //end结束迭代器
- //value填充的值

## 常用集合算法

算法简介:

- set_intersection	//求两个容器的交集
- set_union	//求两个容器的并集
- set_difference	//求两个容器的差集

函数原型:
set_intersection(iterator beg1，iterator end1， iterator beg2，iterator end2， iterator dest);

- //求两个集合的交集
- // 注意:两个容器必须是有序的
- // beg1容器1开始迭代器
- // end1容器1结束迭代器
- // beg2容器2开始迭代器
- // end2容器2结束迭代器
- // dest目标容器开始迭代器
- //set_intersection返回值既是交集中最后一个元素的位置

```
//最特殊情况大容器包含小容器开辟空间取小容器的size即可vTarget.resize(mih(v1.size(), v2.size())) ;
```

函数原型:
set_union(iterator beg1，iterator end1， iterator beg2，iterator end2， iterator dest);

- //求两个集合的并集
- // 注意:两个容器必须是有序的
- // beg1容器1开始迭代器
- // end1容器1结束迭代器
- // beg2容器2开始迭代器
- // end2容器2结束迭代器
- // dest目标容器开始迭代器

函数原型:
set_difference(iterator beg1，iterator end1， iterator beg2，iterator end2， iterator dest);

- //求两个集合的差集
- // 注意:两个容器必须是有序的
- // beg1容器1开始迭代器
- // end1容器1结束迭代器
- // beg2容器2开始迭代器
- // end2容器2结束迭代器
- // dest目标容器开始迭代器

总结:

- 求差集的两个集合必须的有序序列
- 目标容器开辟空间需要从两个容器取较大值
- set_difference返回值既是差集中最后一个元素的位置
