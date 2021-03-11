声明一个int型的list：list<int> a；

**1、list的构造函数**



```cpp
list<int>a{1,2,3}list<int>a(n)    //声明一个n个元素的列表，每个元素都是0list<int>a(n, m)  //声明一个n个元素的列表，每个元素都是mlist<int>a(first, last)  //声明一个列表，其元素的初始值来源于由区间所指定的序列中的元素，first和last是迭代器
```


**2、begin()和end()**



通过调用list容器的成员函数begin()得到一个指向容器起始位置的iterator，可以调用list容器的end()函数来得到list末端下一位置

 

**3、push_back()和push_front()**

使用list的成员函数push_back和push_front插入一个元素到list中。其中push_back()是从list的末端插入，而push_front()是从list的头部插入。

 

**4、empty()**

判断list是否为空

 

**5、resize()**

调用resize(n)将list的长度改为只容纳n个元素，超出的元素将被删除。如果n比list原来的长度长，那么默认超出的部分元素置为0。也可以用resize(n, m)的方式将超出的部分赋值为m。

例子：



```cpp
list<int>b{1, 2, 3, 4};b.resize(2);list中输出元素：1,2list<int>b{1, 2, 3, 4};b.resize(6);list中输出元素：1,2,3,4,0,0list<int>b{1, 2, 3, 4};b.resize(6,9);list中输出元素：1,2,3,4,9,9
```





**6、clear()**

清空list中的所有元素

 

**7、front()和back()**

通过front()可以获得list容器中的头部元素，通过back()可以获得list容器的最后一个元素。注意：当list元素为空时，这时候调用front()和back()不会报错。因此在编写程序时，最好先调用empty()函数判断list是否为空，再调用front()和back()函数。

 

**8、pop_back()和pop_front()**

使用pop_back()可以删掉尾部第一个元素，pop_front()可以删掉头部第一个元素。注意：list必须不为空，如果当list为空的时候调用pop_back()和pop_front()会使程序崩掉。

 

**9、assign()**

有两种使用情况：

（1）a.assign(n, val):将a中的所有元素替换成n个val元素

例如：



```cpp
list<int>b{1,2,3,4,5};b.assign(5,10);
```

b中的元素变为10, 10, 10, 10, 10



（2）a.assign(b.begin(), b.end())



```cpp
list<int>a{6,7,8,9};list<int>b{1,2,3,4,5};b.assign(a.begin(),a.end());
```



b中的元素变为6,7,8,9

 

**10、swap()**

交换两个链表。a.swap(b)和swap(a, b)，都可以完成a链表和b链表的交换。

例子：



```cpp
list<int>a{6,7,8,9};list<int>b{1,2,3,4,5};swap(a, b);  //或a.swap(b)
```



a中元素变为1,2,3,4,5

b中元素变为6,7,8,9

 

**11、reverse()**

可以实现list的逆置

例子：



```cpp
list<int>b{1,2,3,4,5};reverse(b.begin(),b.end());
```



b中元素变为5，4，3，2，1

 

**12、merge()**

a.merge(b) 调用结束后b变为空，a中元素包含原来a和b的元素。

例子：



```cpp
list<int>a{6,7,8,9};list<int>b{2, 1, 3, 6, 5};a.merge(b,greater<int>());
```



a中元素变为：6,7,8,9,2,1,3,6,5

 



```cpp
list<int>a{6,7,8,9};list<int>b{2, 1, 3, 6, 5};a.merge(b);
```



a中元素变为：2,1,3,6,5,6,7,8,9

 

**13、insert()**

在指定位置插入一个或多个元素



```cpp
a.insert(a.begin(),100);  //在a的开始位置（即头部）插入100a.insert(a.begin(),2, 100);   //在a的开始位置插入2个100a.insert(a.begin(),b.begin(), b.end());//在a的开始位置插入b从开始到结束的所有位置的元素
```


**14、erase()**



删除一个元素或一个区域的元素



```cpp
a.erase(a.begin());  //将a的第一个元素删除a.erase(a.begin(),a.end());  //将a的从begin()到end()之间的元素删除。
```





