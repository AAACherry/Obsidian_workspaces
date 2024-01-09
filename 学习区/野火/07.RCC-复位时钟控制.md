
时钟相当于人的心脏，所有外设想工作都得有时钟。

## P 23 第一节-时钟树讲解
由 RCC_CFGR 时钟配置寄存器来控制
![[../../annex/RCC-复位时钟控制_image_1.png]]

![[../../annex/RCC-复位时钟控制_image_2.png]]

128 M 是最高频率


一般配置系统时钟流程就是：利用 HSE 8 M 经过倍频，一般会 9 倍频，配置成 72 M。
当 HSE 故障时，那么 HSI 就是内部备份，被选为系统时钟。此时，整个系统就会被配置成 8 M（跟瘫痪差不多）
![[../../annex/RCC-复位时钟控制_image_3.png]]


###### HSE 时钟
HSE：HIgh Speed External Clock signal，即高速的外部时钟。
来源：无源晶振（4-16 M），通常使用 8 M。
控制：RCC_CR 时钟控制寄存器的位 16：HSEON 控制

对应功能框图：
![[../../annex/RCC-复位时钟控制_image_4.png]]
HSE 是高速的，相对的还有一个内部的叫 LSE, 也是高速的，也是 8 M.
相比之下，HSE 精度较高；LSE 精度较小，受温度的影响比较大，会有温漂。
![[../../annex/RCC-复位时钟控制_image_5.png]]

对应原理图：
![[../../annex/RCC-复位时钟控制_image_6.png]]

通过 OSC_IN, OSC_OUT 进入芯片。
当使用无源晶振的时候，一定要配上起振电容，此处用 20 频反
![[../../annex/RCC-复位时钟控制_image_7.png]]

如果使用的是有源晶振，那么 OSC_OUT 引脚悬空。，通过 OSC_IN 这个引脚进入单片机里面
![[../../annex/RCC-复位时钟控制_image_8.png]]



![[../../annex/RCC-复位时钟控制_image_9.png]]
HSE 第 16 位置 1，表示启动外部的 HSE 晶振。
![[../../annex/RCC-复位时钟控制_image_10.png]]
开启的时候因为是无源的，还需要跟起振电容一起配置来起振。
起振需要一定的时间，如果启动完毕，那么还有一个位，叫 HSERDY（外部高速时钟就绪标志），当这个位为 1 的时候，表示外部的振荡器 HSE 准备就绪。就会进入下一个启动流程
![[../../annex/RCC-复位时钟控制_image_11.png]]

###### HSI 时钟

HSI：Low Speed Internal Clock signal，高速的内部时钟。
来源：芯片内部，大小为 8 M，当 HSE 故障时，系统时钟会自动切换到 HSI，直到 HSE 启动成功。
控制：RCC_CR 时钟控制寄存器的位 0：HSION 控制

HSE 和 HSI 的低 2 位（启动位ON 、标志位 RDY）一致。
弹幕：大家注意一下这两位分别是硬件和软件置1
![[../../annex/RCC-复位时钟控制_image_3.png]]


#### 时钟树主系统时钟讲解
###### 锁相环时钟
弹幕：看完正点原子这部分的前两集再看这部分四路会清晰蛮多
弹幕：锁相环就是倍频器
![[../../annex/RCC-复位时钟控制_image_12.png]]

HSE 选择 8 M，进来之后会有两条线，一条线是不分频，一条线二分频，由开关 （PLLXTPRE，也是寄存器的控制位） 来选择。经过开关选择后来到 PLLSRC（锁相环的时钟源）。
PLLSRC（锁相环的时钟源）有两个，一个是内部是 HSI，是必须会被分频的（8 M ÷ 2=4 M）。由 RCC_CFGR 寄存器来控制
![[../../annex/RCC-复位时钟控制_image_13.png]]

开关 （PLLXTPRE，也是寄存器的控制位）对应的是时钟配置寄存器（RCC_CFGR）的第十七个位。
表示 HSE 进入到锁相环时钟的时候，选择分频还是不分频。
![[../../annex/RCC-复位时钟控制_image_14.png]]
置 0 HSE 不分频，置 1 HSE 分频，通常情况下，一般配置 HSE 不分频，即 8 M，然后进入锁相环去倍频。
一般情况下，PLLXTPRE 置 0，PLLSRC 置 1
![[../../annex/RCC-复位时钟控制_image_15.png]]

8 M 进入，不选择内部时钟，选择 HSE，然后由倍频因子（PLLMUL，也是 RCC_CFGR 寄存器的倍频位）来倍频（可选择 2~16 来倍频），选择 9，得到 72 M 的倍频
![[../../annex/RCC-复位时钟控制_image_16.png]]


![[../../annex/RCC-复位时钟控制_image_17.png]]
最常使用 HSE ，而不是 HSI÷2


弹幕：看不懂的建议安装 stm32cubeMX,里面有棵可以用鼠标点一点的时钟树,配置好就能直接生成代码,不用自己操作寄存器

###### 系统时钟

![[../../annex/RCC-复位时钟控制_image_18.png]]

![[../../annex/RCC-复位时钟控制_image_19.png]]
有三种来源方式：HSE、HSI、PLLCLK。一般配置成系统时钟等于锁相环时钟，由位 SW 来控制

