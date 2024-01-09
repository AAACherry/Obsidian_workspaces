
## P 16 固件库是什么样子的？

![[../../annex/STM32固件库入门_image_1.png]]
内核当中也有外设，如 NVIC、systick

```
库的架构

1-汇编编写的启动文件
startup_stm32f10x_hd.s (hd:high density高密度的,密度表示flash的大小，即容量)
设置堆栈指针、设置PC指针、设置初始化中断向量表、配置系统时钟、调用C库函数_main最终去到C的世界

2-时钟配置文件
system_stm32f10x.c:把外部时钟HSE=8M,经过PLL（锁相环）倍频位72M。锁相环是专门用来倍频的。
当执行完启动文件之后，系统时钟已经配置成72M，所以在main函数的时候，我们不需要再去另外的配置这个系统时钟。

3-外设相关的
stm32f10x.h：实现了内核之外的外设寄存器映射
xxx:GPIO、USRAT、I2C、SPI、FSMC···
stm32f10x_xxx.c(stm32f10x_gpio.c):外设的驱动函数库文件
stm32f10x_xxx.h(stm32f10x_gpio.h):存放外设的初始化结构体、外设初始化结构体成员的参数列表、外设固件库函数的声明
(.h头文件)(【穷举】：表示每一种情况都有描述)


4-内核相关的
内核当中也有外设，如NVIC、systick
符合CMSIS-Cortex:微控制器软件接口标准
core_cm3.h:实现了内核里面外设的寄存器映射
core_cm3.c:内核外设的驱动固件库

NVIC:(嵌套向量中断控制器),与中断相关、SysTick(系统滴答定时器)
misc.h:
misc.c:

5-头文件的配置文件
stm32f10x_conf.h:头文件的头文件
将所有的外设相对头文件stm32f10x_xxx.h都包含在其中
如：
//stm32f10x_usart.h
//stm32f10x_i2c.h
//stm32f10x_spi.h
//stm32f10x_adc.h
//stm32f10x_fsmc.h
...

默认情况下都是被屏蔽掉的，如果想用则把//注释去掉

6-专门存放中断服务函数的c文件
stm32每个外设都有非常多的中断，如果编程的时候用了很多外设，有很多中断的时候，为了方便中断管理，把中断服务函数全部写到一个c文件中
执行程序的时候有可能出现要执行别的紧急程序的情况，就需要先中断执行完别的程序再回来接着运行.
stm32f10x_it.c
stm32f10x_it.h
中断服务函数可以随便放在其他的地方,并非一定要放在stm32f10x_it.c,只是为了方便管理(好看,可读性)才放到其中
操作起来方便，一看就知道系统/程序中写了多少个中断,方便进一步分配每一个中断的优先级。

//<>尖括号去系统安装目录中找,""双引号先在当前自己新建的工程目录下去找,找不到才去keil5软件的根目录下找
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 

int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
}



```

## P 17 固件库目录文件分析

![[../../annex/STM32固件库入门_image_2.png]]
如果使用标准固件库的时候，就包含 conf.h 文件


那么 USE_STDPERIPH_DRIVER 在哪里定义？
在编译器的宏里面定义↓
![[../../annex/STM32固件库入门_image_3.png]]
这样定义的好处就是，新建一个工程模板后以后都可以用。不需要每次都要在 c 文件中定义这个宏（USE_STDPERIPH_DRIVER）

![[../../annex/STM32固件库入门_image_4.png]]
分析不同的单片机所需的 startup 启动文件的类型
四种基本型 ld、md、hd、xl，包括 stm32F101、stm32F102、stm32F103 系列都可以使用。
互联型 cl（connectivity line devices）：有以太网的物理层 MAC，特指 105、107 系列。
超值型 vl（value line devices）：特指 100 系列。


## P 18 新建工程-固件库版本

###### 新建文件工程文件夹
以后看例程，只要看 User 这个文件夹，里面的代码都是自己写的。其他的文件夹都不是我们自己写的。
为了方便别人看我们的程序的时候，我们要做一些说明，比如说该例程是做什么的、需要实现什么功能、有什么要注意的事项...

Doc 文件夹，里面一般放一个 txt 文本，用来对这个工程、程序进行说明。


不要添加这个固定模板 main，最好自己添加，不然后面的工程 main 全是一个模板，第三个工程 main 修改后你的2的工程 main 也跟着修改

这部分就是一个类似配置环境的步骤，不需要懂啥吧，学到后来慢慢就悟了

