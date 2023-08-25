---
title: C++
date: 2023-07-26
tags: ['星海']
categories: 星海
id: C++
---

# C++

# 目录

- [输入输出](#section.2)<a name="context.2"> </a>
  - [getline](#section.3)<a name="context.3"> </a>
  - [cin](#section.4)<a name="context.4"> </a>
- [nullptr](#section.5)<a name="context.5"> </a>
- [左值右值](#section.6)<a name="context.6"> </a>
- [左值引用(&)和右值引用(&&)](#section.7)<a name="context.7"> </a>
- [类](#section.8)<a name="context.8"> </a>
- [友元](#section.9)<a name="context.9"> </a>
  - [友元函数](#section.10)<a name="context.10"> </a>
  - [友元类](#section.11)<a name="context.11"> </a>
- [重载](#section.12)<a name="context.12"> </a>
- [bitset（位库）](#section.13)<a name="context.13"> </a>
- [STL](#section.14)<a name="context.14"> </a>
- [unordered_set方法](#section.15)<a name="context.15"> </a>
- [pair容器](#section.16)<a name="context.16"> </a>
- [vector 容器](#section.17)<a name="context.17"> </a>
- [list（链表）](#section.18)<a name="context.18"> </a>
- [vector和list的区别](#section.19)<a name="context.19"> </a>
- [deque（双端数组）](#section.20)<a name="context.20"> </a>
- [stack / queue](#section.21)<a name="context.21"> </a>
- [priority_queue](#section.22)<a name="context.22"> </a>
- [map / set](#section.23)<a name="context.23"> </a>
- [map / unordered_map](#section.24)<a name="context.24"> </a>
- [multimap](#section.25)<a name="context.25"> </a>
- [C++模板全特化和偏特化](#section.26)<a name="context.26"> </a>
- [lambda表达式](#section.27)<a name="context.27"> </a>
- [智能指针](#section.28)<a name="context.28"> </a>
  - [定义](#section.29)<a name="context.29"> </a>
  - [shared_ptr（常用）](#section.30)<a name="context.30"> </a>
  - [unique_ptr](#section.31)<a name="context.31"> </a>
  - [weak_ptr](#section.32)<a name="context.32"> </a>
  - [auto_ptr](#section.33)<a name="context.33"> </a>
- [function](#section.34)<a name="context.34"> </a>
- [decltype / auto](#section.35)<a name="context.35"> </a>
------------------------------------------------

<!-- more -->

## [输入输出](#context.2)<a name="section.2"> </a>

```c++
int i = 1;
cout << ++i << i++ << i << i++ << ++i << endl;
///< 输出为 &5 3 &5 2 &5
```

cout输出控制台过程如下

![img](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/202308161641648.png)

从 `endl` 开始，从右向左依次入栈，最后依次弹出



### [getline](#context.3)<a name="section.3"> </a>

默认遇换行符或 `EOF` 结束，也可自定义结束符

上限字符数为 `1024`

```c++
/** 
 * @pare s: 字符串变量
 * @pare n: 字符个数 (第n个补'\0')
 * @pare delim: 输入终止条件 (可省略, 默认换行符 '\n')
 * @note: #include <iostream>    ///< 依赖
 */
std::cin.getline(char* s, streamsize n, char delim);

/** 
 * @pare is: 标准输入流函数 (常用 std::cin)
 * @pare str: 存字符的变量名
 * @pare delim: 输入终止条件 (可省略, 默认换行符 '\n')
 * @note: #include <string>    ///< 依赖
 */
string::getline(istream& is, string& str, char delim);
```



### [cin](#context.4)<a name="section.4"> </a>

在上述的停止条件上，遇空格也会停止



## [nullptr](#context.5)<a name="section.5"> </a>

c++11引入 `nullptr` ，为 **右值常量**

原因：

在函数重载的时候，如果还是用C语言的 `NULL` 则会出错，并且 `NULL` 只是单纯的宏定义替换

```c++
#include <iostream>
void foo(int i) {std::cout << "int\n";};
void foo(char *p) {std::cout << "ptr\n";};

int main()
{
    foo(NULL);  ///< 不注释掉的话会报错
    foo(nullptr);  ///< 输出 "ptr"
    return 0;
}
```







## [左值右值](#context.6)<a name="section.6"> </a>

首先区分一下什么是左值，什么是右值。
左值就是可以写在赋值号左边的，右值是写在赋值号右边的。
比如

```c++
Stu foo() {
    return Stu();
}
int a = 5; // a是左值
Stu s = foo(); // s是左值，foo()的返回值是右值
int c = a + b; // a + b 的结果是右值，c是左值
```

`foo()` 返回了一个没有名字的`Stu`对象，你不能写`foo() = s`，所以`foo()`就是个右值。
还有一种定义说是，无法取地址的就是右值，可以取地址的是左值。



## [左值引用(&)和右值引用(&&)](#context.7)<a name="section.7"> </a>

首先要注意一点：`&&`不是“引用的引用”，这仅仅是一个记号，这个记号我改成`$`也没什么问题。不能像理解指针`**`是指针的指针这样去类比。
左值引用就是给左值变量起别名，右值引用就是给右值变量起别名。

```c++
int a = 5;
int &aa = a; //左值引用
Stu &&s = foo(); // 右值引用
```

Stu &&s = foo()，就是给foo()返回的临时对象起了个别名，本来它在foo()返回后生存期就到了，就该析构了，但是由于s对她进行了引用，他的生存期被延长至和s相同。如果是Stu s = foo()则会在赋值时发生一次拷贝构造。

附两篇写的很好的文章
[C++ 11的移动语义 - 行者孙 - 博客园](https://www.cnblogs.com/sunchaothu/p/11392116.html)
[C++11新特性：右值引用和转移构造函数 - DoubleLi - 博客园](https://www.cnblogs.com/lidabo/p/3908681.html)

作者：wangbingbing

出处：https://www.cnblogs.com/wangbingbing/p/15179675.html









## [类](#context.8)<a name="section.8"> </a>

**注意：**

1. 成员变量和成员函数分开存储
2. 静态成员只能在类外初始化，`cosnt` 常量静态成员在类内初始化
   静态成员变量，属于某个类，所有对象共享，无论建立了多少个对象，都只有一个静态数据的拷贝。是在编译阶段就分配好了空间，对象还没有创建时，就已经分配空间。
3. 类的静态成员访问
   用 `::` 或者 `.`
4. 类的静态函数只能访问类中的静态变量，其他函数则都可访问
5. 静态函数没有 `this` 指针，同时静态变量也有访问级别限制（公有，私有，保护）



**类中的虚函数占用为一个指针的大小**

**类的成员函数，实际上相当于普通函数，只不过把类对象隐式的传递进去罢了**

**空类的大小为？**

c++要求每个实例在内存中都有独一无二的地址。空类也会被实例化，所以编译器会给空类隐含的添加一个字节，这样空类实例化之后就有了独一无二的地址了。所以空类的sizeof为1







## [友元](#context.9)<a name="section.9"> </a>

**友元提供了一种突破封装的方式。**友元函数提供了一种在需要时访问类的私有成员的机制，但应该慎重使用，因为过多的友元函数可能破坏类的封装性。



### [友元函数](#context.10)<a name="section.10"> </a>

友元函数可以直接访问类的私有成员，它是定义在类外部的普通函数，不属于任何类，但是需要在类的内部声明，声明时需要加friend关键字。



**注意：**

1. 友元函数可以访问类的所有成员，但此函数不是类的成员函数
2. 友元函数不能用 `const` 修饰
3. 友元函数可以在类定义的任何地方申明，不受类访问限定符限制
4. 一个函数可以是多个类的友元函数
5. 友元函数调用和普通函数的调用原理相同



### [友元类](#context.11)<a name="section.11"> </a>

友元类的所有成员函数都可以是另一个类的友元函数，都可以访问另一个类中的非公有成员



**注意：**

1. 友元关系是单向的，不具有交换性
   比如Time类和Date类，在Time类中声明Date类为其友元类，那么可以在Date类中直接访问Time类的私有成员变量，但想在Time类中访问Date类中私有的成员变量则不行
2. 友元关系不能传递
   如果B是A的友元，C是A的友元，则不能说明C是A的友元







## [重载](#context.12)<a name="section.12"> </a>

形式：

**[返回值] operator[运算符] (参数...) { ... }；**



**不能重载的5种运算符：**

1. `.*`  
   任意字符出现零次或多次
2. `::`
   域作用符  
3. `sizeof`
   关键字 - 大小
4. `?:`
   三目运算符
5. `.`
   点运算符



例：重载类运算

```c++
class Time
{
	int _hour;
	int _min;
	int _sec;
public:
	Time(int hour = 0, int min = 0, int sec = 0)
	{
		_hour = hour;
		_min = min;
		_sec = sec;
	}
	void Print()
	{
		cout << _hour << ":" << _min << ":" << _sec << endl;
 
	}
	//为方便演示，让小时+1，但不再判断时间正确性
	Time& operator++()// ++A  不带参数为前置++
	{
		_hour += 1;
		return (*this);//因为自增直接返回this用引用接收
	}
	Time operator++(int)//A++，参数写int或int i都可以   带参数为后置++
	{
		Time ret(*this);
		_hour += 1;
		return ret;//需要返回this自增之前的结果，所以用临时变量返回
	}
};
————————————————
版权声明：本文为CSDN博主「就要 宅在家」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_61857742/article/details/126010673

///< 友元函数重载流提取 << / >>
// 对于流而言，因为是双目运算符，this指针本应该指向左边的类，但左操作数是一个流，又与this的类型冲突。
// 这时，就需要用到友元函数friend。友元函数本身是一个普通函数，但是作为类的友元，能够调用类内的成员，包括private。而且参数不用被类限制为第一个必须是this所指的对象本身。
/*
class {
	friend ostream& operator<<(ostream& out, Time& t);//友元函数，声明
	friend istream& operator>>(istream& in, Time& t);
}
*/
```



## [bitset（位库）](#context.13)<a name="section.13"> </a>

```c++
#include <bitset>  ///< 所在头文件
using namespace std;  ///< 命名空间std
int main()
{
    // 默认无参构造
    // 初始化全部位为0
    std::bitset<8> bs;
    //bs[0] = 1;            // 0000 0001
    //bs[7] = 1;            // 1000 0000
    
    // 传值构造方式
    std::bitset<8> bs(7);
    bs.to_string()  // 0000 0111
    std::bitset<8> bs(0x07);
    bs.to_string()  // 0000 0111

    // string 构造
    std::bitset<8> bs("00000111");
    bs.to_ulong();      // 7
    return 0;
}    
```



| 成员函数     | 功能                                              |
| ------------ | ------------------------------------------------- |
| .any()       | 是否存在值为1的二进制位                           |
| .none()      | 是否不存在值为1的二进制位<br/>或者说是否全部位为0 |
| .size()      | 位长，也即是非模板参数值                          |
| .count()     | 值为1的个数                                       |
| .test(pos)   | 测试pos处的二进制位是否为1<br/>与0做或运算        |
| .set()       | 全部位置1                                         |
| .set(pos)    | pos位处的二进制位置1<br/>与1做或运算              |
| .reset()     | 全部位置0                                         |
| .reset(pos)  | pos位处的二进制位置0<br/>与0做或运算              |
| .flip()      | 全部位逐位取反                                    |
| .flip(pos)   | pos处的二进制位取反                               |
| .to_ulong()  | 将二进制转换为unsigned long输出                   |
| .to_string() | 将二进制转换为字符串输出                          |
| ~bs          | 按位取反<br/>效果等效为bs.flip()                  |
| os << b      | 将二进制位输出到os流<br/>小值在右，大值在左       |









## [STL](#context.14)<a name="section.14"> </a>

STL提供了六⼤组件，彼此之间可以组合套⽤，这六⼤组件分别是：

1. **容器**

   各种数据结构，如vector、list、deque、set、map等，⽤来存放数据，从实现角度来看，STL 容器是⼀种 `class template`

2. **算法**

   各种常⽤的算法，如sort、find、copy、for_each。从实现的角度来看，STL算法是⼀种 `function template`

3. **迭代器**

   扮演了容器与算法之间的胶合剂，共有五种类型，从实现角度来看，迭代器是⼀种将 operator* , operator-> , operator++, operator– 等指针相关操作予以重载的 `class template`。  

   所有STL容器都附带有⾃⼰专属的迭代器，只有容器的设计者才知道如何遍历⾃⼰的元素。  

   原⽣指针(native pointer)也是⼀种迭代器。

4. **仿函数**

   ⾏为类似函数，可作为算法的某种策略。从实现角度来看，仿函数是⼀种重载了operator() 的 class 或者 class template

5. **适配器（配接器）**

   ⼀种⽤来修饰容器或者仿函数或迭代器接⼝的东西。  

   STL提供的queue 和 stack，虽然看似容器，但其实只能算是⼀种容器配接器，因为它们的底部完全借助deque，所有操作都由底层的deque供应

6. **空间配置器**

   负责空间的配置与管理。从实现角度看，配置器是⼀个实现了动态空间配置、空间管理、空间释放的 class template

   ⼀般的分配器的 std:alloctor 都含有两个函数 allocate 与 deallocte，这两个函数分别调⽤ operator new() 与 delete()，这两个函数的底层⼜分别是malloc() and free(); 但是每次malloc会带来格外开销（因为每次malloc⼀个元素都要带有附加信息）



**容器之间的实现关系以及分类：**

![image-20230615150413812-1692155829457](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/202308161642801.png)

**STL六⼤组件的交互关系：**

1. 容器通过空间配置器取得数据存储空间  
2. 算法通过迭代器存储容器中的内容  
3. 仿函数可以协助算法完成不同的策略的变化 
4. 适配器可以修饰仿函数



**STL的优点**

STL 具有⾼可重⽤性，⾼性能，⾼移植性，跨平台的优点

1. ⾼可重⽤性
   STL 中几乎所有的代码都采⽤了模板类和模版函数的⽅式实现，这相⽐于传统的由函数和类组成的库来说提供了更好的代码重⽤机会
2. ⾼性能
   如 map 可以⾼效地从⼗万条记录⾥⾯查找出指定的记录，因为 map 是采⽤红黑树的变体实现的
3. ⾼移植性
   如在项⽬ A 上⽤ STL 编写的模块，可以直接移植到项⽬ B 上



**STL 的⼀个重要特性是将数据和操作分离**

数据由容器类别加以管理，操作则由可定制的算法定义。迭代器在两者之间充当 "粘合剂" 以使算法可以和容器交互运作



## [unordered_set方法](#context.15)<a name="section.15"> </a>

```c++
unordered_set<int> us;

// 如果 x 在 us 中， 返回1 否则0 
us.count(x);

```









## [pair容器](#context.16)<a name="section.16"> </a>

保存两个数据成员，⽤来⽣成特定类型的模板

用法：

```c++
pair<T1, T2> p;
///< 数据成员是public，两个成员分别是first和second
```



## [vector 容器](#context.17)<a name="section.17"> </a>

**底层：**

Vector在堆中分配了⼀段连续的内存空间来存放元素

底层为动态数组



扩容：

1. 固定扩容
2. 加倍扩容



## [list（链表）](#context.18)<a name="section.18"> </a>

**中间位置的插入与删除是 O(1)**

**实现方式：**双向链表





## [vector和list的区别](#context.19)<a name="section.19"> </a>

1. vector 底层实现是数组；list 是双向链表 
2. vector 是顺序内存，⽀持随机访问，list 不⾏ 
3. vector 在中间节点进⾏插⼊删除会导致内存拷⻉，list不会 
4. vector ⼀次性分配好内存，不够时才进⾏翻倍扩容；list 每次插⼊新节点都会进⾏内存申请 
5. vector 随机访问性能好，插⼊删除性能差；list 随机访问性能差，插⼊删除性能好



## [deque（双端数组）](#context.20)<a name="section.20"> </a>

1. O(1)
   首尾插入，删除，访问
2. O(n)
   中间插入 insert()，erase()







**在头/尾部插入/删除时间复杂度为 O(1)**

**致命缺陷：**不适合遍历，因为在遍历时，deque的迭代器要频繁的去检测其是否移动到某段小空间的边界，导致效率低下

因此，需要线性结构时，大多数情况下优先考虑`vector`和`list`，`deque`的应用并不多，而目前能看到的一个应用就是，STL用其作为`stack`和`queue`的底层数据结构。



⽀持快速随机访问，由于 deque 需要处理内部跳转，因此速度上没有 vector 快

deque 是⼀个**双端开口**的连续线性空间，其内部为分段连续的空间组成，随时可以增加⼀段新的空间并链接

**注意：**

由于deque的迭代器⽐vector要复杂，这影响了各个运算层面，所以除⾮必要尽量使⽤ vector；
为了提⾼效率，在对deque进⾏排序操作的时候，我们可以先把 deque 复制到 vector 中再进⾏排序最后在复制回 deque

![image-20230615153051799-1692155829457](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/202308161643278.png)

deque采⽤⼀块map作为主控，其中的每个元素都是指针，指向另⼀⽚连续线性空间，称之为 缓存区，这个区才是⽤来储存数据的



## [stack / queue](#context.21)<a name="section.21"> </a>

概述：栈与队列被称之为 deque 的配接器，其底层是以 deque 为底部架构。通过 deque 执⾏具体操作



## [priority_queue](#context.22)<a name="section.22"> </a>

 		优先队列是一种容器适配器，采用了堆这样的数据结构，保证了第一个元素总是整个优先队列中最大的(或最小的)元素。  优先队列默认使用vector作为底层存储数据的容器，在vector上使用了堆算法将vector中的元素构造成堆的结构，所以其实我们就可以把它当作堆，凡是需要用堆的位置，都可以考虑优先队列。

​		 默认情况下 `priority_queue` 是大堆

```c++
template <class T, 
          class Container = vector<T>,  ///< 优先队列底层使用的存储结构，可以看出来，默认采用vector
          class Compare = less<typename Container::value_type>  ///< 定义优先队列中元素的比较方式的类，默认是按小于(less)的方式比较，
                                                                ///< 这种比较方式创建出来的就是大堆。所以优先队列默认就是大堆
                                                                ///< 如果需要创建小堆，就需要将less改为greater
          > class priority_queue;

/* 重载比较函数 Compare */
///< Method 1: 重载 ()
class mycomparison {
public:
    bool operator()(const pair<int, int>& lhs, const pair<int, int>& rhs) {
        return lhs.second > rhs.second;  ///< 此处与快排的排序相反， > 表从小到大
    }
};
priority_queue<pair<int, int>, vector<pair<int, int>>, mycomparison> pri_que;

///< Method 2: 函数
bool Cmp(const int &a, const int &b)
{
    return a < b;
}
priority_queue<int, vector<int>, decltype(&Cmp)> pq(Cmp);

///< Method 3: class / struct


```

**注：priority_queue 无迭代器！！**



1. push() / emplace()
   O(log N)，push(x) 令x入队，其中 N 为当前优先队列中元素的个数
2. top() 
   O(1)，获得队首元素(即堆顶元素)
3. pop() 
   O(log N)，令队首元素(即堆顶元素)出队
4. empty() 
   O(1)，检测优先队列是否为空，返回 true则空，返回false 则非空
5. size() 
   O(1)，返回优先队列内元素的个数



**emplace()与push()的区别**

当我们使用push()时，会创建一个对象，然后将其插入优先级队列。而使用emplace()，该对象将原地构造，节省了不必要的副本





## [map / set](#context.23)<a name="section.23"> </a>

有序（从小到大），底层为**红黑树**

**共同点：**都是C++的关联容器，只是通过它提供的接口对里面的元素进行访问，底层都是采⽤红黑树实现

**不同点：**

set：⽤来判断某⼀个元素是不是在⼀个组里面。 
map：映射，相当于字典，把⼀个值映射成另⼀个值，可以创建字典

**优点：**

查找某⼀个数的时间为 `O(logn)`；遍历时采⽤ iterator，效果不错

**缺点：**

每次插⼊值的时候，都需要调整红黑树，效率有⼀定影响



**重载 < 排序方法：**

```c++
class Student{
public:
    std::string name_;
    int age_;
    Student(std::string name, int age):name_(name),age_(age){}
    Student(){}
 
    bool operator < (const Student &s) const {
        if(name_ != s.name_){
            return name_ < s.name_;
        }else{
            return age_ < s.age_;
        }
    }
 
    std::string show(){
        return "name:" + name_ + ", age:" + std::to_string(age_);
    }
 
};
 
int main(int argc, char *argv[]) {
 
    set<Student> s;
    s.emplace(Student("Danney", 22));
    s.emplace(Student("LiMing", 20));
    s.emplace(Student("LiMing", 18));
    for(auto it : s){
        cout << it.show() << endl;
 
    }
    return 0;
}
————————————————
版权声明：本文为CSDN博主「小梦_人生如戏」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/yu532164710/article/details/105194036
```



**细节：**

1. **为什么要成倍的扩容而不是⼀次增加⼀个固定⼤⼩的容量呢？**
   采⽤成倍⽅式扩容，可以保证常数的时间复杂度，⽽增加指定⼤⼩的容量只能达到O(n)的时间复杂度
2. **为什么是以两倍的⽅式扩容而不是三倍四倍，或者其他⽅式呢？**
   考虑可能产⽣的堆空间浪费，所以增⻓倍数不能太⼤，⼀般是 1.5 或 2；GCC 是 2；VS 是 1.5，k = 2 每次扩展的新尺寸必然刚好⼤于之前分配的总和，之前分配的内存空间不可能被使⽤，这样对于缓存并不友好，采⽤ 1.5 倍的增⻓⽅式可以更好的实现对内存的重复利用

**注：C++并没有规定扩容因子K，这是由标准库的实现者决定的**





## [map / unordered_map](#context.24)<a name="section.24"> </a>

map中元素是⼀些 `key-value` 对，关键字起索引作⽤，值表示和索引相关的数据



**底层实现：**

`map` 底层是基于**红黑树**实现的，因此map内部元素排列是有序的
`unordered_map` 底层则是基于**哈希表**实现的，因此其元素的排列顺序是杂乱⽆序的



|      | map                                                          | unordered_map                                                |
| ---- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 优点 | 有序性，这是map结构最⼤的优点，<br />其元素的有序性在很多应⽤中都会简化很多的操作<br />map的查找、删除、增加等⼀系列操作时间复杂度稳定，都为 O(logn) | 查找、删除、添加的速度快，时间复杂度为常数级O(1）            |
| 缺点 | 查找、删除、增加等操作平均时间复杂度较慢，与 n 相关          | 因为unordered_map内部基于哈希表，<br />以（key,value）对的形式存储，因此空间占⽤率⾼<br />unordered_map的查找、删除、添加的时间复杂度不稳定<br />平均为O(1)，取决于哈希函数，极端情况下可能为O(n) |

**问题：**

1. **为什么 `insert` 之后，以前保存的 `iterator` 不会失效？**
   **答：** 因为 map 和 set 存储的是结点，不需要内存拷⻉和内存移动。但是像 vector 在插入数据时如果内存不够会重新开辟一块内存。 map 和 set 的 iterator 指向的是节点的指针，vector 指向的是内存的某个位置
2. **为何 map 和 set 的插入删除效率比其他序列容器高？**
   **答：** 因为 map 和 set 底部使用红黑树实现，插入和删除的时间复杂度是 `O(logn)`，而像 vector 这样的序列容器插入和删除的时间复杂度是 `O(N)`





## [multimap](#context.25)<a name="section.25"> </a>

multimap是C++ STL中的一个关联容器，它与map类似，但可以存储多个具有相同键值的元素。

multimap使用红黑树实现，因此它的插入、查找、删除操作的时间复杂度为O(log n)，其中n为multimap中元素的数量。由于multimap允许重复的键值，因此对于某些操作，如查找、删除等，复杂度可能会更高。

```c++
template < class Key,                                   // 指定键（key）的类型
           class T,                                     // 指定值（value）的类型
           class Compare = less<Key>,                   // 指定排序规则
           class Alloc = allocator<pair<const Key,T> >  // 指定分配器对象的类型
           > class multimap;

// 1.构造函数
multimap();                             ///< 创建一个空的multimap。
multimap(InputIt first, InputIt last);  ///< 创建一个包含[first, last)区间内所有元素的新multimap，要求这些元素必须支持拷贝构造函数。
multimap(const multimap& other);        ///< 拷贝构造函数，创建一个新的multimap，它与另一个multimap中的元素相同。

// 2.迭代器
begin();   ///< 返回指向multimap第一个元素的迭代器。
end();     ///< 返回指向multimap最后一个元素之后位置的迭代器。
rbegin();  ///< 返回指向multimap最后一个元素的反向迭代器。
rend();    ///< 返回指向multimap第一个元素之前位置的反向迭代器。

// 3.容量
empty();     ///< 如果multimap为空，则返回true，否则返回false。
size();      ///< 返回multimap中元素的数量。
max_size();  ///< 返回multimap可以包含的最大元素数量。

// 4.插入与删除
insert(const value_type& val);                     ///< 将val插入到multimap中。如果multimap中已经存在一个键值与val相等的元素，
                                                   ///< 则新的val将会被插入到该键值所对应的元素序列的尾部。
insert(InputIt first, InputIt last);               ///< 将[first, last)区间内的所有元素插入到multimap中。

/* m.emplace(key, value) */
emplace(key, value);                               ///< 在当前multimap容器中的指定位置处构造新键值对。其效果和插入键值对一样，但效率更高

erase(const key_type& key);                        ///< 删除multimap中所有键值为key的元素。
erase(const_iterator position);                    ///< 删除迭代器position所指向的元素。
erase(const_iterator first, const_iterator last);  ///< 删除[first, last)区间内的所有元素。
clear();                                           ///< 清空multimap中的所有元素。

// 5.查找
count(const key_type& key);        ///< 返回multimap中键值为key的元素数量。
find(const key_type& key);         ///< 查找并返回multimap中第一个键值为key的元素的迭代器。如果找不到，则返回end()。
equal_range(const key_type& key);  ///< 返回一个pair，包含两个迭代器，第一个迭代器指向multimap中第一个键值为key的元素，
                                   ///< 第二个迭代器指向multimap中第一个键值大于key的元素。如果找不到任何元素，则两个迭代器都等于end()。

/********** 除了以上方法，multimap还继承了map类的其他方法，如key_comp()、value_comp()等。 **********/

/* 自定义排序 demo */
#include <iostream>
#include <map>

// 自定义比较函数，对键进行降序排序
struct Compare {
    bool operator()(const int& a, const int& b) const {
        return a > b;
    }
};

int main() {
    std::multimap<int, std::string, Compare> myMultimap;

    // 使用 insert() 函数插入键值对
    myMultimap.insert(std::pair<int, std::string>(1, "apple"));
    myMultimap.insert(std::pair<int, std::string>(2, "banana"));
    myMultimap.insert(std::pair<int, std::string>(3, "orange"));

    for (auto& kv : myMultimap) {
        std::cout << "Key = " << kv.first << ", Value = " << kv.second << std::endl;
    }

    return 0;
}
```



## [C++模板全特化和偏特化](#context.26)<a name="section.26"> </a>

**模板分为：**1. 类模板  2. 函数模板

**特化分为：**1. 特例化（全特化）  2. 部分特例化（偏特化）

对模板特例化是因为对特定类型，可以利⽤某些特定知识来提⾼效率，⽽不是使⽤通⽤模板



**对函数模板：**

1. 模板和特例化版本应该声明在同⼀头⽂件，所有同名模板的声明应放在前⾯，接着是特例化版本
2. ⼀个模板被称为全特化的条件：1.必须有⼀个主模板类 2.模板类型被全部明确化



**模板函数：**

```c++
template<typename T1, typename T2>
void fun(T1 a, T2 b)
{
    cout<<"模板函数"<<endl;
}

template<>
void fun<int , char >(int a, char b)
{
    cout<<"全特化"<<endl;
}
///< 函数模板，只有全特化，偏特化的功能可以通过函数的重载完成
```



**对类模板：**

```c++
template<typename T1, typename T2>
class Test
{
public:
    Test(T1 i,T2 j):a(i),b(j){cout<<"模板类"<<endl;}
private:
    T1 a;
    T2 b;
};

template<>
class Test<int , char>
{
public:
    Test(int i, char j):a(i),b(j){cout<<"全特化"<<endl;}
private:
    int a;
    char b;
};

template <typename T2>
class Test<char, T2>
{
public:
    Test(char i, T2 j):a(i),b(j){cout<<"偏特化"<<endl;}
private:
    char a;
    T2 b;
}

///< 对主版本模板类、全特化类、偏特化类的调⽤优先级从⾼到低进⾏排序是：全特化类>偏特化类>主版本模板类
```





## [lambda表达式](#context.27)<a name="section.27"> </a>

lambda表达式**表示⼀个可调⽤的代码单元**，没有命名的内联函数，不需要函数名因为我们直接（⼀次性的）⽤它，不需要其他地⽅调⽤它

表达式语法：

```c++
///< [捕获列表] (参数列表) -> 返回类型 { 函数体 }
///< 只有 [capture list] 捕获列表和 { function body } 函数体是必选的
///< 其中 -> return type 可省略，单行函数自动判断返回类型
[capture list] (parameter list) -> return type {function body };


///< 例：
int count = 0;
auto test = [&count](int x) {
    count += x % 3 == 0;
}

///< count == 0;
test(3);
///< count == 1;
```



| 符号        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| `[]`        | 不捕获任何变量，这种情况下 `lambda` 表达式内部不能访问外部的变量 |
| `[&]`       | 以引用方式捕获所有变量（保证 `lambda` 执行时变量存在）       |
| `[=]`       | 用值的方式捕获所有变量（创建时拷贝，修改对 `lambda` 内对象无影响） |
| `[=, &foo]` | 以引用捕获变量 `foo`，但其余变量都靠值捕获                   |
| `[&, foo]`  | 以值捕获 `foo` ，但其余变量都靠引用捕获                      |
| `[bar]`     | 以值方式捕获 `bar`，不捕获其他变量                           |
| `[this]`    | 捕获所在类的 `this` 指针                                     |



**lambda最大的⼀个优势是在使⽤STL中的算法(algorithms)库**

如数组排序：

![image-20230616102708669-1692155829457](https://cdn.jsdelivr.net/gh/WYinhui/MyPicGo@master/img/202308161644180.png)





set<>

deque

vector<>

map<>





list<>





stack<>



```
# 小端模式，高位存内存高地址，地位存低
浮点数最好用double类型(默认)而不是float

const int c = 12;			# 同c的 #define，声明常量，c++中应使用const

int xxx[3] = { 20, 1, 16 }		# 花括号直接给数组赋值

sizeof xx				# 显示xx变量长度字节，（int）加括号为类型
# char为1bytes
# 整形: char (1), short (2), int (4), long (4), long long (8)
# 浮点型: float (4), double (8), long double (80,96,128位<16字节>  /8)


# 显示个数则  xx/xx[0]
# 数组默认表示地址，不能像python一样直接打印数组

strlen()		# 显示字符串长度，需要include cstring
		# 同python的len()，只针对字符串
.size()		# 同上，需要include string

--> 字符串以空字符结尾 '\0'
--> 字符数组相反
char fish[] = "Bubbles";		
# 字符串常量直接用双引号，末尾会自动加\0，且表地址
# 字符常量用单引号，如：'s' , 且与字符串常量不能互换

面向行的输入：getline(1, 2)
# 不保存换行符！
# 1.用来存储的数组名称 2.读取的字符数(需要-1)
# 例：getline(name, 20)  读19个存到name

面向行的输入：get(1, 2)		# 保留回车
# 可用get()来读取一个字符（包括回车）
# 可用 cin.get(name, 10).get()


string类
# 可以赋值给变量

struct	结构体 --> c需要关键字struct xx  ， c++ 不需要struct
# 注意结构体赋值是加花括号 { }

union	共用体 --> 只能存int,lomg,double

enum xxx {a, b, c}		# 此时xxx为枚举
# xxx ban			# 定义ban为枚举

```



## [智能指针](#context.28)<a name="section.28"> </a>

### [定义](#context.29)<a name="section.29"> </a>

智能指针是一个类，用来存储指向动态分配对象的指针，负责自动释放动态分配的对象，防止堆内存泄漏。动态分配的资源，交给一个类对象去管理，当类对象声明周期结束时，自动调用析构函数释放资源。



使用方法

```c++
#include <iostream>
#include <memory>
using namespace std;
class TC
{
public:
    int a;    
};
int main()
{
    unique_ptr<TC> ptr(new TC());
    // ...
    TC *tc = ptr.get();  ///< 获取原指针
    ptr.reset();  ///< 析构原指针并赋值 nullptr
    ptr.reset(nullptr);  ///< 析构原指针并赋值 nullptr
    return 0;
}
```





### [shared_ptr（常用）](#context.30)<a name="section.30"> </a>

采用引用计数器的方法，允许多个智能指针指向同一个对象，每当多一个指针指向该对象时，指向该对象的所有智能指针内部的引用计数加1，每当减少一个智能指针指向对象时，引用计数会减1，当计数为0的时候会自动的释放动态分配的资源。

**注：**



### [unique_ptr](#context.31)<a name="section.31"> </a>

unique_ptr采用的是独享资源所有权，一个非空的unique_ptr总是拥有它所指向的资源。转移一个unique_ptr将会把所有权全部从源指针转移给目标指针，源指针被置空；所以unique_ptr不支持普通的拷贝和赋值操作，不能用在STL标准容器中；局部变量的返回值除外（因为编译器知道要返回的对象将要被销毁）；如果你拷贝一个unique_ptr，那么拷贝结束后，这两个unique_ptr都会指向相同的资源，造成在结束时对同一内存指针多次释放而导致程序崩溃。



### [weak_ptr](#context.32)<a name="section.32"> </a>

弱引用。 引用计数有一个问题就是互相引用形成环（环形引用），这样两个指针指向的内存都无法释放。需要使用weak_ptr打破环形引用。weak_ptr是一个弱引用，它是为了配合shared_ptr而引入的一种智能指针，它指向一个由shared_ptr管理的对象而不影响所指对象的生命周期，也就是说，它只引用，不计数。如果一块内存被shared_ptr和weak_ptr同时引用，当所有shared_ptr析构了之后，不管还有没有weak_ptr引用该内存，内存也会被释放。所以weak_ptr不保证它指向的内存一定是有效的，在使用之前使用函数lock()检查weak_ptr是否为空指针。



### [auto_ptr](#context.33)<a name="section.33"> </a>

主要是为了解决“有异常抛出时发生内存泄漏”的问题 。因为发生异常而无法正常释放内存。auto_ptr不支持拷贝和赋值操作，不能用在STL标准容器中。

`auto_ptr` 是一个失败设计，很多公司明确要求不能使用 `auto_ptr`



## [function](#context.34)<a name="section.34"> </a>

类似c的函数指针

```c++
function<int(int,int)> f1 = add;//函数指针
function<int(int,int)> f2 = divide();//函数对象类的对象
function<int(int,int)> f3 = [](int a,int b){return a*b;};//lambda表达式
```







## [decltype / auto](#context.35)<a name="section.35"> </a>

**decltype 作用：**用来**推导表达式类型**的关键字，用来在编译时期进行自动类型推导

与 `auto` 的区别：

auto 根据 = 右边的初始值推导出变量的类型，decltype 根据 exp 表达式推导出变量的类型，跟 = 右边的 value 没有关系；
auto 要求变量必须初始化，因为 auto 是根据变量的初始值来推导变量类型的，如果不初始化，变量的类型也就无法推导；
而 decltype 不要求，可不用赋值



**decltype  的几种形式**

```c++
int x = 0;
decltype(x) y = 1;            // y -> int
decltype(x + y) z = 0;        // z -> int
const int& i = x;
decltype(i) j = y;            // j -> const int&
const decltype(z) *p = &z;    // *p -> const int, p -> const int*
decltype(z) *m = &z;          // *m -> int, m -> int*
decltype(m)* n = &m;          // *n -> int*, n -> int**
```



**推导规则**

1. 如果 `exp` 是一个不被括号`()`包围的表达式，或者是一个类成员访问表达式，或者是一个单独的变量，`decltype(exp)` 的类型和 `exp` 一致

   ```c++
   #include<string> 
   #include<iostream>
   using namespace std;
    
   class A {
   public:
       static int total;
       string name;
       int age;
       float scores;
   }
    
   int A::total = 0;
    
   int main()
   {
   	int n = 0;
   	const int &r = n;
   	A a;
   	decltype(n) x = n;           // n 为 int，x 被推导为 int
   	decltype(r) y = n;           // r 为 const int &，y 被推导为 const int &
   	decltype(A::total)  z = 0;   // total 是类 A 的一个 int 类型的成员变量，z 被推导为 int
   	decltype(A.name) url = "www.baidu.com"; // url 为 string 类型
   	
   	return 0;
   }
   ```

   

2. 如果 `exp` 是函数调用，则 `decltype(exp)` 的类型就和函数返回值的类型一致

   ```c++
   int& func1(int, char);   // 函数返回值为 int&
   int&& func2(void);       // 函数返回值为 int&&
   int func3(double);       // 函数返回值为 int
    
   const int& func4(int, int, int);  // 函数返回值为 const int&
   const int&& func5(void);          // 函数返回值为 const int&&
    
   int n = 50;
   decltype(func1(100,'A')) a = n; // a 的类型为 int&
   decltype(func2()) b = 0;        // b 的类型为 int&&
   decltype(func3(10.5)) c = 0;    // c 的类型为 int
    
   decltype(func4(1,2,3)) x = n;    // x 的类型为 const int&
   decltype(func5()) y = 0;         // y 的类型为 const int&&
   ```

   

3. 如果 `exp` 是一个左值，或被括号`()`包围，`decltype(exp)` 的类型就是 `exp` 的引用，假设 `exp` 的类型为 `T`，则 `decltype(exp)` 的类型为 `T&`

   ```c++
   class A 
   {
   public:
      int x;
   }
    
   int main()
   {
   	const A obj;
   	decltype(obj.x) a = 0;   // a 的类型为 int
   	decltype((obj.x)) b = a; // b 的类型为 int&
   	 
   	int n = 0, m = 0;
   	decltype(m + n) c = 0;     // n + m 得到一个右值，c 的类型为 int
   	decltype(n = n + m) d = c; // n = n + m 得到一个左值，d 的类型为 int &
   	return 0;
   }
   /*
   左值：表达式执行结束后依然存在的数据，即持久性数据；右值是指那些在表达式执行结束不再存在的数据，即临时性数据。一个区分的简单方法是：对表达式取地址，如果编译器不报错就是左值，否则为右值。
   */
   ```

   

4. 类的静态成员可以使用 `auto`， 对于类的非静态成员无法使用 `auto`，如果想推导类的非静态成员的类型，只能使用 `decltype`

   ```c++
   template<typename T>
   class A
   {
   private :
      decltype(T.begin()) m_it;
      // typename T::iterator m_it;   // 这种用法会出错
   public:
   	void func(T& container)
   	{
   	   m_it = container.begin();
   	}
   };
    
   int main()
   {
   	const vector<int> v;
   	A<const vector<int>> obj;
   	obj.func(v);
   	return 0;
   }
   ```

   