通常情况下，置 10
![[../../annex/RCC-复位时钟控制_image_20.png]]


配置好系统时钟后，有 AHB 预分频器，配置 AHB 总线的。在 AHB 总线基础上还有两个预分频器（APB 1、APB 2），这两条总线上挂载了很多的外设。
![[../../annex/RCC-复位时钟控制_image_21.png]]

![[../../annex/RCC-复位时钟控制_image_22.png]]

在配置系统时钟的时候，只配置好这三条总线的时钟。具体外设的时钟，使用时再进行配置。

三条总线的倍频因子配置成多少？（由 RCC_CFGR 寄存器的三个段位来配置）
![[../../annex/RCC-复位时钟控制_image_23.png]]
AHB 配置成 1，即系统时钟一分频，那么这个 AHB 总线的时钟=系统时钟。
APB 1 最高只能是 35 M，配置分频因子为 2，72÷2=36 M=最大的时钟。
APB 2 是高速的总线，分频因子配置成 1，等于 72 M。

###### HCLK 时钟

![[../../annex/RCC-复位时钟控制_image_24.png]]

![[../../annex/RCC-复位时钟控制_image_25.png]]
AHB 的外设有 SDIO、FSMC
还为系统编辑器 SystemTick 配置时钟，八分频。72÷8=9 M。
还为系统内核提供时钟
还有一些是 SDIO 的 AHB 总线接口


##### PCLK 1 时钟

![[../../annex/RCC-复位时钟控制_image_26.png]]

倍频因子有1、2、4、8、16。最大为 36 M。
![[../../annex/RCC-复位时钟控制_image_27.png]]

###### 低级定时器时钟（2-7）
如果 APB 1 预分频系数=1 则频率不变（72 M），否则频率 X 2。给定时器 2-7 提供时钟。
APB 1 分频因子配置成 2，2 ≠1，频率 X 2：36 X 2=72 M
![[../../annex/RCC-复位时钟控制_image_28.png]]


##### PCLK 2 时钟

![[../../annex/RCC-复位时钟控制_image_29.png]]

倍频因子有1、2、4、8、16。最大为 72 M。
![[../../annex/RCC-复位时钟控制_image_30.png]]

###### 高级定时器（1 和8）
同样的，还有定时器 1和8（高级定时器）
![[../../annex/RCC-复位时钟控制_image_31.png]]

###### ADC 时钟
特殊的，APB 2 的时钟给 ADC 提供时钟，ADC 最大时钟只能是 14 M。
![[../../annex/RCC-复位时钟控制_image_32.png]]

ADC 的分频因子只能是 2、4、6、8，最大时钟=14 M。
反推过去，当 APB 2 总线时钟配置成 72 M 的时候，往往经过 ADC 预分频器的分频因子, 是达不到 ADC 的最大时钟 14 M 的。只能更小。
![[../../annex/RCC-复位时钟控制_image_33.png]]


#### RTC 时钟
Real，实时时钟
![[../../annex/RCC-复位时钟控制_image_34.png]]
LSE 外部晶振，LSI 内部晶振。都是低速的。

原理图：
![[../../annex/RCC-复位时钟控制_image_35.png]]

通过 0SC32_IN 引脚/0SC32_OUT 进入单片机。此处有两个晶振，实际上开发板加工的时候只选择贴一个。一个封装是插件的，另一个是贴片的。
同样是无缘的，要配置 10 PF 的起振电容。
![[../../annex/RCC-复位时钟控制_image_36.png]]

即：
![[../../annex/RCC-复位时钟控制_image_37.png]]
来源有三个：HSE、外部 LSE（32.768 kHz），内部 LSI（40 kHz）
内部的 LSI 一般是 30~60 HZ 之间偏移，受温度影响较大。一般取值 40 KHz，所以要想精度高的话，都会使用 LSE，由外部的高速晶振来提供给 RTC。

弹幕：周期性的脉冲信号

还有独立看门狗的时钟，这个时钟专门由内部的 LSI 来提供的。
![[../../annex/RCC-复位时钟控制_image_38.png]]


![[../../annex/RCC-复位时钟控制_image_39.png]]

#### MCO 时钟输出
Microcontroller clock output，微控制器的时钟输出引脚，由 PA 8 复用所得。
![[../../annex/RCC-复位时钟控制_image_40.png]]
控制由 CFGR：MCO 这个位来控制。MCO 引脚在 f103 里面对应 PA8 引脚

自己写的时钟配置函数，想检测正确，可以用 MCO 输出引脚，通过示波器来监控波形，看频率对不对。
![[../../annex/RCC-复位时钟控制_image_41.png]]

框图中的梯形是开关，上面的英文代表寄存器的某些位，通过它来进行控制。
![[../../annex/RCC-复位时钟控制_image_42.png]]


弹幕：要输出的矩形波，都是自己配置时钟的来得到的

#### 总结
首先使用外部的高速时钟 HSE 来配置系统时钟（8 M），
然后不分频，进入锁相环的时钟源，配置 PLLSRC 位。
然后进入锁相环的倍频因子，（PLLMUL）配成 9 倍频，出来的锁相环时钟 PLLCLK 则为 72 M。
配置系统时钟时有三种选择，（内部的 HSI，HSE，锁相环时钟 PLLCLK）。选择锁相环时钟=系统时钟