![[../../annex/STM32固件库入门_image_5.png]]
需要手动配置为当前的模板文件，不然会从安装的根目录下去找。
![[../../annex/STM32固件库入门_image_6.png]]

![[../../annex/STM32固件库入门_image_7.png]]


###### 定义整个工程的宏
![[../../annex/STM32固件库入门_image_8.png]]

避免每个程序都定义这个宏，在 IDE 中进行定义
USE_STDPERIPH_DRIVER
![[../../annex/STM32固件库入门_image_9.png]]


![[../../annex/STM32固件库入门_image_10.png]]

![[../../annex/STM32固件库入门_image_11.png]]



![[../../annex/STM32固件库入门_image_12.png]]
四个错误的到 target 里把版本改成5就可以了

###### 修改添加的文件类型
![[../../annex/STM32固件库入门_image_13.png]]

![[../../annex/STM32固件库入门_image_14.png]]

###### 调整版本为 5
有很多错误的到 target 里把版本改成5就可以了
![[../../annex/STM32固件库入门_image_15.png]]

![[../../annex/STM32固件库入门_image_16.png]]

###### 设置编译 HEX 文件
![[../../annex/STM32固件库入门_image_17.png]]

.map 文件对于分析整个工程的时候才有用，比如说用了多少内存，函数在哪个地址、用了多少空间等
![[../../annex/STM32固件库入门_image_18.png]]


![[../../annex/STM32固件库入门_image_19.png]]


![[../../annex/STM32固件库入门_image_20.png]]
复位线不接的话选 system

###### Reset and Run
![[../../annex/STM32固件库入门_image_21.png]]
Reset and Run 不勾选的话，每次上电要手动运行。


显示一个错误的，如果是说不打开 temp 文件等，先把 keil 关了，然后用管理员身份运行，我就是这样解决掉

###### 动态的语法检查
![[../../annex/STM32固件库入门_image_22.png]]


## P 19 GPIO 输出-使用固件库点亮 LED

PMOS 管高电平导通但是有反相器所以低电平导通
Nmos 管高电平导通，Pmos 管低电平导通
ODR 输出高电平，经过反相器为低电平，PMOS 导通，NMOS 关闭，对外输出高电平。
![[../../annex/STM32固件库入门_image_23.png]]

自动补全功能在 Test Complation 中的第一个 Struct/Class Members
![[../../annex/STM32固件库入门_image_24.png]]


![[../../annex/STM32固件库入门_image_25.png]]

![[../../annex/STM32固件库入门_image_26.png]]

选引脚就是为了确定控制端口，配置这个寄存器的具体多少位。如配置选择是引脚 PB0，则配置 0~3 位
如果选择引脚 PB 1，则配置 4~7 位
![[../../annex/STM32固件库入门_image_27.png]]
CRL 配置低八位，CRH 配置高八位

选定函数，按 F12 或右键 go to definition 看函数原型

如果三个 GPIO 口，输出的是 PWM 这样的方波信号的话，RGB 灯可以混合成 256 种颜色


```bsp_led.h
#ifndef _BSP_LED_H	//条件编译，防止重复编译，尽量在每个头文件定义中都加上条件编译。
#define _BSP_LED_H	//头文件建议如果包含在.c里可以的话就不要在.h里包含，否则耦合性较高，以后不方便改动

#include "stm32f10x.h"//在stm32f10x.h文件中已经有#ifndef _STM32F10X_H 了（不能/会被两次编译）

#define LED_G_GPIO_PIN					GPIO_Pin_0	//选择pin的时候用宏来代替，下次如果换了个GPIO，只需要修改这个宏即可，不需要回到这个函数的实体里面来修改
#define LED_G_GPIO_PORT					GPIOB
#define LED_G_GPIO_CLK					RCC_APB2Periph_GPIOB

#define				ON			1
#define				OFF			0


//	\		C语言中的续行符，要求后面不能有任何的东西，只能是回车
#define				LED_G(a)			if(a)	\
																	GPIO_ResetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);	\
														else	GPIO_SetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);	//a是形参，如果a为真,清零

void LED_GPIO_Config(void);

#endif /*_BSP_LED_H*/


```

```bsp_led.c
//bsp:	board support package	板级支持包
#include "bsp_led.h"

void LED_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStruct;
	
	RCC_APB2PeriphClockCmd(LED_G_GPIO_CLK , ENABLE);
	
	GPIO_InitStruct.GPIO_Pin = LED_GPIO_Config;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_Out_PP;//使用推挽输出
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;//不配置速度就默认配置成最高值
	
	GPIO_Init(LED_G_GPIO_PORT, &GPIO_InitStruct);//GPIO_InitStruct定义的是一个变量，获取指针用取地址&
	
}


```


