###  一、inode的定义

​		inode译成中文就是索引节点，它用来存放档案及目录的基本信息，包含时间、档名、使用者及群组等。

### 二、inode的分类及其具体涵义

​	 	实际上，inode是VFS使用的一个对象，用于存放**内核在操作文件或目录时所需要的全部信息**。索引节点有两种，一种是这里所说的==VFS inode==，存在**内存**中；另一种是具体文件系统的==Ext2 inode==，存在于**磁盘**上，使用时将其读入内存填充VFS的索引节点，之后对VFS索引节点的任何修改都将写回磁盘更新磁盘的索引节点。

#### 1、VFS inode包含文件访问权限、属主、组、大小、生成时间、访问时间、最后修改时间等信息。

​		它是linux管理文件系统的最基本单位，也是文件系统连接任何子目录、文件的桥梁。inode结构中的静态信息取自物理设备上的文件系统，由文件系统指定的函数填写，它只存在于内存中，可以通过inode缓存访问。虽然每个文件都有相应的inode结点，但是只有在需要的时候系统才会在内存中为其建立相应的inode数据结构，**建立的inode结构将形成一个链表**，我们可以通过遍历这个链表去得到我们需要的文件结点,VFS也为已分配的inode构造缓存和哈希表，以提 高系统性能。**inode结构中的==struct inode_operations *i_op==为我们提供了一个inode操作列表，通过这个列表提供的函数我们可以对VFS inode结点进行各种操作**。每个inode结构都有一个i结点号i_ino，在同一个文件系统中每个i结点号是唯一的。



-  **文件除了文件名以外的所有文件信息，都存在inode之中。** 	
-  **由于每个文件都必须有一个inode，因此有可能发生inode已经用光，但是硬盘还未存满的情况。这时，就无法在硬盘上创建新文件。** 





**由于inode号码与文件名分离，这种机制导致了一些Unix/Linux系统特有的现象：**

​		**(1) 有时，文件名包含特殊字符，无法正常删除。这时，直接删除inode节点，就能起到删除文件的作用。**

​		**(2) 移动文件或重命名文件，只是改变文件名，不影响inode号码。**

​		**(3) 打开一个文件以后，系统就以inode号码来识别这个文件，不再考虑文件名。因此，通常来说，系统无法从inode号码得知文件名。**

​		**第3点使得软件更新变得简单，可以在不关闭软件的情况下进行更新，不需要重启。因为系统通过inode号码，识别运行中的文件，不通过文件名。更新的时候，新版文件以同样的文件名，生成一个新的inode，不会影响到运行中的文件。等到下一次运行这个软件的时候，文件名就自动指向新版文件，旧版文件的inode则被回收。**



对于inod需要知道：
1）**对于Unix风格的文件系统来说，这些信息可以从磁盘索引节点直接读入**。

​		如果一个文件 系统没有索引节点，那么不管这些相关信息在磁盘上市怎么存放的，文件系统都必须从中提取这些信息。没有索引的文件系统通常将文件的描述信息作为文件的一部分来存放。这些文件系统与Unix风格的文件系统不同，没有将数据与控制信息分开存放。而有些现代的文件系统使用数据库来存储文件的数据。但是不管哪种情况、采用哪种方式，索引节点对象必须在内存中创建，以便文件系统来使用。

2）**一个索引节点代表了文件系统的一个文件，在文件创建时创建文件删除时销毁，但是索引节点仅在当文件被访问时，才在内存中创建，且无论有多少个副本访问这个文件，inode只存在一份。**

3）**inode只是用于描述文件的==元数据信息==，并不是文件的数据**，文件的数据会根据inode的信息存放在一个数据块中（例如：test.txt文件ls -l看到的信息就是它的属性元信息，“hello”数据存放在另一个数据块中）。

4）**可以简单理解为ls -l 看到的就是此文件的inode信息。**