**15、remove()函数**

从list中删除元素



```cpp
list<int>a{6,7,8,9,7,10};a.remove(7);
```



删除了a中所有值为7的元素，此时a中元素为6,8,9,10

 

**16、remove_if()函数**

括号中可以传入

（1）回调函数

回调函数的原型为boolisRemove(T &obj1);

函数名任意，如果obj1需要被移除则返回1，否则返回0

使用方法：list.remove_if(isRemove)

这种方法最简单，但是无法向回调函数中传递参数，每一个条件就要有一个回调函数，因此不推荐使用

（2）创建用于比较的类，传入类名及初始化参数

用于比较的类必须重载bool operator()(T &obj1)方法，如果obj1需要被移除则返回1，否则返回0.

用于比较的类还应当包含必要的构造函数，用于传递参数。

使用方法：list.remove_if(classname(args))

例1：



```cpp
bool is_odd(constint& value){    return (value==4);} int main(){    list<int> a{6,7,4,9,7,10};    a.remove_if(is_odd);     list<int>::iterator it = a.begin();    while(it != a.end()){        cout<<*it<< " ";        it++;    }      return 0;}
```





输出：

6 7 9 7 10

 

例2：



```cpp
class single_digit{public:    bool operator()(const int& value){        return (value<10);    }}; int main(){    list<int> a{6,7,4,9,7,10};   a.remove_if(single_digit());     list<int>::iterator it = a.begin();    while(it != a.end()){        cout<<*it<<" ";        it++;    }      return 0;}
```





输出：

10









# 一. 简介

- list是一种序列式容器。list容器完成的功能实际上和数据结构中的**双向链表**是极其相似的。
- list中的数据元素是通过链表指针串连成逻辑意义上的线性表，也就是list也具有链表的主要优点，即：在链表的任一位置进行元素的插入、删除操作都是快速的。
- list的实现大概是这样的：list的每个节点有三个域：前驱元素指针域、数据域和后继元素指针域。
  前驱元素指针域保存了前驱元素的首地址；数据域则是本节点的数据；后继元素指针域则保存了后继元素的首地址。
- 其实，list和**循环链表**也有相似的地方，即：头节点的前驱元素指针域保存的是链表中尾元素的首地址，list的尾节点的后继元素指针域则保存了头节点的首地址，这样，list实际上就构成了一个双向循环链。
- 由于list元素节点并不要求在一段连续的内存中，显然在list中是不支持快速随机存取的，因此对于迭代器，只能通过“++”或“--”操作将迭代器移动到后继/前驱节点元素处。而不能对迭代器进行+n或-n的操作，这点，是与vector等不同的地方。

 

# 二. 使用方法

## 1. 构造函数

list() 声明一个空列表；

list(n) 声明一个有n个元素的列表，每个元素都是由其默认构造函数T()构造出来的

list(n,val) 声明一个由n个元素的列表，每个元素都是由其复制构造函数T(val)得来的

list(n,val) 声明一个和上面一样的列表

list(first,last) 声明一个列表，其元素的初始值来源于由区间所指定的序列中的元素

## 2. 其他常用函数

```c++
assign() 给list赋值 
back() 返回最后一个元素 
begin() 返回指向第一个元素的迭代器 
clear() 删除所有元素 
empty() 如果list是空的则返回true 
end() 返回末尾的迭代器 
erase() 删除一个元素 
front() 返回第一个元素 
get_allocator() 返回list的配置器 
insert() 插入一个元素到list中 
max_size() 返回list能容纳的最大元素数量 
merge() 合并两个list 
pop_back() 删除最后一个元素 
pop_front() 删除第一个元素 
push_back() 在list的末尾添加一个元素 
push_front() 在list的头部添加一个元素 
rbegin() 返回指向第一个元素的逆向迭代器 
remove() 从list删除元素 
remove_if() 按指定条件删除元素 
rend() 指向list末尾的逆向迭代器 
resize() 改变list的大小 
reverse() 把list的元素倒转 
size() 返回list中的元素个数 
sort() 给list排序 
splice() 合并两个list 
swap() 交换两个list 
unique() 删除list中重复的元素
```

 

