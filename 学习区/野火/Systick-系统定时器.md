
## P 30 功能框图讲解和优先级配置
系统定时器是内核里面的一个定时器
### 01 Systick 简介

![[../../annex/Systick-系统定时器_image_1.png]]

### 02 Systick 功能框图讲解

![[../../annex/Systick-系统定时器_image_2.png]]

递减计数器和重装载寄存器都是 24 位的，它们的最大值都是 2 <sup>24</sup>

![[../../annex/Systick-系统定时器_image_3.png]]

![[../../annex/Systick-系统定时器_image_4.png]]

![[../../annex/Systick-系统定时器_image_5.png]]


###### Systick 控制及状态寄存器--CTRL
![[../../annex/Systick-系统定时器_image_6.png]]
COUNTFLAG, 递减寄存器从 reload 的值开始往下递减的时候，递减到 0 的时候，这个第 16 位会置 1。如果读取了这个位, 这么位就会清零。（需要软件来读取这个位）

CLKSOURCE，时钟源选择位，如果该第 2 位=0，时钟等于 AHB 的时钟/8=（72/8=9 M）；如果该第 2 位=1 ，时钟等于处理器的时钟 AHB（CM 3 的话就是72 M，F4 的话就是 180 M）。
↓由第二位 CLKSOURCE 来控制
![[../../annex/Systick-系统定时器_image_7.png]]

TICKINT，第一位，中断使能。如果说从 reload 开始递减，减到 0 的时候，如果等于 0，表示不使能中断（不执行中断服务程序 ）。如果等于 1, 就会产生中断，如果写了中断服务程序，就会进入到中断服务程序里面执行这个程序。

ENABLE，第 0 位，用来使能定时器（置 0 关闭，置 1 打开）。

###### Systick 重装载寄存器--reload
RELOAD，可读可写（R/W）类型，复位值 0，表示当倒数计数到 0 的时候，将被重装载的数值（24 位有效）。

###### Systick 当前数值寄存器--CURRENT
就是刚刚提到的功能框图里面的 STK_VAL 这个值
这个值是实时的记录者递减计数器的值，可读可写（R/W）类型，复位值 0，表示读取时返回当前倒计数的值，写它则使之清零。（如果想实时监控这个值是多少的时候，就可以读取这个寄存器）



![[../../annex/Systick-系统定时器_image_8.png]]

###### Systick 定时时间计算
![[../../annex/Systick-系统定时器_image_9.png]]
1-t：一个计数循环的时间，跟 reload 和 CLK 有关。（一个计数循环就是计数器的值从 reload 开始递减计数到 0）。（与时钟相关，因为时钟跳一次，递减计数器的值就减 1）。（减多少次，跟 reload 的值相关）
2-CLK：72 M 或 9 M，由控制及状态寄存器 CTRL 第 2 位 CLKSOURCE 来控制
3-RELOAD：24 位，由用户自己配置

![[../../annex/Systick-系统定时器_image_10.png]]

![[../../annex/Systick-系统定时器_image_11.png]]
最好使用毫秒级。

![[../../annex/Systick-系统定时器_image_12.png]] 
第四个，校正寄存器一般很少用

###### Systick 寄存器
![[../../annex/Systick-系统定时器_image_13.png]]

![[../../annex/Systick-系统定时器_image_14.png]]

###### Systick 库函数
![[../../annex/Systick-系统定时器_image_15.png]]

内核里面的中断的中断编号都是小于 0 的
如果是内核的外设就配置 SCB->SHP 这个寄存器,
如果是普通的外设（GPIO、EXTI 等）就配置 NVIC->IP 这个寄存器
![[../../annex/Systick-系统定时器_image_16.png]]

内核：
![[../../annex/Systick-系统定时器_image_17.png]]

![[../../annex/Systick-系统定时器_image_18.png]]
也是 4 个位有效。STM 32 在使用中断优先级的时候，都是用 4 个位来表示的。
虽然 core-CM 3 规定用 8 个位来表示中断优先级，但是 stm 32 用不了这么多，低 4 位就把把它去掉了
![[../../annex/Systick-系统定时器_image_19.png]]
只使用高 4 位表示中断优先级

外设：
![[../../annex/Systick-系统定时器_image_20.png]]
内核的外设和普通的外设的中断优先级配置的寄存器不一样。
在比较中断优先级的时候怎么比较？内核的中断优先级普遍会大于普通的中断优先级

![[../../annex/Systick-系统定时器_image_21.png]]


###### Systick中断优先级
![[../../annex/Systick-系统定时器_image_22.png]]