5）**inode可以描述像普通文件、目录这样的磁盘文件，他也可以描述设备或者管道这样的文件**，不过这些**特殊的文件一般只存在inode块不分配数据块**（因为索引节点中有一些特殊文件相关的项，比如**==i_pipe==项就指向一个代表有名管道的数据结构**，**==i_bdev==块设备结构体**，**==i_cdev指向==字符设备结构体**。**这三个指针被放在一个共用体中，因为一个给定的索引节点每次只能表示三者之一（或者均不）**）。

6）**有时，某些文件系统可能并不能完整地包含索引节点结构体所要求的所有信息。**例如，有的文件系统可能并不记录文件的访问时间，这时，该文件系统可以在实现中选择合适的办法来解决和这个问题。它可以在i_atime中存储0，或者让i_atime等于i_mtime,或者只在内存中更新i_atime而不将其写回磁盘，或者由文件系统 的实现者来决定。 



```c++

struct inode {
    struct list_head i_hash;
    struct list_head i_list;
    struct list_head i_dentry;
    struct list_head i_dirty_buffers;
    unsigned longi_ino; /*每一个inode都有一个序号，经由super block结构和其序号，我们可以很轻易的找到这个inode。*/
    atomic_t i_count; /*在Kernel里，很多的结构都会记录其reference count，以确保如果某个结构正在使用，它不会被不小心释放掉，i_count就是其reference count。*/
    kdev_t i_dev; /* inode所在的device代码 */
    umode_t i_mode; /* inode的权限 */
    nlink_t i_nlink; /* hard link的个数 */
    uid_t i_uid; /* inode拥有者的id */
    gid_t i_gid; /* inode所属的群组id */
    kdev_t i_rdev; /* 如果inode代表的是device的话，那此字段将记录device的代码 */ 
    off_t i_size; /* inode所代表的档案大小 */
    time_t i_atime; /* inode最近一次内容访问时间 */
    time_t i_mtime; /* inode最近一次内容修改时间 */
    time_t i_ctime; /* inode的产生时间 */ 
    unsigned long i_blksize; /* inode在做IO时的区块大小 */
    unsigned long i_blocks; /* inode所使用的block数，一个block为512 byte*/
    unsigned long i_version; /* 版本号码 */ 
    unsigned short i_bytes;
    struct semaphore i_sem;
    struct rw_semaphore i_truncate_sem;
    struct semaphore i_zombie;
    struct inode_operations *i_op;
    struct file_operations *i_fop;/* former ->i_op->default_file_ops */
    struct super_block *i_sb; /* inode所属档案系统的super block */
    wait_queue_head_t i_wait;
    struct file_lock *i_flock; /* 用来做file lock */
    struct address_space *i_mapping;
    struct address_space i_data;
    struct dquot *i_dquot [MAXQUOTAS];
    /* These three should probably be a union */
    struct pipe_inode_info *i_pipe;
    struct block_device *i_bdev;
    struct char_device *i_cdev;
    unsigned longi_dnotify_mask; /* Directory notify events */
    struct dnotify_struct *i_dnotify; /* for directory notifications */
    unsigned long i_state; /* inode目前的状态，可以是I_DIRTY，I_LOCK和 I_FREEING的OR组合 */ 
    unsigned int i_flags; /* 记录此inode的参数 */ 
    unsigned char i_sock; /* 用来记录此inode是否为socket */ 
    atomic_t i_write count;
    unsigned int i_attr_flags; /* 用来记录此inode的属性参数 */ 
    __u32 i_generation;
    
    union {
        struct minix_inode_info minix_i;
        struct ext2_inode_info ext2_i;
        struct ext3_inode_info ext3_i;
        struct hpfs_inode_info hpfs_i;
        struct ntfs_inode_info ntfs_i;
        struct msdos_inode_info msdos_i;
        struct umsdos_inode_info umsdos_i;
        struct iso_inode_info isofs_i;
        struct sysv_inode_info sysv_i;
        struct affs_inode_info affs_i;
        struct ufs_inode_info ufs_i;
        struct efs_inode_info efs_i;
        struct romfs_inode_info romfs_i;
        struct shmem_inode_info shmem_i;
        struct coda_inode_info coda_i;
        struct smb_inode_info smbfs_i;
        struct hfs_inode_info hfs_i;
        struct adfs_inode_info adfs_i;
        struct qnx4_inode_info qnx4_i;
        struct reiserfs_inode_info reiserfs_i;
        struct bfs_inode_info bfs_i;
        struct udf_inode_info udf_i;
        struct ncp_inode_info ncpfs_i;
        struct proc_inode_info proc_i;
        struct socketsocket_i;
        struct usbdev_inode_info usbdev_i;
        struct jffs2_inode_infojffs2_i;
        void *generic_ip;
    } u;
};

```



