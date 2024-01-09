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

外设的定义在 stm 32 f 10 x. H
内核的定义在 core_cm 3. H

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
编写的固件库函数不是放在 core_cm 3. H，而是放在 FWLIB 中的 misc. H 这里。
![[../../annex/STM32的中断_image_11.png]]
有所有 NVIC 所有已经实现的固件库函数。

遇到跟内核相关的函数，在 core_cm 3. H 和 misc. H 里面找。
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

弹幕：注意这里是在分组，而老师这里的意思是只有 5 个组而不是最多只能分 5 组<--我认为是错的，最多就只能分为 5 个组

如果配置成组 0 的话，主优先级就 0 位（用来设置主优先级一个位都没有）。子优先级有 4 个位（高 4 位都用来设置子优先级=16 种数值）
以此类推。
![[../../annex/STM32的中断_image_18.png]]

在 misc. H 里面有相应的增值表
![[../../annex/STM32的中断_image_19.png]]

这些优先级分组配置的时候也是只会写组 0、1、2、3、4。
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

在 misc. H 文件中
![[../../annex/STM32的中断_image_25.png]]
![[../../annex/STM32的中断_image_25.png]]

配置好优先级分组后，初始化 NVIC 结构体。
![[../../annex/STM32的中断_image_26.png]]
结构体在 misc. H 中定义：
![[../../annex/STM32的中断_image_27.png]]
第一个成员：指定中断源--内核响应什么中断？如串口的发送中断、接收中断
第二个成员：主优先级（抢占优先级）--根据分组来配置
第三个成员：子优先级
第四个成员：使能（小门就是具体的外设进行请求中断，里面的大门（小门请求完后内核的 NVIC 响不响应就是使能）。配置成 ENABLE or DISABLE）


在 stm 32 f 10 x. H 文件中穷举了所有类型的 IRQn_Type 数据类型的定义。
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

有很多的（外设）中断，为了方便管理，把所有的中断服务函数放到 stm 32 f 10 x. It. C。It 就是 interal
![[../../annex/STM32的中断_image_35.png]]
随便挑个地方写都是可以的。



###### 总结
总的来说先 NVIC 分组再调用外设中断（固件库中有 IT 的函数） 最后调用 NVIC_Init（）函数配置 NVIC 结构体配置这个外设的中断请求以及优先级


## P 28 EXTI 功能框图、初始化结构体讲解

STM 32 每个 GPIO 都可以产生中断，产生中断体现在 GPIO 上是电平的变化
电平的变化由外设来管理，再传给 NVIC（内核中的嵌套向量中断控制器）来处理中断
管理 GPIO 变化来产生中断的外设是 EXTI

![[../../annex/STM32的中断_image_36.png]]

### EXTI 简介
EXTI：External interrupt / event controller
外部中断/事件控制器
外部：GPIO
电平的变化有：上升沿、下降沿。这两种信号由 EXTI 外设来产生一个中断, 然后交给内核 NVIC

弹幕：NVIC 管理所有的中断,EXTI 是一个中断类型,没问题
![[../../annex/STM32的中断_image_37.png]]


### EXTI 功能框图讲解

![[../../annex/STM32的中断_image_38.png]]

![[../../annex/STM32的中断_image_39.png]]

###### 输入线

![[../../annex/STM32的中断_image_40.png]]
输入线总共有 20 根，每一根可以对应很多个。
通过外部中断配置寄存器来选择（AFIO_EXTICR 1、2、3、4）

![[../../annex/STM32的中断_image_41.png]]

EXTI 可以管理外部的 20 根中断的线。
![[../../annex/STM32的中断_image_42.png]]

0~15 配置 PXx
EXTI 0 对应 GPIO 端口的 0： PX 0（X=A、B、C、D、E、F、G、H、I）
以此类推
16~19 是特殊的配置
EXTI 19 是互联型的（105、107 才有，基础型的 103 是没有的）

![[../../annex/STM32的中断_image_43.png]]

![[../../annex/STM32的中断_image_44.png]]

###### 外部中断配置寄存器来选择（AFIO_EXTICR 1、2、3、4）
![[../../annex/STM32的中断_image_45.png]]
通过外部中断配置寄存器来选择（AFIO_EXTICR 1、2、3、4）
例如外部中断配置寄存器 1 是低 16 位有效，高 16 位无效。