![[../../annex/Systick-系统定时器_image_23.png]]
内核可以配置 0~15，15 的时候二进制是 1111，0 的时候二进制是 0000。如果要比较内核和外设的优先级，就把内核的二进制手动分成主优先级和子优先级进行比较（外设本身就有主优先级和子优先级之分）。相同的情况下去比较中断向量表里面的硬件编号（通常情况下，内核优先）。

弹幕：这里说的是优先级分组，即0-5组，这5组是由外设的高四位分出来的
弹幕：外设分组后，抢占和子优先级确定，将内核优先级数字拆成四位与外设比较得到优先级，相同时内核优先


### 03 Systick 定时实验讲解 ↓

## P 31 微妙和毫秒演示代码讲解

应该是71和71999
↑红字说错了吧，固件库函数里面已经-1了，51的才需要自己-1

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
		//Delay(0xFFFFF);//通过软件来延时，这是不准的，而且仿真也不准
		SysTick_Delay_ms(500);
		//GPIO_ResetBits(LED_G_GPIO_PORT, LED_G_GPIO_PIN);//重新亮灯
		LED_G(ON);
		//Delay(0xFFFFF);
		SysTick_Delay_ms(500);
	}

}


```

```bsp_systick.c
#include "bsp_systick.h"

#if 0
static __INLINE uint32_t SysTick_Config(uint32_t ticks)
{ 
	//判断 tick 的值是否大于2^24，如果大于，不符合规则
  if (ticks > SysTick_LOAD_RELOAD_Msk)  return (1);            //判断ticks的值，这个值是要写到reload寄存器中的，如果不合规（大于2^24）就返回1
                                                    
	//初始化reload寄存器的值
  SysTick->LOAD  = (ticks & SysTick_LOAD_RELOAD_Msk) - 1;      /* set reload register */ //tick写到reload寄存器中
	
	//配置中断优先级，配置为15（4个位，最大为15），默认为（内核外设里）最低的优先级
  NVIC_SetPriority (SysTick_IRQn, (1<<__NVIC_PRIO_BITS) - 1);  	
	//注意括号(1<<__NVIC_PRIO_BITS),__NVIC_PRIO_BITS这个的参数是4，相当于(1<<4)即2^4，前面有兄弟说是要占用一周期所以要-1.

	// 初始化counter的值为0
  SysTick->VAL   = 0;    

	//配置 systick 的时钟为 72M
	//使能中断
	//使能systick
  SysTick->CTRL  = SysTick_CTRL_CLKSOURCE_Msk | 
                   SysTick_CTRL_TICKINT_Msk   | 
                   SysTick_CTRL_ENABLE_Msk;                    
  return (0);                                                  
}
#endif


void SysTick_Delay_us(uint32_t us)//微秒级延时//如果传进来的形参是1000，循环1000次1us就是1000us
{
	uint32_t i;
	
	SysTick_Config(72);
	
	//一直判断RTCL寄存器CLF这个位是否置1，置1就进入下一个循环，表示一微秒时间到了。这个for循坏就是计数n个us用的，计完n个微秒后跳出循环
	for(i=0;i<us;i++)
	{
		while( !((SysTick->CTRL) & (1<<16)) );//&第16位。//一读这个位的话又会清零，而且计数器的值又会从reload的值开始往下递减计数。一个循环就是1us
	}
	
	SysTick->CTRL &= ~ SysTick_CTRL_ENABLE_Msk;//&=不能分开
}


void SysTick_Delay_ms(uint32_t ms)//毫秒级延时
{
	uint32_t i;
	
	SysTick_Config(72000);
	
	//一直判断RTCL寄存器CLF这个位是否置1，置1就进入下一个循环，表示一毫秒时间到了
	for(i=0;i<ms;i++)
	{
		while( !((SysTick->CTRL) & (1<<16)) );//&第16位
	}
	
	SysTick->CTRL &= ~ SysTick_CTRL_ENABLE_Msk;//&=不能分开
}


```

```bsp_systick.h
#ifndef _BSP_SYSTICK_H
#define _BSP_SYSTICK_H

#include "stm32f10x.h"//因为bsp_led的驱动led编程的是GPIO这个外设，GPIO寄存器所有定义都在stm32f10x.h文件中

#include "core_cm3.h"//今天编译的是内核里面的外设

void SysTick_Delay_us(uint32_t us);//微秒级延时
void SysTick_Delay_ms(uint32_t ms);//毫秒级延时

#endif /*_BSP_SYSTICK_H*/



```


























