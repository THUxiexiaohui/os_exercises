#lec 3 SPOC Discussion

## 第三讲 启动、中断、异常和系统调用-思考题

## 3.1 BIOS
 1. 比较UEFI和BIOS的区别。
 1. 描述PXE的大致启动流程。

## 3.2 系统启动流程
 1. 了解NTLDR的启动流程。
 1. 了解GRUB的启动流程。
 1. 比较NTLDR和GRUB的功能有差异。
 1. 了解u-boot的功能。

## 3.3 中断、异常和系统调用比较
 1. 举例说明Linux中有哪些中断，哪些异常？
 1. Linux的系统调用有哪些？大致的功能分类有哪些？  (w2l1)


>  Linux的系统调用
>  * Linux的系统调用有fork(创建一个新进程)、clone(按指定条件创建子进程)、exit(中止进程)、fcntl(文件控制)等上百个
>  * Linux的系统调用的大致功能分类有：进程控制、文件系统控制、系统控制、内存管理、网络管理、socket控制、用户管理、进程间通讯



```
  + 采分点：说明了Linux的大致数量（上百个），说明了Linux系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
 1. 以ucore lab8的answer为例，uCore的系统调用有哪些？大致的功能分类有哪些？(w2l1)
 
>  * uCore的系统调用:

```
[SYS_exit]              sys_exit,
[SYS_fork]              sys_fork,
[SYS_wait]              sys_wait,
[SYS_exec]              sys_exec,
[SYS_yield]             sys_yield,
[SYS_kill]              sys_kill,
[SYS_getpid]            sys_getpid,
[SYS_putc]              sys_putc,
[SYS_pgdir]             sys_pgdir,
[SYS_gettime]           sys_gettime,
[SYS_lab6_set_priority] sys_lab6_set_priority,
[SYS_sleep]             sys_sleep,
[SYS_open]              sys_open,
[SYS_close]             sys_close,
[SYS_read]              sys_read,
[SYS_write]             sys_write,
[SYS_seek]              sys_seek,
[SYS_fstat]             sys_fstat,
[SYS_fsync]             sys_fsync,
[SYS_getcwd]            sys_getcwd,
[SYS_getdirentry]       sys_getdirentry,
[SYS_dup]               sys_dup,
 ```
>  *大致的功能分类有：进程控制、进程间通信、文件系统控制、系统控制、文件系统控制

  + 采分点：说明了ucore的大致数量（二十几个），说明了ucore系统调用的主要分类（文件操作，进程管理，内存管理等）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.4 linux系统调用分析
 1. 通过分析[lab1_ex0](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex0.md)了解Linux应用的系统调用编写和含义。(w2l1)
 
>  * 先将系统调用号SYS_write、标准输出STDOUT、字符串常量hello、字符串长度放入指定寄存器，然后使用"int 0x80"调用系统调用

 ```
  + 采分点：说明了objdump，nm，file的大致用途，说明了系统调用的具体含义
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 
 ```
 
 1. 通过调试[lab1_ex1](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab1/lab1-ex1.md)了解Linux应用的系统调用执行过程。(w2l1)
 

> strace用途：显示程序执行过程中每个系统调用的 名称、时间、占时间百分比、调用次数、错误次数 等
>
> 系统调用具体执行过程：
> * 应用准备相应参数
> * 应用调用系统调用接口
> * 操作系统内核对系统调用进行识别和运行
> * 操作系统内核根据不同情况使用不同硬件进行操作
 

 ```
  + 采分点：说明了strace的大致用途，说明了系统调用的具体执行过程（包括应用，CPU硬件，操作系统的执行过程）
  - 答案没有涉及上述两个要点；（0分）
  - 答案对上述两个要点中的某一个要点进行了正确阐述（1分）
  - 答案对上述两个要点进行了正确阐述（2分）
  - 答案除了对上述两个要点都进行了正确阐述外，还进行了扩展和更丰富的说明（3分）
 ```
 
## 3.5 ucore系统调用分析
 1. ucore的系统调用中参数传递代码分析。
 1. ucore的系统调用中返回结果的传递代码分析。
 1. 以ucore lab8的answer为例，分析ucore 应用的系统调用编写和含义。
 1. 以ucore lab8的answer为例，尝试修改并运行ucore OS kernel代码，使其具有类似Linux应用工具`strace`的功能，即能够显示出应用程序发出的系统调用，从而可以分析ucore应用的系统调用执行过程。
 
## 3.6 请分析函数调用和系统调用的区别
 1. 请从代码编写和执行过程来说明。
   1. 说明`int`、`iret`、`call`和`ret`的指令准确功能
 