比如 EXTI 0 这个中断输入线 0 配置成 PA 0 还是 PB 0 由位 15:0配置
有 4 个位，2<sup>4</sup>=16 种配置，与 GPIO 端口有 16 个是对应的。
![[../../annex/STM32的中断_image_46.png]]
![[../../annex/STM32的中断_image_47.png]]
此处有 7 种配置。

![[../../annex/STM32的中断_image_48.png]]
弹幕：所以是十六组但是说错了不是 GPIO 没列完
弹幕：对的对的是每个 GPIO 一个引脚组成一组 EXTI
弹幕：16个引脚不是十六组 IO
弹幕：不是，四位是让你对应选择 A,B...，那几个段口的，对应的不是位口

###### 边缘检测电路
边缘检测电路由上升沿触发选择寄存器（EXTI_RTSR）和下降沿选择寄存器 （EXTI_FTSR）来控制
![[../../annex/STM32的中断_image_49.png]]
有 20 根线，每一根线都可以配置成上升沿/下降沿。
###### 上升沿触发选择寄存器 EXTI_RTSR
如果想配置成上升沿就往相应的位写 1，复位值都是 0，表示禁止上升沿
![[../../annex/STM32的中断_image_50.png]]
###### 下降沿选择寄存器  EXTI_FTSR
同样, 想配置成下降沿就往相应的位写 1 ，默认都是 0
![[../../annex/STM32的中断_image_51.png]]

PA 0 默认是低电平，按键按了之后，就被拉高了，就应该是上升沿。那么就应该往上升沿触发选择寄存器里面写 1。写 1 之后外部的机械按键执行动作之后，边缘检测电路就会检测到这个上升沿，然后输出 1。
![[../../annex/STM32的中断_image_52.png]]
如果配置成下降沿，那么按键按下，PA 0 被拉高，上升沿不检测。按键一松开，PA 0 被外部的电阻拉低了。那么下降沿就刚好跟我们的寄存器配置一样，这个时候边缘检测电路也会输出 1。
上升沿捕捉的就是按键按下的这个脉冲，
下降沿捕捉的就是按键弹开的这个脉冲。

###### 软件中断事件寄存器 EXTI_SWIER
随后是或门，或门上面有一个软件控制（软件中断事件寄存器->EXTI_SWIER）
![[../../annex/STM32的中断_image_53.png]]
默认值也是 0，而且每根线都对应了里面的一个位。（20 根线。刚好有 20 个位有效，其他位保留，是无效的。）
![[../../annex/STM32的中断_image_54.png]]

######  挂起寄存器 EXTI_PR
EXTI_SWIER 寄存器置 1 的时候，设置 EXTI_PR（挂起寄存器）中相应的挂起位。表示有中断来了（捕捉到了）
![[../../annex/STM32的中断_image_55.png]]
但是先挂起（先标志一下）, 具体响不响应中断再说，还有一个总开关来控制。


![[../../annex/STM32的中断_image_56.png]]

然后两个 1 ，或为真。输出一个 1。
![[../../annex/STM32的中断_image_57.png]]

接着有两条路，往上走就是中断，往下走就是事件。
![[../../annex/STM32的中断_image_58.png]]



##### 往上，中断
![[../../annex/STM32的中断_image_59.png]]

软件中断事件寄存器写 1 的话，请求挂起寄存器就相当于表示已经捕捉到了外部的请求信号。

###### 中断屏蔽寄存器 EXTI_IMR
来到一个与门（两个为 1 才为 1），由中断屏蔽寄存器来控制。
![[../../annex/STM32的中断_image_60.png]]
中断屏蔽寄存器（EXTI_IMR），总共也有 20 个位，写 1 的话相应的中断线就会被使能。这个相当于大开关。
![[../../annex/STM32的中断_image_61.png]]

输出 1，然后送到 NVIC 中断控制器（由内核来相应这个中断，内核就会去查找相应的中断服务函数 ESR）
![[../../annex/STM32的中断_image_62.png]]

PA 0 变化电平从输入线进来，可以通过软件设置成上升沿中断和下降沿中断，在 ESR 中断服务程序这里可以做自己的事情。如：让一个 LED 灯点亮/熄灭。通知其他外设报警等...


##### 往下，事件