```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 

#include "bsp_led.h"

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	LED_GPIO_Config();//亮灯
	
	while(1)
	{
		//GPIO_SetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);//灭灯
		LED_G(OFF);
		Delay(0xFFFFF);
		//GPIO_ResetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);//重新亮灯
		LED_G(ON);
		Delay(0xFFFFF);
	}
	
}


```


## P 20 GPIO 输入-按键检测

###### 按键介绍
本节课的输入：通过一个按键来改变外部的电平状态，让 IO 口来读取这个电平的状态

![[../../annex/STM32固件库入门_image_28.png]]

K 1 按键按下，接高电平，3.3 V，电流是比较大的
![[../../annex/STM32固件库入门_image_29.png]]

所以，为了保护 GPIO ，GPIO 的最大输出电流是有限制的，加了一个限流电阻
![[../../annex/STM32固件库入门_image_30.png]]

如果按键没有按下，GPIO 通过电阻接地
![[../../annex/STM32固件库入门_image_31.png]]

GPIO 默认是低电平，按下之后是高电平，所以是上升沿输入。

因为 PA0 还具有这种自动唤醒的功能，叫 wakeup。Wakeup 是一定要上升沿才能唤醒的。
所以为了统一风格，两个按键都设置成上升沿。

![[../../annex/STM32固件库入门_image_32.png]]
还有个电容，51 单片机写按键程序的时候，要写软件消抖。因为按键是机械按键，按下可能需要 20 ms 左右的时间来稳定。

当它没有稳定时，可能按下是跳动的。这样子跳动就好像不断的接通 3.3 V，就像交流电一样
通俗的讲：机械开关在刚接触触点是不稳定，电流断断续续，消抖就是吧这种现象造成的不良反应给他消除掉
![[../../annex/STM32固件库入门_image_33.png]]
那么如果接了电容的话，就好像不断的给这个电容充放电。如果等到稳定状态之后，那么这个电容的电又可以通过电阻释放到地里面。
![[../../annex/STM32固件库入门_image_34.png]]
所以这个是硬件消抖，所以软件上就不需要软件消抖了。不需要延时来确定这个按键的电平状态。

###### 代码

读取了 GPIO 口的电平，如果它等于高电平，那么就确定按键按下了，可以做相应的动作。
比如说让我们的 LED 灭或者亮。
输出对应的是这个寄存器或者 BSRR/BRR，都可以实现 GPIO 口输出。

输入对应的是 IDR（低 16 位有效），如果读回来是 0，那么 GPIO 表示检测到的低电平（接地），如果是 1，表示检测到的是高电平（3.3 V）。

这个按键采用查询的方法，不使用中断（后面有中断检测函数）。

弹幕：记住：不管什么外设初始化都要开时钟

硬件本身外部下拉了，所以不用通用推挽输出，且我们所需的是输入，所以我们配置成浮空模式，用 GPIO_Mode_IN_FLOATING

弹幕：这里有人说为什么是浮空输入，是因为这个 STM32的手册中就说明了这个 PA0就是浮空输入的，这个是可以在手册中找到出处的

弹幕：要粗略知道，外部电压流入 IDR 地址，IDR 是 CPU 片外 RAM，通过总线扩展，传入 CPU 所谓缓存，进行运算处理

```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 

#include "bsp_led.h"
#include "bsp_key.h"

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	LED_GPIO_Config();//亮灯
  KEY_GPIO_Config();
	
	while(1)
	{
		if(Kye_Scan(KEY1_GPIO_PORT,KEY1_GPIO_PIN) == KEY_ON )
			LED_G_TOGGLE;
	}
	
}



```


```bsp_led.c
//bsp:	board support package	板级支持包
#include "bsp_led.h"

void LED_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStruct;
	
	RCC_APB2PeriphClockCmd(LED_G_GPIO_CLK , ENABLE);
	
	GPIO_InitStruct.GPIO_Pin = LED_GPIO_Config;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_Out_PP;//使用推挽输出
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;//不配置速度就默认配置成最高值
	
	GPIO_Init(LED_G_GPIO_PORT, &GPIO_InitStruct);//GPIO_InitStruct定义的是一个变量，获取指针用取地址&
	
}


```