接下来配置三条总线的分频因子。
首先是 AHB 高速总线的预分频器，配置成 1 分频。
又会经过 APB 1、APB 2 的预分频因子。
APB 1 最大 36 M，所以选择 2 分频。
APB 2 是高速总线，最大是 72 M，所以配置成 1 分频。

总线时钟配置好后，其余外设的时钟再根据使用需求来配置分频因子。

#### 时钟安全系统 -CSS
![[../../annex/RCC-复位时钟控制_image_43.png]]
当 HSE 时钟发生故障，就会产生一个中断（时钟安全中断），这个中断连接到 NMI 中断（不可屏蔽中断）。同时还连接到时钟失效事件，送到高级定时器（TIM 1 和 TIM 8）的刹车输入端。

这个刹车输入端有一个外部的 IO
中断源可以是由外部的 GPIO 的电平的极性来控制（高低电平）。
时钟失效事件（CSSI），表示 HSE 出现故障，会产生一个中断连接到刹车输入端。
![[../../annex/RCC-复位时钟控制_image_44.png]]

此时我们可以在时钟失效中断服务函数里面写一些营救工作，比如说保存我们最终的数据/开机重启/启用内部的 HSI 来作为系统时钟（不是说系统时钟就配置成 8 M，而是说在锁相环源的时候，选择 HSI÷2=4 M 来配置我们的时钟），然后倍频因子选择 16，把系统时钟配置成 64 M。
![[../../annex/RCC-复位时钟控制_image_45.png]]

CSS 还有就是 HSE 出现问题，系统、单片机会硬件上把我们的系统时钟切换到 HSI，如果没有使能 CSS 中断的话，用户软件上没有写的话，HSI 就被选成系统时钟。这个系统时钟只有 8 M ，低速运行。
![[../../annex/RCC-复位时钟控制_image_46.png]]


## P 24 系统时钟配置函数 SetSysClockTo 72 ()

默认情况下配置成 SYSCLK_FREQ_72MHz 

![[../../annex/RCC-复位时钟控制_image_47.png]]
参考这两部分资料
![[../../annex/RCC-复位时钟控制_image_48.png]]

预取指（Prefetch Buffer）：M3内核在Flash 中的读了一条之后，在执行。那么会把第二条执行的也先取好。
![[../../annex/RCC-复位时钟控制_image_49.png]]

而且取指的时候，需要等待时间，不能取很快，等待时间的周期跟频率是一样的。如果是 72 M，两条指令之间取得时间要等待多少个周期？
手册（STM32F10xxx 闪存编程参考手册）中有规定。
![[../../annex/RCC-复位时钟控制_image_50.png]]
![[../../annex/RCC-复位时钟控制_image_51.png]]
![[../../annex/RCC-复位时钟控制_image_52.png]]

接下来配置 AHB,APB1,APB2的时钟分频因子，即下面这三个位段
![[../../annex/RCC-复位时钟控制_image_53.png]]

PLL 翻译过来就是锁相环，接下来配置锁相环。
![[../../annex/RCC-复位时钟控制_image_54.png]]
配置锁相环 PLLCLK 配置成72 M。
对应这三位，首先配置位 17, 让 HSE 不分频进入锁相环
![[../../annex/RCC-复位时钟控制_image_55.png]]

![[../../annex/RCC-复位时钟控制_image_56.png]]

使能（打开）锁相环。控制这个位。同样锁相环有个 READY 位 (标志位)。
![[../../annex/RCC-复位时钟控制_image_57.png]]


弹幕：这个＆=是干啥的，感觉|=那一行代码就行了啊

选择系统时钟的来源(这里选择 PLLCLK 作为系统时钟)。配置的是寄存器 SW 位。
![[../../annex/RCC-复位时钟控制_image_58.png]]
配置成 10，系统时钟=锁相环时钟
![[../../annex/RCC-复位时钟控制_image_59.png]]
同样的，系统时钟切换的时候，如果切换成功了，那么还有相应的状态位。
等待 PLLCLK 成功切换为系统时钟。

锁相环倍频因子可以是 2-16。
想超频（大于 72 M），可以将 9 改成 16，就可以 8 X 16=128 M 了。这是一个宏定义：
![[../../annex/RCC-复位时钟控制_image_60.png]]

但是为了保证库的完整性，不要进行修改，自己另外写一个这样的函数。
static void SetSysClockTo 128(void)

