# 一、简介

> vector，是同一类型的对象的集合，这一集合可看作可变大小的数组，是容器的一种。

1. 对于容器来说，其重要特性之一便是于可以**在运行时高效地添加元素**。

2. 类似于数组，vector采用连续内存地址来存储元素，因此vector属于顺序容器。也就意味着可以采用下标对vector的元素进行访问，

   和数组一样高效

   ；同时它

   比数组更加灵活

   ，它的

   大小

   （size）是可以动态改变的，且它的大小会被容器自动处理。

   - vector分配空间策略：vector会分配一些额外的空间以适应可能的增长，使得分配的存储空间（capacity）比实际需要的存储空间更大。不同的库采用不同的策略权衡空间的使用和重新分配，vc里是每次增大当前capacity的一半。但是无论如何，重新分配总是对数增长的间隔大小，在末尾插入一个元素的时候则能够在常数时间的复杂度完成的。

3. 在这里应理解好

   size

   与

   capacity

   之间的关系，这将有助于理解

   ```
   vec.resize()
   ```

   与

   ```
   vec.reserve()
   ```

   的区别：

   - 容器的capacity是其size的上界，size总是<=capacity；当所需求的size>当前capacity时，vector便会如上文所述一般对capacity进行动态分配，以满足目标size需求。
   - 我们使用`[]`操作符时，只能访问size大小内的容器空间，这才是真正存在对象的内存空间；而>size同时<capacity的内存则属于“野”内存。

4. 与其它动态序列容器相比（deques, lists and forward_lists）， vector在访问元素的时候更加高效，在末尾添加和删除元素相对高效。对于其它不在末尾的删除和插入操作，效率更低。比起lists和forward_lists统一的迭代器和引用更好。

# 二、用法

## 1. 头文件[#](https://www.cnblogs.com/Bylight/p/10409974.html#18852218)

```
Copy#include<vector>
```

## 2. vector的声明及初始化[#](https://www.cnblogs.com/Bylight/p/10409974.html#3962762656)

**（1）不带参数的构造函数初始化**

```
Copy// 初始化一个size为0的vector
vector<int> vec;
```

**（2）带参数的构造函数初始化**
仅指定vector大小，此时每个元素值为默认值0

```
Copyvector<int> vec(10);    //初始化了10个默认值为0的元素
```

指定vector大小和元素初始值

```
Copyvector<int> vec(10，1); //初始化了10个值为1的元素
// 或是
vettor<int> vec = {1, 2, 3}; //初始化了1，2，3这3个元素
```

**（3）通过同类型的vector初始化**

```
Copyvector<int> temp(5,1);
// 通过temp容器初始化一个元素相同的vec向量
vector<int> vec(temp);
```

通常来说，前三种便足够我们平时使用了。

**（4）通过数组地址初始化**

```
Copyint a[5] = {1,2,3,4,5};
// 以数组a的元素初始化vector，注意地址是从0到5（左闭右开区间）
vector<int> vec(a, a+5);
```

**（5）通过insert函数初始化**
使用同类型的vector以及insert函数初始化

```
Copy// insert初始化方式将同类型的迭代器对应的始末区间（左闭右开区间）内的值插入到vector中
vector<int> temp(6,6);
vecot<int> vec;
// 将temp[0]~a[2]插入到vec中，vec.size()由0变为3
vec.insert(vec.begin(), temp.begin(), temp.begin() + 3);
```

使用数组以及insert函数初始化

```
Copyint a[6] = {6,6,6,6,6,6};
vector<int> vec;
// 将a的所有元素插入到vec中
vec.insert(vec.begin(), a, a+7);
```

通过insert函数添加m个值为n的元素

```
Copy// 在vec开始位置处插入6个1
vec.insert(vec.begin(), 6, 1);
```

**（6）通过copy函数赋值**

```
Copyvector<int> vec(5,1);
int a[5] = {2,2,2,2,2};
vector<int> target(10);

// 将vec中元素全部拷贝到target开始的位置中,注意拷贝的区间为vec.begin() ~ vec.end()的左闭右开的区间
copy(vec.begin(), vec.end(), target.begin());

// 拷贝区间也可以是由数组地址构成的区间
copy(a, a+5, vec.begin() + 5);
```

## 3. vector基本操作[#](https://www.cnblogs.com/Bylight/p/10409974.html#1112456770)

### 1）容量相关[#](https://www.cnblogs.com/Bylight/p/10409974.html#1621689515)