# 三. list、vector、deque对比

## 1. vector

vector和built-in数组类似，拥有一段连续的内存空间，能非常好的支持随即存取，即[]操作符。存储时间复杂度为O(1)

但由于它的内存空间是连续的，所以在中间进行插入和删除会造成内存块的拷贝。插入删除操作时间复杂度O(n)。另外，当插入较多的元素后，预留内存空间可能不够，需要重新申请一块足够大的内存并把原来的数据拷贝到新的内存空间。这些影响了vector的效率，但是实际上用的最多的还是vector容器，建议大多数时候使用vector效率一般是不错的。

## 2. list

list就是数据结构中的双向链表，因此它的内存空间是不连续的，通过指针来进行数据的访问，这个特点使得它的随即存取变的非常没有效率（时间复杂度O(n)），因此它没有提供[]操作符的重载。但由于链表的特点，它可以以很好的效率支持任意地方的删除和插入。

## 3. deque

deque是一个double-ended queue，它的具体实现不太清楚，但知道它具有以下两个特点：它支持[]操作符，也就是支持随即存取，并且和vector的效率相差无几，它支持在两端的操作：push_back,push_front,pop_back,pop_front等，并且在两端操作上与list的效率也差不多。

## 4. 应用场景

因此在实际使用时，如何选择这三个容器中哪一个，应根据你的需要而定，具体可以遵循下面的原则：
①如果你需要高效的随即存取，而不在乎插入和删除的效率，使用vector。
②如果你需要大量的插入和删除，而不关心随机存取，则应使用list。
③如果你需要随即存取，而且关心两端数据的插入和删除，则应使用deque。

 

# 四. forward_list

- 与其他的基本标准序列容器（array，vector和deque）相比，forward_list一般在容器内的任何位置中的元素的插入、提取和移动操作效率更高，因此在算法中较密集的使用这些操作，例如排序算法。
- **list和forward_list的主要区别在于：forward_list是传统数据结构意义中的单链表，而list是双链表。**
- 与其他的基本标准序列容器（array，vector和deque）相比，forward_list一般在容器内的任何位置中的元素的插入、提取和移动操作效率更高，因此在算法中较密集的使用这些操作，例如排序算法。
- 相比其他序列容器，forward_lists的主要缺点是缺乏直接访问他们的位置的元素，例如，要进入第六个元素在forward_list的一个遍历从一开始就到那个位置，这需要线性时间之间的距离。他们也消耗了一些额外的内存保持连接信息相关联的每个元素（这可能是一个小型的元素的大链表的重要因素）。

## 1. 构造、复制与析构

```cpp

	forward_list<Elem> c //默认构造函数；创建一个空
    forward_list forward_list<Elem> c(c2) //复制构造函数；创建一个新的forward_list作为c2的副本（所有元素都被复制） 
    forward_list<Elem> c = c2 //复制构造函数；创建一个新的forward_list作为c2的副本（所有元素都被复制） 
    forward_list<Elem> c(rv) //移动构造函数；使用右值对象rv创建一个新forward_list 
    forward_list<Elem> c = rv //移动构造函数；使用右值对象rv创建一个新forward_list 
    forward_list<Elem> c(n) //使用默认构造函数创建含有n个元素的forward_list 
    forward_list<Elem> c(n,elem) //创建一个forward_list，并使用n个elem进行初始化 
    forward_list<Elem> c(beg,end) //创建一个forward_list，并使用beg到end范围内的值进行初始化 
    forward_list<Elem> c(initlist) //创建一个forward_list，并使用初始化列表进行初始化 
    forward_list<Elem> c = initlist //创建一个forward_list，并使用初始化列表进行初始化 
    c.~forward_list() //销毁所有元素并释放内存
```

## 2. 非变动性操作

