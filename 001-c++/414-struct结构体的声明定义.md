```c++
/* struct.c
*  本程序演示了struct的种种写法。注意，所有这些写法都能编译通过
*  author: smileonce
*  date  : 2005-01-14
*/


// 这种写法是最要命的，但是它却可以通过, 最后面那个foo是结构体的实例
struct foo
{
    int foo;
} foo;

// 有了typedef，意义明显与上面不同，注意的是struct后面那个标签可以不写，最后面那个bar是匿名结构体的别名
typedef struct 
{
    int bar;
} bar;

// 这种写法是最没人用的，因为它的定义是一次性的,struct1是一个匿名struct的实例
struct 
{
    int aaa;
} mystruct1;

//最常用的写法，清晰不言自明，my_struct_tag是结构体标签，my_struct_type是struct my_struct_tag的别名
typedef struct my_struct_tag
{
    int aaa;
} my_struct_type;

// 下面两种定义的写法都可以，当然使用typedef定义的写法更简洁些
my_struct_type mystruct2;             //通过typedef定义的结构别名来定义变量
struct my_struct_tag mystruct3;       //通过结构标签定义变量


int main(int argc, char *argv[])
{
 	return 0;
}
```

