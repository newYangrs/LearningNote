## 介绍

数组是固定大小的序列容器:它们包含按严格的线性顺序排列的特定数量的元素。一个array——也就是容器类array<>的一份实体——模塑出一个static array。它包覆一个寻常的static C-style array 并提供一个STL容器接口。

通用格式：array<类型名, 元素个数> 数组名;

注意，因为长度固定，这里的元素个数不能是变量。

在内部，数组除了它所包含的元素之外不保留任何数据(甚至不保留它的大小，这是一个模板参数，在编译时固定)。就存储大小而言，它与使用该语言的括号语法([])声明的普通数组一样有效。这个类只是向它添加了一层成员和全局函数，它比寻常的数组安全，而且效率并没有因此变差，因此数组可以用作标准容器。

与其他标准容器不同，数组具有固定的大小，并且不通过分配器管理其元素的分配:它们是封装固定大小的元素数组的聚合类型。因此，它们不能动态地展开或收缩。

大小为零的数组是有效的，但是不应该取消对它们的引用(成员 front, back, 和 data)。

数组容器的另一个独特的特性是它们可以被视为元组对象:array头文件重载get函数来访问数组的元素，就好像它是一个元组一样，还有专门的tuple_size和tuple_element类型。

array并不支持（也就是不允许你指定）分配器（allocator）

## array成员函数

| 函数                                 | 功能                                                         |
| ------------------------------------ | ------------------------------------------------------------ |
| begin()，end() ,cbegin()，cend()     | 提供正向迭代器支持                                           |
| rbegin()，rend()，crbegin()，crend() | 提供反向迭代器支持                                           |
| size()                               | 返回数组大小                                                 |
| max_size()                           | 返回数组最大大小（由于array为固定序列，该函数返回值与size()相同） |
| empty()                              | 判断数组是否为空 (几乎没用)                                  |
| at()，operator[]                     | 获取数组元素                                                 |
| front()                              | 返回数组第一个元素的引用                                     |
| back()                               | 返回数组最后一个元素的引用                                   |
| data()                               | 返回指向数组对象包含的数据的指针                             |
| fill()                               | 用值填充数组                                                 |
| swap()                               | 交换两个数组元素                                             |
| get(array)                           | 返回某一个数组元素的引用                                     |

array会把元素复制到其内部的 static C-style array 中。这些元素总是拥有一个明确次序。因此 array 是一种有序集合。array 允许随机访问，也就是你可以在常量时间内直接访问任何元素， 前提是你得知道元素位置。 array 的迭代器属于随机访问迭代器，所以你可以对它运用任何STL算法。

## 成员函数用法示例

#### array用法及初始化

> template < class T, size_t N > class array;
> //T为所包含元素的类型，别名为成员类型value_type 。N为数组的大小，以元素数表示。

在使用array前，首先要添加array这个头文件，即`#include `。

注意：array<>是唯一一个无任何东西被指定为初值时，会被预初始化的容器。这意味着对于基础类型，初值可能不明确，而不是0，例如：下面定义一个有妖妖灵个int元素的数组arr：

```cpp
     std::array<int,110> arr;
1
```

上面定义的arr并未进行初始化。未初始化将会分配随机值，如图：

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191104224520637.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NTgyNjAyMg==,size_16,color_FFFFFF,t_70)
所以尽量不要定义未初始化的数组，否则访问数组元素，而该元素恰好未初始化时，可能出现意想不到的错误。

array数组对象初始化与标准数组初始化一模一样，如：

```cpp
    std::array<int,110> arr  { };//将数组所有元素初始化为0
1
    std::array<int,110> arr{1,2,3,4};//将数组前4个元素分别初始化为1，2，3，4，其余全部为0
1
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20191104230148404.png)
另外由于没有提供针对初值而写的构造函数或assignment 操作符，因此“ 在array声明期间完成初始化”是使用初值列的唯一途径。基于这个原因，你无法使用小括号指明初值（此不同于其他容器类型）

```cpp
std::array<int,5> a({1,2,3,4,5})  //错误
1
```

- **fill()**
  fiil() 函数可以用指定值给数组中所有元素赋值。

fill()函数原型如下：

> void fill(const value_type & u); //value_type为数组元素类型；

【例】

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,110> arr{1,2,4,5,6,7,8,9};
    arr.fill(3); //用3对数组所有元素赋值
    for(auto i:arr)
        cout<<i<<" ";
    cout<<endl;
    
    return 0;
}
1234567891011121314
```

输出结果为：

> 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3 3

#### array元素的获取

**特别注意！array，string，vector等容器的第一个元素下标为0！！！**

- array元素的获取可以使用下标[]（与标准数组用法相同），以及STL容器特有的at()。
  [] 与 at() 的区别在于，[]不会进行检查数字是否越界，而 at() 会进行检查（时间开销很少），如果越界则抛出 std::out_of_rang 异常。
  因此，除非确定访问没有越界，否则应该尽量使用更安全的 at() 。

