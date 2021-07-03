### 信号集

 信号集顾名思义就是信号的集合，信号集的类型为**sigset_t**，每一种信号用1bit来表示，前面我们提到信号有64种，那么这个sigset_t类型至少占64bit,可以通过sizeof(sigset_t)来查看。

 每个进程的PCB进程控制块中都有两个信号集，一个叫作未决信号集，一个叫作信号屏蔽字，信号集的每一位不是0就是1，初始状态下，两个信号集的值都为0。

 当有信号传递到该进程的时候，未决信号集的对应位设置为1，其他位不变，这个时候信号只是传递到进程，并未被处理，叫作未决状态。常规信号在递达之前产生多次只计一次，而实时信号在递达之前产生多次可以依次放在一个队列里。用户只能获取未决信号集值，无法改变其值。

 未决信号想要递达程序的信号处理函数(默认、忽略、自定义)，还要经过信号屏蔽字的过滤，一旦该信号对应bit为1，则该信号将阻塞，不能传递到信号的处理函数。用户可以设置获取信号屏蔽字的值。

#### 信号集处理函数

```c++
#include <signal.h>
int sigemptyset(sigset_t *set) //将set每一位都置0
int sigfillset(sigset_t *set) //将set每一位都置1
int sigaddset(sigset_t *set, int signo) //将set中signo信号对应的bit置1
int sigdelset(sigset_t *set, int signo) //将set中signo信号对应的bit置0
int sigismember(const sigset_t *set, int signo) //判断set中signo信号对应bit是否为1,返回1或者0。
```



#### sigprocmask

调用函数sigprocmask可以读取或更改进程的信号屏蔽字。

```c++
#include <signal.h>
int sigprocmask(int how, const sigset_t *set, sigset_t *oset);
/*
how参数：
    SIG_BLOCK set包含了我们希望添加到当前信号屏蔽字的信号，相当于mask=mask|set
    SIG_UNBLOCK set包含了我们希望从当前信号屏蔽字中解除阻塞的信号，相当于mask=mask&~set
    SIG_SETMASK 设置当前信号屏蔽字为set所指向的值，相当于mask=set
set参数:传入参数
oset参数:传出参数，获取原先的信号屏蔽字，一般可以填0
返回值：若成功则为0，若出错则为-1
*/
```

 如果调用sigprocmask解除了对当前若干个未决信号的阻塞，则在sigprocmask返回前，至少将其中一个信号递达。



#### sigpending

 读取当前进程的未决信号集

```c++
#include <signal.h>
int sigpending(sigset_t *set);
/*
通过set参数传出。调用成功则返回0，出错则返回-1。
*/
```

#### 综合示例

```c++
#include <signal.h>
#include <stdio.h>
void printsigset(const sigset_t *set)
{
    int i;
    for (i = 1; i < 32; i++)
        if (sigismember(set, i) == 1)
            putchar('1');
        else
            putchar('0');
    puts("");
}
int main(void)
{
    sigset_t s, p;
    sigemptyset(&s);
    sigaddset(&s, SIGINT);  //将信号集s中的SIGINT置为1
  
    sigprocmask(SIG_BLOCK, &s, NULL); //设置信号屏蔽字
    while (1) 
    {
        sigpending(&p);  //获取未决信号集
        printsigset(&p); //打印未决信号集
        sleep(1);
    }
    return 0;
}
```

