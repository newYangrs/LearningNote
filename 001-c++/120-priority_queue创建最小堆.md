# [用priority_queue创建最小堆](https://www.cnblogs.com/cs0915/p/12986279.html)

标准库里的priority_queue默认创建的是最大堆，要创建最小堆的话，需要下面的形式。

```
1 priority_queue<int,vector<int>,greater<int>> minheap;//最小堆
2 priority_queue<int,vector<int>,less<int>> maxheap;//最大堆
```

默认的就是最大堆，可以直接像下面那样写

```
priority_queue<int> maxheap;//最大堆
```

例子：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 #include <iostream>
 2 #include <queue>
 3 using namespace std;
 4 int main()
 5 {
 6     priority_queue<int,vector<int>,greater<int>> pqueue;
 7     int a[] = { 2,14,53,1,45 };
 8     for (int i = 0; i < 5; i++)
 9     {
10         pqueue.push(a[i]);
11     }
12     for (int i = 0; i < 5; i++)
13     {
14         cout << pqueue.top() << endl;
15         pqueue.pop();
16     }
17     return 0;
18 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

![img](https://img2020.cnblogs.com/blog/1881355/202005/1881355-20200529105758886-875977049.png)

 

 

对于自己构建的类型，还需要对运算符进行重载。需要最小堆的时候，需要对大于号重载；需要最大堆的时候，需要对小于号重载。

比如需要最小堆

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
struct Node {
     double value;
     int idx;
     Node (double v, int i): value(v), idx(i) {}
     friend bool operator > (const struct Node &n1, const struct Node &n2) {             
     return n1.value > n2.value;
     } 
};
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
//新建一个最小堆 
 priority_queue<Node, vector<Node>, greater<Node>> minheap;
```