- **介绍完了inode，接下来看看inode的操作i_op：**（在给定的节点上，可能是由VFS执行这些函数，也可能由具体的文件系统执行）

```c
struct inode_operations {
    //该函数在在特定目录中寻找索引节点，改索引节点要对应于dentry中给出的文件名。
    struct dentry * (*lookup) (struct inode *,struct dentry *, unsigned int);
    
    void * (*follow_link) (struct dentry *, struct nameidata *);
    int (*permission) (struct inode *, int);
    struct posix_acl * (*get_acl)(struct inode *, int);

    int (*readlink) (struct dentry *, char __user *,int);
    void (*put_link) (struct dentry *, struct nameidata *, void *);

    //VFS通过系统调用create()和open()来调用改函数，从而为dentry对象创建一个新的索引节点。在创建时使用mode指定初始模式
    int (*create) (struct inode *,struct dentry *, umode_t, bool);
    
    //该函数被系统调用link()调用，用来创建硬链接。硬链接名称由dentry指定，连接对象是dir目录中old_denrty目录项所代表的文件。
    int (*link) (struct dentry *,struct inode *,struct dentry *);
    
    //该函数被系统调用ulink()调用，从目录dir中删除由目录项dentry指定的索引节点对象。
    int (*unlink) (struct inode *,struct dentry *);
    
    //该函数被系统调用symlink()调用,创建符号链接。改符号链接的名称由symname指定，连接对象是dir目录中的dentry目录项。
    int (*symlink) (struct inode *,struct dentry *,const char *);
    
    //该函数被系统调用mkdir()调用,创建一个新的目录。创建时使用mode指定初始模式。
    int (*mkdir) (struct inode *,struct dentry *,umode_t);
    
    //该函数被系统调用rmdir()调用,删除dir目录中的dentry 目录项代表的文件。
    int (*rmdir) (struct inode *,struct dentry *);
    
    //该函数被系统调用mknod()调用,创建特殊文件（设备文件、命名管道或套接字）。要创建的文件在dir目录中，其目录项为dentry，关联的设备为rdev,初始权限有mode指定。
    int (*mknod) (struct inode *,struct dentry *,umode_t,dev_t);
    
    //VFS调用该函数来移动文件。文件路径在old_dir目录中，源文件由old_dentry目录项指定，目标路径在new_dir中，目标文件由new_dentry指定。
    int (*rename) (struct inode *, struct dentry *,
            struct inode *, struct dentry *);
    
    int (*setattr) (struct dentry *, struct iattr *);
    int (*getattr) (struct vfsmount *mnt, struct dentry *, struct kstat *);
    int (*setxattr) (struct dentry *, const char *,const void *,size_t,int);
    ssize_t (*getxattr) (struct dentry *, const char *, void *, size_t);
    ssize_t (*listxattr) (struct dentry *, char *, size_t);
    int (*removexattr) (struct dentry *, const char *);
    int (*fiemap)(struct inode *, struct fiemap_extent_info *, u64 start,
              u64 len);
    int (*update_time)(struct inode *, struct timespec *, int);
    int (*atomic_open)(struct inode *, struct dentry *,
               struct file *, unsigned open_flag,
               umode_t create_mode, int *opened);
    int (*tmpfile) (struct inode *, struct dentry *, umode_t);
    int (*set_acl)(struct inode *, struct posix_acl *, int);
} ____cacheline_aligned;

```







