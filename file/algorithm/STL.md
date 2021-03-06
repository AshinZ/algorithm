### C++ STL

#### 前言

STL， standard template library，中文翻译为标准模板库，其封装了很多常用的数据结构，以及很多的经常使用的算法。在算法设计的过程中，如果可以使用STL的话，可以节省很多的时间和经历在常见数据结构和算法的设计上。

此外，在$Leetcode$等$online\ \    judge$网站上，使用STL会极大的加速程序设计的过程，考虑到程序设计的过程中，会大量的使用这些模板方法，如果每次都需要去查询的话会比较耗时间，故尝试对STL的一些常见内容进行整理。

我们先看一个简单的例子，当我们需要用到一个数据结构中的**顺序表**的时候，我们常常使用数组来实现。于是我们常常会遇到**无法控制空间分配大小**的问题，也即我们不知道需要多大的数组，开小了放不下，开大了浪费空间。但是如果我们如果通过**vector**实现**顺序表**的话，我们就无需考虑这种大小的问题，其内部已经封装好了相关的内容，我们只需要使用它就可以。

STL主要由三个部分组成，算法、容器和迭代器，其中迭代器和容器常常放在一起使用。

#### 算法（Algorithms）

作用于容器的算法。

#### 容器（Containers）

主要管理某一类对象的集合。在使用时如果当前**namespace**不是**std**则不需要说明命名空间，否则要使用**std::**作为前缀。



##### array

**array**就是字面意思上的数组，是一个定长的序列容器。简单地理解，它就是在 C++ 普通数组的基础上，添加了一些成员函数和全局函数。

###### 定义

```c++
namespace std{
    template <typename T, size_t N>
    class array;
}
```

###### 引入

```c++
#include <array>
using namespace std;
```

###### 初始化

```c++
std::array<double, 10> values {0.5,1.0,1.5,,2.0};
```

其中前四个数是初始化的结果，其他的初始化为0.0。

###### 访问元素

可以通过如下两种方式访问元素：

```c++
values[4] = values[3] + 2.O*values[1];
```

```c++
values.at (4) = values.at(3) + 2.O*values.at(1);
```

其中后者可以进行越界检查，如果越界会抛出异常。

###### 成员函数

| 成员函数            | 功能                                                         |
| ------------------- | ------------------------------------------------------------ |
| begin()             | 返回指向容器中第一个元素的随机访问迭代器。                   |
| end()               | 返回指向容器最后一个元素之后一个位置的随机访问迭代器，通常和 begin() 结合使用。 |
| rbegin()            | 返回指向最后一个元素的随机访问迭代器。                       |
| rend()              | 返回指向第一个元素之前一个位置的随机访问迭代器。             |
| cbegin()            | 和 begin() 功能相同，只不过在其基础上增加了 const 属性，不能用于修改元素。 |
| cend()              | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crbegin()           | 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crend()             | 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| size()              | 返回容器中当前元素的数量，其值始终等于初始化 array 类的第二个模板参数 N。 |
| max_size()          | 返回容器可容纳元素的最大数量，其值始终等于初始化 array 类的第二个模板参数 N。 |
| empty()             | 判断容器是否为空，和通过 size()==0 的判断条件功能相同，但其效率可能更快。 |
| at(n)               | 返回容器中 n 位置处元素的引用，该函数自动检查 n 是否在有效的范围内，如果不是则抛出 out_of_range 异常。 |
| front()             | 返回容器中第一个元素的直接引用，该函数不适用于空的 array 容器。 |
| back()              | 返回容器中最后一个元素的直接应用，该函数同样不适用于空的 array 容器。 |
| data()              | 返回一个指向容器首个元素的指针。利用该指针，可实现复制容器中所有元素等类似功能。 |
| fill(val)           | 将 val 这个值赋值给容器中的每个元素。                        |
| array1.swap(array2) | 交换 array1 和 array2 容器中的所有元素，但前提是它们具有相同的长度和类型。 |

##### vector

**vector**能实现一个动态数组，我们无需考虑其空间问题。

###### 引入

```c++
#include <vector>
using namespace std;
```

###### 初始化

```c++
std::vector<double> values;//只声明容器
std::vector<int> primes {2, 3, 5, 7, 11, 13, 17, 19};//声明容器的初值
std::vector<double> values(20); //设置容器有20个位置
std::vector<double> values(20, 1.0);//设置容器内容为20个1.0
```

###### 成员函数

| 函数成员         | 函数功能                                                     |
| ---------------- | ------------------------------------------------------------ |
| begin()          | 返回指向容器中第一个元素的迭代器。                           |
| end()            | 返回指向容器最后一个元素所在位置后一个位置的迭代器，通常和 begin() 结合使用。 |
| rbegin()         | 返回指向最后一个元素的迭代器。                               |
| rend()           | 返回指向第一个元素所在位置前一个位置的迭代器。               |
| cbegin()         | 和 begin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| cend()           | 和 end() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crbegin()        | 和 rbegin() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| crend()          | 和 rend() 功能相同，只不过在其基础上，增加了 const 属性，不能用于修改元素。 |
| size()           | 返回实际元素个数。                                           |
| max_size()       | 返回元素个数的最大值。这通常是一个很大的值，一般是 232-1，所以我们很少会用到这个函数。 |
| resize()         | 改变实际元素的个数。                                         |
| capacity()       | 返回当前容量。                                               |
| empty()          | 判断容器中是否有元素，若无元素，则返回 true；反之，返回 false。 |
| reserve()        | 增加容器的容量。                                             |
| shrink _to_fit() | 将内存减少到等于当前元素实际所使用的大小。                   |
| operator[ ]      | 重载了 [ ] 运算符，可以向访问数组中元素那样，通过下标即可访问甚至修改 vector 容器中的元素。 |
| at()             | 使用经过边界检查的索引访问元素。                             |
| front()          | 返回第一个元素的引用。                                       |
| back()           | 返回最后一个元素的引用。                                     |
| data()           | 返回指向容器中第一个元素的指针。                             |
| assign()         | 用新元素替换原有内容。                                       |
| push_back()      | 在序列的尾部添加一个元素。                                   |
| pop_back()       | 移出序列尾部的元素。                                         |
| insert()         | 在指定的位置插入一个或多个元素。                             |
| erase()          | 移出一个元素或一段元素。                                     |
| clear()          | 移出所有的元素，容器大小变为 0。                             |
| swap()           | 交换两个容器的所有元素。                                     |
| emplace()        | 在指定的位置直接生成一个元素。                               |
| emplace_back()   | 在序列尾部生成一个元素。                                     |

###### 访问元素

```c++
a[n]
```

```c++
a.at(1)
```

值得注意的是，后者可以避免越界问题。

此外，我们还可以使用迭代器访问元素。

##### list



##### deque



##### pair



##### multiset



##### set



##### multimap



##### map





##### stack 



##### queue



##### priority_queue



##### string



##### bitset





#### 迭代器（iterators）

主要用于遍历容器。