```bsp_led.h
#ifndef _BSP_LED_H	//条件编译，防止重复编译，尽量在每个头文件定义中都加上条件编译。
#define _BSP_LED_H	//头文件建议如果包含在.c里可以的话就不要在.h里包含，否则耦合性较高，以后不方便改动

#include "stm32f10x.h"//在stm32f10x.h文件中已经有#ifndef _STM32F10X_H 了（不能/会被两次编译）

#define LED_G_GPIO_PIN					GPIO_Pin_0	//选择pin的时候用宏来代替，下次如果换了个GPIO，只需要修改这个宏即可，不需要回到这个函数的实体里面来修改
#define LED_G_GPIO_PORT					GPIOB
#define LED_G_GPIO_CLK					RCC_APB2Periph_GPIOB

#define				ON			1
#define				OFF			0


//	\		C语言中的续行符，要求后面不能有任何的东西，只能是回车
#define				LED_G(a)			if(a)	\
																	GPIO_ResetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);	\
														else	GPIO_SetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);	//a是形参，如果a为真,清零

//	^	异或，C语言的一个二进制的运算符
//	与1异或要改变，与0异或不变。--异或：相异为1，相同为0

#define LED_G_TOGGLE		{LED_G_GPIO_PORT->ODR ^= LED_G_GPIO_PIN;}	

//注意异或符合是上尖角，一开始搞成波浪号了，修改了一天才发现
//总之这个toggle的作用就是 通过异或，使得 GPIOB ODR的 第0位不断翻转(0变1,1变0)，达到使绿灯亮灭的效果

void LED_GPIO_Config(void);

#endif /*_BSP_LED_H*/


```


```bsp_key.c
#include "bsp_key.h"

void KEY_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStruct;	//同样，要先定义一个初始化的结构体
	
	RCC_APB2PeriphClockCmd(KEY1_GPIO_CLK , ENABLE);//记住：不管什么外设初始化都要开时钟
	
	GPIO_InitStruct.GPIO_Pin = KEY1_GPIO_PIN;
	//硬件本身外部下拉了，所以不用通用推挽输出，且我们所需的是输入，所以我们配置成浮空模式，用 GPIO_Mode_IN_FLOATING
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_IN_FLOATING;//使用浮空模式
//	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;//当配置成输入的时候，Speed就没有意义了
	
	GPIO_Init(KEY1_GPIO_PORT, &GPIO_InitStruct);//GPIO_InitStruct定义的是一个变量，获取指针用取地址&
	
}

uint8_t Kye_Scan(GPIO_TypeDef *GPIOx,uint16_t GPIO_Pin)//按键扫描函数
{
	if(GPIO_ReadInputDataBit(GPIOx, GPIO_Pin) == KEY_ON )
	{
		// 松手检测
		while(GPIO_ReadInputDataBit(GPIOx, GPIO_Pin) == KEY_ON);
		return KEY_ON;
	}
	else return KEY_OFF;
}


```

```bsp_key.h
#ifndef _BSP_KEY_H	
#define _BSP_KEY_H

#include "stm32f10x.h"

#define KEY1_GPIO_PIN					GPIO_Pin_0	
#define KEY1_GPIO_PORT				GPIOA
#define KEY1_GPIO_CLK					RCC_APB2Periph_GPIOA

#define KEY_ON								1
#define KEY_OFF								0

void KEY_GPIO_Config(void);
uint8_t Kye_Scan(GPIO_TypeDef *GPIOx,uint16_t GPIO_Pin);

#endif /*_BSP_KEY_H*/

```



## P 21-位带操作-GPIO 的输入和输出

![[../../annex/STM32固件库入门_image_35.png]]

![[../../annex/STM32固件库入门_image_36.png]]

![[../../annex/STM32固件库入门_image_37.png]]

通俗解释：原本 ODR 寄存器控制一个端口16个 IO，现在在芯片内规定一个区域，为每一个 IO 口添加一个32位寄存器控制高低电平

![[../../annex/STM32固件库入门_image_38.png]]

![[../../annex/STM32固件库入门_image_39.png]]

弹幕：如果不设置位带别名区，之前操作一个位需要：读-修改-写，三个过程，比较耗时间。现在用1个32位的寄存器映射一个位，这样操作这个32位的寄存器直接就实现了对1个位的操作，效率提高了。

弹幕：位带区放的是外设寄存器的位地址。位带区：可以进行位带操作的区域

###### 地址转换

![[../../annex/STM32固件库入门_image_40.png]]

