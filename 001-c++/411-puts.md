# [puts()_C语言](https://www.cnblogs.com/zhangyongjian/p/3628201.html)

puts()函数用来向标准输出设备,　scanf函数是格式输入函数，即按用户指定的格式从键盘上把数据输入到指定的变量之中。

puts就是输出字符串啊。

int puts(
  const char* string
);

MSDN的例子
/* PUTS.C: This program uses puts
 \* to write a string to stdout.
 */

\#include <stdio.h>

void main( void )
{
  puts( "Hello world from puts!" );
}

运行结果就是
Hello world from puts!

你要输出换行的话，就用 puts( "\n" );

用法很简单啊，就是把一个C样式的字符串当参数传过去。

//-----------------------------------------

我刚刚试过了
puts( "" )的确可以起到换行的作用。

The puts function writes string to the standard output stream stdout, replacing the string's terminating null character ('\0') with a newline character ('\n') in the output stream.

当puts遇到\0时，会输出一个\n，也就是换行。
所以puts( "" )时，因为字符串本身长度为0，所以第一个字符就是\0，puts会输出一个\n,所以起到了换行的效果。

也就是说, puts( "" )跟puts( "\0" )是等效的，也等效於printf( "\n" )

在gets();前面加一个getchar();因为scanf()输入后有一个回车，gets()接收的回车符，要加个getchar();就是为了接受那个回车符

 

 

 

```
举个例子：char c[] = "good";puts(c);c中并没有换行符，但是puts打印玩good后会默认换行，这个换行就是它自己增加的，也就是老谭所说的意思。char *gets( char *str );
```

The gets() function reads characters from STDIN and loads them into str, until a newline or EOF is reached. The newline character is translated into a null termination. The return value of gets() is the read-in string, or NULL if there is an error.  

 

gets函数从标准输入设备读取字符串，直到遇到换行或者EOF。换行符被认为是终止字符。若函数调用成功，返回字符串；否则返回NULL。

 

 

 

int puts( char *str );

 

The function puts() writes str to STDOUT. puts() returns non-negative on success, or EOF on failure. 

 

puts函数项标准输出设备写出字符串。若成功调用，返回非负值；否则EOF。

 

 

 

注意：这两个函数都是c语言标准输入输出库中的函数，在使用时要包含<stdio.h>；在c++中应包括<cstdio>。同时这两个函数的参数都是字符数组，而不能用c++中的字符串对象。

```

```

 

```
puts就是输出字符串啊。int puts(    const char* string );MSDN的例子/* PUTS.C: This program uses puts * to write a string to stdout. */#include <stdio.h>void main( void ){   puts( "Hello world from puts!" );}运行结果就是Hello world from puts!你要输出换行的话，就用 puts( "\n" );用法很简单啊，就是把一个C样式的字符串当参数传过去。//-----------------------------------------我刚刚试过了puts( "" )的确可以起到换行的作用。The puts function writes string to the standard output stream stdout, replacing the string's terminating null character ('\0') with a newline character ('\n') in the output stream.当puts遇到\0时，会输出一个\n，也就是换行。所以puts( "" )时，因为字符串本身长度为0，所以第一个字符就是\0，puts会输出一个\n,所以起到了换行的效果。也就是说, puts( "" )跟puts( "\0" )是等效的，也等效於printf( "\n" )
```

分类: [C/C++](https://www.cnblogs.com/zhangyongjian/category/559405.html)