往下走就产生脉冲。这个脉冲是内部的。
同样，也是来到一个与门（两个 1 才为 1）

###### 事件屏蔽寄存器 EXTI_EMR
![[../../annex/STM32的中断_image_63.png]]
这个 1 是由事件屏蔽寄存器（ EXTI_EMR ）来控制的。同样有 20 个位，如果写 1 的话就是使能。

###### 脉冲发生器
使能之后，就会来到脉冲发生器。如果是 1 的话，就会产生一个脉冲。脉冲的话，相当于是一个方波。
![[../../annex/STM32的中断_image_64.png]]
脉冲有什么作用？
这个脉冲是在单片机内部的一个脉冲。这个脉冲可以触发 ADC，开始转换。
![[../../annex/STM32的中断_image_65.png]]
比如说写了一个程序：ADC 采集，通过外部 (GPIO) 的电平的变化来告诉他。--按下按键，相当于一个开关，ADC 开始采集。那么就是通过这个事件来触发。
还可以触发定时器开始（什么时候开始计时）
这个是事件的作用。

#### 固件库编程

##### 初始化结构体
![[../../annex/STM32的中断_image_66.png]]

![[../../annex/STM32的中断_image_67.png]]

```
typedef struct
{
  uint32_t EXTI_Line;               /*!< Specifies the EXTI lines to be enabled or disabled.
                                         This parameter can be any combination of @ref EXTI_Lines */
   
  EXTIMode_TypeDef EXTI_Mode;       /*!< Specifies the mode for the EXTI lines.
                                         This parameter can be a value of @ref EXTIMode_TypeDef */

  EXTITrigger_TypeDef EXTI_Trigger; /*!< Specifies the trigger signal active edge for the EXTI lines.
                                         This parameter can be a value of @ref EXTIMode_TypeDef */

  FunctionalState EXTI_LineCmd;     /*!< Specifies the new state of the selected EXTI lines.
                                         This parameter can be set either to ENABLE or DISABLE */ 
}EXTI_InitTypeDef;

```

###### 成员 1： EXTI_Line
EXTI_Line ：用于产生中断/事件线。成员有 20 个（0~19）
对应功能框图中的输入线
![[../../annex/STM32的中断_image_68.png]]

![[../../annex/STM32的中断_image_69.png]]

但是具体配置哪一根应该是外部中断配置寄存器（AFIO_EXTICR 1）才对
![[../../annex/STM32的中断_image_70.png]]

那么 EXIT_Line 有什么用呢？
这个 Line 配置是为了等一下编程的时候，是编程这些寄存器的哪些位。
选择 Line 0 的话，初始化寄存器的时候，就会初始化寄存器位 0，Line 1 初始化位 1。
所以跟真正的输入线是没有关系的。
![[../../annex/STM32的中断_image_71.png]]


###### 成员 2：EXTI_Mode
EXTI 有两种模式：Interrupt（中断）、Event（事件）
![[../../annex/STM32的中断_image_72.png]]

###### 成员 3：EXTI_Trigger
EXTI_Trigger：触发
触发可以有上升沿和下降沿，或者上升沿和下降沿都捕获
![[../../annex/STM32的中断_image_73.png]]


###### 成员 4：EXTI_LineCmd
EXTI_LineCmd，对应中断屏蔽寄存器（EXTI_IMR）或事件屏蔽寄存器（EXTI_EMR）
![[../../annex/STM32的中断_image_74.png]]
![[../../annex/STM32的中断_image_75.png]]


Line 是选择寄存器的某一些位，来确定具体操作的是哪些位
![[../../annex/STM32的中断_image_76.png]]


弹幕：第四条描述是 ENABLE 或者 DISABLE。应该是使能了。
弹幕：EXIT_LineCmd 功能：控制是否使能 EXIT 线，使能 ENABLE，禁用 DISABLE
弹幕：第一个选线，第四个再确定这根线使能不使能？


输入线要选 PA 0 还是 PB 0，应该操作的是外部中断配置寄存器 AFIO_EXTICRx 


void GPIO_EXTILineConfig(uint8_t GPIO_PortSource, uint8_t GPIO_PinSource);
有两个形参。
uint8_t GPIO_PortSource：先确定 EXTILine 连接 GPIO 的端口，
uint8_t GPIO_PinSource：然后再看一下 GPIO 具体是哪一个。
![[../../annex/STM32的中断_image_77.png]]
uint8_t GPIO_PortSource 配置端口（A~G）
uint8_t GPIO_PinSource   配置引脚（0~15）