###### 代码
```SetSysClockTo72
static void SetSysClockTo72(void)

{

  __IO uint32_t StartUpCounter = 0, HSEStatus = 0;

  /* SYSCLK, HCLK, PCLK2 and PCLK1 configuration ---------------------------*/    

  /* 使能 HSE */    

  RCC->CR |= ((uint32_t)RCC_CR_HSEON);

  /* 等待HSE就绪，并做超时处理 */

  do//首先读取寄存器的CR位和READY位，如果READY了，那么返回HSEStatus就置1，置1while为假，就跳出do循环

  {

    HSEStatus = RCC->CR & RCC_CR_HSERDY;

    StartUpCounter++;  

  } while((HSEStatus == 0) && (StartUpCounter != HSE_STARTUP_TIMEOUT));

  

    //再判断一下

  if ((RCC->CR & RCC_CR_HSERDY) != RESET)

  {

    HSEStatus = (uint32_t)0x01;

  }

  else

  {

    HSEStatus = (uint32_t)0x00;

  }  

  

    //如果HSE启动成功，程序则继续往下执行

  if (HSEStatus == (uint32_t)0x01)

  {

    /* Enable Prefetch Buffer: 使能预取指 */

    FLASH->ACR |= FLASH_ACR_PRFTBE;

  

    /* Flash 2 wait state: Flash等待两个周期 */

    FLASH->ACR &= (uint32_t)((uint32_t)~FLASH_ACR_LATENCY);

    FLASH->ACR |= (uint32_t)FLASH_ACR_LATENCY_2;    

//这三句代码操作的就不是RCC寄存器了,而是跟Flash相关的

//接下来配置AHB,APB1,APB2的时钟分频因子

    /* HCLK = SYSCLK: AHB总线时钟=系统时钟=72M */

    RCC->CFGR |= (uint32_t)RCC_CFGR_HPRE_DIV1;

    /* PCLK2 = HCLK = 72M :一分频 */

    RCC->CFGR |= (uint32_t)RCC_CFGR_PPRE2_DIV1;

    /* PCLK1 = HCLK = 36M :二分频 */

    RCC->CFGR |= (uint32_t)RCC_CFGR_PPRE1_DIV2;

    /*  PLL configuration: PLLCLK = HSE * 9 = 72 MHz --配置锁相环 PLLCLK配置成72M */

    RCC->CFGR &= (uint32_t)((uint32_t)~(RCC_CFGR_PLLSRC | RCC_CFGR_PLLXTPRE |

                                        RCC_CFGR_PLLMULL));

    RCC->CFGR |= (uint32_t)(RCC_CFGR_PLLSRC_HSE | RCC_CFGR_PLLMULL9);//9可以改成16,(配置成超频)。这是一个宏，

  
  

    /* Enable PLL --使能（打开）锁相环 */

    RCC->CR |= RCC_CR_PLLON;

  

    /* Wait till PLL is ready --等待锁相环稳定 */

    while((RCC->CR & RCC_CR_PLLRDY) == 0)

    {

    }

    /* Select PLL as system clock source --选择系统时钟的来源(这里选择PLLCLK作为系统时钟) */

    RCC->CFGR &= (uint32_t)((uint32_t)~(RCC_CFGR_SW));//弹幕:这个＆=是干啥的，感觉|=那一行代码就行了啊

    RCC->CFGR |= (uint32_t)RCC_CFGR_SW_PLL;    

  

    /* Wait till PLL is used as system clock source --等待PLLCLK成功切换为系统时钟 */

    while ((RCC->CFGR & (uint32_t)RCC_CFGR_SWS) != (uint32_t)0x08)

    {

    }

  }

  else

  { /* 如果HSE启动失败，用户可以在这里添加处理错误的代码 */

  }

}
```


## P 25 使用 HSE 配置系统时钟并使用 MCO 输出监控系统时钟

回顾：bsp=board support package 板级支持程序包

弹幕：保持随时编译的习惯，避免产生大量错误难以修改
弹幕：时钟树的流程图就是配置顺序，各位同学清醒一点
弹幕：想看哪个参数或者哪个函数在哪定义就选中然后邮件跳转

###### 代码

```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 
#include "bsp_led.h"
#include "bsp_rccclkconfig.h"

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	//HSE_SetSysClk( uint32_t RCC_PLLMul_10);//先看配置成72M时的频率是什么样的(看灯的闪烁情况)
	HSE_SetSysClk(RCC_PLLMul_16);//超频，将时钟频率改成128M，灯泡闪烁速度更快。
	
	MCO_GPIO_Config();
	
	LED_GPIO_Config();//亮灯
	
	while(1)
	{

		LED_G(OFF);
		Delay(0xFFFFF);

		LED_G(ON);
		Delay(0xFFFFF);
	}

}




```

```bsp_rccclkconfig.c
#include "bsp_rccclkconfig.h"

void HSE_SetSysClk( uint32_t RCC_PLLMul_x)
{
	//变量的声明要在大括号前面
	ErrorStatus HSEStatus;//ErrorStatus是一个预先定义好的数据类型声明，有两个值，一个是0，另一个是SUCCESS(非0)。通常情况下是1。
	
	//重写时钟的时候，一定要把RCC的所有的寄存器复位成复位值(刚上电时候的值。就是复位的状态)
	RCC_DeInit();
	
	//使能 HSE
	RCC_HSEConfig(RCC_HSE_ON);
	
	HSEStatus = RCC_WaitForHSEStartUp();//返回ERROR或者SUCCESS
	
	if( HSEStatus == SUCCESS )
	{
		// 使能预取指
		FLASH_PrefetchBufferCmd(FLASH_PrefetchBuffer_Enable);
		//设置两个等待周期
		FLASH_SetLatency( FLASH_Latency_2 );
		
		//配置三个总线的分频因子
		RCC_HCLKConfig(RCC_SYSCLK_Div1);
		RCC_PCLK1Config(RCC_SYSCLK_Div2);
		RCC_PCLK2Config(RCC_SYSCLK_Div1);
		
		//配置锁相环
		//配置 PLLCLK = HSE * RCC_PLLMul_x
		RCC_PLLConfig(RCC_PLLSource_HSE_Div1, RCC_PLLMul_x);//想超频，通过函数的形参来超频，即把RCC_PLLMul_9替换成RCC_PLLMul_x。形参是32位的,x换成2~16任何一个整数值都可
		//使能PLL
		RCC_PLLCmd(ENABLE);
		//等待PLL稳定
		while( RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET );
		
		//选择系统时钟
		RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);//RCC_GetFlagStaus();返回的类型里定义了RESET
		
		while( RCC_GetSYSCLKSource() != 0x08 );
	}
	else
	{
		/* 如果HSE启动失败，用户可以在这里添加处理错误的代码 */
	}
}


```

