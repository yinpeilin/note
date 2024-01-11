# linux内核

## 中断工作流程

​		做CPU工作模式的转化

​		进行寄存器的拷贝和压栈

​		设置中断异常向量表

​		保存正常运行的函数返回值

​		跳转到对应的中断服务函数上并运行

​		进行模式的复原以及寄存器的复原

​		跳转回正常代码执行

1. 将所有的寄存器入栈：

​		（8086中）SS EFLAGS ESP CS EIP（错误码） ARM中的r0到r15

2. 将异常码入栈（中断号）
3. 将当前的函数返回值进行入栈（为了中断执行后能够复原）
4. 调用对应的中断服务函数
5. 出栈函数返回值
6. 返回所有入栈的寄存器值

set_trap_gate 设置权限较高， 只能由用户程序调用

set_system_gate 设置权限较低， 用户系统都可以调用

idt, gdt, 

system_call



进程管理：

10ms 系统滴答： 定时器中断， 首先进行jiffies的自加， timer_interrupt, call do_timer

```c
if(cpl)
    current->utimer ++;// utime用户程序运行时间
else
    current->stimer ++;// stime内核程序运行时间
```

next_timer 是嫁接与jiffies变量的所有定时器的事件链表

current->counter -> 进程时间片 

counter – 在哪里用 进程的调度就是task_struct[]进程链表的检索， 找时间片最大的那个进程对象（task_struct），然后进行调用， 直到时间片为0，推出之后进行新一轮的分配调用。

counter– 在哪里被设置 当全部的task_struct[] 所有的江进程counter 都为0， 就进行新一轮的时间片分配。

优先级分配

![image-20240108211342486](http://image.coder-no-bug.com/202401082113520.png)

LDT 局部描述符

TSS进程描述符

![image-20240108213027987](http://image.coder-no-bug.com/202401082130015.png)

内核态不可抢占， 用户态可以抢占。

内核初始化过程中，手动创建0号进程，所有进程父进程

“/dev/tty0” 标准输入控制台

在零号进程中：

 1. 打开标准输入 输出 错误的控制台句柄

    创建1号进程，如果创建成功， 则在一号进程中

    ​	首先打开了“/etc/rc”文件

    ​	执行SHELL程序“/bin/sh”

    0号进程不可能结束， 它会在没有其他进程调用的时候调用， 只会执行for（；；）pause；

进程创建：

​	fork：

![image-20240108214943023](http://image.coder-no-bug.com/202401082149051.png)

1. 在task链表中找一个进程空位存放当前进程
2. 创建一下task_struct
3. 设置task_struct

进程的创建就是对0号进程或者当前进程的复制

把task[0]对应的task_struct复制给新创建的task_struct

对于栈堆的拷贝 

1. 给当前要创建的进程分配一个进程号：find_empty_process
2. 进程的创建主体：创建一个子进程

![image-20240108215516226](http://image.coder-no-bug.com/202401082155252.png)

3. 将当前的子进程放入到整体进程链表中
4. 设置创建的进程的task_struct

![image-20240108220259954](http://image.coder-no-bug.com/202401082202989.png)



## 进程调度

schedule

 

进程状态： 运行状态（就绪状态，进程切换只有在运行状态）， 可中断睡眠状态（可以被信号中断， 使其变成running）， 不可中断睡眠状态（只能被wakeup唤醒）， 暂停状态（收到 SIGSTOP SIGTSTP SIGTTIN 信号暂停）， 僵死状态（进程停止运行了， 但是父进程还没有将其清空，`waitpid`）

switch_to

把进程切换到当前执行进程

 1. 将需要切换的进程赋值给当前进程指针

 2. 进行进程的上下文切换

    ## 进程的销毁

exit是销毁函数， 一个系统调用 do-exit

​	该函数会释放进程的代码段和数据段占用

关闭文件，对当前目录和i节点进行同步

如果当前销毁的进程有子进程， 就让1号进程作为新的父进程

如果是会话头进程，则会终止会话中的所有进程

改变当前进程的运行状态，变成task_zombie 假死状态，并且向父进程发送sigchld信号。

父进程在运行子进程的时候，一般都会运行wait waitpid这两个函数（父进程等待某个子进程终止）

父进程会把子进程的运行时间累积到自己的变量中，把对应的子进程的进程描述结构体进行释放， 置空任务数组中的空槽。

终止会话， 终止当前进程的会话，给其发送signal-up

kill -不是杀死的意思， 向对应的进程号或者进程组号发送任何信号。

pid > 0 给对应的pid发送sig

pid=0 给当前进程的进程组发送sig

pid=-1 给任何进程发送命令

pid<-1 给进程组好为-pid的进程组发送信号

bootsect.s

​	磁盘引导块程序，在磁盘的第一个扇区中的程序（0磁道 0磁头 1扇区）

​	作用：首先将后续的setup.s代码从磁盘中加载到紧接着bootsect这个地方。在显示屏上显示loading system 再将system模块加载到0x10000的地方。最后跳转到setup.s中运行。

setup.s 

​	解析BIOS/BOOTLOADER船体来的参数

​	设置系统内核运行的LDT（局部描述符） IDT（中断描述符寄存器） 全局描述符（设置全局描述符寄存器）

​	设置中断控制芯片， 进入保护模式运行

​	跳转到system模块的最前面的代码运行（head.s）

head.s

​	加载内核运行时的各数据段寄存器，重新设置中断描述符表，跳转到mian.c开始运行

![image-20240109104843101](http://image.coder-no-bug.com/202401091048158.png)

![image-20240109110556039](http://image.coder-no-bug.com/202401091105107.png)