> reference at(size_type n);
> const_reference at(size_type __n) const;

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,11> arr{1,2,3,4,5,6,7,8,9};
    arr.at(9) = arr.at(3) + arr.at(5);
    arr[10] = arr[1] + arr[8];
    cout<<arr.at(9)<<endl;
    cout<<arr[10]<<endl;

    //arr[11] = 6;越界访问，程序异常，但Qt Creator并未报错
    arr.at(11) = 6; //异常
    return 0;
}
12345678910111213141516
```

程序运行结果为：

> 10
> 11
>
> terminate called after throwing an instance of ‘std::out_of_range’
> what(): array::at: __n (which is 11) >= _Nm (which is 11)

- **front() 和 back()**
  front() 返回数组第一个元素的引用。
  back() 返回数组最后一个元素的引用。

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,11> arr{1,2,3,4,5,6,7,8,9};
    arr.front() = 666;
    arr.back() = 666;
    for(auto i:arr)
        cout<<i<<" ";
    cout<<endl;
    return 0;
}
1234567891011121314
```

程序运行结果为：

> 666 2 3 4 5 6 7 8 9 0 666

（注：这两个函数极少使用）

- **data()**
  返回指向数组对象中第一个元素的指针。
  该函数无参数。

```cpp
#include<iostream>
#include<array>
#include<cstring>
using namespace std;

int main()
{   
    std::array<int,10> arr{1,2,3,4,5,6,7,8,9,10};

    cout<< *arr.data()<<endl;
    cout<< *(arr.data()+6)<<endl;//获取元素

    const char* cstr = "Test string";
    std::array<char,12> charray;
    std::memcpy (charray.data(),cstr,charray.size());

    cout << charray.data() << endl;


    return 0;
}

12345678910111213141516171819202122
```

程序运行结果为：

> 1
> 7
> Test string

- **size() 和 max_size()**
  由于array在创建的时候必须明确指定大小，而且array为固定容器，因此max_size()与size()的返回值相同。
  通常使用 size() 函数。
  返回值类型为size_t 即无符号整数

使用size()可以有效避免使用for遍历数组时，发生越界的情况。同时它也使得你在写程序的时候，不必去花心思记忆数组的大小。

```cpp
    std::array<int,10> arr{1,2,3,4,5,6,7,8,9,10};

    for(unsigned int i(0);i< arr.size();++i)
        cout<< arr[i] <<" ";
    cout<<endl;
//程序运行结果为1 2 3 4 5 6 7 8 9 10
123456
```

- **empty()**
  该函数在array中为鸡肋函数。这是因为array在定义的时候已经指明了大小，不可能为空。除非你写出如此代码`std::array var;`但这毫无意义。
  然而，对于其他元素可变或者元素可删除的容器来说，它们使用 empty() 时的机制是一样的，因此为它们提供了一个一致性的操作。

#### Tuple接口

array 提供 tuple 接口。因此可以使用表达式 **tuple_size<>::value** 取得元素个数，用 **tuple_element<>::type** 取得某特定元素的类型，用 **get<>()** 取得某特定元素。

- **get()**
  get() 函数为非成员函数重载 。
  该函数返回array中指定元素的引用

函数原型如下：

> template <size_t I, class T, size_t N> T& get (array<T,N>& arr) noexcept;
> template <size_t I, class T, size_t N> T&& get (array<T,N>&& arr) noexcept;
> template <size_t I, class T, size_t N> const T& get (const array<T,N>& arr) noexcept;
> //参数l为：元素在数组中的位置，第一个元素的位置为0 。
> //参数 T为：数组中包含的元素类型（通常从arr隐式获得）。
> //参数 N为：数组的大小，以元素数为单位（通常从arr隐式获得）。

由于参数T，N可以从arr中隐式获得。因此在使用时一般不显式指定二者。
【例】

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,10> arr{1,2,3,4,5,6,7,8,9,10};

    //int a = std::get<6,int,10>(arr); 一般不这么写

    int a = std::get<6>(arr);
    cout<<a<<endl;

    std::get<5>(arr) = 666;
    for(auto i:arr)
        cout<<i<<" ";
    cout<<endl;

    return 0;
}
1234567891011121314151617181920
```

程序运行结果为：

> 7
> 1 2 3 4 5 666 7 8 9 10

另外参数l必须是明确的数字，不能使用变量。如：

```cpp
    int n = 3;
    std::get<n>(arr);//错误写法
12
```

- **tuple_size<>::value 和 tuple_element<>::type**
  在array中用途不大，示例如下：

```cpp
#include <iostream>
#include <array>
#include <string>
using namespace std;