```bsp_rccclkconfig.h
#ifndef _BSP_RCCCLKCONFIG_H
#define _BSP_RCCCLKCONFIG_H

#include "stm32f10x.h"

void HSE_SetSysClk( uint32_t RCC_PLLMul_x);

#endif /*_BSP_RCCCLKCONFIG_H*/

```


##### 运用示波器

MCO 时钟输出
![[../../annex/RCC-复位时钟控制_image_61.png]]

如果把系统时钟通过外部引脚 MCO 输出，然后用示波器来检测。
![[../../annex/RCC-复位时钟控制_image_62.png]]

指南者的 Pin 8 连接了蜂鸣器。跳帽可以把 Pin 8 蜂鸣器断开。
![[../../annex/RCC-复位时钟控制_image_63.png]]


![[../../annex/RCC-复位时钟控制_image_64.png]]

Pin 8 接示波器的一个通道。
![[../../annex/RCC-复位时钟控制_image_65.png]]

而且要共地，共地从板子上引一个地出来
![[../../annex/RCC-复位时钟控制_image_66.png]]

![[../../annex/RCC-复位时钟控制_image_67.png]]

接到鳄鱼夹。
![[../../annex/RCC-复位时钟控制_image_68.png]]

然后给开发板上电。打开示波器

![[../../annex/RCC-复位时钟控制_image_69.png]]

改成 80 M 看看
![[../../annex/RCC-复位时钟控制_image_70.png]]

倍频因子设置成 16，超频成 128 M。
![[../../annex/RCC-复位时钟控制_image_71.png]]

系统的72 M
![[../../annex/RCC-复位时钟控制_image_72.png]]

###### 代码
```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 
#include "bsp_led.h"
#include "bsp_rccclkconfig.h"

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	//HSE_SetSysClk( uint32_t RCC_PLLMul_10);//先看配置成72M时的频率是什么样的(看灯的闪烁情况)
	HSE_SetSysClk(RCC_PLLMul_16);//超频，将时钟频率改成128M，灯泡闪烁速度更快。
	
	MCO_GPIO_Config();
	RCC_MCOConfig(RCC_MCO_SYSCLK);//烧录后用示波器看Pin8引脚是不是等于72M//蜂鸣器会响，因为指南者的Pin8接了蜂鸣器
	
	LED_GPIO_Config();//亮灯
	
	while(1)
	{

		LED_G(OFF);
		Delay(0xFFFFF);

		LED_G(ON);
		Delay(0xFFFFF);
	}

}


```

```bsp_rccclkconfig.c
#include "bsp_rccclkconfig.h"

void HSE_SetSysClk( uint32_t RCC_PLLMul_x)
{
	//变量的声明要在大括号前面
	ErrorStatus HSEStatus;//ErrorStatus是一个预先定义好的数据类型声明，有两个值，一个是0，另一个是SUCCESS(非0)。通常情况下是1。
	
	//重写时钟的时候，一定要把RCC的所有的寄存器复位成复位值(刚上电时候的值。就是复位的状态)
	RCC_DeInit();
	
	//使能 HSE
	RCC_HSEConfig(RCC_HSE_ON);
	
	HSEStatus = RCC_WaitForHSEStartUp();//返回ERROR或者SUCCESS
	
	if( HSEStatus == SUCCESS )
	{
		// 使能预取指
		FLASH_PrefetchBufferCmd(FLASH_PrefetchBuffer_Enable);
		//设置两个等待周期
		FLASH_SetLatency( FLASH_Latency_2 );
		
		//配置三个总线的分频因子
		RCC_HCLKConfig(RCC_SYSCLK_Div1);
		RCC_PCLK1Config(RCC_SYSCLK_Div2);
		RCC_PCLK2Config(RCC_SYSCLK_Div1);
		
		//配置锁相环
		//配置 PLLCLK = HSE * RCC_PLLMul_x
		RCC_PLLConfig(RCC_PLLSource_HSE_Div1, RCC_PLLMul_x);//想超频，通过函数的形参来超频，即把RCC_PLLMul_9替换成RCC_PLLMul_x。形参是32位的,x换成2~16任何一个整数值都可
		//使能PLL
		RCC_PLLCmd(ENABLE);
		//等待PLL稳定
		while( RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET );
		
		//选择系统时钟
		RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);//RCC_GetFlagStaus();返回的类型里定义了RESET
		
		while( RCC_GetSYSCLKSource() != 0x08 );
	}
	else
	{
		/* 如果HSE启动失败，用户可以在这里添加处理错误的代码 */
	}
}

void MCO_GPIO_Config()
{
		GPIO_InitTypeDef GPIO_InitStruct;
	
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA , ENABLE);
	
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_8;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_AF_PP;//选用复用推挽输出
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
	
	GPIO_Init(GPIOA, &GPIO_InitStruct);
}



```

