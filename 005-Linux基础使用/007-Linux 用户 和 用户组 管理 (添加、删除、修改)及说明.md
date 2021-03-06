

# Linux 用户 和 用户组 管理 (添加、删除、修改)及说明

From：http://www.cnblogs.com/xd502djj/archive/2011/11/23/2260094.html

鸟哥官网 Linux 帐号管理与 ACL 权限设定：http://linux.vbird.org/linux_basic/0410accountmanager.php

鸟哥官网（简体中文）：http://cn.linux.vbird.org/linux_basic/linux_basic.php

Linux 中将用户添加到组的指令：https://cnzhx.net/blog/linux-add-user-to-group/

 

su 默认切到 root

su 与 su - 的区别：

- su 是不改变当前变量
- su - 是切换到用户的变量

su root 和 su - root 有什么区别?

- su 只能获得 root 的执行权限，不能获得环境变量
- su - 是切换到 root 并获得 root 的环境变量及执行权限。

 

 

## 理解Linux的单用户多任务，多用户多任务概念

 

Linux 是一个多用户、多任务的操作系统；我们应该了解单用户多任务和多用户多任务的概念；

 

### **1、Linux 的单用户多任务；**

单用户多任务；比如我们以beinan 登录系统，进入系统后，我要打开gedit 来写文档，但在写文档的过程中，我感觉少点音乐，所以又打开xmms 来点音乐；当然听点音乐还不行，MSN 还得打开，想知道几个弟兄现在正在做什么，这样一样，我在用beinan 用户登录时，执行了gedit 、xmms以及msn等，当然还有输入法fcitx ；这样说来就有点简单了，一个beinan用户，为了完成工作，执行了几个任务；当然beinan这个用户，其它的人还能以远程登录过来，也能做其它的工作。

 

### **2、Linux 的多用户、多任务；**

有时可能是很多用户同时用同一个系统，但并不所有的用户都一定都要做同一件事，所以这就有多用户多任务之说；

举个例子，比如LinuxSir.Org 服务器，上面有FTP 用户、系统管理员、web 用户、常规普通用户等，在同一时刻，可能有的弟兄正在访问论坛；有的可能在上传软件包管理子站，比如luma 或Yuking 兄在管理他们的主页系统和FTP ；在与此同时，可能还会有系统管理员在维护系统；浏览主页的用的是nobody 用户，大家都用同一个，而上传软件包用的是FTP用户；管理员的对系统的维护或查看，可能用的是普通帐号或超级权限root帐号；**不同用户所具有的权限也不同，要完成不同的任务得需要不同的用户**，也可以说**不同的用户，可能完成的工作也不一样**；

值得注意的是：多用户多任务并不是大家同时挤到一接在一台机器的的键盘和显示器前来操作机器，多用户可能通过远程登录来进行，比如对服务器的远程控制，只要有用户权限任何人都是可以上去操作或访问的；

 

### **3、用户的角色区分；**

用户在系统中是分角色的，在Linux 系统中，由于角色不同，权限和所完成的任务也不同；值得注意的是用户的角色是通过UID和识别的，特别是UID；在系统管理中，系统管理员一定要坚守UID 唯一的特性；

root 用户：系统唯一，是真实的，可以登录系统，可以操作系统任何文件和命令，拥有最高权限；
虚拟用户：这类用户也被称之为伪用户或假用户，与真实用户区分开来，这类用户不具有登录系统的能力，但却是系统运行不可缺少的用户，比如bin、daemon、adm、ftp、mail等；这类用户都系统自身拥有的，而非后来添加的，当然我们也可以添加虚拟用户；
普通真实用户：这类用户能登录系统，但只能操作自己家目录的内容；权限有限；这类用户都是系统管理员自行添加的；

 

### **4、多用户操作系统的安全；**

多用户系统从事实来说对系统管理更为方便。从安全角度来说，多用户管理的系统更为安全，比如beinan用户下的某个文件不想让其它用户看到，只是设置一下文件的权限，只有beinan一个用户可读可写可编辑就行了，这样一来只有beinan一个用户可以对其私有文件进行操作，Linux 在多用户下表现最佳，Linux能很好的保护每个用户的安全，但我们也得学会Linux 才是，再安全的系统，如果没有安全意识的管理员或管理技术，这样的系统也不是安全的。

从服务器角度来说，多用户的下的系统安全性也是最为重要的，我们常用的Windows 操作系统，它在系纺权限管理的能力只能说是一般般，根本没有没有办法和Linux或Unix 类系统相比；

 

 

 

# **用户(user）和用户组（group）概念**

 

## **1、用户（user）的概念**

​       通过前面对Linux 多用户的理解，我们明白Linux 是真正意义上的多用户操作系统，所以我们能在Linux系统中建若干用户（user）。比如我们的同事想用我的计算机，但我不想让他用我的用户名登录，因为我的用户名下有不想让别人看到的资料和信息（也就是隐私内容）这时我就可以给他建一个新的用户名，让他用我所开的用户名去折腾，这从计算机安全角度来说是符合操作规则的；

​     当然用户（user）的概念理解还不仅仅于此，在Linux系统中还有一些用户是用来完成特定任务的，比如nobody和ftp 等，我们访问LinuxSir.Org 的网页程序，就是nobody用户；我们匿名访问ftp 时，会用到用户ftp或nobody ；如果您想了解Linux系统的一些帐号，请查看 /etc/passwd ；

 

## **2、用户组（group）的概念**

​    用户组（group）就是具有相同特征的用户（user）的集合体；比如有时我们要让多个用户具有相同的权限，比如查看、修改某一文件或执行某个命令，这时我们需要用户组，我们把用户都定义到同一用户组，我们通过修改文件或目录的权限，让用户组具有一定的操作权限，这样用户组下的用户对该文件或目录都具有相同的权限，这是我们通过定义组和修改文件的权限来实现的；

​    举例：我们为了让一些用户有权限查看某一文档，比如是一个时间表，而编写时间表的人要具有读写执行的权限，我们想让一些用户知道这个时间表的内容，而不让他们修改，所以我们可以把这些用户都划到一个组，然后来修改这个文件的权限，让用户组可读，这样用户组下面的每个用户都是可读的；

 

**用户和用户组的对应关系是：一对一、多对一、一对多或多对多；**

一对一：某个用户可以是某个组的唯一成员；
多对一：多个用户可以是某个唯一的组的成员，不归属其它用户组；比如beinan和linuxsir两个用户只归属于beinan用户组；
一对多：某个用户可以是多个用户组的成员；比如beinan可以是root组成员，也可以是linuxsir用户组成员，还可以是adm用户组成员；
多对多：多个用户对应多个用户组，并且几个用户可以是归属相同的组；其实多对多的关系是前面三条的扩展；理解了上面的三条，这条也能理解；

 

**实际用户 和 有效用户 区别**

***\*Unix系统通过进程的有效用户ID和有效用户组ID来决定进程对系统资源的访问权限。\****

