# [typedef 和define的区别](http://www.cnblogs.com/li-peng/p/3853818.html)

总结一下typedef和#define的区别

**1.概念**

 \#define 它在编译预处理时进行简单的替换，不作正确性检查。它是预处理指令。

 typedef 它在自己的作用域内给一个已经存在的类型一个别名。它是在是在编译时处理的。

　　例子：

\#define:

```
#define DO double
DO val =5;//相当于 double va =5;

#define FUN(X)  X+5
int va = FUN(3); //相当于 va = 3+5
```

typedef:　　

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0))

```
typedef double D;//就是给double起一个别名D
D val;//相当于是double val

//用dypedef定义数组
typedef double arr[3];//arr就是一个长度为3的double类型数组
arr arr1;   //相当于 double arr1[3]

//用dypedef定义函数指针
typedef void (*fun)();//表示fun是一个指向 void类型的指针函数

void StrGet() {
    qDebug()<<"str";
}

fun f1=StrGet;
f1();
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0))

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**2.区别**

　　**执行时间不一样**
　　　　#define 是在预处理阶段，也就是在编译之前进行替换工作
　　　　typedef 是在编译阶段，会进行类型检查

　　**作用域不同**
　　　　#define 无论在哪定义过，在其它的地都可以使用
　　　　typedef 只能在定义的作用域内使用

　　**对指针的操作不一样**

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0))

```
#define PINT int *
    typedef int * pint;

    int a =3;
    int b =5;
    const pint p1=&a; //相当于 int * const p1;  p1 不可以更改但指向的内容能被修改
    *p1=1;
    p1=&b;  //error
    const PINT p2 =&a;//相当于 const int * p2;  p2 可以被修改但指向的内容不可以修改
    p2=&b;
    *p2=1;  //error
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0))

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