#### 2、EXT2 inode用来定义文件系统的结构以及描述系统中每个文件的管理信息，每个文件都有且只有一个inode，即使文件中没有数据，其索引结点也是存在的。

​		每个文件用一个单独的Ext2 inode结构来描述，而且每一个inode都有唯一的标志号。Ext2 inode为内存中的inode结构提供了文件的基本信息，随着内存中inode结构的变化，系统也将更新Ext2 inode中相应的内容。Ext2 inode对应的是Ext2_inode结构。

```c++
struct ext2_inode {
    __u16 i_mode;/*

     File mode */
    __u16 i_uid;/* Low 16 bits of Owner Uid */
    __u32 i_size;/* Size in bytes */
    __u32 i_atime;/* Access time */
    __u32 i_ctime;/* Creation time */
    __u32 i_mtime;/* Modification time */
    __u32 i_dtime;/* Deletion Time */
    __u16 i_gid;/* Low 16 bits of Group Id */
    __u16 i_links_count;/* Links count */
    __u32 i_blocks;/* Blocks count */
    __u32 i_flags;/* File flags */
    
    union {
        struct {
        	__u32 l_i_reserved1;
    	} linux1;
    
    	struct {
   		 	__u32 h_i_translator;
        } hurd1;
    
    	struct {
    		__u32 m_i_reserved1;
    	} masix1;
    } osd1;/* OS dependent 1 */
    
    
    __u32 i_block[EXT2_N_BLOCKS];/* Pointers to blocks */
    __u32 i_generation;/* File version (for NFS) */
    __u32 i_file_acl;/* File ACL */
    __u32 i_dir_acl;/* Directory ACL */
    __u32 i_faddr;/* Fragment address */
    
    union {
        struct {
            __u8l_ i_frag;/* Fragment number */
            __u8l_ i_fsize;/* Fragment size */
            __u16 i_pad1;

            __u16l_ i_uid_high;/* these 2 fields */
            __u16l_ i_gid_high;/* were reserved2[0] */
            __u32l_ i_reserved2;
        } linux2;
        
        struct {
            __u8h_ i_frag;/* Fragment number */
            __u8h_ i_fsize;/* Fragment size */
            __u16h_ i_mode_high;

            __u16h_ i_uid_high;
            __u16h_ i_gid_high;
            __u32h_ i_author;
        } hurd2;
        
        struct {
            __u8m_ i_frag;/* Fragment number */
            __u8m_ i_fsize;/* Fragment size */
            __u16m_ pad1;
            __u32m_ i_reserved2[2];
        } masix2;
        
	} osd2;/* OS dependent 2 */
};
```

  		从结构的定义中可以看出来inode(VFS inode)与ext2_inode的差别是很大的，它们都包含动态信息和静态信息，通过union指定的内容一定是动态的。inode结构中的union u实际上反映了VFS支持的文件系统。
可以看出inode结构与ext2_inode结构有些内容是相似的，如：

- inode定义的

```c++
unsigned long i_ino;
umode_t i_mode;
nlink_t i_nlink;
uid_t i_uid;
gid_t i_gid;
loff_t i_size;
time_t i_atime;
time_t i_mtime;
time_t i_ctime;
unsigned long i_blksize;
unsigned long i_blocks;
```

- 和ext2_inode定义的定义部分

```c++
__u16 i_mode;/* File mode */
__u16 i_uid;/* Low 16 bits of Owner Uid */
__u32 i_size;/* Size in bytes */
__u32 i_atime;/* Access time */
__u32 i_ctime;/* Creation time */
__u32 i_mtime;/* Modification time */
__u32 i_dtime;/* Deletion Time */
__u16 i_gid;/* Low 16 bits of Group Id */
__u16 i_links_count;/* Links count */
__u32 i_blocks;/* Blocks count */
__u32 i_flags;/* File flags */
```

这些都可以对应上，当然还有一些不同的地方，如inode中定义的

