- GDT可以存放代码段，数据段和系统段三种类型
- IDT中全部是系统段，包含了3种门描述符
	- 任务门描述符
	- 中断门描述符
	- 陷阱门描述符

# #中断门
- int/iret #IRET 
	- 中断门提权，堆栈会保存 SS ESP EFLAGS CS EIP(返回地址) 
	- 中断门不提权，堆栈会保存  CS EFLAGS EIP(返回地址) 
- 会将**IF标志位**清零。**比如不接受键盘输入**
	- IF=0时，程序不再接收可屏蔽中断
- 
- 可屏蔽中断：比如 通过键盘敲击了锁屏的快捷键
	- 若IF位为1，CPU就能够接收到键盘指令并锁屏
- 不可屏蔽中断：断电时，电源会向CPU发出一个请求
	- 此时不管IF位是否为0，CPU都要去处理这个请求
	- 实际上主板上电容可以让CPU在断电后在运行一段时间去做一些清理工作

Windows使用中断门
- 系统调用
	- Windows没有使用调用门，但是使用了中断门。	
	- Windows很多3环API最终都要使用0环的代码，是使用中断门实现的这种功能。
	- 注意:老的CPU是用的中断门而最新的CPU是使用的快速调用。
- 异常中断
	- 在像OD这种调试工具中按F12下的断点就是将BYTE内容变为0xCC指令位int3这是中断门效果

#中断门 和 #调用门 的区别
- 中断门和调用门很相似他们最大的区别是查表的不同，
- 调用门是使用的GDT而中断门使用的是IDT。

IDT
- IDT占8个字节
- D == 1，表示32位的中断门  
- D == 0，表示16位的中断门
![](../../photo/Pasted%20image%2020221208145708.png)

IDT
- IDT中的第一个元素不为null
- INT 0到INT 31都是已经被使用了
- INT中断向量号一共有256个，32-255向量号都可以被用户使用
- 256x8-1=2047=0x7ff
![](../../photo/Pasted%20image%2020221208150434.png)

![](../../photo/Pasted%20image%2020221208161112.png)
![](../../photo/Pasted%20image%2020221208163140.png)
## 中断门的Call调用流程
![](../../photo/Pasted%20image%2020221208171318.png)
观看上图可以得知
1. 首先寻址到中断门 ,然后从中断门中分别取出记录的 offset(函数偏移) + 代码段段选择子
2. 然后根据代码段段选择子. 去GDT或者LDT中查询 代码段描述符
3. 从代码段描述符中取出记录的 Base(基地址)
4. 取出的基地址与中断门中记录额函数偏移(offset)相加得到一个真正的函数地址.
5. 进行调用.

# #陷阱门
- 和 #中断门 很像
- 中断门执行时会将IF标志位清零，但陷阱门**不会**
- **IF位是否会被清零**是陷阱门与中断门**唯一**的区别

![](../../photo/Pasted%20image%2020221208163252.png)
# #任务门
## 任务门段描述符
- **存在于IDT中**
![](../../photo/Pasted%20image%2020221208185721.png)
任务门执行过程
1. INT 0x20
2. 查IDT，找到中断门描述符（此处为任务门）
3. 查看任务门中的TSS段选择子，查GDT表，找到任务段描述符
4. 将任务段描述符加载到TR寄存器，之后通过TR寄存器指向的104字节的TSS段，将全部寄存器的值进行修改。
5. IRETD返回

## TSS段描述符
- 存在于GDT全局描述符表中
- G=0, Limit字节为单位,最大范围0xFFFFF/1M
- S=0, TYPE=10B1 是任务段描述符 
![](../../photo/Pasted%20image%2020221208182521.png)

## TSS任务段 
- TSS是一块内存，该数据结构大小为**104字节**，共**26个DWORD属性**。
- 通过TSS可以同时替换一堆寄存器，包括通用寄存器和段寄存器。
- TSS结构是一个**链表**，**开头4字节指向上一个TSS段选择子**
```
typedef struct TSS {
	DWORD link;	 //指向上一个TSS段选择子	
	DWORD esp0; 
	DWORD ss0;  
	DWORD esp1; 
	DWORD ss1;  
	DWORD esp2;
	DWORD ss2; 		
	DWORD cr3;
	DWORD eip;
	DWORD eflags;
	DWORD eax;
	DWORD ecx;
	DWORD edx;
	DWORD ebx;
	DWORD esp;
	DWORD ebp;
	DWORD esi;
	DWORD edi;
	DWORD es;
	DWORD cs;
	DWORD ss;
	DWORD ds;
	DWORD fs;
	DWORD gs;
	DWORD ldt;//LDT局部描述符表，windows没有使用
	DWORD io_map;//IO权限位图
} TSS;
```