### GPIO 中断实验讲解

## P 29 EXTI--GPIO 外部中断代码讲解

寄存器的映射是在 stm32f10x.h 文件里面

![[../../annex/STM32的中断_image_78.png]]
1-初始化要连接到 EXTI 的 GPIO
2-初始化 EXTI 用于产生中断/事件
3-初始化 NVIC，用于处理中断
4-编写中断服务函数
5-main 函数


![[../../annex/STM32的中断_image_79.png]]

配置成浮空输入，浮空输入的意思就是说这个 GPIO 的默认电平由外部决定

![[../../annex/STM32的中断_image_80.png]]

弹幕：只有使用了 AFIO 的事件控制寄存器、AFIO 的重映射功能以及外部中断(EXTI)控制寄存器才需要开启 AFIO 的时钟。
弹幕：这个一定不要漏，我就是漏了，断点调试好久才发现 AFIO 未使能（打开 AFIO 的时钟）
弹幕：打开终端看一下，我都没见 exti 时钟，咋使能

在 stm32f10x.h 文件中
EXTIx_IRQn（0~4 ） 
如果是 PA 5 等，在 STM32F10X_MD, 是共用一个函数名的 (应该是总共有七个中断函数名)
5~9 的表达：EXTI 9_5_IRQn，
10-15 的表达：EXTI 15_10_IRQn
例如：直接改成 EXTI 7_IRQn 是错误的
![[../../annex/STM32的中断_image_81.png]]
两个中断，先比较主（抢占）优先级，其次再比较子优先级也就是响应优先级，如果相同，比较其在中断向量表中的排列顺序

所有中断函数的地址--向量表
地址确定，中断服务函数的名称也就确定。
所以编写中断服务函数的时候回到启动文件 startup_stm32f10x_hd.x 中去找。


弹幕：原子的板子你得根据实际情况来更改一些细节，原子的 key0是 PE4，key1是 PE3，key_up 是 PA0
而且硬件方面原子的没有做外部上下拉电路，所以需要把 key 的 mode 设置成内部上下拉，不能按火哥的设置成浮空
我用的是正点原子 f103精英板，原子哥和火哥的视频结合起来看，火哥的讲的细，原子的就是讲实现这个功能的模板

###### 代码

```main.c
#include "stm32f10x.h"  //相当于51单片机中的    #include <reg51.h> 

#include "bsp_led.h"

#include "bsp_exti.h"

void Delay(uint32_t count)	//写延时函数，让灯闪烁
{
	for(;count!=0;count--);
}	


int main(void)
{
    //  来到这里的时候,系统的时钟已经被配置成72M。不需要自己配置，启动文件中已经配置好了。
	LED_GPIO_Config();//亮灯
	EXIT_Key_Config();//初始化按键
	
	while(1)
	{
		
	}

}

//1-把KEY2-PC13 也用 EXTI 的方式实现
//2-如果PA0 PB0 PC0 都连接到 EXTI0 的时候怎么办？
// 端口 0 只能连接到 EXTI0，此时怎么区分。
//以按键为例，当进入中断服务函数的时候，怎么判断是PB0还是PC0？
//此时必须手动判断，我们知道，GPIO可以检测外部的电平，外部是通过什么来检测的？
//会体现在寄存器上，通过功能框图来看，有一个叫输入数据寄存器（IDR），外部的电平会在IDR中实时的反映出来，可以判断具体哪一个位。
//GPIOA->IDR	GPIOB->IDR	可以看看这些IDR的最低位是否为1

//3-把硬件相关的GPIO都通过宏定义来实现


```