```c++
kdev_t i_rdev;
kdev_t i_dev;
unsigned short i_bytes;
struct semaphore i_sem; 
```

在ext2_inode中没有体现，不过这部分对ext2_inode是没有用途而且无法确定的。类似的，可以推广到两个结构的其余部分，最终在代码中的区别还是与原理中分析的区别相关的，也是原理的具体体现。  

####  3、从上面的描述，我们可以对VFS inode与ext2 inode做出比较：

- **位置：**VFS inode结构位于内存中，而Ext2_inode位于磁盘。

- **生存期：**VFS inode在需要时才会被建立，如果系统断电，此结构也随之消失。

  ​	Ext2_inode的存在与系统是否上电无关，**而且无论文件是否包含数据，Ext2_inode都是存在的。**

- **唯一性**：两者在自己的作用域中都是唯一的。

- **关系：**VFS inode是Ext2 inode的抽象、映射与扩充，而后者是前者的静态信息部分，也是对前者的具体化、实例化和持久化。

- **操作：** **对VFS inode的操作具有通用性**，**对文件系统inode的操作则是文件系统相关的**，依赖于特定的实现。

- **组织管理：**

  - 系统通过**VFS inode链表**来对其进行组织，并且为了提高访问效率相应地构造了**inode构造缓存**和**hash table**。

  - Ext2 inode的信息位于EXT2文件系统的划分的块组中，在每个块组中包含相应的inode位图、inode表指定具体的inode信息，每个inode对应Ext2_inode结构。 





###  三、 inode的分配 (对于ext2 inode)

1、每个inode大小**128 byte**
2、在分割扇区时，系统会先做出一堆inode以供以后使用，**inode 的数量关系着系统中可以建立的档案及目录总数**。有时候，这些inode或许会不够用，例如当一个程序产生大量小文件的时候，此时文件系统就需要增加索引节点。
3、同样，如果我们事先知道此文件系统只用来存放少数大文件，我们就可以通过减少索引节点的数目来达到节省磁盘空间的目的——毕竟每个索引节点占用128字节。

- 创建文件系统的时候，可以使用newfs命令的-i选项来增加或者减少索引节点的数目。

```shell
newfs -i 2048 -b 8192 -f 1024
    newfs创建新的文件系统
    -i 2048更改inode每2KB创建一个
    -b 8192设置block size的大小为8kB
    -f 1024设置fragments的大小为1KB  
```





###  四、inode与ln (对于ext2 inode)

#### 1、当我们用ls 查看某个目录或文件时，如果加上-i 参数，就可以看到inode节点了；

```powershell
[root@localhost ~]# ls -li lsfile.sh
2408949 -rwxr-xr-x 1 root root 7 04-21 12:47 lsfile.sh
可见lsfile.sh 的inode值是2408949 ； 
```





#### 2、创建硬链接，硬链接和源文件关系；

用ln创建文件硬链接的语法：

```
[root@localhost ~]#ln 源文件 目标文件
```

下面我们举一个例子，在这个例子中，我们要为sun.txt创建其硬链接sun002.txt。然后看一下 sun.txt和

```powershell
sun002.txt的属性的变化；
[root@localhost ~]# ls -li sun.txt 注：查看sun.txt的属性；
2408263 -rw-r--r-- 1 root root 29 04-22 21:02 sun.txt 注：这是sun.txt的属性；
[root@localhost ~]# ln sun.txt sun002.txt 注：我们通过ln 来创建sun.txt的硬链接文件sun002.txt
[root@localhost ~]# ls -li sun* 注：我们列一下sun.txt 和sun002.txt
2408263 -rw-r--r-- 2 root root 29 04-22 21:02 sun002.txt
2408263 -rw-r--r-- 2 root root 29 04-22 21:02 sun.txt
```

