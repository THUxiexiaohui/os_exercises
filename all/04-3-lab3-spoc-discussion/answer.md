## Answer
1.	试图用一个超过段界限的偏移量访问段
	> * 触发：修改entry.S第44行SEG_ASM的第三个参数为0，这样一旦访问就肯定超段界限
	> * 效果：死机

2.	将一个不可执行的段装入CS寄存器
	> * 触发：在init.c中第44行后面加入
	```
	asm ("ljmp $7, $0x20");
	```
	> * 效果：Page Fault


3.	写只读段
	> * 触发：将pmm.c中的get_pte中的地址去掉PTE_W位
	> × 效果：不停reboot

4.	低优先级代码访问高优先级的段
	> * 触发：将pmm.c中的get_pte中的地址去掉PTE_U位
	> × 效果：……

5.	没有设置GDT/段不存在
	> * 触发：在pmm.c的gdt_init函数中，注释第132行
	> * 效果：不停reboot

6.	没有合法的TSS
	> * 触发：在pmm.c的gdt_init函数中，注释第129行
	> * 效果：不停reboot

7.	没有设置IDT
	> * 触发：在init.c中注释第36行
	> * 效果：不停reboot

8.	访问的是非法指令
	> * 在init.c中加入
	```
	asm ("jmp 0xffffffff");
	```
	> * 效果：不停reboot

9.	页目录项映射不存在
	> * 触发：在pmm.c的get_pte函数中返回NULL
	> * 效果：check错误

10.	页表项映射不存在
	> * 触发：在pmm.c的get_page函数中返回NULL
	> * 效果：不停reboot

11. 访问非法地址
	> * 触发：在vmm.c的check_pgfault函数中注释全部，加入
	```
    uint32_t addr = 0;
    *(uint32_t *)addr = 0;
    ```
    使访问一个非法地址
    > * 效果：显示Page Fault
