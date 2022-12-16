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

![](../../photo/Pasted%20image%2020221208210833.png)

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
## 流程
- 任务门段描述符【**也可以不使用任务门**】
	- 查IDT，找到任务门
	- 分析任务门，找到TSS段选择子
	- 查GDT，找到任务段描述符
	- 将任务段描述符加载到TR寄存器
	- **通过 int xx 调用**
- TR寄存器（96位）【**真正开始的位置**】
	- 段选择子可见
	- 通过段选择子，查找gdt，找到tss段描述符
	- **ring0, 通过 LTR指令 调用**
	- **ring3, 通过 jmp far 和 call far 调用**
- tss段描述符
	- 段描述符.base = TSS首地址
	- 段描述符.limit = TSS这块内存的大小，**104字节**
- tss任务段
	- 任务切换【替换一套寄存器】
	- 使用TSS段中的值修改寄存器

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
	DWORD link;	 //指向上一个TSS段选择子，当发生切换时由CPU自动完成	
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
![](../../photo/Pasted%20image%2020221208204104.png)

## 一个例子
```
// 特别注意，看注释
/* 任务门例子
1. tss结构体
2. TSS任务段描述符。需要tss的地址和长度104直接
3. 任务门描述符，需要tss任务段描述符
4. int xx //调用任务门
*/

#include "stdafx.h"
#include <Windows.h>
#include <stdio.h>

DWORD *TSS;
DWORD dwOk;

// 任务切换后的EIP
void __declspec(naked) R0Func()
{
	dwOk = 1;
	__asm
	{
		iretd
	}
}

int _tmain(int argc, _TCHAR* argv[])
{	
	DWORD dwCr3; // windbg获取
	char esp[0x1000]; // 任务切换后的栈，数组名就是ESP
	
	// 此数组的地址就是TSS描述符中的Base
	TSS = (DWORD*)VirtualAlloc(NULL,104,MEM_COMMIT,PAGE_READWRITE);
	if (TSS == NULL)
	{
		printf("VirtualAlloc 失败，%d\n", GetLastError());
		getchar();
		return -1;
	}
	// GDT：TSS描述符
	printf("请在windbg执行: eq 8003f048 %02x00e9%02x`%04x0068\n", ((DWORD)TSS>>24) & 0x000000FF,((DWORD)TSS>>16) & 0x000000FF, (WORD)TSS);
	// IDT：任务门描述符
	printf("请在windbg执行: eq 8003f500 0000e500`00480000\n");
	printf("请在windbg中执行!process 0 0，复制TSS.exe进程DirBase的值，并输入.\nCR3: "); // 在windbg中执行 !process 0 0 获取，DirBase: 13600420  这个数要启动程序后现查
	scanf("%x", &dwCr3); // 注意是%x
	
	TSS[0] = 0x00000000; // Previous Task Link CPU填充，表示上一个任务的选择子
	TSS[1] = 0x00000000; // ESP0
	TSS[2] = 0x00000000; // SS0
	TSS[3] = 0x00000000; // ESP1
	TSS[4] = 0x00000000; // SS1
	TSS[5] = 0x00000000; // ESP2
	TSS[6] = 0x00000000; // SS2
	TSS[7] = dwCr3; // CR3 学到页就知道是啥了
	TSS[8] = (DWORD)R0Func; // EIP
	TSS[9] = 0x00000000; // EFLAGS
	TSS[10] = 0x00000000; // EAX
	TSS[11] = 0x00000000; // ECX
	TSS[12] = 0x00000000; // EDX
	TSS[13] = 0x00000000; // EBX
	TSS[14] = (DWORD)esp+0x500; // ESP，解释：esp是一个0x1000的字节数组，作为裸函数的栈，这里传进去的应该是高地址，压栈才不会越界
	TSS[15] = 0x00000000; // EBP
	TSS[16] = 0x00000000; // ESI
	TSS[17] = 0x00000000; // EDI
	TSS[18] = 0x00000023; // ES
	TSS[19] = 0x00000008; // CS 0x0000001B
	TSS[20] = 0x00000010; // SS 0x00000023
	TSS[21] = 0x00000023; // DS
	TSS[22] = 0x00000030; // FS 0x0000003B
	TSS[23] = 0x00000000; // GS
	TSS[24] = 0x00000000; // LDT Segment Selector
	TSS[25] = 0x20ac0000; // I/O Map Base Address

	char buff[6] = {0,0,0,0,0x48,0};	
	__asm
	{
		//call fword ptr[buff]
		//jmp fword ptr[buff]
		int 0x20
	}
	printf("ok: %d\n",dwOk);

	return 0;
}

```