我们可以看到sun.txt在没有创建硬链接文件sun002.txt的时候，其链接个数是1（也就是-rw-r--r--后的那个数值），创建了硬链接 sun002.txt创建后，这个值变成了2。也就是说，我们每次为sun.txt创建一个新的硬链接文件后，其硬链接个数都会增加1。
**inode值相同的文件，他们的关系是互为硬链接的关系。**当我们修改其中一个文件的内容时，互为硬链接的文件的内容也会跟着变化。如果我们删除互为硬链接关系的某个文件时，其它的文件并不受影响。比如我们把sun.txt删除后，我们还是一样能看到sun002.txt的内容，并且sun02.txt仍是存在的。
可以这么理解，互为硬链接关系的文件，他们好像是克隆体，他们的属性几乎是完全一样；
下面的例子，我们把sun.txt删除，然后我们看一下sun002.txt是不是能看到其内容。

```powershell
[root@localhost ~]# rm -rf sun.txt
[root@localhost ~]# more sun002.txt
```

**==注意：硬链接不能为目录创建，只有文件才能创建硬链接。==** 

#### 3、软链接的创建，及软接与源文件的关系；

创建软链接（也被称为符号链接）的语法； 

```powershell
[root@localhost ~]# ln -s 源文文件或目录 目标文件或目录
```



软链接也叫符号链接，他和硬链接有所不同，软链接文件只是其源文件的一个标记。当我们删除了源文件后，链接文件不能独立存在，虽然仍保留文件名，但我们却不能查看软链接文件的内容了。



```powershell
[root@localhost ~]# ls -li linuxsir001.txt
2408274 -rw-r--r-- 1 root root 29 04-22 21:53 linuxsir001.txt
[root@localhost ~]# ln -s linuxsir001.txt linuxsir002.txt
[root@localhost ~]# ls -li linuxsir001.txt linuxsir002.txt
2408274 -rw-r--r-- 1 root root 29 04-22 21:53 linuxsir001.txt
2408795 lrwxrwxrwx 1 root root 15 04-22 21:54 linuxsir002.txt -> linuxsir001.txt
```



上面的例子，首先我们查看 linuxsir001.txt 的属性，比如inode、所属文件种类、创建或修改时间等... ...我们来对比一下：
首先 对比一下节点：

- 两个文件的节点不同；
- 其次 两个文件的归属的种类不同 linuxsir001.txt是-，也就是普通文件，而linuxsir002.txt 是l，它是一个链接文件；
- 第三 两个文件的读写权限不同 linuxsir001.txt 是rw-r--r-- ，而linuxsir002.txt的读写权限是 rwxrwxrwx
- 第三 两者的硬链接个数相同；都是1
- 第四 两文件的属主和所归属的用户组相同；
- 第五 修改(或访问、创建）时间不同；
- 我们还注意到了linuxsir002.txt 后面有一个标记 ->，这表示linuxsir002.txt 是linuxsir001.txt的软链接文件。

​		值得我们注意的是：**当我们修改链接文件的内容时，就意味着我们在修改源文件的内容。当然源文件的属性也会发生改变，链接文件的属性并不会发生变化。当我们把源文件删除后，链接文件只存在一个文件名，因为失去了源文件，所以软链接文件也就不存在了。这一点和硬链接是不同的；**

```powershell


[root@localhost ~]# rm -rf linuxsir001.txt 注：删除linuxsir001.txt
[root@localhost ~]# ls -li linuxsir002.txt 注：查看linuxsir002 的属性；
2408795 lrwxrwxrwx 1 root root 15 04-22 21:54 linuxsir002.txt -> linuxsir001.txt
[root@localhost ~]# more linuxsir002.txt 注：查看linuxsir002.txt的内容；
linuxsir002.txt: 没有那个文件或目录 注：得到提示，linuxsir002.txt不存在。
```



- 上面的例子告诉我们，**如果一个链接文件失去了源，就意味着他已经不存在了；**我们可以看到**软链接文件，其实只是源文件的一个标记**，**当源文件失去时，他也就是存在了**。**==软链接文件只是占用了inode来存储软链接文件属性等信息，但文件存储是指向源文件的。==**
- 软链接，可以为文件或目录都适用。
- 无论是软链接还是硬链接，都可以用rm来删除。rm工具是通用的。 