```
位带别名区的起始地址 ± 寄存器所在的地址 - 起始地址
*8，一个字节有 8 个位
*4，一个位可以膨胀成4个字节
n*4，第几位
```

###### 统一地址与解析
((addr & 0xF000 0000)+0x0200 0000+((addr & 0x00FF FFFF)<<5)+(bitnum<<2)) 
所需知道的是 addr--寄存器地址，bitnum--位
![[../../annex/STM32固件库入门_image_41.png]]

![[../../annex/STM32固件库入门_image_42.png]]

高四位与 0xF0000000 相与，4&F 还是 4, 2&F 还是 2。
![[../../annex/STM32固件库入门_image_43.png]]

做减法的时候，把高 2 位直接屏蔽掉
![[../../annex/STM32固件库入门_image_44.png]]
比如：将 0x4010 0000 用极限将它等于 0x400F FFFF（10 0000 与 0F FFFF 极限近似相等）
那么 0x4010 0000 和 0x4000 0000 相减的时候，高三位就都为 0。
都是 0 的话，

![[../../annex/STM32固件库入门_image_45.png]]
高两位每次都被减掉，那么跟 0 相与掉就可以了。
真正有效减的时候，不管是什么，减 0 的话，都还是保留后 6 位
![[../../annex/STM32固件库入门_image_46.png]]
那么要保留，就只需要跟 FF FFFF 相与就可以了。（后 6 位跟起始地址的后六位，00 0000 相减。所以都是等于原来的那个值，所以跟 F 相与就对了）

![[../../annex/STM32固件库入门_image_47.png]]
左移 5 位相当于 X8 X4（=32=2<sup>5</sup>）
+位号 X 4 =（左移 2 位）=2<sup>2</sup>

###### 使用位带方式访问 GPIO 的 ODR 寄存器

![[../../annex/STM32固件库入门_image_48.png]]
访问 ODR 实现输出，访问 IDR 实现输入

弹幕：之前的操作一直是改写整个寄存器，位操作可以不改变别的位只改变你需要的位，所以要计算需要位的地址

![[../../annex/STM32固件库入门_image_49.png]]

![[../../annex/STM32固件库入门_image_50.png]]

![[../../annex/STM32固件库入门_image_51.png]]

###### 代码
```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 

#include "bsp_led.h"
#include "bsp_key.h"

//先宏定义GPIO-ODR的寄存器地址找出来
//GPIO-ODR这个寄存器第0位的位地址经过公式转换出来，然后把这个立即数经过强制类型转换成一个指针，然后再对这个指针进行操作，再将这一串定义成一个宏
#define GPIOB_ODR_Addr	(GPIOB_BASE + 0X0C)	//GPIOB_ODR_Addr是我们要操作这个位所在的寄存器的地址,GPIO_BASE是我们固件库已经定义好的外设的起始地址，+ODR的偏移地址
#define PBout(n)		*(unsigned int*)((GPIOB_ODR_Addr & 0xF0000000)+0x02000000+((GPIOB_ODR_Addr & 0x00FFFFFF)<<5)+(n<<2)) 	//GPIOB_ODR_Addr--addr
//经过宏展开之后，经过运算就是一个数字，在编译器看来是一个立即数，要想让编译器看来它是一个地址的话，我们需要经过强制类型转换（32位）。对指针操作，前面还要加个操作符*

//按键1：Pin0
#define GPIOA_IDR_Addr	(GPIOA_BASE + 0X08)
#define PAin(n)		*(unsigned int*)((GPIOA_IDR_Addr & 0xF0000000)+0x02000000+((GPIOA_IDR_Addr & 0x00FFFFFF)<<5)+(n<<2))
//按键2：Pin13
#define GPIOC_IDR_Addr	(GPIOC_BASE + 0X08)
#define PAin(n)		*(unsigned int*)((GPIOC_IDR_Addr & 0xF0000000)+0x02000000+((GPIOC_IDR_Addr & 0x00FFFFFF)<<5)+(n<<2))

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	LED_GPIO_Config();//亮灯
  KEY_GPIO_Config();
	
#if 0
	while(1)
	{
		//GPIO_SetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);//灭灯
		//LED_G(OFF);
		PBout(0) = 1;//置位setbits写1
		Delay(0xFFFFF);
		//GPIO_ResetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);//重新亮灯
		//LED_G(ON);
		PBout(0) = 0;//复位resetbits写0
		Delay(0xFFFFF);
	}
	
#else
	while(1)
	{
		if( PAin(0) == KEY_ON )//是否读取，IDR寄存器的第0位是否等于1，1表示按键按下+
		{
			while( PAin(0) == KEY_ON );//“就一直停在while（PAin（0）==KEY_ON）这里等待”，当按键按下，条件满足后会一直卡在第二个while循环里，直到按键松开
			LED_G_TOGGLE; //还要检测按键释放
		}
			//当你按下的时候是高电平，也就是ON，这个时候检测到高电平，if为真，执行if里的第一条while语句，单个while语句为真，表示死循环，
			//松开按键，while条件为假，跳出循环执行下一条语句
		
		if( PAin(13) == KEY_ON )//Key1可以控制亮灭，Key2也可以控制亮灭
		{
			while( PAin(13) == KEY_ON );
			LED_G_TOGGLE; 
		}
	}

#endif
}


```

