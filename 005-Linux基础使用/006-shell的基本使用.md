## shell的使用

### 1.命令操作相关

|     命令     |        解析         |
| :----------: | :-----------------: |
|    ctrl+p    |     上一条命令      |
|    ctrl+n    |     下一条命令      |
|    ctrl+a    |  调到当前命令行首   |
|    ctrl+e    |  调到当前命令行尾   |
|    ctrl+b    |    向光标前移动     |
|    ctrl+f    |    向光标后移动     |
|    ctrl+h    |      backspace      |
|    ctrl+d    |   删除光标后字符    |
|    ctrl+u    |   删除光标前字符    |
|   history    |    查看历史命令     |
| clear/ctrl+l |        清屏         |
|     tab      |     补齐命令行      |
| echo $shell  | 查看当前使用的shell |

### 2.八种文件类型

| 类型 |     解析     |
| :--: | :----------: |
|  -   |   普通文件   |
|  d   |   目录文件   |
|  c   | 字符设备文件 |
|  b   |  块设备文件  |
|  l   |    软连接    |
|  p   |   管道文件   |
|  s   |  套接字文件  |
|      |   未知文件   |

### 3.常见目录说明

|  目录  |           解析           |
| :----: | :----------------------: |
|  /bin  |      存放可执行文件      |
| /boot  |       存放启动文件       |
|  /dev  |       存放设备文件       |
|  /etc  |       存放配置文件       |
| /home  |       存放用户目录       |
|  /lib  |        存放库文件        |
|  /mnt  |                          |
| /media |       存放挂载文件       |
|  /opt  |       存放操作文件       |
| /proc  |     存放进程相关文件     |
| /root  |       root用户目录       |
|  /usr  | 存放用户安装的第三方软件 |

### 4.常见文件、目录操作

|  操作  |               解析               |
| :----: | :------------------------------: |
| mkdir  |             创建目录             |
| rmdir  |            删除空目录            |
| rm -rf | 删除目录，并递归删除目录里的文件 |
|   mv   |               改名               |
|   cd   |             跳转目录             |
|   .    |             当前目录             |
|   ..   |            上一级目录            |
|   ~    |              家目录              |
|  cd -  |           返回上一目录           |
|   ls   |   list ，查看当前目录下的文件    |
| touch  |             创建文件             |
| which  |           查看命令路径           |
|  pwd   |           查看当前路径           |
|   cp   |   复制，`-r`递归，`-a`所有属性   |
|   mv   |      移动文件或目录（剪切）      |
|   ln   |            创建硬连接            |
| ln -s  |            创建软连接            |
|   du   |           查看目录大小           |
|   df   |         查看磁盘使用情况         |

### 5.文件查看命令

| 命令 |             解析             |
| :--: | :--------------------------: |
| cat  |           显示文件           |
| tac  |         倒着显示文件         |
| more | 分页查看文件，不可以向上查看 |
| less |  分页查看文件，可以向上查看  |
| head |        显示文件头10行        |
| tail |        显示文件后10行        |

### 6.文件相关操作

```shell
whoami      //查看当前用户
chmod   u (user)  [+ - =]  [mod]  文件名 //=代表赋予给定权限，并取消其它所有权限
        g (group)
        o (other)
        a (all)
//示例
chmod u + r file.cpp  //为file.cpp文件的所有者增加read权限

sudo chown newOwner a.c  //改变文件a.c所属者为newOwner
sudo chgrp newGroup a.c  //改变文件a.c所属组为newGroup
sudo chown nobody:nogroup a.c  //改变文件a.c所属者为nobody，所属组为nogourp
```

- 数字设定法

  | 数字 | 解析 |
  | :--: | :--: |
  |  4   |  r   |
  |  2   |  w   |
  |  1   |  x   |
  |  -   |  0   |

### 7.用户级别操作

```shell
sudo adduser userName  //增加一个名为userName的新用户
sudo deluser oldUser   //删除名为oldUser的用户
su userName            //切换成名为userName的用户
sudo addgroup newGroup //增加名为newGroup的用户组
sudo delgoup oldGroup  //删除名为oldGroup的用户组
sudo passwd userName   //更改用户userName的密码
```

### 8.shell中的查找

- **find查找（按文件属性查找）**

```shell
find ./ -maxdepth 1 -name '*.mp3'   //在当前目录中，搜索一层，按名字搜索找到所有'.mp3'文件
                    -type
                    -size  +20M  -size  -50M  //按文件大小，大于20M，小于50M
                    -atime 2 //最后一次读的前48小时到前24小时
                    -mtime 2 //最后一次修改文件内容
                    -ctime 2 //最后一次改变文件属性或内容
                           +2 //48小时之后
                           -2 //48小时到现在
find /usr/ -name "*.temp" -exec -ls -l {} \; //用exec工具对前面搜索到的内容，进行长列表操作
                                             //exec不分片
find /usr/ -name "*.temp" | xargs -ls -l  //将前面搜索到的内容，通过管道传给xargs操作
                          		       //xargs性能比exec好，分片处理，默认以空格作为分隔依据
find /usr -maxdepth 1 -name "*.temp" print0 | xargs -print0 ls -l 
										//print0代表以0作为分隔符号
```

- **grep查找（按文件内容查找）**

```shell
grep -r "copy" ./ -n  //递归搜索当前目录下文件中的“copy”，并显示行号
```

### 9.ps进程指令

|  参数  |                  解析                  |
| :----: | :------------------------------------: |
|   -e   |              显示所有进程              |
|   -f   |                 全格式                 |
|   -h   |               不显示标题               |
|   -l   |                 长格式                 |
|   -w   |                 宽输出                 |
|   -r   |          只显示正在运行的进程          |
|   -a   | all 显示所有用户的所有进程（当前系统） |
|   -u   |     查看进程所有者和其它的详细信息     |
|   -x   |         显示没有控制终端的进程         |
| ctrl+c |                退出进程                |

- **与管道搭配使用**

```shell
ps -aux | grep "kernal"  //如果只搜索到一条，则代表没有
```

### 10.软件的安装

```shell
sudo vim /etc/apt/sources.list  //更新源服务器列表
sudo apt-get updata             //更新表单
sudo apt-get upgrade			//更新全部软件
sudo apt-get install packege 	//安装软件
sudo apt-get remove sfwName		//卸载软件
sudo aptitude show tree			//查看tree的安装状态
								//aptitude等价于apt-get,show是aptitude独有命令
```

- **Ubuntu版本号说明**

```shell
ubuntu 14.04  //14代表年份，04代表稳定版,10代表测试版
```

### 11.解压缩

- **tar解压缩**

```shell
tar -zcvf 包名 材料 	//压缩
tar -jcvf 包名 材料 	//压缩
```

> -z：gzip，真正压缩的命令，只能压缩一个文件，只能压缩文件
>
> -j：bzip2
>
> -c：creat
>
> -v：显示压缩过程
>
> -f：file

```shell
tar -zxvf 包名 材料 	//解压
tar -jxvf 包名 材料 	//解压
```

> x：解压

- **rar解压缩**

```shell
rar a -r newdir dir  //压缩
unrar -x dir		 //解压
```

- **zip解压缩**

```shell
zip -r dir.zip dir	//压缩
unzip dir.zip		//解压
```

### 12.shell其它命令

| 命令 |           解析           |
| :--: | :----------------------: |
| tree | 以树结构查看当前目录结构 |
|  wc  |         统计命令         |
|  od  |         指定格式         |
|      |                          |
|      |                          |
|      |                          |
|      |                          |
|      |                          |
|      |                          |
|      |                          |