Unix高级编程第八章提到实际用户和有效用户，那么这两个用户到底是什么，它们之间有什么区别呢？

- 实际用户ID（实际组ID）：标识当前用户（所属组）是谁，当用户登陆时取自口令文件。即标识我是谁。也就是登录用户的uid和gid。比如我的Linux以king用户登录，在Linux运行的所有的命令的实际用户ID都是king的uid，实际用户组ID都是king的gid（可以用id命令查看）
- 有效用户ID（有效组ID）：用来决定我们（当前进程）对资源的访问权限或者对文件的访问权（即实际该进程是以那个用户运行的）

一般情况下，有效用户ID等于实际用户ID，有效用户组ID等于实际用户组ID。当设置-用户-ID（SUID）位设置，则有效用户ID等于文件的所有者的uid，而不是实际用户ID；同样，如果设置了设置-用户组-ID（SGID）位，则有效用户组ID等于文件所有者的gid，而不是实际用户组ID。

一个测试验证程序

**![img](https://img-blog.csdn.net/20171027104744740)**

这个程序非常简单没有什么好说的。我们编译这个程序生成test 程序

**![img](https://img-blog.csdn.net/20171027104801729)**

通过id命令看到当前登录用户为root，uid=0，gid=0。通过ls命令我们可以看出test程序没有设置SUID和SGID，所有者是root，所有组也是root。执行test我们发现有效用户ID等于实际用户ID（0），有效用户组ID等于实际用户组ID（0）。

你可能注意到test的所有者root，组也是root，和实际用户，实际用户组是一样的。下一步我们修改一下test所有者和组，再看结果。

![img](https://img-blog.csdn.net/20171027103809431)

发现结果和上面一样，test进程的有效用户ID等于实际用户ID（0），有效用户组ID等于实际用户组ID（0）。

下面我们给test程序设置SUID

![img](https://img-blog.csdn.net/20171027103929223)

发现设置test程序的SUID位之后，test进程的有效用户ID等于文件所有者的UID（gkh的uid为500），有效用户组ID还是等于实际用户组ID（0）。这样程序就可以访问只有gkh才能访问的资源了

 

### linux 特殊权限 SUID、SGID、SBIT

参考：http://www.cnblogs.com/javaee6/p/4026108.html

先看看下面两个的权限是什么

![img](https://img-blog.csdn.net/20171027105920345)

![img](https://img-blog.csdn.net/20171027105943620)

非常奇怪，/tmp目录和 passwd文件的权限怎么怪怪的，怎么有s和t权限呢。看了下面的内容你就明白了？

 

### **SUID**

当s出现在文件拥有者的x权限上时,如我们上面看到的/usr/bin/passwd这个文件的权限时-rwsr-xr-x,此时就被称为SET UID简称SUID.SUID对于一个文件有什么限制和功能呢？

- 1.**SUID权限仅对二进制可执行文件有效**
  \2. 执行者对于该文件具有x的权限
  \3. 本权限仅在执行该文件的过程中有效
  \4. 执行者将具有该文件拥有者的权限

例如普通用户用passwd修改自己的命令，实际上最终更改的是/etc/passwd文件. 此文件时用户管理配置文件,只有root权限才能更改

![img](https://img-blog.csdn.net/20171027110217403)

既然是root用户才拥有此权限，为什么我们可以通过passwd命令来修改密码呢，那这就要归功于passwd设置了suid权限位了

![img](https://img-blog.csdn.net/20171027110301393)

此时普通用户通过执行passwd命令，临时拥有root权限，间接的修改/etc/passwd，以达到修改自己密码的权限

 

### **SGID**

当s出现在目录或文件所属群的x权限上时，此时就称为SET GID简称SGID,那SGID对文件和目录分部有哪些功能呢？

SGID对目录

- \1. 使用者若对于此目录具有 r 与 x 的权限时，该使用者能够进入此目录
  \2. 使用者在此目录下的群组将会变成该目录的群组
  \3. 若使用者在此目录下具有 w 的权限(可以新建文件)，则使用者所创建的新文件，该新文件的群组与此目录的群组相同

SGID对文件

- \1. SGID 对二进制可执行文件有效
  \2. 程式执行者对于该文件来说，需具备 x 的权限
  \3. 执行者在执行的过程中将会获得该文件群组的支援（用于改文件群组的权限）

 

### **SBIT**

当s出现在目录其他用户的x权限上时，此时就称为Sticky Bit简称SBIT,那SBIT有哪些限制和作用呢?

- \1. 仅对目录有效，对文件无效
  \2. 当使用者在该目录下建立文件或目录时(有权限的情况下)，**仅自己与 root 才有权力删除新建的目录或文件**

我们知道/tmp目录是这样的权限。

![img](https://img-blog.csdn.net/20171027110714637)

现在我们来验证下，先用root账号在tmp文件中创建一个文件test,然后用openstack(其他账号)进入该目录,删除test文件，看看发生什么情况

![img](https://img-blog.csdn.net/20171027110755382)

我们看到这样是不能删除文件的。因为/temp目录有SBIT权限

 

### **怎么操作 SUID、SBID、SBIT ？**

操作这些标志与操作文件权限的命令是一样的, 都是 chmod. 有两种方法来操作,

(1)符号类型改变权限（文字法：SUID: u+s ，SGID: g+s，SBIT: o+t ）

- \1. chmod u+s testbin-- 为testbin文件加上setuid标志.
  \2. chmod g+s testdir-- 为testdir目录加上setgid标志
  \3. chmod o+t testdir-- 为testdir目录加上sticky标志

(2) 数字类型改变档案权限（数字法：将原来的三位数扩展为四位数即可，SUID为4，SGID为2，SBIT为1，把它们放在权限数字的最开头。例如设置SUID，可以写成4777，设置SGID可以写成，2777）

采用八进制方式. 对一般文件通过三组八进制数字来置标志, 如 666, 777, 644等. 如果设置这些特殊标志, 则在这三组数字之外再加一组八进制数字. 如 4666, 2777等. 这一组八进制数字三位的意义如下,

- \1. setuid位, 如果该位为1, 则表示设置setuid 4---
  \2. setgid位, 如果该位为1, 则表示设置setgid 2---
  \3. sticky位, 如果该位为1, 则表示设置sticky 1---

设置完这些标志后, 可以用 ls -l 来查看. 如果有这些标志, 则会在原来的执行标志位置上显示. 如

- rwsrw-r-- 表示有setuid标志
  rwxrwsrw- 表示有setgid标志
  rwxrw-rwt 表示有sticky标志

 

 

 

# **管理用户（user）和用户组（group）相关命令**

 

**1. 管理用户（user）的工具或命令；**

```properties
useradd    注：添加用户 
adduser    注：添加用户
两个用户创建命令之间的区别
adduser： 会自动为创建的用户指定主目录、系统shell版本，会在创建时输入用户密码。（一般使用 adduser）
useradd： 需要使用参数选项指定上述基本设置，如果不使用任何参数，则创建的用户无密码、无主目录、没有指定shell版本。
          需要单独设置用户密码、指定家目录、指定shell等

usermod    注：修改已经存在用户的信息 
userdel    注：删除用户
passwd     注：为用户设置密码
chage      注：更改用户密码过期信息 
chfn       注：改变用户备注信息 
chsh       注：更改登录 shell
usermod    注：修改用户命令，可以通过usermod 来修改登录名、用户的家目录等等。即更改passwd和shadow文件中用户的相关属性 
pwcov      注：同步用户从/etc/passwd 到/etc/shadow 
pwck       注：pwck是校验用户配置文件/etc/passwd 和/etc/shadow 文件内容是否合法或完整；
pwunconv   注：是pwcov 的立逆向操作，是从/etc/shadow和 /etc/passwd 创建/etc/passwd ，然后会删除 /etc/shadow 文件；
finger     注：查看用户信息工具
id         注：查看用户的UID、GID及所归属的用户组
chfn       注：更改用户信息工具
su         注：用户切换工具（su 和 su - 这两个切换用户是有区别的。）
           su 命令 和 su - 命令最大的本质区别就是：前者只是切换了root身份，但Shell环境仍然是普通用户的Shell；
           而后者连用户和Shell环境一起切换成root身份了。只有切换了Shell环境才不会出现PATH环境变量错误。su切换成root用户以后。
           pwd一下，发现工作目录仍然是普通用户的工作目录；而用su -命令切换以后，工作目录变成root的工作目录了。
           用echo $PATH命令看一下su 和su - 以后的环境变量有何不同。
           要从当前用户切换到其它用户，推荐使用su - 命令，这样连shell环境也切换了。
sudo       注：sudo 是通过另一个用户来执行命令（execute a command as another user）。
           su 是用来切换用户，然后通过切换到的用户来完成相应的任务，
           但是 sudo 能后面直接执行命令，比如 sudo 不需要root 密码就可以执行只有root用户才能执行的命令；
           但是的通过visudo 来编辑/etc/sudoers来实现；
visudo     注：visodo 是编辑 /etc/sudoers 的命令；也可以不用这个命令，直接用vi 来编辑 /etc/sudoers 的效果是一样的；
sudoedit   注：和sudo 功能差不多；
```

 

**2. 管理用户组（group）的工具或命令；**

```properties
groupadd     注：添加用户组；
groupdel     注：删除用户组；
groupmod     注：修改用户组信息 
newgrp       注：切换到一个新组 
gpasswd      注：管理组和组密码的命令。man gpasswd
groups       注：显示用户所属的用户组
grpck
grpconv      注：通过/etc/group和/etc/gshadow 的文件内容来同步或创建/etc/gshadow ，如果/etc/gshadow 不存在则创建；
grpunconv    注：通过/etc/group 和/etc/gshadow 文件内容来同步或创建/etc/group ，然后删除gshadow文件；
```

**3. /etc/skel 目录；**

/etc/skel目录一般是存放用户启动文件的目录，这个目录是由root权限控制，当我们添加用户时，这个目录下的文件自动复制到新添加的用户的家目录下；/etc/skel 目录下的文件都是隐藏文件，也就是类似.file格式的；我们可通过修改、添加、删除/etc/skel目录下的文件，来为用户提供一个统一、标准的、默认的用户环境；

```yaml
[root@localhost beinan]# ls -la /etc/skel/
总用量 92
drwxr-xr-x    3 root root  4096  8月 11 23:32 .
drwxr-xr-x  115 root root 12288 10月 14 13:44 ..
-rw-r--r--    1 root root    24  5月 11 00:15 .bash_logout
-rw-r--r--    1 root root   191  5月 11 00:15 .bash_profile
-rw-r--r--    1 root root   124  5月 11 00:15 .bashrc
-rw-r--r--    1 root root  5619 2005-03-08  .canna
-rw-r--r--    1 root root   438  5月 18 15:23 .emacs
-rw-r--r--    1 root root   120  5月 23 05:18 .gtkrc
drwxr-xr-x    3 root root  4096  8月 11 23:16 .kde
-rw-r--r--    1 root root   658 2005-01-17  .zshrc 
```

/etc/skel 目录下的文件，一般是我们用useradd 和adduser 命令添加用户（user）时，系统自动复制到新添加用户（user）的家目录下；如果我们通过修改 /etc/passwd 来添加用户时，我们可以自己创建用户的家目录，然后把/etc/skel 下的文件复制到用户的家目录下，然后要用chown 来改变新用户家目录的属主；

**4. /etc/login.defs 配置文件；**

/etc/login.defs 文件是当创建用户时的一些规划，比如创建用户时，是否需要家目录，UID和GID的范围；用户的期限等等，这个文件是可以通过root来定义的；

比如Fedora 的 /etc/logins.defs 文件内容；

```apache
# *REQUIRED*
#   Directory where mailboxes reside, _or_ name of file, relative to the
#   home directory.  If you _do_ define both, MAIL_DIR takes precedence.
#   QMAIL_DIR is for Qmail
#
#QMAIL_DIR      Maildir
MAIL_DIR        /var/spool/mail  注：创建用户时，要在目录/var/spool/mail中创建一个用户mail文件；
#MAIL_FILE      .mail

# Password aging controls:
#
#       PASS_MAX_DAYS   Maximum number of days a password may be used.
#       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
#       PASS_MIN_LEN    Minimum acceptable password length.
#       PASS_WARN_AGE   Number of days warning given before a password expires.
#
PASS_MAX_DAYS   99999   注：用户的密码不过期最多的天数；
PASS_MIN_DAYS   0       注：密码修改之间最小的天数；
PASS_MIN_LEN    5       注：密码最小长度；
PASS_WARN_AGE   7       注：

#
# Min/max values for automatic uid selection in useradd
#
UID_MIN                   500  注：最小UID为500 ，也就是说添加用户时，UID 是从500开始的；
UID_MAX                 60000   注：最大UID为60000；

#
# Min/max values for automatic gid selection in groupadd
#
GID_MIN                   500   注：GID 是从500开始；
GID_MAX                 60000

#
# If defined, this command is run when removing a user.
# It should remove any at/cron/print jobs etc. owned by
# the user to be removed (passed as the first argument).
#
#USERDEL_CMD    /usr/sbin/userdel_local

#
# If useradd should create home directories for users by default
# On RH systems, we do. This option is ORed with the -m flag on
# useradd command line.
#
CREATE_HOME     yes   注：是否创用户家目录，要求创建；
```

**5. /etc/default/useradd 文件；**

通过useradd 添加用户时的规则文件；

```ini
# useradd defaults file
GROUP=100
HOME=/home  注：把用户的家目录建在/home中；
INACTIVE=-1  注：是否启用帐号过期停权，-1表示不启用；
EXPIRE=   注：帐号终止日期，不设置表示不启用；
SHELL=/bin/bash  注：所用SHELL的类型；
SKEL=/etc/skel   注： 默认添加用户的目录默认文件存放位置；也就是说，当我们用adduser添加用户时，用户家目录下的文件，都是从这个目录中复制过去的；
```

 

 

 

# 用户管理

 

在linux中为了保障系统用户的安全信息，所以将用户帐号，用户密码，用户组信息和用户组密码分开存放在不同的配置文件中的。

1. 用户名配置文件：/etc/passwd
2. 用户密码配置文件：/etc/shadow
3. 用户组配置文件：/etc/group
4. 用户组密码配置文件：/etc/gshadow

把这四个文件弄清楚，添加、删除用户和用户组可以通过修改这4个文件来完成。

 

## **用户分类**

linux用户有三类：根用户（root用户，命令提示符介面显示#）、普通用户 （命令提示符显介面示#）、虚拟用户。

1. root用户：根用户也就是超级用户，拥有最高权限任务,任何文件权限对根用户无效，用户UID为0
2. 普通用户：可以登陆系统，只能操作自己拥有文件权限的文件。是由系统管理员创建的。用户UID从501开始（500~60000 之间）
3. 虚拟用户：又称伪用户，不具备系统登陆的权限，一般由系统创建或某些程序安装后创建，

 

## **用户名文件 /etc/passwd 文件结构**

![img](https://img-blog.csdn.net/20141019205939421)

以上/etc/passwd文件的属性分解如下，共七个属性（每个属性用分号分割）

***\*( /etc/passwd 每行7个字段 “ 账号:密码:UID:GID:账号信息说明:家目录:Shell ” )\****

```groovy
示例                smbuser:x:500:500::/home/smbuser:/bin/bash
对应字段解释        用户名： 加密口令： UID： 用户所属组的GID: 个人信息描述： 用户主目录： 登陆shell

1.用户名  ：由字母、数字组成。必须以字母开头，区分大小写，长度不超过8个字符
2.加密口令：在早期的UNIX版本中，用户和密码放在一起的，用户名在第一个选项位，密码在第二个选项位，
            后来出于账户安全的考虑，对此进行了改进，将密码另外存放，并用一个特殊字符X占用第二个密码选项位。
            因为passwd文件对所有用户都是可读的，所以为了安全起见，系统将加密口令存放于/etc/shadow文件中且只有超级用户才有读取权限。
3.用户UID： 系统中用来标识用户的数字（root：UID为0  ，普通用户UID为500-60000之间）
        管理员：0。其中0表示系统管理员ROOT账户。 一般用户1-65535（一般用户又分系统用户和普通用户）
        系统用户：1-499（系统默认创建）。    1－499用于系统内置账户，如nobody,bin等
        普通用户：500-65535（由管理员创建）。500－65535普通系统本地账户，系统安装完成后，由ROOT用户创建分配给有需要接入Linux主机的使用者使用。
        如果因为需要，可以将普通系统本地账户的UID改为0，就样该用户就有了ROOT用户一样的权限了，但不建议这样设置。 
4.用户所属组GID：默认刚创建，主GID和用户UID相同，是主用户组，可以更改（一般组分为主用户组和附加组）
    用户组的GID，用户组名保存在/etc/group文件中，有时为了便于权限的分配，可以将多个用户加入到同一个用户组中，对用户组分配相应权限，
    这样该用户组中的所有用户都同时获得了相同的权限，可以简化对每一个用户来分配权限。
    将用户加入其它用户组中的操作可以在/etc/group文件中的用户组名后批量添加相应的用户名称便可，不需要在此进行修改。
5.用户的全名信息，或描述等内容
6.用户主目录：   创建普通系统帐户时会默认创建一个和用户名相同的帐户，用户登陆后默认所处的目录一般为~（[root@centso-test ~]）
7.用户登陆后使用的shell，其中/sbin/nologin为限制用户登陆系统
        用户登陆后启动以接收并解析执行用户输入命令的程序，如./bin/bash  /bin/csh.     
        虚拟用户一般该属性为空或者是 /sbin/nologin     /bin/fase 表示禁止用户登陆
```

 

## **用户密码文件 /etc/shadow 文件结构（\**\*\*以：作为分隔符，一共有九栏\*\**\*）**

```properties
[root@serverln tmp]# cat /etc/shadow
root:$6$hgwagXkBWGArriiQ$sajFOnZmrW2x0S6dipxvXXAJNfB1/BC0OvzjqeGZqQBVbng3aD9/CeD2j3YOnd6g2NJfPnk7zI1.9xqQ5aO6d0:16381:0:99999:7:::
bin:*:15980:0:99999:7:::
jacob:!!:16382:0:99999:7:::
帐号名称：密码：最近更改密码日期：密码不可被更改日期：密码需重设定日期：密码到期警告天数：密码过期后能使用的天数：帐号失效的日期：保留

1 用户名
2 经过加密码后的密码信息。
  这个才是该账号的真正的密码，不过这个密码已经加密过了，但是有些黑客还是能够解密的。所以为了安全，该文件属性设置为600，只允许root读写。
3 最近改动密码的日期,该日期参数是以1970年1月1日作为第1天而累加计算出的
4 密码不可改动的天数,由选项3中的日期为第1天,如果该选项参数为0,表示随时可以更改
5 密码需要改动的天数,即强制用户每多少天修改密码,密码过期后需要更新密码后才可以登陆
6 密码快要到期的前多少天提示警告信息
7 密码到期后可以宽限的天数,密码过期后,在此时间内通过更改密码后,并登陆来重新激活账号
8 账号失效日期,在一些收费使用系统的应用中,可以设置此选项内容.
9 预留选项,未使用
```

 

## 用户管理

 

### 添加用户 useradd 命令

root@kali:~# useradd -h

```sql
选项：
  -b, --base-dir BASE_DIR       设置基本路径作为用户的登录目录  
  -c, --comment COMMENT         对用户的注释  
  -d, --home-dir HOME_DIR       设置用户的登录目录  
  -D, --defaults                改变设置  
  -e, --expiredate EXPIRE_DATE  新账户的过期日期。设置用户的有效期  
  -f, --inactive INACTIVE       用户过期后，让密码无效  
  -g, --gid GROUP               使用户 “只属于某个组 ”            （只能属于一个组）
  -G, --groups GROUPS           新账户的附加组列表。使用户加入某个组（可以属于多个组）  
  -h, --help                    帮助
  -k, --skel SKEL_DIR           使用此目录作为骨架目录
  -K, --key KEY=VALUE           不使用 /etc/login.defs 中的默认值
  -l, --no-log-init             不把用户加入到lastlog文件中  
  -m, --create-home             自动创建登录目录  
  -M, --no-create-home          不自动创建登录目录  
  -N, --no-user-group           不创建同名的组
  -o, --non-unique              允许使用重复的 UID 创建用户
  -p, --password PASSWORD       为新用户使用加密密码  
  -r, --system                  创建一个系统账户
  -R, --root CHROOT_DIR         chroot 到的目录
  -s, --shell SHELL             登录时候的shell  
  -u, --uid UID                 为新用户指定一个UID  
  -U, --user-group              创建与用户同名的组
  -Z, --selinux-user SEUSER     为 SELinux 用户映射使用指定 SEUSER
```

在创建用户时，需要为新建用户指定一用户组，如果不指定其用户所属的工作组，自动生成一个同名同ID（UID＝GID）的用户组，即与用户名同名的工作组。

```groovy
useradd -g users user1    // 创建用户 user1 的时候指定其所属工作组 users

useradd test   // 增加用户test，有一点要注意的，useradd增加一个用户后，不要忘了给他设置密码，不然不能登录的。
passwd test    // 设置用户test 的密码   

useradd -g test phpq   //新建phpq用户并让用户 “只属于test工作组”。  注：：-g 所属组 -d 家目录 -s 所用的SHELL

useradd 用户名 //创建一个新用户 
useradd -G 用户组 用户名 //新建一个用户，同时添加到附加组中 
useradd -u 600 liuyalei //创建一个用户liuyalei，指定uid为600
useradd -M /sbin/nologin liuyalei //创建liuyalei用户，不创建家目录，不允许登录
useradd -e //帐号中指日期，日期的指定格式为MM/DD/YY
useradd -g //grop名称或以数字来作为用户登入起始用户组。
//用户组名需为系统现有存在的名称。用户组数字也需为现有存在的用户组，预设的用户组数字为1
useradd -G //定义此用户为多个不同groups的成员。每个用户组使用逗号隔开。用户组名同-g的限制。默认值为用户起始用户组
```

添加用户和组

```bash
使用 useradd test123 命令给linux系统添加一个新的用户。
使用 passwd test123 命令给用户“test123”添加密码。输入两次一样的密码即可完成创建。
使用 groupadd groupTest 命令创建用户组。
使用 usermod -G groupTest test123 将已有的用户添加到已有的组中。
使用 cat /etc/group |grep group 命令查看是否将刚才的用户添加到组中。
使用 useradd -g test king 命令将新建的“king”用户添加到已有的“test”组
```

 

```ruby
示例： [root@serverln user_test]# useradd -c administrator -d /home/admin -e 2014-12-12 -g root -G,bin,mail

创建admin帐户 用户信息，指定用户信息为administrator,家目录为/home/admin，失效日期为2014年12月12日，指定默认组为root，附加组为bin,mail
```

 

### 设置帐号属性 usermod (user modify) 命令

对于已创建好的用户，可使用usermod命令来修改和设置账户的各项属性，包括登录名，主目录，用户组，登录shell等，

命令用法：usermod [option] username

更多参数看帮助文档：man usermod   或者   usermod -h

（1）改变用户帐户名

```ruby
使用-l参数来实现，命令用法为：usermod -l 新用户名 原用户名

例如，若要将用户lijie更名为lijunjie，则操作命令为：

[root@localhost ~]# usermod -l lijunjie lijie
[root@localhost ~]# tail -1 /etc/passwd
lijunjie:x:502:502::/home/lijie:/bin/bash

从输出结果可见，用户名已更改为lijunjie。主目录仍为原来的/home/lijie，若也要更改为/home/lijunjie，则可通过执行以下命令来实现

[root@localhost ~]# usermod -d /home/lijunjie lijunjie
[root@localhost ~]# tail -1 /etc/passwd
lijunjie:x:502:502::/home/lijunjie:/bin/bash
[root@localhost ~]# mv /home/lijie /home/lijunjie
```

（2）锁定账户
若要临时禁止用户登录，可将该用户账户锁定。锁定账户可利用-L参数来实现，命令用法为：usermod -L 要锁定的账户
linux锁定用户，是通过在密码文件shadow的密码字段前加“！”来标识该用户被锁定。

（3）解锁账户

要解锁账户，可以使用带-U参数的usermod命令来实现

 

 

**修改用户加入单个组和多个组**

```markdown
********************注意和创建用户的 useradd 命令 -G 参数的区别*********************************
usermod -d /home/test -G test2 test   //将test用户的登录目录改成/home/test，并加入test2组，注意这里是大G。
注意：将一个用户添加到用户组中，千万不能直接用： usermod -G groupA userName 
这样做会使你离开其他用户组，仅仅做为 这个用户组 groupA 的成员。 
usermod -g 组名 用户名  // 是修改用户所在组，其它组都给删除了。

usermod -G 添加多个组要用","号隔开而且一次性添加，分开添加只能添加最后的那个组。
# usermod -G groupA,groupB,groupC user

如果真要使用 -G 选项追加组，应该加上 -a 选项（-a 代表 append， 也就是 将自己添加到 用户组groupA 中，而不必离开 其他用户组。 ）： 
usermod -a -G groupA user    // 示例： usermod -a G dba grid
```

![img](https://img-blog.csdn.net/20171012112216384)

```
# Linux把用户加入某个组（不退出当前所属组 同时属于多个组）usermod -a -G groupname username #查看某用户所属组groups username
```

gpasswd -a test test2 //将用户test加入到test2组
gpasswd -d test test2 //将用户test从test2组中移出

 

### 删除用户 userdel

更多选项参看：man userdel  或者  userdel -h

```crystal
userdel 用户名        //删除用户  示例：userdel test //将test用户删除
userdel -r liuyalei    //连同家目录删掉
```

生产环境中，我们可以通过注视配置文件/etc/passwd，选择先注销而不删除用户，类似回收站的作用，即使出了问题也能恢复。

 

### 将文件夹赋给相应用户以及所在组

```ruby
给oinstall组中的oracle相应的拥有者权限
[root@gl oracle]# chown -R oracle:oinstall /data/encryption/  

然后指定相应的用户或组的相应权限
[root@gl oracle]# chmod -R 775 /data/encryption/
```

 

### 查看用户

```ruby
a），查看当前登录用户
        [root@krlcgcms01 ~]# w
        [root@krlcgcms01 ~]# who
b），查看自己的用户名
        [root@krlcgcms01 ~]# whoami
c），查看单个用户信息
        [root@krlcgcms01 ~]# finger apacheuser
        [root@krlcgcms01 ~]# id apacheuser
d），查看用户登录记录
        [root@krlcgcms01 ~]# last 查看登录成功的用户记录
        [root@krlcgcms01 ~]# lastb 查看登录不成功的用户记录
e），查看所有用户
        [root@krlcgcms01 ~]# cut -d : -f 1 /etc/passwd
        [root@krlcgcms01 ~]# cat /etc/passwd |awk -F \: '{print $1}'
```

 

### 查看某个用户所属的组

命令：groups 用户名
例如:[root@xxx~]# groups root
root : root bin daemon sys adm disk wheel
用户root属于root bin daemon sys adm disk wheel这7个group 

 

### 查看系统中所有组

命令：more /etc/group 

 

## 用户口令(密码)管理

用户管理的一项重要内容是用户口令（密码）的管理。用户账号刚创建时没有口令，但是被系统锁定，无法使用，必须为其指定口令后才可以使用，即使是指定空口令。

指定和修改用户口令的Shell命令是：passwd [用户名]。*超级用户可以为自己和其他用户指定口令，普通用户只能用它修改自己的口令。*
命令的格式为：passwd 选项 用户名
可使用的选项：

```sql
名称：passwd - 更改用户密码
用法：passwd [选项] [登录用户名]
描述：passwd 命令用来更改用户账户的密码。
      普通用户通常只更改其自己账户的密码，而超级用户可以更改任何账户的密码。
      passwd 也能更改账户或相关的密码有效期。

   密码更改
       如果有旧密码，首先提示用户输入旧密码。加密这个密码然后和存储的密码进行比较。用户只有一次机会输入正确密码。
       允许超级用户略过这个步骤，以便更改忘记了的密码。

选项
       passwd 命令可以接受的选项有：

       -a, --all       此选项只能和 -S 一起使用，来显示所有用户的状态。
       -d, --delete    使账号无口令。 删除用户密码(让它为空)。这是禁用一个用户密码的快速方法。
       -e, --expire    让一个账户的密码立即过期。这可以强制一个用户下次登录时更改密码。
       -f              强迫用户下次登录时修改口令。
       -h, --help      现实帮助信息并退出。
       -i, --inactive INACTIVE    密码过期一定天数之后禁用账户。
       -k, --keep-tokens    在过期后修改密码
       -l, --lock     锁定口令，即禁用账号。此选项通过将密码更改为一个不可能与加密值匹配的值来禁用(它在密码开头添加一个“!”)。
           注意，这并没有禁用此账户。用户仍然可以通过其它认证方式(如 SSH 密码)来登录。要禁用此账户，管理员需要使用
           usermod --expiredate 1 (设置账户的过期时间为1970年1月2日)。
           被锁定了密码的用户不允许更改密码。

       -n, --mindays MIN_DAYS    设置到下次修改密码所须等待的最短天数（更改密码之间的时间间隔，即多长时间改一次密码）
           在密码更改之间的最小天数设置为 MIN_DAYS。此字段中的 0 值表示用户可以在任何时间更改其密码。

       -q, --quiet    安静模式。
       -r, --repository REPOSITORY    在 REPOSITORY 中更改密码

       -R, --root CHROOT_DIR
           Apply changes in the CHROOT_DIR directory and use the configuration files from the CHROOT_DIR directory.

       -S, --status
           显示账户状态信息。状态信息包含 7 个字段。首个字段是用户的登录名，第二个字段表示用户账户是否已经锁定密
           码(L)、没有密码 (NP)或者密码可用(P)，第三个字段给出最后一次更改密码的日期。接下来的四个字段分别是密码的最小
           年龄、最大年龄、警告期和禁用期。这些年龄以天为单位计算。

       -u, --unlock    口令解锁。解锁指定用户的密码。此操作通过将密码改回先前值(改回使用 -l 之前的值)重新启用密码。
       -w, --warndays WARN_DAYS
           设置在要求更改密码之前警告的天数。WARN_DAYS 选项是在密码过期之前提前警告的天数。

       -x, --maxdays MAX_DAYS
           设置密码仍然有效的最大天数。MAX_DAYS 之后，密码会要求更改。


文件
       /etc/passwd          用户账户信息。
       /etc/shadow          用户账户的密码信息。
       /etc/pam.d/passwd    passwd 的 PAM 配置。

退出值
       passwd 命令退出，并返回如下值：
       0    成功
       1    权限不够
       2    无效的选项组合
       3    意外的失败，什么也没有做。
       4    意外的失败，passwd 文件丢失
       5    passwd 文件忙，请重试
       6    给了选项一个无效的参数
参阅
       chpasswd(8), passwd(5), shadow(5), usermod(8).
```

示例

```php
例如，假设当前用户是sam，则下面的命令修改该用户自己的口令：
$ passwd
Old password:******
New password:*******
Re-enter new password:*******

如果是超级用户，可以用下列形式指定任何用户的口令：
# passwd sam
New password:*******
Re-enter new password:*******

普通用户修改自己的口令时，passwd命令会先询问原口令，验证后再要求用户输入两遍新口令，如果两次输入的口令一致，则将这个口令指定给用户；而超级用户为用户指定口令时，就不需要知道原口令。

为用户指定空口令时，执行下列形式的命令：
# passwd -d sam
此命令将用户sam的口令删除，这样用户sam下一次登录时，系统就不再询问口令。

passwd命令还可以用-l(lock)选项锁定某一用户，使其不能登录，例如：
# passwd -l sam
```

 

**锁定/解锁账户密码**

***\**在linux中，除了用户账户可被锁定外，账户密码也可被锁定，任何一方被锁定后，都将无法登录系统。\*\****

只有root用户才有权执行该命令，锁定账户密码使用带-l参数的passwd命令，其用法为：
passwd -l 帐户名
passwd -u 帐户名 #解锁账户密码

**查询密码状态**

要查询当前账户的密码是否被锁定，可以使用带-S参数的passwd命令来实现，其用法为：
passwd -S 账户名
例如
[root@localhost etc]# passwd -S lijunjie
lijunjie LK 2011-03-25 0 99999 7 -1 (密码已被锁定。)
[root@localhost etc]# passwd -u lijunjie    // 解锁用户 lijunjie 的密码 。

[root@localhost etc]# passwd -S lijunjie
lijunjie PS 2011-03-25 0 99999 7 -1 (密码已设置，使用 SHA512 加密。)

**删除账户密码**
如要删除账户的密码，使用带-d参数的passwd命令来实现，该命令也只有root用户才有权执行，其用法为：
passwd -d 帐户名
帐户密码被删除后，将不能登录系统，除非重新设置密码。

 

 

# 用户组管理

 

***\**涉及命令：gpasswd （管理组）、groupadd（添加组）、groupmod （修改组） 、groupdel（删除组）\*\****   

LINUX下把一个用户加入多个组 ，实现方法：# gpasswd -a 用户 组    // 示例：gpasswd -a user group  // 将 user 用户加入到 group 组中

如果查看/etc/shadow 或/etc/passwd等文件看不出来用户已经加入想要的组时，此时可以用 id 命令 来查看

```ruby
用户ruan隶属于ruan组，id为500，现在想要同时加入组ruanrongjian中
[root@diandianrong ~]# gpasswd -a rong ruanrongjian
Adding user rong to group ruanrongjian
先切换到ruan用户，然后用ID查看
[root@diandianrong ~]# su ruan
[ruan@diandianrong root]$ id
uid=500(ruan) gid=500(ruan) groups=500(ruan),501(ruanrongjian) context=user_u:system_r:unconfined_t
当然直接用id ruan 也是可以的

[ruan@diandianrong root]$ id ruan
uid=500(ruan) gid=500(ruan) groups=500(ruan),501(ruanrongjian)
此时group加有两个，组ruan是在用户被创建时自发产生的，ruanrongjian则是后面手动添加进去的。
```

另一种实现方法：

```ini
[root@diandianrong ~]# usermod -a -G rong ruanrongjian   // 必须加上 -a 选项才是追加组，否则是清除原来的组，只加入组
[root@diandianrong ~]# id rong
uid=502(rong) gid=502(rong) groups=502(rong),501(ruanrongjian)
```

 

## 用户组文件 /etc/group

 

**/etc/group 共4个字段   群组名:群组密码:GID:群组支持的群组名称(即可以加入多个群组)**

```less
[root@localhost ~]# cat /etc/group
root:x:0:root
bin:x:1:root,bin,daemon
daemon:x:2:root,bin,daemon
……
```

用户组的选项内容只有四个，其选项意义如下：

1. 用户组名
2. 用户组密码，同样经改进后，用户组密码不再保存在用户组名文件中，另存放在/etc/gshadow文件中，此选项使用字符x占位
3. 用户组GID
4. 用户组中包含的用户名，默认只含有同用户组名的用户，如需往用户组中添加多个用户名，可以在此选项中添加，并用逗号隔开

```ruby
[root@localhost ~]# groups
root bin daemon sys adm disk wheel
[root@localhost ~]# 
以上操作，表示用户名root同时属于root,bin,daemon,sys,adm,disk,wheel等用户组中
或者 
[root@localhost ~]# groups root
```

 

## 用户组密码文件 /etc/gshadow

 

组密码文件示例：

```ruby
[root@localhost etc]# cat gshadow
root:::root
bin:::root,bin,daemon
daemon:::root,bin,daemon
……
cisco:!::
huawei:!::
named:!::
```

用户组密码文件中的内容和用户组名文件中的内容几乎一样，因为一般很少用到需要对用户组设置密码等的操作

1. 组名称
2. 密码栏，同样的，开头为 ! 表示无合法密码，所以无群组管理员
3. 群组管理员的帐号 (相关资讯在 gpasswd 中介绍)
4. 该群组的所属帐号 (与 /etc/group 内容相同！)

 

## 用户组的管理操作

 

### 添加、删除用户和组

groupadd 命令

```sql
root@kali:~# groupadd -h
用法：groupadd [选项] 组

选项:
  -f, --force           如果组已经存在则成功退出
                        并且如果 GID 已经存在则取消 -g
  -g, --gid GID                 为新组使用 GID
  -h, --help                    显示此帮助信息并推出
  -K, --key KEY=VALUE           不使用 /etc/login.defs 中的默认值
  -o, --non-unique              允许创建有重复 GID 的组
  -p, --password PASSWORD       为新组使用此加密过的密码
  -r, --system                  创建一个系统账户
  -R, --root CHROOT_DIR         chroot 到的目录
```

可以使用的选项有：
  -g GID 指定新用户组的组标识号（GID）。
  -o 一般与 -g 选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。 

例如：

```markdown
# groupadd group1 
此命令向系统中增加了一个新组group1，新组的组标识号是在当前已有的最大组标识号的基础上加1。 
#groupadd -g 101 group2 
此命令向系统中增加了一个新组group2，同时指定新组的组标识号是101。 
```


**删除用户组**

删除用户组使用 groupdel 命令来实现，命令用法：groupdel 用户组   // 示例：groupdel liuyalei  //删除liuyalei组

在删除用户组时，被删除的用户组不能是某个账户的私有用户组，否则将无法删除，若要删除，则应先删除引用该私有用户组的账户，然后再删除用户组。

```csharp
[root@localhost ~]# groupdel teacher
groupdel: cannot remove the primary group of user 'zhangjie'
[root@localhost ~]# userdel -r zhangjie
[root@localhost ~]# groupdel teacher
[root@localhost ~]# grep teacher /etc/group #没有输出，说明teacher用户组已经不存在，删除成功
```

一般直接通过usermod命令来更改用户的相关属性（包括用户组属性）

```bash
groupadd test            //增加一个test组
groupmod -n test2 test   //将test组的名子改成test2
groupdel test2           //删除组 test2
```

 

**修改组** 

命令：groupmod 选项 用户组 

更多参数看帮助：man groupmod   或者   groupmod -h

常用的选项有：
  -g GID 为用户组指定新的组标识号。
  -o 与-g选项同时使用，用户组的新GID可以与系统已有用户组的GID相同。
  -n新用户组 将用户组的名字改为新名字

例如：

```cpp
# groupmod -g 102 group2     //此命令将组group2的组标识号修改为102。 
# groupmod –g 10000 -n group3 group2     //此命令将组group2的标识号改为10000，组名修改为group3。 
```

修改用户组属性

 用户组创建后，根据需要可对用户组的相关属性进行修改。对用户组属性的修改，主要是修改用户组的名称和用户组的GID值。

（1）改变用户组的名称（***\*重命名组名\****）

```less
    若要对用户组进行重命名，可使用带-n参数的groupmod命令来实现，其用法为：
    groupmod -n 新用户组名  原用户组名
    对于用户组改名，不会改变其GID的值
    比如，若要将student用户组更名为teacher用户组，则操作命令为：
    [root@localhost ~]# tail -3 /etc/group
    student:x:501:
    lijie:x:502:
    vodup:x:503:
    [root@localhost ~]# groupmod -n teacher student
    [root@localhost ~]# tail -3 /etc/group
    lijie:x:502:
    vodup:x:503:
    teacher:x:501:
```

（2）重设用户组的GID

 

 用户组的GID值可以重新进行设置修改，但不能与已有用户组的GID值重复。对GID进行修改，不会改变用户名的名称。

 要修改用户组的GID，可使用带-g参数的groupmod命令，其用法为：groupmod -g new_GID 用户组名称

```less
例如，若要将teacher组的GID更改为504，则操作命令为：
[root@localhost ~]# tail -3 /etc/group
lijie:x:502:
vodup:x:503:
teacher:x:501:
[root@localhost ~]# groupmod -g 504 teacher
[root@localhost ~]# tail -3 /etc/group
lijie:x:502:
vodup:x:503:
teacher:x:504:
```

 

**添加用户到指定的组/从指定的组中移除用户**

gpasswd命令是 Linux 下工作组文件/etc/group 和 /etc/gshadow 管理工具。主要功能：管理组。

 

```sql
root@kali:~# gpasswd -h
用法：gpasswd [选项] 组

选项：
  -a, --add USER                向组 GROUP 中添加用户 USER
  -d, --delete USER             从组 GROUP 中添加或删除用户
  -h, --help                    显示此帮助信息并推出
  -Q, --root CHROOT_DIR         要 chroot 进的目录
  -r, --remove-password         移除组 GROUP 的密码
  -R, --restrict                向其成员限制访问组 GROUP
  -M, --members USER,...        设置组 GROUP 的成员列表
  -A, --administrators ADMIN,...        设置组的管理员列表
除非使用 -A 或 -M 选项，不能结合使用这些选项。
```

可以将用户添加到指定的组，使其成为该组的成员。其实现命令为：gpasswd -a 用户账户 用户组名

若要从用户组中移除某用户，其实现命令为：gpasswd -d 用户账户 用户组名

```csharp
例如，现在将上面创建的lijunjie用户添加到vodup用户组中

[root@localhost ~]# groups lijunjie
lijunjie : lijie
[root@localhost ~]# gpasswd -a lijunjie vodup  #添加用户到指定的用户组
Adding user lijunjie to group vodup
[root@localhost ~]# groups lijunjie
lijunjie : lijie vodup
[root@localhost ~]# gpasswd -d lijunjie vodup  #从指定的用户组中移除用户
Removing user lijunjie from group vodup
[root@localhost ~]# groups lijunjie   #查看用户所属的组
lijunjie : lijie
```

**设置 用户组管理员**

添加用户到组和从组中移除某用户，除了 root 用户可以执行该操作外，用户组管理员也可以执行该操作。

将某用户指派为某个用户组的管理员命令：gpasswd -A 用户账户 要管理的用户组
命令功能：将指定的用户设置为指定用户组的用户管理员。用户管理员只能对授权的用户组进行用户管理(添加用户到组或从组中删除用户)，无权对其他用户组进行管理。

示例

```csharp
[root@localhost ~]# gpasswd -A lijunjie vodup
[root@localhost ~]# useradd fen
[lijunjie@localhost ~]# gpasswd -a fen vodup
Adding user fen to group vodup
[lijunjie@localhost ~]# groups fen
fen : fen vodup
[lijunjie@localhost ~]# gpasswd -d fen vodup
Removing user fen from group vodup
[lijunjie@localhost root]$ gpasswd -d fen fen  #试图将fen用户从fen用户组中移除
gpasswd: Permission denied.                    #操作被拒绝，说明无权对其他用户组进行管理
```

 

## 切换用户所属的组

 

如果一个用户同时属于多个用户组，那么用户可以在用户组之间切换，以便具有其他用户组的权限。
用户可以在登录后，使用命令newgrp切换到其他用户组，这个命令的参数就是目的用户组。

```ruby
例如： $ newgrp root 
```

这条命令将当前用户切换到root用户组，前提条件是root用户组确实是该用户的主组或附加组。

 

## 查看组

```perl
a），查看当前登录用户所在的组 groups，查看apacheuser所在组groups apacheuser
b），查看所有组 cat /etc/group
c），有的linux系统没有/etc/group文件的，这个时候看下面的这个方法
cat /etc/passwd |awk -F [:] '{print $4}' |sort|uniq | getent group |awk -F [:] '{print $1}'
这里用到一个命令是getent,可以通过组ID来查找组信息,如果这个命令没有的话,那就很难查找,系统中所有的组了.
```

 

![img](https://img-blog.csdn.net/20171011164751543)

 

```ruby
查看用户所属的组使用命令： $ groups user
或者查看文件： $ cat /etc/group
查看一个用户的UID和GID： $ id user 或者 cat /etc/passwd
finger命令 ——可以查看用户的主目录、启动shell、用户名、地址、电话等信息
例：finger root
```

 

# 文件或目录权限的简介

 

鸟哥 Linux 的文件权限与目录配置：http://linux.vbird.org/linux_basic/0210filepermission.php

Linux中的特殊权限粘滞位(sticky bit)详解：http://www.cnblogs.com/MrListening/p/5813281.html

 

在Linux系统中，对每个文件和目录都有定义相应的属主和属组，以用来管理用户对文件和目录的访问和操作
chown [用户名][文件或目录] 更改文件或目录的所有者（用户），即属主
chgrp [用户组名][文件或目录] 更改文件或目录所属的用户组，即属组
也可以使用“chown 用户名:用户组名(用户名.用户组名) 文件或目录”直接对文件或目录的用户和用户组进行修改
chmod [ugoa][+-=][rwx] 文件或目录名，更改用户和用户组对目录或文件的访问权限，
其简化操作为 chmod 755 /home/user01/ ＝ chmod u=rwx /home/user01|chmdo g=rw /home/user01|chmod o=rw /home/user01

```yaml
[root@localhost home]# ls -l
total 32
drwxr-xr-x. 30 cisco cisco 4096 Sep 10 03:52 cisco
drwx------. 4 huawei huawei 4096 Sep 8 18:54 huawei
drwx------. 2 root root 16384 Sep 3 05:17 lost+found
drwxr-xr-x. 4 nobody nobody 4096 Sep 9 20:39 samba
drwxr-xr-x. 2 root root 4096 Sep 9 01:24 share
[root@localhost home]# 

1. 第一个选项代表这个档案是‘目录、档案或连结档等等’：
    当为[ d ]则是目录，
    当为[ - ]则是档案，
    若是[ l ]则表示为链接文件(link file)，目录的快捷方式
    若是[ b ]则表示为装置档里面的可供储存的周边设备(可随机存取装置)；
    若是[ c ]则表示为装置档里面的序列埠设备，例如键盘、滑鼠(一次性读取装置)。 
2.表示快捷方式指向本文件或目录的数，最少是一个
3.目录或文件的所有者用户
4.目录或文件的所有组
5.目录或文件的大小，目录大小都为4096，文件大小为实际大小
6.目录或文件的最近修改日期
7.目录或文件的名称
```

 

 

# 【综合案例】

【题1.1】建立两个用户组group1和group2，以及三个用户dennis、daniel、abigale，并且将前2个用户分配在group1用户组下，后一个分配在group2用户组下

 ![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80MTI5MjQvMjAxNDEyLzI3MTkzNTQ3MjAyOTE4MS5wbmc)

【题1.2】以dennis用户登录，创建一个Hello.java文件

【题1.3】以daniel用户登录，观察是否可以访问/home/dennis目录以及读或写其创建的Hello.java文件

【题1.4】以dennis用户登录，修改目录/home/dennis及Hello.java文件的读写权限（更正：修改目录权限的时候，应该使用770，而不是760，否则权限不足）

 ![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80MTI5MjQvMjAxNDEyLzI3MTkzNjIwNzMzNDE3MC5wbmc)

【题1.5】重复【题1.3】

【题1.6】改变abigale的用户组由group2变为group1

 ![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80MTI5MjQvMjAxNDEyLzI3MTkzNjU0MjMzNjQwMi5wbmc)

然后，可以使用cat /etc/passwd查看并确定

 ![img](https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlcy5jbml0YmxvZy5jb20vYmxvZy80MTI5MjQvMjAxNDEyLzI3MTkzNzEyMzU4NjAzMi5wbmc)