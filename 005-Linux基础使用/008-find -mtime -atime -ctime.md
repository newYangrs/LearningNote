# **find -mtime -atime -ctime**

考虑一个场景，如果想要定期删除服务产生的日志，比如自动打包3天前的日志，启动删除7天前的日志等。可以使用crontab+find的组合命令。
find命令就需要判定文件的时间，有三种-atime，-ctime和-mtime。

先看下三者的区别：
-atime：最后一次读的时间戳；
-mtime：最后一次修改内容的时间戳；
-ctime：最后一次修改文件属性的时间戳，比如文件的owner和group；当然修改内容也会修改ctime。

那么开头场景自然是使用-mtime较为合适了，下面就用-mtime来看看具体的用法

-mtime后面跟一个整数，表示几天前修改过。也有三种：
-mtime n
-mtime -n
-mtime +n

先看-mtime n。假设指定命令的这一瞬间的时间戳是t，文件的mtime时间戳是m，那么如果（t-m）/（24*3600*1000）== 1成立，就会被find命令选中。
相应的，-n就是小于n，+n就是大于n。
这是最最原始的理解方法。
如果想要通俗一点呢？
n就是距离现在相隔了n天整。具体点就是：

n就是距离现在（也就是t时刻）n天前再往前推24小时。---只有24小时
-n就是距离现在（也就是t时刻）n天前到现在的时间段。---可能不止24小时
+n就是距离现在（也就是t时刻）n天前再往前推24小时之前。---可能不止24小时

表述能力有点捉急了。。。

举个例子。
t=20181012-14:30:00

那么
-mtime 2:表示20181009-14:29:59 - 20181010-14:30:00
-mtime +2:表示20181009-14:29:59之前
-mtime -2:表示20181010-14:30:00到现在

 

# **参考：**


https://www.unix.com/tips-and-tutorials/20526-mtime-ctime-atime.html
find command -mtime -ctime -atime
The find command uses arguments like:
-mtime -2
-mtime +2
-mtime 2

There are -ctime and -atime options as well. Since we now understand the differences among mtime, ctime, and atime, by understanding how find uses the -mtime option, the other two become understood as well. So I will describe find's use of the -mtime option.

As you probably know, the find command can run for minutes or hours depending on the size of the filesystem being searched. The find command makes a note of its own start time. It then looks at a file's mtime and computes how many seconds ago the file was modified. By dividing the seconds by 86,400 (and discarding any remainder), it can calculate the file's age in days:
Code:
0 days in seconds:    0  -  86399
1 day in seconds:   86400  -  172799
2 days in seconds:  172800  -  259159

So now that we know how many days ago a file was modified, we can use stuff like "-mtime 2" which specifies files that are 172800 to 259159 seconds older than the instant that the find command was started. 

"-mtime -2" means files that are less than 2 days old, such as a file that is 0 or 1 days old.

"-mtime +2" means files that are more than 2 days old... {3, 4, 5, ...}

It may seem odd, but +0 is supposed to work and would mean files more than 0 days old. It is very important to recognize that find's concept of a "day" has nothing to do with midnight.

from:https://blog.csdn.net/u010900754/article/details/83027831?ops_request_misc=%25257B%252522request%25255Fid%252522%25253A%252522161337090016780266285973%252522%25252C%252522scm%252522%25253A%25252220140713.130102334.pc%25255Fall.%252522%25257D&request_id=161337090016780266285973&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_v2~rank_v29-1-83027831.pc_search_result_no_baidu_js&utm_term=find+-%25E9%2598%25BFtime