```bsp_rccclkconfig.h
#ifndef _BSP_RCCCLKCONFIG_H
#define _BSP_RCCCLKCONFIG_H

#include "stm32f10x.h"

void HSE_SetSysClk( uint32_t RCC_PLLMul_x);
void MCO_GPIO_Config();

#endif /*_BSP_RCCCLKCONFIG_H*/


```

稳定的 8 M，而且峰值是三点多 V
![[../../annex/RCC-复位时钟控制_image_73.png]]

霸道，右下角蓝色的线引脚 Pin 8，共地，黄色的地线，连接的是示波器的鳄鱼夹
![[../../annex/RCC-复位时钟控制_image_74.png]]



## P 26 使用 HSI 配置系统时钟并使用 MCO 输出监控系统时钟

使用 HSI 是没有太大意义的，但是在 64 M 的情况下，很多外设也能够工作。

从功能框图来看，配置锁相环来源的时候，使用 HSI 时只能用二分频。
![[../../annex/RCC-复位时钟控制_image_75.png]]


systemInit()函数里面默认会配置为72MHz

###### 代码
```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 
#include "bsp_led.h"
#include "bsp_rccclkconfig.h"

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	//HSE_SetSysClk( uint32_t RCC_PLLMul_10);//先看配置成72M时的频率是什么样的(看灯的闪烁情况)
	//HSE_SetSysClk(RCC_PLLMul_16);//超频，将时钟频率改成128M，灯泡闪烁速度更快。
	
	//HSI_SetSysClk(RCC_PLLMul_16);//72M改为64M
	//HSI_SetSysClk(RCC_PLLMul_12);//72M改为48M
	
	MCO_GPIO_Config();
	RCC_MCOConfig(RCC_MCO_SYSCLK);//烧录后用示波器看Pin8引脚是不是等于72M//蜂鸣器会响，因为指南者的Pin8接了蜂鸣器
	//RCC_MCOConfig(RCC_MCO_HSI);//配置成HSE、HSI
	
	LED_GPIO_Config();//亮灯
	
	while(1)
	{

		LED_G(OFF);
		Delay(0xFFFFF);

		LED_G(ON);
		Delay(0xFFFFF);
	}

}


```

```bsp_rccclkconfig.c
#include "bsp_rccclkconfig.h"

void HSE_SetSysClk( uint32_t RCC_PLLMul_x)
{
	//变量的声明要在大括号前面
	ErrorStatus HSEStatus;//ErrorStatus是一个预先定义好的数据类型声明，有两个值，一个是0，另一个是SUCCESS(非0)。通常情况下是1。
	
	//重写时钟的时候，一定要把RCC的所有的寄存器复位成复位值(刚上电时候的值。就是复位的状态)
	RCC_DeInit();
	
	//使能 HSE
	RCC_HSEConfig(RCC_HSE_ON);
	
	HSEStatus = RCC_WaitForHSEStartUp();//返回ERROR或者SUCCESS
	
	if( HSEStatus == SUCCESS )
	{
		// 使能预取指
		FLASH_PrefetchBufferCmd(FLASH_PrefetchBuffer_Enable);
		//设置两个等待周期
		FLASH_SetLatency( FLASH_Latency_2 );
		
		//配置三个总线的分频因子
		RCC_HCLKConfig(RCC_SYSCLK_Div1);
		RCC_PCLK1Config(RCC_SYSCLK_Div2);
		RCC_PCLK2Config(RCC_SYSCLK_Div1);
		
		//配置锁相环
		//配置 PLLCLK = HSE * RCC_PLLMul_x
		RCC_PLLConfig(RCC_PLLSource_HSE_Div1, RCC_PLLMul_x);//想超频，通过函数的形参来超频，即把RCC_PLLMul_9替换成RCC_PLLMul_x。形参是32位的,x换成2~16任何一个整数值都可
		//使能PLL
		RCC_PLLCmd(ENABLE);
		//等待PLL稳定
		while( RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET );
		
		//选择系统时钟
		RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);//RCC_GetFlagStaus();返回的类型里定义了RESET
		
		while( RCC_GetSYSCLKSource() != 0x08 );
	}
	else
	{
		/* 如果HSE启动失败，用户可以在这里添加处理错误的代码 */
	}
}



void HSI_SetSysClk( uint32_t RCC_PLLMul_x)
{
	//变量的声明要在大括号前面
	__IO uint32_t HSIStatus = 0;//定义成是32位，加个IO表示勿扰态，即防止编译器优化。//因为这个是寄存器，每次都要回到它的原始地址里面去读，所以加个IO稳妥一点//初始化成0
	
	//重写时钟的时候，一定要把RCC的所有的寄存器复位成复位值(刚上电时候的值。就是复位的状态)
	RCC_DeInit();
	
	//使能 HSI
	RCC_HSICmd(ENABLE);
	
	HSIStatus = RCC->CR & RCC_CR_HSIRDY;//返回的值不是两个状态位ERROR或者SUCCESS，而是32位的值。
	//如果HSIRDY这个位置1的话就表示启动成功。//一&的话，其他位都被清零了，只有这个位能取得出来
	
	if( HSIStatus == RCC_CR_HSIRDY )//等于这个值就表示启动成功了。
	{
		// 使能预取指
		FLASH_PrefetchBufferCmd(FLASH_PrefetchBuffer_Enable);
		//设置两个等待周期
		FLASH_SetLatency( FLASH_Latency_2 );
		
		//配置三个总线的分频因子
		RCC_HCLKConfig(RCC_SYSCLK_Div1);
		RCC_PCLK1Config(RCC_HCLK_Div2);
		RCC_PCLK2Config(RCC_HCLK_Div1);
		
		//配置锁相环
		//配置 PLLCLK = HSE * RCC_PLLMul_x
		RCC_PLLConfig(RCC_PLLSource_HSI_Div2, RCC_PLLMul_x);//想超频，通过函数的形参来超频，即把RCC_PLLMul_9替换成RCC_PLLMul_x。形参是32位的,x换成2~16任何一个整数值都可
		//使能PLL
		RCC_PLLCmd(ENABLE);
		//等待PLL稳定
		while( RCC_GetFlagStatus(RCC_FLAG_PLLRDY) == RESET );
		
		//选择系统时钟
		RCC_SYSCLKConfig(RCC_SYSCLKSource_PLLCLK);//RCC_GetFlagStaus();返回的类型里定义了RESET
		
		while( RCC_GetSYSCLKSource() != 0x08 );
	}
	else
	{
		/* 如果HSI启动失败，用户可以在这里添加处理错误的代码 */
	}
}

void MCO_GPIO_Config()
{
	GPIO_InitTypeDef GPIO_InitStruct;
	
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_GPIOA , ENABLE);
	
	GPIO_InitStruct.GPIO_Pin = GPIO_Pin_8;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_AF_PP;//选用复用推挽输出
	GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;
	
	GPIO_Init(GPIOA, &GPIO_InitStruct);
}

```

