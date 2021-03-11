### 文章目录

#### strcpy

strcpy(dest,src) 把src地址开始且含有 null 结束符的字符串复制到以 dest 开始的地址空间

```cpp
char* strcpy(const char* strDest,const char* strSrc){
	assert(strDest != NULL && strSrc != NULL);
	char *address = strDest;
	while((*strDest++ = *strSrc++) != '\0');
	return address;
}
123456
```

#### memcpy

```cpp
void* memcpy(void* dest,void* src,size_t n);
1
```

功能：从源src所指的内存的起始位置开始拷贝 n 个字节到目标dest所指的内存地址的起始位置。函数返回指向dest的指针；

#### strcpy与memcpy区别

都是标准C库函数，区别如下：

1. **复制的内容不同**
   strcpy只能复制字符串，而memcpy可以复制任何内容，例如字符数组、整型、结构体、类等，用途更广
2. **复制的方法不同**
   strcpy不需要指定长度，遇到被复制的字符串的结束符 ‘\0’ 时结束，所以容易溢出；mencpy则是根据第三个参数决定复制的长度
3. **用途不同**
   通常在字符串复制时用strcpy，需要复制其他类型数据时用 memcpy