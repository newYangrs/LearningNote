# volitile 和 restict 关键字

volatile 可能我们用的都比较多也比较常见吧，主要就是告诉编译器，每次在使用volatile指定的变量时总是重新去获取他的值，更简单的理解我是这样的，

为了计算某个内容地址所存的内容会把他放入CPU寄存器，为了优化，下一次就直接从寄存器里取值了
volatile就是为了告诉编译器，不管什么情况你都要去内存里重新获取他的内容！

c99中新增加了一个类型定义，就是restrict。 看了下网上的相关贴子，但还是问题解决的不够。下面是相关一个文章，我将在后面再加相关说明：
那么restrict的意义是什么呢？

概括的说，关键字restrict只用于限定指针；该关键字用于告知编译器，所有修改该指针所指向内容的操作全部都是基于(base on)该指针的，即不存在其它进行修改操作的途径；这样的后果是帮助编译器进行更好的代码优化，生成更有效率的汇编代码。

举个简单的例子

```c
int foo (int* x, int* y){
    *x = 0;
    *y = 1;
    return *x;
}
```

很显然函数foo()的返回值是0，除非参数x和y的值相同。可以想象，99％的情况下该函数都会返回0而不是1。然而编译起必须保证生成100%正确的代码，因此，编译器不能将原有代码替换成下面的更优版本

```c
int f (int* x, int* y){
    *x = 0;
    *y = 1;
    return 0;
}
```

啊哈，现在我们有了restrict这个关键字，就可以利用它来帮助编译器安全的进行代码优化了

```c
int f (int *restrict x, int *restrict y){
    *x = 0;
    *y = 1;
    return *x;
}
```

此时，由于指针 x 是修改 *x的唯一途径，编译起可以确认 “*y=1; ”这行代码不会修改 *x的内容，因此可以安全的优化为

```c
int f (int *restrict x, int *restrict y){
    *x = 0;
    *y = 1;
    return 0;
}
```

最后注意一点，restrict是C99中定义的关键字，C++目前并未引入；在GCC可通过使用参数" -std=c99"
来开启对C99的支持



下面是我从C语言核心技术一书上摘的：
void *memcpy( void * restrict dest ,const void * restrict src,sizi_t n) 这是一个很有用的内存复制函数，由于两个参数都加了restrict限定，所以两块区域不能重叠，即 dest指针所指的区域，不能让别的指针来修改，即src的指针不能修改. 相对应的别一个函数 memmove(void *dest,const void * src,size_t)则可以重叠。