int main()
{   

    std::array<string,5> arr = {"one","two","three","four","five"};;
    //返回元素个数
    cout<< std::tuple_size<decltype(arr)>::value <<endl;
    
    //获取第3个元素类型
    std::tuple_element<1,decltype(arr)>::type type = get<3>(arr);
    cout<< type <<endl;

    return 0;
}
123456789101112131415161718
```

程序运行结果为：

> 5
> four

#### array元素的修改

array元素的修改与标准数组用法几乎相同。
如：

```cpp
    std::array<int,10> arr{1,2,3,4,5,6,7,8,9,10};
    arr[3] = 6;
    arr[2] = arr[0] + arr[1];
123
```

同样也可以使用指针、引用对其进行修改。

不同与标准数组的是：只要两个array类型的数组的数据类型和大小相同，就可以用 = 赋值。
如：

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,3> arr_1 {1,2,3};
    std::array<int,3> arr_2 {4,5,6};

    arr_1 = arr_2; //用arr_2的元素覆盖arr_1的元素

    for(auto i:arr_1)
        cout<<i<<" ";
    cout<<endl;

    for(auto i:arr_2)
        cout<<i<<" ";
    cout<<endl;

    return 0;
}
123456789101112131415161718192021
```

输出结果为：

> 4 5 6
> 4 5 6

- **swap()**
  交换两个array的值。注：这两个数组的大小，数据类型必须相同！
  无返回值，无参数。

与标准库中的其他容器不同，交换两个数组容器是一种线性操作，涉及单独交换范围内的所有元素，这通常效率较低

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,10> arr{1,2,3,4,5,6,7,8,9,10};
    std::array<int,10> var{0,0,0,0,0,0,0,0,0,1};

    arr.swap(var);

    for(auto i:arr)
        cout<<i<<" ";
    cout<<endl;

    for(auto i:var)
        cout<<i<<" ";
    cout<<endl;

    return 0;
}

12345678910111213141516171819202122
```

运行结果为：

> 0 0 0 0 0 0 0 0 0 1
> 1 2 3 4 5 6 7 8 9 10

#### array迭代器

| 名称      | 功能                                                         |
| --------- | ------------------------------------------------------------ |
| begin( )  | 返回指向数组第一个元素的迭代器。                             |
| end()     | 返回指向数组最后一个元素后紧跟的的理论元素的迭代器。         |
| cbegin()  | 返回指向数组第一个元素的const类型迭代器。                    |
| cend()    | 返回指向数组最后一个元素后紧跟的的理论元素的const类型迭代器。 |
| rbegin()  | 返回一个反向迭代器，该迭代器指向数组的最后一个元素（即它的反向开始）。 |
| rend()    | 返回一个反向迭代器，该迭代器指向数组第一个元素之前的理论元素（该元素被视为反向元素）。 |
| crbegin() | 返回一个const类型反向迭代器，该迭代器指向数组的最后一个元素（即它的反向开始）。 |
| crend()   | 返回一个const类型反向迭代器，该迭代器指向数组第一个元素之前的理论元素（该元素被视为反向元素）。 |

下面为数组迭代器的简单用法：

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,10> arr{1,2,3,4,5,6,7,8,9,10};

    array<int,10>::iterator it_beg = arr.begin();  //或者 auto it_beg = arr.begin();
    array<int,10>::reverse_iterator rit_beg = arr.rbegin(); //或者 auto rit_brg = arr.rbegin();

    for(;it_beg!=arr.end();it_beg++)
        cout<< *it_beg * 2<<" "; //二倍输出数组所有元素
    cout<<endl;

    for(;rit_beg != arr.rend();rit_beg++)
        cout<< *rit_beg << " "; //倒序输出数组元素
    cout<<endl;

    return 0;
}
123456789101112131415161718192021
```

输出结果为：

> 2 4 6 8 10 12 14 16 18 20
> 10 9 8 7 6 5 4 3 2 1

在使用时最好用全局的 begin() 和 end() 函数从容器中获取迭代器，因为它们是通用的。
如上述`array::iterator it_beg = arr.begin();` 可以写为`array::iterator it_beg = std::begin(arr);`

#### array元素的比较

可以用任何比较运算符比较两个数组容器，只要它们有相同的大小，保存的是相同类型的元素，而且这种类型的元素还要支持比较运算符。示例如下：

```cpp
#include<iostream>
#include<array>
using namespace std;

int main()
{   
    std::array<int,3> arr_1 {1,2,3};
    std::array<int,3> arr_2 {1,2,3};
    std::array<int,3> arr_3 {1,3,2};

    if (arr_1 == arr_2)
        cout << "arr_1 = arr_2" << endl;
    if (arr_1 != arr_3)
        cout << "arr_1 != arr_3"<< endl;
    if (arr_1 < arr_3)
        cout << "arr_1 < arr_3"<< endl;

    return 0;
}
12345678910111213141516171819
```

输出结果为：

> arr_1 = arr_2
> arr_1 != arr_3
> arr_1 < arr_3

容器被逐元素地比较。对 ==,如果两个数组对应的元素都相等，会返回 true。对于 !=，两个数组中只要有一个元素不相等，就会返回 true。这也是字典中单词排序的根本方式，两个单词中相关联字母的不同决定了单词的顺序。