```bsp_rccclkconfig.h
#ifndef _BSP_RCCCLKCONFIG_H
#define _BSP_RCCCLKCONFIG_H

#include "stm32f10x.h"

void HSE_SetSysClk( uint32_t RCC_PLLMul_x);
void MCO_GPIO_Config();
void HSI_SetSysClk( uint32_t RCC_PLLMul_x);

#endif /*_BSP_RCCCLKCONFIG_H*/


```



## P 27 STM 32 中断应用总结

中断简介
![[../../annex/STM32的中断_image_1.png]]
无特殊说明，异常就是中断，中断就是异常

中断优先级，数值越小优先级越高。
![[../../annex/STM32的中断_image_2.png]]


### 01 异常类型

![[../../annex/STM32的中断_image_3.png]]

![[../../annex/STM32的中断_image_4.png]]
灰色部分为系统异常。（10 个）

![[../../annex/STM32的中断_image_5.png]]
剩余部分表示外设中断。（60 个）


### NVIC 简介
Nested Vectored Interrupt Controller
内核的一个外设，专门用来管理中断。
![[../../annex/STM32的中断_image_6.png]]

![[../../annex/STM32的中断_image_7.png]]

![[../../annex/STM32的中断_image_8.png]]

外设的定义在 stm32f10x.h
内核的定义在 core_cm3.h

![[../../annex/STM32的中断_image_9.png]]
NVIC 寄存器的结构体定义

![[../../annex/STM32的中断_image_10.png]]
最常用的是中断优先级寄存器。
每个寄存器里面都定义了一个数组，数组里面成员很多，实际上是用不到这么多成员的。
例如：
中断优先级寄存器。
我们知道内核里面有 10 个，外设有 60 个。一共只能 70 个需要设置中断优先级。
但是这里设置了 240 个，预留了很多，为了让芯片厂商 (ST) ，在往后设计功能更加强大的 MCU 的时候，可以只用这些位。

注意：设置优先级的寄存器是 8 位有效位。(ARM 规定的)

实际上，经过 ST 设计之后，这个内核 (NVIC) 都会裁剪，比如某些位寄存器还有很多，用不了这么多，就会进行裁剪。经过裁剪后，还需要编写相应的固件库函数。
编写的固件库函数不是放在 core_cm3.h，而是放在 FWLIB 中的 misc.h 这里。
![[../../annex/STM32的中断_image_11.png]]
有所有 NVIC 所有已经实现的固件库函数。

遇到跟内核相关的函数，在 core_cm3.h 和 misc.h 里面找。
Systick 系统定时器和 NVIC 嵌套向量中断控制器都可以在这里面找到相应的寄存器定义、相应的固件库函数实现。

### 优先级的定义
重点讲中断的优先级定义
![[../../annex/STM32的中断_image_12.png]]

具体的描述：
![[../../annex/STM32的中断_image_13.png]]


对一个中断定义中断优先级的时候，宽度是 8 个位。(ARM 公司设计的)。
但是，ST 针对外设进行精简：只用高 4 位，低 4 位不使用。
在使用高 4 位的时候还进行了一个设计：在高 4 位当中分成了主优先级（抢占优先级）和子优先级。
例如：A 和 B 两个外设，A 和 B 的中断同时到来，内核怎么响应？
首先比较主优先级，如果主优先级相同的话，再比较子优先级。优先级越高（数值越小）会先执行。如果主优先级和子优先级都相同，就比较他们的硬件中断编号。
![[../../annex/STM32的中断_image_14.png]]