```bsp_led.c
//bsp:	board support package	板级支持包
#include "bsp_led.h"

void LED_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStruct;
	
	RCC_APB2PeriphClockCmd(LED_G_GPIO_CLK , ENABLE);
	
	GPIO_InitStruct.GPIO_Pin = LED_GPIO_Config;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_Out_PP;//使用推挽输出
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;//不配置速度就默认配置成最高值
	
	GPIO_Init(LED_G_GPIO_PORT, &GPIO_InitStruct);//GPIO_InitStruct定义的是一个变量，获取指针用取地址&
	
}

```

```bsp_led.h
#ifndef _BSP_LED_H	//条件编译，防止重复编译，尽量在每个头文件定义中都加上条件编译。
#define _BSP_LED_H	//头文件建议如果包含在.c里可以的话就不要在.h里包含，否则耦合性较高，以后不方便改动

#include "stm32f10x.h"//在stm32f10x.h文件中已经有#ifndef _STM32F10X_H 了（不能/会被两次编译）

#define LED_G_GPIO_PIN					GPIO_Pin_0	//选择pin的时候用宏来代替，下次如果换了个GPIO，只需要修改这个宏即可，不需要回到这个函数的实体里面来修改
#define LED_G_GPIO_PORT					GPIOB
#define LED_G_GPIO_CLK					RCC_APB2Periph_GPIOB

#define				ON			1
#define				OFF			0


//	\		C语言中的续行符，要求后面不能有任何的东西，只能是回车
#define				LED_G(a)			if(a)	\
																	GPIO_ResetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);	\
														else	GPIO_SetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);	//a是形参，如果a为真,清零

//	^	异或，C语言的一个二进制的运算符
//	与1异或要改变，与0异或不变。--异或：相异为1，相同为0

#define LED_G_TOGGLE		{LED_G_GPIO_PORT->ODR ^= LED_G_GPIO_PIN;}	

//注意异或符合是上尖角，一开始搞成波浪号了，修改了一天才发现
//总之这个toggle的作用就是 通过异或，使得 GPIOB ODR的 第0位不断翻转(0变1,1变0)，达到使绿灯亮灭的效果

void LED_GPIO_Config(void);

#endif /*_BSP_LED_H*/

```

```bsp_key.c
#include "bsp_key.h"

void KEY_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStruct;	//同样，要先定义一个初始化的结构体
	
	RCC_APB2PeriphClockCmd(KEY1_GPIO_CLK , ENABLE);//记住：不管什么外设初始化都要开时钟
	
	GPIO_InitStruct.GPIO_Pin = KEY1_GPIO_PIN;
	//硬件本身外部下拉了，所以不用通用推挽输出，且我们所需的是输入，所以我们配置成浮空模式，用 GPIO_Mode_IN_FLOATING
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_IN_FLOATING;//使用浮空模式
//	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;//当配置成输入的时候，Speed就没有意义了
	
	GPIO_Init(KEY1_GPIO_PORT, &GPIO_InitStruct);//GPIO_InitStruct定义的是一个变量，获取指针用取地址&
		
	//初始化按键2的
	RCC_APB2PeriphClockCmd(KEY2_GPIO_CLK , ENABLE);
	GPIO_InitStruct.GPIO_Pin = KEY2_GPIO_PIN;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_IN_FLOATING;
	GPIO_Init(KEY2_GPIO_PORT, &GPIO_InitStruct);
	
}

uint8_t Kye_Scan(GPIO_TypeDef *GPIOx,uint16_t GPIO_Pin)//按键扫描函数
{
	if(GPIO_ReadInputDataBit(GPIOx, GPIO_Pin) == KEY_ON )
	{
		// 松手检测
		while(GPIO_ReadInputDataBit(GPIOx, GPIO_Pin) == KEY_ON);
		return KEY_ON;
	}
	else return KEY_OFF;
}

```

