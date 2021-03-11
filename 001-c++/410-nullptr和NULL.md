# nullptr和NULL详解



转自：https://blog.csdn.net/u012707739/article/details/77915483

### C中的NULL

在C语言中我们将NULL用作空指针之用，NULL在C语言中的定义如下：

```
#define NULL    ((void *)0)
1
```

可以看到，C语言中NULL是一个void*指针，我们常用其来对指针变量进行初始化赋值或者作为返回类型为指针的函数的返回值（如函数执行失败时）等等。

### C++中的NULL

在C++中，NULL的定义如下：

```
/* Define NULL pointer value */
#ifndef NULL
    #ifdef __cplusplus
        #define NULL    0
    #else  /* __cplusplus */
        #define NULL    ((void *)0)
    #endif  /* __cplusplus */
#endif  /* NULL */
12345678
```

可以看到，在C++中，NULL被定义为0，而不是(void*)0，为什么C++在NULL的定义上不继续兼容C延续(void*)0的定义呢？因为C++中不能将`void*`类型的指针隐式转换成其他指针类型，所以将NULL定义为(void*)0的话并不能起到空指针的作用，如以下代码：

```
int main() {
    void* a = (void*) 0;
    char* b = a;     //在C中可以，C++不行
    return 0;
}
12345
```

因为C++中不能将`void*`类型的指针隐式转换成其他指针类型，所以用`(void*)0`对其他类型指针赋初值是不行的。既然`(void*)0`不能起到空指针的作用，不如干脆将NULL定义为0，引入0来表示空指针，可以对各种类型的指针进行赋值。

### C++中的nullptr

用过C++的童鞋都知道C++中有个nullptr的关键字可以用作空指针，既然已经有了定义为0的NULL，为何还要nullptr呢？这是因为定义为0的NULL很容易引起混淆，尤其是函数重载调用时，比如说：

```
void hello(char* a) {  //C++支持函数名重载，C不支持
    printf("char\n");
}
void hello(int a) {
    printf("int\n");
}

int main() {
    hello（NULL）;    //调用hello(int a)函数
    return 0;
}
1234567891011
```

如果在C++中还是用NULL来用作空指针的话，我们会以为hello(NULL)调用的是hello(char* a)，但实际上，因为NULL定义为0，所以调用的是hello(int a)。所以为了避免这种混淆，C++定义了nullptr关键字用作空指针。

### nullptr的使用

nullptr关键字用于标识空指针，是std::nullptr_t类型的（constexpr）变量。它可以转换成任何指针类型和bool布尔类型（主要是为了兼容普通指针可以作为条件判断语句的写法），但是不能被转换为整数。

```
char *p1 = nullptr;     // 正确
int  *p2 = nullptr;     // 正确
bool b = nullptr;       // 正确. if(b)判断为false
int a = nullptr;        // 错误
1234
```

注意nullptr和NULL以及0在作为条件判断时值都为false，它们两两之间进行等于（==）判断时值为true。