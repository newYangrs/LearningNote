## Makefile的使用

**Makefile主要内容为1个规则、2个函数、3个自动变量**



### 1.一个规则

- 规则的格式如下

```c++
目标：依赖对象
  （tab）命令
```

- 规则示例

```makefile
ALL:hello.out //ALL表示Makefile文档的终极目标

hello.out:hello.o
	gcc hello.o add.o -o hello.out
hello.o:hello.c
	gcc -c hello.c -o hello.o
add.o:add.c
	gcc -c add.c -o add.o
```

- **make文档只能叫Makefile或makefile**
- 文件更新注意要点：
  1. 如果想生成目标，则检查规则中的依赖条件是否存在，如不存在，则检查是否有规则生成该依赖条件
  2. 检查规则中的目标是否需要更新，必须先检查它的所有依赖，依赖中有任意一个被更新，则目标必须更新
  3. **目标的时间必须晚于依赖条件的时间，否则更新目标**



### 2.两个函数

```makefile
src = $(wildcard ./*.c) //找到当前目录下所有的 .c 文件，赋值给 src
obj = $(patsubst %.c,%.o,$(src)) //将 src 中的 .c 文件替换成 .o 文件，并赋值给 obj
```

- 则上述规则示例可更改为

```makefile
src = $(wildcard ./*.c) //找到当前目录下所有的 .c 文件，赋值给 src
obj = $(patsubst %.c,%.o,$(src)) //将 src 中的 .c 文件替换成 .o 文件，并赋值给 obj

.PHONY:clean ALL //伪目标，如果当前目录下存在 clean 或 ALL 文件，则如此声明

ALL:hello.out //ALL表示Makefile文档的终极目标

hello.out:hello.o
	gcc $(obj) -o hello.out
hello.o:hello.c
	gcc -c hello.c -o hello.o
add.o:add.c
	gcc -c add.c -o add.o	
	
clean:
	-rm -rf $(obj) a.out //'-'表示出错仍然执行
```

- 使用 `make clean -n` 测试，`-n`表示尝试执行，并非真正执行



### 3.三个自动变量

- **三个自动变量只能出现在规则的命令行中**

```Makefile
$@ /表示规则中的目标
$< /表示规则中的第一个依赖条件
$^ /表示规则中的所有依赖条件
```

- 则上述代码可更改为

```makefile
src = $(wildcard ./*.c) //找到当前目录下所有的 .c 文件，赋值给 src
obj = $(patsubst %.c,%.o,$(src)) //将 src 中的 .c 文件替换成 .o 文件，并赋值给 obj

.PHONY:clean ALL //伪目标，如果当前目录下存在 clean 或 ALL 文件，则如此声明

ALL:hello.out //ALL表示Makefile文档的终极目标

hello.out:hello.o
	gcc $(obj) -o $@
hello.o:hello.c
	gcc -c $< -o $@
add.o:add.c
	gcc -c $< -o $@	
	
clean:
	-rm -rf $(obj) a.out //'-'表示出错仍然执行
```

- 可衍生为**模式规则**

```makefile
src = $(wildcard ./*.c) //找到当前目录下所有的 .c 文件，赋值给 src
obj = $(patsubst %.c,%.o,$(src)) //将 src 中的 .c 文件替换成 .o 文件，并赋值给 obj

//加警告信息
myargs = -Wall -g

gcc -c $< -o $@ $(myargs)



.PHONY:clean ALL //伪目标，如果当前目录下存在 clean 或 ALL 文件，则如此声明

ALL:hello.out //ALL表示Makefile文档的终极目标

hello.out:$(obj)
	gcc $(obj) -o $@
%.o:%.c
	gcc -c $< -o $@

	
clean:
	-rm -rf $(obj) a.out //'-'表示出错仍然执行
```

- 同时有**静态模式规则**

```makefile
src = $(wildcard ./*.c) //找到当前目录下所有的 .c 文件，赋值给 src
obj = $(patsubst %.c,%.o,$(src)) //将 src 中的 .c 文件替换成 .o 文件，并赋值给 obj

.PHONY:clean ALL //伪目标，如果当前目录下存在 clean 或 ALL 文件，则如此声明

ALL:hello.out //ALL表示Makefile文档的终极目标

hello.out:$(obj)
	gcc $(obj) -o $@
%(obj):%.o:%.c         //同时存在两个来源的 .o 文件时，选取这一个
	gcc -c $< -o $@
	
%.o:%.s
	gcc -S $< -o $@

	
clean:
	-rm -rf $(obj) a.out //'-'表示出错仍然执行
```