```bsp_key.h
#ifndef _BSP_KEY_H	
#define _BSP_KEY_H

#include "stm32f10x.h"

#define KEY1_GPIO_PIN					GPIO_Pin_0	
#define KEY1_GPIO_PORT				GPIOA
#define KEY1_GPIO_CLK					RCC_APB2Periph_GPIOA

#define KEY2_GPIO_PIN					GPIO_Pin_13
#define KEY2_GPIO_PORT				GPIOC
#define KEY2_GPIO_CLK					RCC_APB2Periph_GPIOC

#define KEY_ON								1
#define KEY_OFF								0

void KEY_GPIO_Config(void);
uint8_t Kye_Scan(GPIO_TypeDef *GPIOx,uint16_t GPIO_Pin);

#endif /*_BSP_KEY_H*/

```


## P 22 启动文件讲解

启动文件由汇编语言编写

```
启动文件的作用：
1-初始化堆栈指针 SP
2-初始化 PC 指针，指向复位程序
3-初始化中断向量表
4-配置系统时钟
5-调用 C 库函数_main，最终进入 C 的世界
```

```
汇编程序如何注释：
1-汇编注释用 “；”
2-C 语言注释用“//”或者“/**/”
```




###### 1、Stack--栈
用于局部变量、函数调用、函数形参的开销

EQU：宏定义的伪指令，相当于 define，给一个数字常量取名字
AREA：告诉汇编器一个新的代码段或者数据段
AREA STACK, NOINIT, READWRITE, ALIGN=3
汇编一个数据段，名字叫 STACK，NOINTI 表示不初始化，只是分配一段内存，READWRITE 属性可读可写, 以 2<sup>3</sup>=8 字节对齐
SPACE：用于分配一定大小的内存空间，单位为字节。
标号_initial_sp 紧挨着 SPACE 语句放置，表示栈的结束地址，即栈顶地址，栈是由高向低生长的。

![[../../annex/STM32固件库入门_image_52.png]]

![[../../annex/STM32固件库入门_image_53.png]]

![[../../annex/STM32固件库入门_image_54.png]]

###### 2、Heap 堆
堆用于动态内存的分配，malloc 函数

PRESERVE8： 指定当前文件的堆栈按照 8 字节对齐
THUMB： 表示后面指令为 THUMB 指令。 THUBM 是 ARM 以前的指令集， 16 bit，现在 Cortex-M 系列的都使用 THUMB-2 指令集， THUMB-2 是 32 位的，兼容 16 位和 32 位的指令，是 THUMB 的超级。
EXPORT： 声明一个标号具有全局属性，可被外部的文件使用。如果是 IAR 编译器，则使用的是 GLOBAL 这个指令。
DCD：分配一个或者多个以字为单位的内存，以四字节对齐，并要求初始化这些内存。在向量表中， DCD 分配了一堆内存，并且以 ESR 的入口地址初始化它们。

![[../../annex/STM32固件库入门_image_55.png]]

![[../../annex/STM32固件库入门_image_56.png]]


###### 3、初始化中断向量表
1-向量表实际上是一个 32 位的整型数组，一个元素对应一个异常（ESR），数组元素存的就是 ESR 的入口地址。
2-向量表在复位后从 FLASH 的 0 地址开始，具体的初始化值请查询参考手册的中断章节。
从代码上看，向量表中存放的都是中断服务函数的函数名，可我们知道 C 语言中的函数名就是一个地址。
![[../../annex/STM32固件库入门_image_57.png]]
FLASH 地址是从 0X0800 0000 开始，向量表又是从 0 地址开始存放，这是否矛盾？？
不矛盾，0 地址是相对而言。是相对地址。


EXPORT 相当于 import。  弹幕：不对，应该相当于 C 中无 static 修饰的函数或者全局变量
DCD：表示分配一个或者多个以字为单位的内存，以四字节对齐，并要求初始化这些内存。

![[../../annex/STM32固件库入门_image_58.png]]
中断服务函数一定要跟表里的函数是一样的

![[../../annex/STM32固件库入门_image_59.png]]

![[../../annex/STM32固件库入门_image_60.png]]
优先级的数字，数值越小中断优先级越高
![[../../annex/STM32固件库入门_image_61.png]]


