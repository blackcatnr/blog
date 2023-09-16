---
title: STL容器
date: 2023-09-16
tags: ['星海']
categories: 星海
id: STL容器
---
<!-- more -->
# STL容器
# 目录
  - [1.vector容器](#section.1)<a name="context.1"> </a>
      - [vector容器和普通数组的区别：](#section.2)<a name="context.2"> </a>
      - [vector的构造函数](#section.3)<a name="context.3"> </a>
      - [vector的赋值操作](#section.4)<a name="context.4"> </a>
      - [vector的容量与大小](#section.5)<a name="context.5"> </a>
      - [vector的插入和删除](#section.6)<a name="context.6"> </a>
      - [vector数据存取](#section.7)<a name="context.7"> </a>
      - [vector互换容器](#section.8)<a name="context.8"> </a>
      - [vector预留空间](#section.9)<a name="context.9"> </a>
  - [2.deque容器](#section.10)<a name="context.10"> </a>
      - [创建deque容器的几种方式：](#section.11)<a name="context.11"> </a>
      - [deque容器的函数](#section.12)<a name="context.12"> </a>
      - [assign函数使用](#section.13)<a name="context.13"> </a>
      - [insert员函数使用](#section.14)<a name="context.14"> </a>
      - [max_size员函数使用](#section.15)<a name="context.15"> </a>
      - [emplace_front()、emplace_back()函数使用](#section.16)<a name="context.16"> </a>
  - [3.List容器](#section.17)<a name="context.17"> </a>
  - [4.map容器](#section.18)<a name="context.18"> </a>
      - [不经常使用函数：](#section.19)<a name="context.19"> </a>
      - [map中的insert的使用](#section.20)<a name="context.20"> </a>
      - [1.map中的swap用法](#section.21)<a name="context.21"> </a>
      - [2.map中的sort问题](#section.22)<a name="context.22"> </a>
------------------------------------------------  

### [1.vector容器](#context.1)<a name="section.1"> </a>

功能：vector容器的功能和数组非常相似，使用时可以把它看成一个数组。

##### [vector容器和普通数组的区别：](#context.2)<a name="section.2"> </a>

1.数组是静态的，长度不可以改变，而vector可以动态扩展，增加长度。（注：==动态扩展并不是在原空间之后续接新空间，而是找到比原来更大的内存空间，将原数据拷贝到新空间，释放原空间==）
2.数组内数据通常存储在栈上，而vector中数据存储在堆上。

##### [vector的构造函数](#context.3)<a name="section.3"> </a>

函数原型：
1.vector<T>v;  //使用模板类，默认构造函数
2.vector(v.begin(),v.end());  //将(v.begin(),v.end())区间中的元素拷贝给本身
3.vector(n,elem);  //将n个elem拷贝给本身|
4.vector(const  vector &v);   //拷贝构造函数

##### [vector的赋值操作](#context.4)<a name="section.4"> </a>

函数原型：
1.vector&  operator=(const   vector &v); //重载赋值运算符
2.assign(v.begin(),v.end());   //将(v.begin(),v.end())区间中的元素赋值给本身
3.assign(n,elem);   //将n个elem赋值给本身

```c++
void text02()
{
	vector<int> v1,v2;
	for (int i = 0; i < 5; ++i)
	{
		v1.push_back(i);
	}
	v2 = v1;                        //调用1，赋值运算符重载
	vector<int> v3,v4;
	v3.assign(v1.begin(), v1.end());//调用2，区间赋值
	v4.assign(5, 9);                //调用3
	cout << "打印v2: ";
	printVector(v2);
	cout << "打印v3: ";
	printVector(v3);
	cout << "打印v4: ";
	printVector(v4);
}

```

<img src="https://img-blog.csdnimg.cn/690b220bfcd547e092c8d8061dfce699.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16" alt="在这里插入图片描述" style="zoom:80%;" />

##### [vector的容量与大小](#context.5)<a name="section.5"> </a>

函数原型：
1.empty();  //判断容器是否为空，为空返回1，否则返回0
2.capacity();  //返回容器的容量
3.size();     //返回容器的大小，即容器中元素的个数
4.resize(int num);  //重新指定容器的长度为num,若容器变长，则以默认值0填充新位置，如果容器变短，则末尾超过容器长度的元素被删除。
5.resize(int num, int  elem);   //重新指定容器的长度为num，若容器变长，则以elem填充新位置，如果容器变短，则末尾超过容器长度的元素被删除。

```c++
void text03()
{
	vector<int> v1;
	if (v1.empty())//调用1，如果容器为空，则给其赋值
	{
		for (int i = 0; i < 5; ++i)
		{
			v1.push_back(i);
		}
	}
	cout << "打印v1: ";
	printVector(v1);
	cout << "v1的容量为：" << v1.capacity() << endl;//调用2
	cout << "v1的大小为：" << v1.size() << endl;//调用3

	//重新指定容器大小使其变长
	v1.resize(10);       //调用4，增加的长度默认值为0
	cout << "调用4,增加长度后,打印v1: ";
	printVector(v1);
	v1.resize(15, 9);    //调用5，增加的长度赋值为9
	cout << "调用5,增加长度后,打印v1: ";
	printVector(v1);
	//重新指定容器大小使其变短
	v1.resize(10);       //调用4，删除了上一步中最后赋值为9的5个长度
	cout << "调用4,减小长度后,打印v1: ";
	printVector(v1);
	v1.resize(5, 9);     //调用5，删除了上一步中默认值为0的5个长度
	cout << "调用5,减小长度后,打印v1: ";
	printVector(v1);
}

```

<img src="https://img-blog.csdnimg.cn/da5bb81203234ad89e386e948fb6b7ee.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBA77yG5LiN6YCd,size_20,color_FFFFFF,t_70,g_se,x_16" alt="img" style="zoom:80%;" />

##### [vector的插入和删除](#context.6)<a name="section.6"> </a>

函数原型：
1.push_back(ele);   //尾部插入元素ele
2.pop_back();    //删除最后元素
3.insert(const_iterator pos,ele);  //在迭代器指向的位置pos处插入一个元素ele
4.insert(const_iterator pos,int count,ele);    //在迭代器指向的位置pos处插入count个元素ele
5.erase(const_iterator pos);   //删除迭代器指向的元素
6.erase(const_iterator begin,const_iterator end);  //删除迭代器从begin到end之间的元素
7.clear();   //删除容器中所有元素

```c++
void text04()
{
	vector<int> v1;
	for (int i = 0; i < 5; ++i)
	{
		v1.push_back(6);//调用1，尾部插入元素6
	}
	cout << "打印v1: ";
	printVector(v1);
	v1.pop_back();//调用2，删除最后一个元素
	cout << "调用2，删除最后一个元素后，打印v1: ";
	printVector(v1);
	v1.insert(v1.begin(),20);//调用3，在首位插入20
	cout << "调用3，在首位插入20，打印v1: ";
	printVector(v1);
	v1.insert(v1.end(), 3, 20);//调用4，在尾部插入3个20
	cout << "调用4，在尾部插入3个20，打印v1: ";
	printVector(v1);
	v1.erase(v1.begin()); //调用5，在首位删除一个元素
	cout << "调用5，在首位删除一个元素，打印v1: ";
	printVector(v1);
	v1.erase(v1.begin(),v1.end()); //调用6，删除首位到末尾所有元素，也就是删除全部元素
	cout << "调用6，删除首位到末尾所有元素，打印v1: ";
	printVector(v1);
	v1.clear();//调用7，清空所有元素
}

```

<img src="https://img-blog.csdnimg.cn/9cf4254ab7bc4a1999572b4e6572add4.png" alt="在这里插入图片描述" style="zoom:80%;" />

##### [vector数据存取](#context.7)<a name="section.7"> </a>

函数原型：
1.at(int idx);  //返回索引idx所指的数据
2.operator[];  //返回[]内索引所指的数据
3.front();  //返回容器中第一个元素
4.back();    //返回容器中最后一个元素

```c++
void text05()
{
	vector<int> v;
	for (int i = 0; i < 5; ++i)
	{
		v.push_back(i);
	}
	//利用at访问v
	cout << "调用1，打印v: ";
	for (int i = 0; i < v.size(); ++i)
	{
		cout << v.at(i) << " ";//调用1
	}
	cout << endl;
	//利用[]访问v
	cout << "调用2，打印v: ";
	for (int i = 0; i < v.size(); ++i)
	{
		cout << v[i] << " ";//调用2
	}
	cout << endl;
	cout << "容器中第一个元素是：" << v.front() << endl;//调用3
	cout << "容器中最后一个元素是：" << v.back() << endl;//调用4
}

```

<img src="https://img-blog.csdnimg.cn/0c96a078b3704d2f9aa3573df1cdf9ba.png" alt="在这里插入图片描述" style="zoom:80%;" />

##### [vector互换容器](#context.8)<a name="section.8"> </a>

函数原型：
swap(v);  //容器v和当前容器互换

```c++
void text06_1()
{
	vector<int> v1,v2;
	for (int i = 0; i < 10; ++i)
	{
		v1.push_back(i);
		v2.push_back(9 - i);
	}
	cout << "交换前：" << endl;
	printVector(v1);
	printVector(v2);
	cout << "交换后：" << endl;
	v1.swap(v2);   //调用互换函数
	printVector(v1);
	printVector(v2);
}

```

<img src="https://img-blog.csdnimg.cn/5927604994cc47b581df59236d4a4382.png" alt="在这里插入图片描述" style="zoom:80%;" />

##### [vector预留空间](#context.9)<a name="section.9"> </a>

功能：减少vector在动态扩容时的扩展次数，每次使用push_back(v)时，如果容器v的大小要超过v的容量时，系统就会对v进一次动态扩容，至于扩大多少空间，由系统决定。

函数原型：
reserve(int  len);   //容器预留len个元素长度，也就是把容量扩为len，预留的位置不初始化，同时也不可以访问。



### [2.deque容器](#context.10)<a name="section.10"> </a>

deque----双端队列容器
deque和vector有很多的相似之处，比如：
1.deque容器也擅长在序列尾部添加或删除元素（时间复杂度为O（1）），而不擅长在序列中间添加或删除元素
2.deque容器也可根据需要修改自身的容量和大小。

不同之处：
1.==deque还擅长在序列头部添加和删除元素==，耗费时间复杂度为O(1)。
2.==deque容器中存储元素并不能保证所有元素都存储到连续的内存空间中==。

##### [创建deque容器的几种方式：](#context.11)<a name="section.11"> </a>

1.创建一个没有任何元素的空deque容器：

```c++
deque<int> d;
```

和空数组容器不同，空的deque容器在创建之后可以做添加和删除元素的操作。

2.创建一个具有n个元素的deque容器，其中每个元素都采取对应类型的默认值：

```c++
deque<int> d(10);
```

此代码创建一个具有10个元素的deque容器。

3.创建一个具有n个元素的deque容器，并为每个元素指定初始值：

```c++
deque<int> d(10, 5);
```

此代码创建了一个具有10个元素（元素的值都为5）的deque的容器.

4.在已有的deque容器的情况下，可以通过拷贝该容器创建一个新的deque容器：

```c++
std::deque<int> d1(5);
std::deque<int> d2(d1);
```

必须保证新旧容器存储的元素类型一致。

##### [deque容器的函数](#context.12)<a name="section.12"> </a>

|       函数        |                           函数功能                           |
| :---------------: | :----------------------------------------------------------: |
|     begin（）     |              返回指向容器的第一个元素的迭代器。              |
|      end（）      |     返回指向容器最后一个元素所在位置后一个位置的迭代器。     |
|    rbegin（）     |                返回指向最后一个元素的迭代器。                |
|     rend（）      |        返回指向第一个元素所在位置前一个位置的迭代器。        |
|     size（）      |                      返回实际元素个数。                      |
|   max_size（）    |              返回容器所能容纳元素个数的最大值。              |
|    resize（）     |                     改变实际元素的个数。                     |
|     empty（）     |  判断容器中有元素，若无元素，则返回true；反之，返回false。   |
|      at（）       |               使用经过边界检查的索引访问元素。               |
|     front（）     |                    返回第一个元素的索引。                    |
|     back（）      |                   返回最后一个元素的索引。                   |
|    assign（）     |                   用新元素替换原有的元素。                   |
|   push_back（）   |                  在序列的尾部添加一个元素。                  |
|  push_front（）   |                  在序列的头部添加一个元素。                  |
|   pop_back（）    |                     移除容器尾部的元素。                     |
|   pop_front（）   |                     移除容器头部的元素。                     |
|    insert（）     |               在指定的位置插入一个或多个元素。               |
|     erase（）     |                   移除一个元素或一段元素。                   |
|     clear（）     |                 移除所有元素，容器大小为0。                  |
|     swap（）      |                   交换两个容器的所有元素。                   |
|    emplace（）    |                在指定的位置直接生成一个元素。                |
| emplace_front（） | 在容器头部生成一个元素。和push_front()的区别是，该函数直接在容器头部构造元素，省去了复制移动元素的过程。 |
| emplace_back（）  | 在容器尾部生成一个元素。和push_back（）的区别是，该函数直接在容器尾部构造元素，省去复制移动元素的过程。 |

##### [assign函数使用](#context.13)<a name="section.13"> </a>

在同一程序中被多次调用后，该函数将销毁先前元素的值，并将新的元素集重新分配给容器。
1.使用:
		deque_name.assign(size,val); 
参数：1.size：指定要分配给容器的值的数量。2.val：指定要分配给容器的值。

案例：

```c++
#include <bits/stdc++.h> 
using namespace std; 
int main() 
{ 
    deque<int> dq; 
  
    // assign 5 values of 10 each 
    dq.assign(5, 10); 
  
    cout << "The deque elements: "; 
    for (auto it = dq.begin(); it != dq.end(); it++) 
        cout << *it << " "; 
  
    // re-assigns 10 values of 15 each 
    dq.assign(10, 15); 
  
    cout << "\nThe deque elements: "; 
    for (auto it = dq.begin(); it != dq.end(); it++) 
        cout << *it << " "; 
    return 0; 
}
```

输出：

```c++
The deque elements: 10 10 10 10 10 
The deque elements: 15 15 15 15 15 15 15 15 15 15
```

2.使用：
			deque1_name.assign(iterator1, iterator2)
	参数：1.iterator1:它指定迭代器，该迭代器指向容器的起始元素，该容器的元素将被传输到deque1 。   
				2.iterator2:它指定迭代器，该迭代器指向容器的最后一个元素，该容器的元素将被传输到deque1。
案例：

```c++
#include <bits/stdc++.h> 
using namespace std; 
int main() 
{ 
    deque<int> dq; 
  
    // assign 5 values of 10 each 
    dq.assign(5, 10); 
  
    cout << "The deque elements: "; 
    for (auto it = dq.begin(); it != dq.end(); it++) 
        cout << *it << " "; 
  
    deque<int> dq1; 
  
    // assigns all elements from 
    // the second position to deque1 
    dq1.assign(dq.begin() + 1, dq.end());
  
    cout << "\nThe deque1 elements: "; 
    for (auto it = dq1.begin(); it != dq1.end(); it++) 
        cout << *it << " "; 
    return 0; 
}
```

输出：

```c
The deque elements: 10 10 10 10 10 
The deque1 elements: 10 10 10 10
```

##### [insert员函数使用](#context.14)<a name="section.14"> </a>

三种使用方法：
 		1.通过在一个为位置插入新元素val来扩展双端队列。
		 2.通过在双端队列中插入n个新值val来扩展双端队列。
		 3.通过在（first，last）范围内插入新元素来扩展双端队列。
	deque_name.insert(iterator  position, const  value_type&  val);
	deque_name.insert(iterator  position,size_type  n,const  value_type&  val);
	deque_name.insert(iterator  position,InputIterator  first,InputIterator  last);
	position———指定要插入元素的位置
	val———指定要插入的元素数。每个元素都初始化为val的副本
	first，last———指定迭代器，该迭代器指定要插入的元素范围。范围包括first和last之间的所有元素，包括first指向的元素，和last指向的元素。

案例：

```c++
#include <bits/stdc++.h> 
using namespace std; 
  
int main() 
{ 
    deque<int> dq = { 1, 2, 3, 4, 5 }; 
  
    deque<int>::iterator it = dq.begin(); 
    ++it; 
  
    it = dq.insert(it, 10); // 1 10 2 3 4 5 
  
    std::cout << "Deque contains:"; 
    for (it = dq.begin(); it != dq.end(); ++it) 
        cout << ' ' << *it; 
    cout << '\n'; 
  
    return 0; 
}
```

输出：

```c++
Deque contains: 1 10 2 3 4 5
```

```c++
#include <bits/stdc++.h> 
using namespace std; 
  
int main() 
{ 
    deque<int> dq = { 1, 2, 3, 4, 5 }; 
  
    deque<int>::iterator it = dq.begin(); //it取dq容器的起始位置
  
    // 0 0 1 2 3 4 5 
    dq.insert(it, 2, 0); //插入两个00;
  
    std::cout << "Deque contains:"; 
  
    for (it = dq.begin(); it != dq.end(); ++it) 
        cout << ' ' << *it; 
    cout << '\n'; 
  
    return 0; 
}
```

输出：

```c++
Deque contains: 0 0 1 2 3 4 5
```

```c++
#include <bits/stdc++.h> 
using namespace std; 
  
int main() 
{ 
    deque<int> dq = { 1, 2, 3, 4, 5 }; 
  
    deque<int>::iterator it = dq.begin(); 
    ++it; //dq的第二个位置
    vector<int> v(2, 10); 
  
    // 1 10 10 2 3 4 5 
    dq.insert(it, v.begin(), v.end()); 
  
    std::cout << "Deque contains:"; 
    for (it = dq.begin(); it != dq.end(); ++it) 
        cout << ' ' << *it; 
    cout << '\n'; 
  
    return 0; 
}
```

输出：

```c++
Deque contains: 1 10 10 2 3 4 5
```

##### [max_size员函数使用](#context.15)<a name="section.15"> </a>

该函数返回双端队列容器可以容纳的最大元素数。
deque_name.max_size()

##### [emplace_front()、emplace_back()函数使用](#context.16)<a name="section.16"> </a>

此函数用于将新元素插入到双端队列容器，并将新元素添加到双端队列的开头或者结尾。

emplace_front()案例：

```c++
Input :mydeque{1, 2, 3, 4, 5};
         mydeque.emplace_front(6);
Output:mydeque = 6, 1, 2, 3, 4, 5

Input :mydeque{};
         mydeque.emplace_front(4);
Output:mydeque = 4
```

```c
#include <deque> 
#include <iostream> 
#include <string> 
using namespace std; 
  
int main() 
{ 
    deque<string> mydeque; 
    mydeque.emplace_front("portal"); 
    mydeque.emplace_front("science"); 
    mydeque.emplace_front("computer"); 
    mydeque.emplace_front("a"); 
    mydeque.emplace_front("is"); 
    mydeque.emplace_front("GEEKSFORGEEKS"); 
  
    // deque becomes GEEKSFORGEEKS, is, a, 
    // computer, science, portal 
  
    // printing the deque 
    for (auto it = mydeque.begin();it != mydeque.end(); ++it) 
        cout << ' ' << *it; 
  
    return 0; 
}
```

输出：

```c
GEEKSFORGEEKS is a computer science portal
```

emplace_back()案例：

```c
Input :mydeque{1, 2, 3, 4, 5};
         mydeque.emplace_back(6);
Output:mydeque = 1, 2, 3, 4, 5, 6

Input :mydeque{};
         mydeque.emplace_back(4);
Output:mydeque = 4
```

```c++
#include <deque> 
#include <iostream> 
using namespace std; 
  
int main() 
{ 
    deque<string> mydeque; 
    mydeque.emplace_back("Hi"); 
    mydeque.emplace_back("this"); 
    mydeque.emplace_back("is"); 
    mydeque.emplace_back("geeksforgeeks"); 
    // deque becomes Hi this is geeksforgeeks 
  
    // printing the deque 
    for (auto it = mydeque.begin();it != mydeque.end(); ++it) 
        cout << ' ' << *it; 
  
    return 0; 
}
```

输出：

```c++
Hi this is geeksforgeeks
```



### [3.List容器](#context.17)<a name="section.17"> </a>

list容器，又称双向链表容器。该容器底层是以双向链表的形式实现的。
list容器可以在序列已知的任何位置快速插入或删除元素（时间复杂度O（1））。并且在list容器中移动元素，也比其他容器效率高。==不能像array和vector一样通过位置直接访问元素。只有运用迭代器，才能访问list容器中存储的各个元素==。

注：
	1.双向迭代器不能通过下标访问List容器中指定位置处的元素。
	2.双向迭代器不支持使用-=、+=、+、-运算符。
	3.双向迭代器不支持使用<、>、<=、>=比较运算符。

几种不常使用的函数：
	1.insert()   在容器中的指定位置插入元素。
	2.splice()   将一个list容器中的元素插入到另一个容器的指定位置。
	3.remove(val)   删除容器中所有等于val的元素。
	4.remove_if()    删除容器中满足条件的元素。
	5.unique()    删除容器中相邻的重复元素，只保留一个。
	6.merge()    合并两个事先已排好序的list容器，并且合并之后的list容器依然是有序的。
	7.sort()      通过更改容器中元素的位置，将它们进行排序。
	8.reverse()    反转容器中元素的顺序。

案例：

```c++
#include <iostream>
#include <list>
int main()
{
	std::list<int> test{ 1,2 };
	test.insert(test.begin(), 0);//指定的位置之前插入 1 个元素 3 {0,1,2}
	test.insert(test.end(), 2, 3);//指定的位置之前插入 2 个元素 3 {0,1,2,3,3}
	std::array<int, 3>tmp{ 4,5,6 };
	test.insert(test.end(), tmp.begin(), tmp.end());//将其他容器中特定位置的元素插入指定位置 {0,1,2,3,3,4,5,6}
	test.insert(test.end(),{7,8});//插入初始化列表中所有的元素{0,1,2,3,3,4,5,6,7,8}
	for (auto i : test) {
		std::cout << i << " ";
	}
}
```

输出：

```c++
0 1 2 3 3 4 5 6 7 8
```

```c++
#include <iostream>
#include <list>
int main()
{
	std::list<int> test1{ 1,2,3,4 };
	std::list<int> test2{ 5,6,7 };
	std::list<int>::iterator it = ++test1.begin(); //it 迭代器指向元素 2

	//第一种用法：将test2的所有元素移动到2所在的位置:
	test1.splice(it, test2);//test1:{1,5,6,7,2,3,4}; test2:{}; it迭代器仍然指向元素2
													
	//第二种用法：将 it 指向的元素 2 移动到 test2.begin()位置:
	test2.splice(test2.begin(), test1, it);	//test1:{1,5,6,7,3,4}; test2:{2}; it仍然指向元素2

	//第三种用法：，将 [test1.begin(),test1.end())范围内的元素移动到 test2.begin() 位置:
	test2.splice(test2.begin(), test1, test1.begin(), test1.end());	//test1:{}; test2:{1,5,6,7,3,4,2};it仍然指向元素2
																
	std::cout << "test1.size(): " << test1.size() << std::endl;
	std::cout << "test2.size(): " << test2.size() << std::endl;
	std::cout << "it: " << *it << std::endl;
	for (auto i : test2) {
		std::cout << i << " ";
	}
}
```

输出：

```c++
test1.size(): 0
test2.size(): 7
it: 2
1 5 6 7 3 4 2
```

```c++
#include <iostream>
#include <list>
int main()
{
	std::list<int> test{ 0,1,2,3,3,3,4,5,6,7,6,5,4,3,2,1,99,0 };
	test.pop_back();//移除最后一个元素{0,1,2,3,3,3,4,5,6,7,6,5,4,3,2,1,99}
	test.pop_front();//移除第一个元素{1,2,3,3,3,4,5,6,7,6,5,4,3,2,1,99}
	test.erase(++test.begin()); //删除第二个元素{1,3,3,3,4,5,6,7,6,5,4,3,2,1,99}
	//删除指定元素
	for (std::list<int>::iterator i = test.begin(); i != test.end();++i) {
		if ((*i) == 7) {
			test.erase(i);
		}
	}
	//{1,3,3,3,4,5,6,6,5,4,3,2,1,99}
	test.remove(99);//删除元素99 {1,3,3,3,4,5,6,6,5,4,3,2,1}
	
	for (auto i : test) {
		std::cout << i << " ";
	}
	std::cout<< std::endl;
	test.erase(++test.begin(), --test.end());//删除指定一段元素 {1,1}
	for (auto i : test) {
		std::cout << i << " ";
	}
	test.clear();//清空
}
```

输出：

```c++
1 3 3 3 4 5 6 6 5 4 3 2 1
1 1
```

```c++
#include <iostream>
#include <list>
//二元谓词函数
bool fuc1(int a, int b) {
	if (a / 10 == b / 10) {
		return true;
	}
	return false;
}

int main()
{
	std::list<int> test{10,20,30,30,31,32,33,40,50,60,30,50,60};
	//方式一：
	test.unique();//删除相邻重复的元素，仅保留一份
	for (auto i : test) {
		std::cout << i << " ";
	}
	std::cout << std::endl;
	//方式二:
	test.unique(fuc1);//fuc1 为二元谓词函数，是我们自定义的去重规则
	for (auto i : test) {
		std::cout << i << " ";
	}
	std::cout << std::endl;
}
```

输出：

```c++
10 20 30 31 32 33 40 50 60 30 50 60
10 20 30 40 50 60 30 50 60
```

```c++
#include <iostream>
#include <list>
int main()
{
	std::list<int> test{1,2,3,4,5,6,7,8,9,10};
	test.remove_if([](int n){return (n % 2 != 0);} ); //删除test容器中能够使lamba表达式成立的所有元素。
	for (auto i : test) {
		std::cout << i << " ";
	}
	std::cout << std::endl;
}
```

输出：

```c++
2 4 6 8 10
```

```c++
#include <iostream>
#include <list>
int main()
{
	std::list<int> test{1,2,3,4,5,6,7,8,9,10};
	auto it = test.begin();
	std::advance(it, 5);
	*it = 60;
	for (auto i : test) {
		std::cout << i << " ";
	}
	std::cout << std::endl;
}
```

输出：

```c++
1 2 3 4 5 60 7 8 9 10
```







### [4.map容器](#context.18)<a name="section.18"> </a>

map中所有元素都是pair，pair中的第一个元素为key（键值），起到索引作用，第二个元素为value（实值）；所有的元素都会根据元素的键值自动排序（键值小到大排序），map/multimap属于关联式容器，底层实现是二叉树（红黑树）。
map和multimap的区别：
1.map不允许容器中有重复key值
2.multimap允许有重复的key值





 







##### [不经常使用函数：](#context.19)<a name="section.19"> </a>

count()  //返回指定元素出现的次数
equal_range()      //返回特殊条目的迭代器对
max_size()    //返回容器可以容纳的最大元素个数
swap()     //交换两个map容器内容
find()   //查找一个元素
erase()   //删除一个元素  ；erase(start，last)删除区间（start，last）的所有元素。



##### [map中的insert的使用](#context.20)<a name="section.20"> </a>

插入的三种方式：

```c++
void test（）
{
   map<int,int>m;
    //第一种插入
	m.insert(pair<int,int>(1,10));
    //第二种
	m.insert(make_pair(2,20));
	//第三种
	m.insert(map<int,int>::value_type(3,30));
}
```

 

##### [1.map中的swap用法](#context.21)<a name="section.21"> </a>

map中的swap不是一个容器中的元素交换，而是两个容器所有元素的交换。

##### [2.map中的sort问题](#context.22)<a name="section.22"> </a>

map中的元素是自动按照key升序排序的，所以不能直接用sort函数。