```bsp_exti.c
#include "bsp_exti.h"

static void EXTI_NVIC_Config(void)//static，说明这个函数只能被bsp_exti.c的文件中调用，其他文件调用不了
{
	NVIC_InitTypeDef NVIC_InitStruct;
	
	NVIC_PriorityGroupConfig(NVIC_PriorityGroup_1);//配置中断优先级分组
	
	//配置好中断优先级分组后，再配置NVIC初始化结构体中的每一个成员
	NVIC_InitStruct.NVIC_IRQChannel =  EXTI0_IRQn;
	NVIC_InitStruct.NVIC_IRQChannelPreemptionPriority = 1;
	NVIC_InitStruct.NVIC_IRQChannelSubPriority = 1;
	NVIC_InitStruct.NVIC_IRQChannelCmd = ENABLE;//使能，开关
	
	NVIC_Init(&NVIC_InitStruct);
}

void EXIT_Key_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStruct;
	EXTI_InitTypeDef EXTI_InitStruct;//注意：变量一定要放在函数体的开头//定义一个初始化的结构体
	
	//	配置中断优先级（写一个函数，通过调用子函数来实现）
	 EXTI_NVIC_Config();
	
	//	1-初始化GPIO --开时钟、取引脚、定模式、设速度 --输入:开时钟，设io，选模式，无速度，初始寄存器
	RCC_APB2PeriphClockCmd(KEY_INT_GPIO_CLK , ENABLE);
	
	GPIO_InitStruct.GPIO_Pin = KEY_INT_GPIO_PIN;
	GPIO_InitStruct.GPIO_Mode = GPIO_Mode_IN_FLOATING;//配置成浮空输入，浮空输入的意思就是说这个GPIO的默认电平由外部决定
	//GPIO_InitStruct.GPIO_Speed = GPIO_Speed_50MHz;//输入的时候没有速度之分，可以删掉
	
	GPIO_Init(KEY_INT_GPIO_PORT, &GPIO_InitStruct);
	
	//	2-初始化EXTI
	RCC_APB2PeriphClockCmd(RCC_APB2Periph_AFIO , ENABLE);//调用哪个函数？打开时钟有3个总线（EXTI在APB2上），再选形参为AFIO
	GPIO_EXTILineConfig(GPIO_PortSourceGPIOA, GPIO_PinSource0);//首先先选择输入线，输入线由固件库函数来配置，操作的是AFIO寄存器。
	//默认情况下，STM32的IO时钟是关闭的，所以要打开时钟：17行
	
	EXTI_InitStruct.EXTI_Line = EXTI_Line0;
	EXTI_InitStruct.EXTI_Mode = EXTI_Mode_Interrupt;//中断还是事件
	EXTI_InitStruct.EXTI_Trigger = EXTI_Trigger_Rising;//电平是上升沿还是下降沿产生中断
	EXTI_InitStruct.EXTI_LineCmd = ENABLE;//中断屏蔽事件寄存器--打开响应中断
	
	EXTI_Init(&EXTI_InitStruct);//传的是指针，我们定义的要&升级
	
	
}


//1、初始化要连接到 EXTI 的 GPIO
//20-21定义结构体
//29-31初始化GPIO结构体成员
//33调用GPIO固件库函数

//2、初始化 EXTI 用于产生中断/事件
//36-37打开时钟、选线
//40-43初始化EXTI结构体
//45-调用EXTI_Init固件库函数，把结构体成员写到相应的寄存器中，这样才能起作用

//3、初始化 NVIC，用于处理中断
//5-定义NVIC结构体
//7-配置中断优先级分组
//9-13配置NVIC的结构体成员
//15-调用NVIC初始化函数

//4、编写中断服务函数
//写在stm32f10x_it.c

```

```bsp_exti.h
#ifndef _BSP_EXTI_H
#define _BSP_EXTI_H

#include "stm32f10x.h"

#define KEY_INT_GPIO_PIN					GPIO_Pin_0	
#define KEY_INT_GPIO_PORT					GPIOA
#define KEY_INT_GPIO_CLK					RCC_APB2Periph_GPIOA

void EXIT_Key_Config(void);

#endif /*_BSP_EXTI_H*/


```


```stm32f10x_it.c
void SysTick_Handler(void)
{
}

void EXTI0_IRQHandler (void)
{
	if(EXTI_GetITStatus(EXTI_Line0) != RESET)
	{
		LED_G_TOGGLE;
	}
	EXTI_ClearITPendingBit(EXTI_Line0);//为了防止一直在中断里面，清除一下中断位 //清除的是挂起寄存器的中断请求标志
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

#define				LED_G_TOGGLE				{LED_G_GPIO_PORT->ODR ^= GPIO_Pin_0;}

void LED_GPIO_Config(void);

#endif /*_BSP_LED_H*/


```





































