###### 4、复位程序
1-复位程序是上电后单片机执行的第一个程序
2-调用 SystemInit 函数配置系统时钟；调用 C 库函数_main，并最终进入 C 的世界


![[../../annex/STM32固件库入门_image_62.png]]

![[../../annex/STM32固件库入门_image_63.png]]

WEAL：弱定义，优先级不高，可以由用户先定义。即 Reset_Handler 可以由其他文件（C 文件）重新实现。如果其他文件实现了，那么在启动的时候就优先调用你自己定义的。如果没有定义就执行该汇编文件编写的。
WEAK：表示弱定义，如果外部文件优先定义了该标号则首先引用该标号，如果外部文件没有声明也不会出错。这里表示复位子程序可以由用户在其他文件重新实现，这里并不是唯一的。
![[../../annex/STM32固件库入门_image_64.png]]IMPORT：表示该标号来自外部文件，跟 C 语言中的 EXTERN 关键字类似。这里表示 SystemInit 和__main 这两个函数均来自外部的文件。
SystemInit：系统时钟配置函数，需要我们用户编写。
BLX 执行完还要返回，BX 执行完不用返回
![[../../annex/STM32固件库入门_image_65.png]]

###### 5、中断服务程序
防止用户的 C 文件里面编写的中断服务函数错误的时候，还有一个保险，让它无限的循环。

B：跳转指定。
. ：点表示 infinite，表示无限循环

ALIGN 表示对齐，不是 ARM 指令，是编译器的指令。一般会跟一个立即数，不跟默认是四字节对齐。

![[../../annex/STM32固件库入门_image_66.png]]

![[../../annex/STM32固件库入门_image_67.png]]
弱定义，在 C 函数里面定义了，那么首先会执行我们自己写的。但是假如把函数名跟中断向量表里面的名字不一样，而且函数名写了之后编译不报错（函数名可以随便写）。但是又有 systick 中断响应。那么内核就会去调用汇编文件已经写好的空的中断服务程序，一直 B . 无限循环。程序基本会死掉。
所以，编写中断服务函数的时候，函数名一定要跟 Vectors（向量表）里面的名字（Systick_Handler）一样，不然内核在向量表中是查询不到的。
![[../../annex/STM32固件库入门_image_68.png]]


###### 6、用户堆栈初始化
由标准的 C 库函数_main 来完成。

IF, ELSE, ENDIF：汇编的条件分支语句，跟 C 语言的 if ,else 类似。
END：文件结束。
ALIGN：对指令或者数据存放的地址进行对齐，后面会跟一个立即数。缺省表示 4 字节对齐。
![[../../annex/STM32固件库入门_image_69.png]]




![[../../annex/STM32固件库入门_image_70.png]]

在 IDE（开发环境），新建工程的时候在 target 勾选 use MicroLIB，就相当于定义了_MICROLIB 这个宏
![[../../annex/STM32固件库入门_image_71.png]]

![[../../annex/STM32固件库入门_image_72.png]]

R0、R1、R2、R3 是 CPU 的暂存器。有 15 个。（在 CM3 权威指南的 3.1.1 通用目的寄存器 R0~R7）
![[../../annex/STM32固件库入门_image_73.png]]

![[../../annex/STM32固件库入门_image_74.png]]

R15 是 PC 指针，一般初始化指成 Reset，程序一上电执行（复位程序）。
R14 是连接寄存器，一般是函数调用的时候用的，用来保存函数的返回地址的。
R13 是堆栈指针，我们程序执行完汇编程序的时候，堆栈用的是线程模式，优先级是特权级，主堆栈。
剩下的 R1~R12 的寄存器一般是用来做中转站，数据运算的时候/执行程序的时候，用来暂存数据的。
![[../../annex/STM32固件库入门_image_75.png]]



![[../../annex/STM32固件库入门_image_76.png]]
然后程序就进入到 main 函数

先把程序烧录到开发板里面（并上电）。仿真按钮
![[../../annex/STM32固件库入门_image_77.png]]

![[../../annex/STM32固件库入门_image_78.png]]


复位按钮
![[../../annex/STM32固件库入门_image_79.png]]
复位：一开始程序是从哪里执行的
![[../../annex/STM32固件库入门_image_80.png]]

over 一下，相当于下一步↓
![[../../annex/STM32固件库入门_image_81.png]]


勾选上 Debug 中的 Run to main，就看不到汇编了，一来就会执行 main 函数。当复位的情况下才会进入到这个汇编函数里面去执行。
![[../../annex/STM32固件库入门_image_82.png]]







