- 容器目前大小：`vec.size()`
- 容器目前容量：`vec.capacity()`
- 容器最大允许容量：`vec.max_size();`
- 判断容器是否为空：`vec.empty()`
- 请求容器capacity减少至size大小：`vec.shrink_to_fit()`
- 更改容器容量：`vec.reverse(size_type n)`
- 更改容器大小
  - 目标size小于当前size则截取前目标size个元素；大于则以元素填充存储空间至目标size
  - `vec.resize(size_type n)`仅指定size修改后大小n，需元素填充则以默认值0填充
  - `vec.resize(size_type n, value_type val)`指定目标size大小n以及填充元素的值val

### 2）修改元素[#](https://www.cnblogs.com/Bylight/p/10409974.html#149846267)

- 末尾添加元素：`vec.push_back(value_type val)`

- 末尾删除元素：`vec.pop_back()`

- 对容器赋值：

  - `vec.assign(const_iterator first, const_iterator last)`将同类型容器目标区间[first, last)内的元素赋给调用者
  - `vec.assign(size_type n, const T& x = T())`将n个x赋给调用者

- 在指定位置插入元素：

  - `vec.insert(const_iterator position, value_type& val)`在指定位置position插入元素val
  - `vec.insert(const_iterator position, size_type n, value_type& val)`在指定位置position插入n个元素val
  - `vec.insert(const_iterator position, InputIterator first, InputIterator last)`在指定位置插入同类型容器目标区间内的元素

- 在指定位置删除元素

  - `vec.erase(const_iterator position)`删除指定位置的素
  - `vec.erase(const_iterator first, const_iterator last)`删除指定区间内的元素

- 与另一个容器交换元素：`vec.swap(vector& x)`

- 清空容器元素：

  ```
  vec.clear()
  ```

  - 调用这个方法后，vec的size置为0但capacity不一定会重新分配

### 3）使用迭代器[#](https://www.cnblogs.com/Bylight/p/10409974.html#2823986540)

- 声明迭代器(类似指针)：

  ```
  vector<size_type>::Iterator i
  ```

  - 访问迭代器指向的元素：`*i`

- 可写迭代器

  - 指向容器开头：`vec.begin()`
  - 指向容器结尾(指向最后一个元素再往后的一个内存)：`vec.end()`

- 只读迭代器(不能通过该指针来修改元素)

  - 指向容器开头：`vec.cbegin()`
  - 指向容器结尾：`vec.cend()`

### 4）访问元素[#](https://www.cnblogs.com/Bylight/p/10409974.html#318875002)

- 访问指定位置元素。通过下面两个方法的比较可以看到，我们平时应优先使用

  ```
  vec.at(i)
  ```

  :

  - `vec[i]`下标访问。不会下标检查是否越界，越界时返回一串无规律整形。
  - `vec.at(i)`通过at函数访问。如果越界会抛出*out of range*的异常

- 访问第一个元素：`vec.front()`

- 访问最后一个元素：`vec.back()`

- 返回一个元素组成的数组的指针：`int* p = vec.data()`

## 4. 常用操作[#](https://www.cnblogs.com/Bylight/p/10409974.html#3571855519)

### 1）遍历元素[#](https://www.cnblogs.com/Bylight/p/10409974.html#1359859338)

```
Copy```
vector<int>::iterator it;
for (it = vec.begin(); it != vec.end(); it++) {
    cout << *it << endl;
}    
//或者
for (size_t i = 0; i < vec.size(); i++) {
    cout << vec.at(i) << endl;
}
​```
```

### 2）元素翻转[#](https://www.cnblogs.com/Bylight/p/10409974.html#1634922232)

```
Copy```
#include <algorithm>
reverse(vec.begin(), vec.end());
​```
```

### 3）元素排序[#](https://www.cnblogs.com/Bylight/p/10409974.html#445225355)

```
Copy```
#include <algorithm>
sort(vec.begin(), vec.end()); //采用的是从小到大的排序
//如果想从大到小排序，可以采用上面反转函数，也可以采用下面方法:
bool Comp(const int& a, const int& b) {
    return a > b;
}
sort(vec.begin(), vec.end(), Comp);
​```
```

------

文章最后，我个人认为头文件``中的`reverse()`函数写得特别优雅，在此贴出源码

```
Copytemplate <class BidirectionalIterator>
void reverse (BidirectionalIterator first, BidirectionalIterator last)
{
  while ((first != last) && (first != --last)) {
    std::iter_swap (first,last);
    ++first;
  }
}
```