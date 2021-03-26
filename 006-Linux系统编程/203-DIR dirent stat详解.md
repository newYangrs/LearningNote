最近在看Linux下文件操作相关章节，遇到了这么几个结构体，被搞的晕乎乎的，今日有空，仔细研究了一下，受益匪浅。

首先说说DIR这一结构体，以下为DIR结构体的定义：

```c
struct** __dirstream{
    void *__fd; 					/* `struct hurd_fd' pointer for descriptor.   */
    char *__data; 					/* Directory block.   */
    int __entry_data; 				/* Entry number `__data' corresponds to.   */
    char *__ptr; 					/* Current pointer into the block.   */
    int __entry_ptr; 				/* Entry number `__ptr' corresponds to.   */
    size_t __allocation; 			/* Space allocated for the block.   */
    size_t __size;					/* Total valid data in the block.   */
    __libc_lock_define (, __lock) 	/* Mutex lock for this structure.   */

  };  

 

typedef struct __dirstream DIR; 
```

 

DIR结构体类似于FILE，是一个内部结构，以下几个函数用这个内部结构保存当前正在被读取的目录的有关信息（摘自[《UNIX环境高级编程（第二版）》](http://product.dangdang.com/product.aspx?product_id=9171119)）。函数 DIR *opendir(const char *pathname)，即打开文件目录，返回的就是指向DIR结构体的指针，而该指针由以下几个函数使用:

 

```c
struct dirent *readdir(DIR *dp);  

 
void rewinddir(DIR *dp);  



int closedir(DIR *dp);  

 

long telldir(DIR *dp);  

 

void seekdir(DIR *dp,**long** loc); 

 
```

关于DIR结构，我们知道这么多就可以了，没必要去再去研究他的结构成员。

接着是dirent结构体，首先我们要弄清楚目录文件（directory file）的概念：这种文件包含了其他文件的名字以及指向与这些文件有关的信息的指针（摘自[《UNIX环境高级编程（第二版）》](http://product.dangdang.com/product.aspx?product_id=9171119)）。从定义能够看出，dirent不仅仅指向目录，还指向目录中的具体文件，readdir函数同样也读取目录下的文件，这就是证据。以下为dirent结构体的定义：

 

```c
struct dirent  

{  

      long d_ino; /* inode number 索引节点号 */ 



      off_t d_off; /* offset to this dirent 在目录文件中的偏移 */ 



      unsigned short d_reclen; /* length of this d_name 文件名长 */ 



      unsigned char d_type; /* the type of d_name 文件类型 */ 



      char d_name [NAME_MAX+1]; /* file name (null-terminated) 文件名，最长255字符 */ 

} 
```

 

从上述定义也能够看出来，dirent结构体存储的关于文件的信息很少，所以dirent同样也是起着一个索引的作用，如果想获得类似ls -l那种效果的文件信息，必须要靠stat函数了。

通过readdir函数读取到的文件名存储在结构体dirent的d_name成员中，而函数

int stat(const char *file_name, struct stat *buf);

的作用就是获取文件名为d_name的文件的详细信息，存储在stat结构体中。以下为stat结构体的定义：

```c


struct stat {  



​    mode_t   st_mode;    //文件访问权限  



​    ino_t   st_ino;    //索引节点号  



​    dev_t   st_dev;    //文件使用的设备号  



​    dev_t   st_rdev;    //设备文件的设备号  



​    nlink_t  st_nlink;   //文件的硬连接数  



​    uid_t   st_uid;    //所有者用户识别号  



​    gid_t   st_gid;    //组识别号  



​    off_t   st_size;    //以字节为单位的文件容量  



​    time_t   st_atime;   //最后一次访问该文件的时间  



​    time_t   st_mtime;   //最后一次修改该文件的时间  



​    time_t   st_ctime;   //最后一次改变该文件状态的时间  



​    blksize_t st_blksize;  //包含该文件的磁盘块的大小  



​    blkcnt_t  st_blocks;   //该文件所占的磁盘块  



}; 
```



 