硬件中断编号在中断向量表里面看
![[../../annex/STM32的中断_image_15.png]]
![[../../annex/STM32的中断_image_16.png]]

优先级的分组也是内核的外设
![[../../annex/STM32的中断_image_17.png]]
如果三个位全部有效，2<sup>3</sup>=8 组。
但是 ST 只用了 4 个位，4 个位只能分为 5 组

弹幕：注意这里是在分组，而老师这里的意思是只有5个组而不是最多只能分5组<--我认为是错的，最多就只能分为 5 个组

如果配置成组 0 的话，主优先级就 0 位（用来设置主优先级一个位都没有）。子优先级有 4 个位（高 4 位都用来设置子优先级=16 种数值）
以此类推。
![[../../annex/STM32的中断_image_18.png]]

在 misc.h 里面有相应的增值表
![[../../annex/STM32的中断_image_19.png]]

这些优先级分组配置的时候也是只会写组0、1、2、3、4。
这些组对应 32 位的数值。
![[../../annex/STM32的中断_image_20.png]]
这些数值当我们初始化的时候，会写到 SCB_AIRCR 寄存器的三个位里面来配置分组
![[../../annex/STM32的中断_image_21.png]]
配置分组的时候就可以配置中断优先级了。

弹幕：这里建议看火哥的 PDF 教程讲的很清楚

### 中断编程

如果软件配置的优先级的一样的话就比较中断向量表里面的硬件编号，并且可以实现中断嵌套。

编程就要进行初始化，初始化总结成四个步骤。必不可少
![[../../annex/STM32的中断_image_22.png]]


使能中断请求是外设的中断。外设应该是配置外设相应的寄存器（小门）
还有一个是 NVIC（总开关），总开关没有打开（不接受外设的中断请求），那么中断也是不能够响应的。
大门就是 NVIC 的中断使能寄存器来配置↓
![[../../annex/STM32的中断_image_23.png]]

外设使能了相关的中断请求之后，外设（中断）很多，就涉及到了中断优先级
涉及中断优先级的时候就先配置中断优先级的分组。
![[../../annex/STM32的中断_image_24.png]]
调用哪个固件库函数？

在 misc.h 文件中
![[../../annex/tmp1700213416902_STM32的中断_image_25.png]]
![[../../annex/STM32的中断_image_25.png]]

配置好优先级分组后，初始化 NVIC 结构体。
![[../../annex/STM32的中断_image_26.png]]
结构体在 misc.h 中定义：
![[../../annex/STM32的中断_image_27.png]]
第一个成员：指定中断源--内核响应什么中断？如串口的发送中断、接收中断
第二个成员：主优先级（抢占优先级）--根据分组来配置
第三个成员：子优先级
第四个成员：使能（小门就是具体的外设进行请求中断，里面的大门（小门请求完后内核的 NVIC 响不响应就是使能）。配置成 ENABLE or DISABLE）


在 stm32f10x.h 文件中穷举了所有类型的 IRQn_Type 数据类型的定义。
![[../../annex/STM32的中断_image_28.png]]
找到对应的 Flash 类型的定义（HD）。配置 channel 的时候，值只能从这里取。
先选串口中断后，再中断来临的时候，来判断相应的标志位（发送还是接收），再做相应的具体的中断处理。
![[../../annex/STM32的中断_image_29.png]]
（找中断源在 IRQn_Type 结构体里面找）

配置完结构体成员后，会调用另一个初始化函数：NVICInit
![[../../annex/STM32的中断_image_30.png]]
这个 Init 就会把刚刚结构体里面的成员全部写到相应的寄存器里面（这是固件库来完成的）
![[../../annex/STM32的中断_image_31.png]]

中断初始化完成了，最后要编写中断服务函数（名称一定要跟启动文件中的一致）


![[../../annex/STM32的中断_image_32.png]]

启动文件有个作用就是初始化中断向量表。
中断向量表里面的内容是什么？
![[../../annex/STM32的中断_image_33.png]]
这个表里面的内容，每一个中断类型都对应一个函数名称。

在启动文件里面已经把函数名称全部初始化好了。（在启动文件的时候，中断向量表已经规定了全部中断函数的地址（函数名就是地址））
并且对于每个函数都有一个关键字（WEAK，弱定义（可以在其他文件里面定义并优先执行，如果其他文件里面写错了，就会执行此处的中断服务函数）相当于低保）
在这里面，所有的中断服务函数都是空的。函数只有一句话：B .
所以一定要名称一定要跟启动文件里面的名称一致。
![[../../annex/STM32的中断_image_34.png]]
中断服务函数写错了是不会报错的。

有很多的（外设）中断，为了方便管理，把所有的中断服务函数放到 stm32f10x.it.c。It 就是 interal
![[../../annex/STM32的中断_image_35.png]]
随便挑个地方写都是可以的。



###### 总结
总的来说先 NVIC 分组再调用外设中断（固件库中有 IT 的函数） 最后调用 NVIC_Init（）函数配置 NVIC 结构体配置这个外设的中断请求以及优先级