```cs
	c.empty() //判断容器是否为空 
    c.max_size() //返回可容纳的元素最大数量 
    c1 == c2 //判断c1与c2是否相等 
    c1 != c2 //判断c1与c2是否不相等，等同于!(c1==c2) 
    c1 < c2 //判断c1是否小于c2 
    c1 > c2 //判断c1是否大于c2 
    c1 <= c2 //判断c1是否小于等于c2 
    c1 >= c2 //判断c1是否大于等于c2
```

## 3. 赋值

```cs
	c = c2 //将c2所有元素赋值给c 
    c = rv //将右值对象rv的所有元素移动赋值给c 
    c = initlist //使用初始化列表进行赋值 
    c.assign(initlist) //使用初始化列表进行赋值 
    c.assign(n,elem) //使用n个elem元素进行赋值 
    c.assign(beg,end) //使用beg到end范围内的元素进行赋值 
    c1.swap(c2) //交换c1和c2的数 
    swap(c1,c2) //交换c1和c2的数
```

## 4. 元素存取

```cpp
	c.front() //返回第一个元素，不检查第一个元素是否存在
```

## 5. 迭代器相关函数

```cpp
	c.begin() //返回一个双向迭代器，指向第一个元素 
    c.end() //返回一个双向迭代器，指向最后一个元素
    c.cbegin() //返回一个双向常迭代器，指向第一个元素 
    c.cend() //返回一个双向常迭代器，指向最后一个元素 
    c.before_begin() //返回一个前向迭代器，指向第一个元素之前的位置 
    c.cbefore_begin() //返回一个前向常迭代器，指向第一个元素之前的位置
```

## 6. 插入和移除元素

```cpp
	c.push_front(elem) //在头部添加一个elem副本 
    c.pop_front() //移除头部元素（但不回传） 
    c.insert_after(pos,elem) //在迭代器位置之后插入一个elem副本，并返回新元素的位置 
    c.insert_after(pos,n,elem) //在迭代器位置之后插入n个elem副本，并返回第一个新元素的位置；若无新插入值，返回原位置 
    c.insert_after(pos,beg,end) //在迭代器位置之后插入范围beg到end的所有元素的副本，并返回第一个新元素的位置；若无新插入值，返回原位置 
    c.insert_after(pos,initforward_list) //在迭代器位置之后插入初始化列表的所有元素的副本，并返回第一个新元素的位置；若无新插入值，返回原位置 
    c.emplace_after(pos,args...) //在迭代器位置之后插入一个使用args初始化的元素副本，并返回新元素的位置 
    c.emplace_front(args...) //在头部添加一个使用args初始化的元素副本，无返回值 c.erase_after(pos) //移除迭代器位置的元素，无返回值 
    c.erase_after(beg,end) //移除beg到end范围内的所有元素，无返回值 
    c.remove(val) //移除所有值为val的元素 
    c.remove_if(op) //移除所有满足op条件的元素 
    c.resize(num) //将元素数量设为num（如果size()增大，多出来的元素使用默认构造函数创建） 
    c.resize(num,elem) //将元素数量设为num（如果size()增大，多出来的元素都是elem的副本） 
    c.clear() //移除所以元素，清空容器
```

## 7. 特殊修改操作

```cpp
	c.unique() //若存在相邻而数值相等的元素，移除重复元素 
    c.unique(op) //若存在相邻而数值相等的元素，且满足op条件时，移除重复元素 
    c.splice_after(pos,c2) //将c2内的所有元素转移到c1内pos所指的位置之后 
    c.splice_after(pos,c2,c2pos) //将c2内c2pos之后的元素转移到c1内pos所指的位置之后 
    c.splice_after(pos,c2,c2beg,c2end) //将c2内从c2beg到c2end区间内的所有元素转移到c1内pos所指的位置之后 
    c.sort() //以operator<为准则，对所有元素排序 
    c.sort(op)  //以op为准则，对所有元素排序 
    c.merge(c2) //假设c1和c2都包含已序元素，将c2的全部元素转移到c1.并保证合并后的forward_list仍为已序 
    c.merge(c2,op) //假设c1和c2都包含op原则下已序元素，将c2的全部元素转移到c1.并保证合并后的forward_list在op原则下仍为已序 
    c.reverse() //将所有元素反序
```

 