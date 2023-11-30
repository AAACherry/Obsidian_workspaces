
## P 44 I2C 物理层介绍

I2C 协议就是 EEPROM 和 STM 32 互相之间传输数据使用的一种协议。

![[../../annex/I2C--读写EEPROM_image_1.png]]

![[../../annex/I2C--读写EEPROM_image_2.png]]
引脚少，只需要 2 根。不需要加外部的芯片转换电路。
模块与模块之间、传感器之间的通讯，很多都是用 IIC 的。

![[../../annex/I2C--读写EEPROM_image_3.png]]

![[../../annex/I2C--读写EEPROM_image_4.png]]
有电阻特性（要求连接 n 个设备之后，整个电路的负载不能超过 x）

SCL 总线：serial clock--串行时钟线（采用同步通讯的方式）。作为输出时钟线。比如 STM 32 控制 SCL 线输出一个时钟来协调通讯，从机通过接收时钟来进行数据采集/发送。
SDA 总线：也是一个串行通讯，数据是一位一位传输出去的。数据传输方向可以从主机到从机，也可以从从机到主机。线的数据直接用电压来表示数据逻辑（3.3 V 逻辑 1；0 V 逻辑 0）。数据怎么传，通过 SDA 线。
通过（地址）寻址机制，让主机跟特定的设备进行通讯。

接了上拉电阻，（上面接了电源2-15 V）。作用：可以实现高阻态。线与的特性。
线与就是结点进行与逻辑操作。
设备空闲，输出高阻态就相当于接了 MOS 管，输出一个很高的电阻，相当于这根线断开了。（GPIO 中的推挽/开漏）

弹幕：从机想输出1的话只能对外呈现高阻态，由上拉电阻将总线拉高
弹幕：当工作从设备输出1时，若空闲从设备为0时会产生短路，当工作从设备输出为0时，若空闲从设备为3.3v 时仍然会短路，因此采用高阻态
弹幕：能用不用的状态，叫做高阻态
弹幕：cpu 检测的是总线的高低电平，将总线上拉后，输出0就接地总线就为0，输出1就高阻态，外设不分电流总线就是高电平
弹幕：（接上条）所以说从机以高阻态表示高电平。（工作的从机和其他从机 MCU 会区别）

![[../../annex/I2C--读写EEPROM_image_5.png]]



## P 45 I2C 协议层介绍

主机写数据到从机方向：
![[../../annex/I2C--读写EEPROM_image_6.png]]

通过 SDA 线一位一位的传输数据。抽象出来数据包形式。

弹幕：启动信号：上课了从机地址：班长读写位：作业你记一下      A：好的老师你说          data：第一项练习册10-20页        A：记下了

主机每传完一个字节，都会等待从机的一个响应信号


主机从从机中读数据方向：
![[../../annex/I2C--读写EEPROM_image_7.png]]


复合格式：
![[../../annex/I2C--读写EEPROM_image_8.png]]
产生原因：数据读取过程需要一个寄存器地址或者设备地址

弹幕：设备地址相当于一个大楼的地址写如数据的地址相当于一个大楼里面的一个房间号

弹幕：总结：第一次通讯是告诉寄存器，哪个位
弹幕：第二次通讯：从机告诉主机寄存器里面的数据


![[../../annex/I2C--读写EEPROM_image_9.png]]


![[../../annex/I2C--读写EEPROM_image_10.png]]

![[../../annex/I2C--读写EEPROM_image_11.png]]

弹幕：7数据位和一个读写位
弹幕：地址一般是8位，现在是7位，少一位怎么办

![[../../annex/I2C--读写EEPROM_image_12.png]]

弹幕：(0x78<<1)+1/0
弹幕：读：0xF1，写：0xF0
弹幕：也可以直接先把地址写成8位的，再加不加1就完了

![[../../annex/I2C--读写EEPROM_image_13.png]]




![[../../annex/I2C--读写EEPROM_image_14.png]]


## P 46  I2C 功能框图讲解

![[../../annex/I2C--读写EEPROM_image_15.png]]

![[../../annex/I2C--读写EEPROM_image_16.png]]

![[../../annex/I2C--读写EEPROM_image_17.png]]

![[../../annex/I2C--读写EEPROM_image_18.png]]

![[../../annex/I2C--读写EEPROM_image_19.png]]

###### 时钟控制逻辑
![[../../annex/I2C--读写EEPROM_image_20.png]]

![[../../annex/I2C--读写EEPROM_image_21.png]]

PCK1 = APB1 的时钟
![[../../annex/I2C--读写EEPROM_image_22.png]]

![[../../annex/I2C--读写EEPROM_image_23.png]]

![[../../annex/I2C--读写EEPROM_image_24.png]]

###### 数据控制逻辑

![[../../annex/I2C--读写EEPROM_image_25.png]]


![[../../annex/I2C--读写EEPROM_image_26.png]]

![[../../annex/I2C--读写EEPROM_image_27.png]]
IIC 是高位先行的。
数据寄存器作为缓存数据，方便 STM 32 存储或接收
数据移位寄存器负责移位而存在的



###### 整体控制逻辑

![[../../annex/I2C--读写EEPROM_image_28.png]]

![[../../annex/I2C--读写EEPROM_image_29.png]]

控制寄存器去控制 IIC 的模式，状态寄存器来了解整个 IIC 的工作状态


## P 47 I2C 的通讯过程

###### 主发送器 （STM 32 作为主机对外发送数据）
写方向
![[../../annex/I2C--读写EEPROM_image_30.png]]

![[../../annex/I2C--读写EEPROM_image_31.png]]

TXE 判断数据寄存器是否为空，如果为空，那么数据寄存器就可以继续存入下一个 DATA（原来的拷贝到了数据移位寄存器中）。

弹幕：数据放进 DR 里，DR 马上复制到移位寄存器。

在发送结束信号之前，需要再检测一下 BTF 位，BTF 位就是字节发送结束的标志位（判断数据移位寄存器是否为空）。数据一位一位的发送出去的时候，可能数据移位寄存器中的数据还没发完，需要 BTF 来进行检测判断

上面是要发送的数据内容，下面是我们的状态寄存器要检测的内容。
每做一个步骤，相应的状态寄存器就会产生一个相应的事件来供我们去检测来了解是否被发送出去。
STM 32 检测这些标志的意义就是等待，放慢发送的速度，等待 IIC 工作完成再接着做后面的事情

![[../../annex/I2C--读写EEPROM_image_32.png]]

![[../../annex/I2C--读写EEPROM_image_33.png]]


###### 主接收器（STM 32 作为主机接收数据）
读方向
![[../../annex/I2C--读写EEPROM_image_34.png]]

![[../../annex/I2C--读写EEPROM_image_35.png]]

IIC 规定：只有主机才能产生通讯（起始条件、时钟等都是由主机来产生发送的）

如果没有检测到信号，可能就是有其他设备在占用总线。可能要等待总线空闲的时候再发送数据。

![[../../annex/I2C--读写EEPROM_image_36.png]]
注意：第一个应答信号是 STM 32 发送设备地址，然后从机响应信号。
第二个应答信号是从机发送数据之后，由 STM 32 产生应答信号的

![[../../annex/I2C--读写EEPROM_image_37.png]]
STM 32 在 ACK 预先配置，在准备接收数据之前先配置 ACK，如果置 1 了，那么当 STM 32 接收到一个字节之后，就会自动发送一个响应信号（应答信号）。
那么从机接收到这个应答信号，它就认为主机还希望接收信号，那么从机就接着发送数据。

![[../../annex/I2C--读写EEPROM_image_38.png]]
EV7_1，作为最后一个字节，再接收一个字节就结束了。

最后的 EV7 事件来知道 DataN 接收到了。


![[../../annex/I2C--读写EEPROM_image_39.png]]
![[../../annex/I2C--读写EEPROM_image_40.png]]
读取完之后要清除。

![[../../annex/I2C--读写EEPROM_image_41.png]]



## P 48 I2C 库函数介绍

![[../../annex/I2C--读写EEPROM_image_42.png]]

#### 初始化结构体
###### I2C_ClockSpeed
![[../../annex/I2C--读写EEPROM_image_43.png]]

###### I2C_Mode
![[../../annex/I2C--读写EEPROM_image_44.png]]

![[../../annex/I2C--读写EEPROM_image_45.png]]
Doxygen 注释标准

###### I2C_DutyCycle
![[../../annex/I2C--读写EEPROM_image_46.png]]

###### I2C_OwnAddress 1
![[../../annex/I2C--读写EEPROM_image_47.png]]

![[../../annex/I2C--读写EEPROM_image_48.png]]
想要配置地址 2 的时候就可以用这个函数（第二个地址只能是 7 位的）

###### I2C_Ack_Enable
![[../../annex/I2C--读写EEPROM_image_49.png]]

###### I2C_AcknowledgeAddress
![[../../annex/I2C--读写EEPROM_image_50.png]]
要协调好所有设备都工作在 7 位地址或者 10 位地址，不能混着使用，否则会出现通讯错误。

![[../../annex/I2C--读写EEPROM_image_51.png]]
STM 32 自己的地址写在 ADD0 到 ADD9，一共 10 位

![[../../annex/I2C--读写EEPROM_image_52.png]]
7 位数据位作为地址

## P 49 EEPROM 简介

![[../../annex/I2C--读写EEPROM_image_53.png]]

![[../../annex/I2C--读写EEPROM_image_54.png]]

![[../../annex/I2C--读写EEPROM_image_55.png]]

![[../../annex/I2C--读写EEPROM_image_56.png]]

![[../../annex/I2C--读写EEPROM_image_57.png]]

A0~A2 Address Inputs，地址输入
SDA Serial Data，串行 Data 线
SCL Serial Clock Input，串行时钟线
WP write protect，写保护
NC no connect，空引脚

![[../../annex/I2C--读写EEPROM_image_58.png]]
WP 接地，不使能保护了


![[../../annex/I2C--读写EEPROM_image_59.png]]

7 位地址：高 4 位是固定好的，A0~A2 可以自己配置。预留出来方便扩展。

![[../../annex/I2C--读写EEPROM_image_60.png]]

![[../../annex/I2C--读写EEPROM_image_61.png]]

电气特性
![[../../annex/I2C--读写EEPROM_image_62.png]]

EEPROM 的时序图
![[../../annex/I2C--读写EEPROM_image_63.png]]

![[../../annex/I2C--读写EEPROM_image_64.png]]

7 位地址：
1010000 <<1 |0x01 才会变成一个 8 位地址

Byte Write
![[../../annex/I2C--读写EEPROM_image_65.png]]

EERPOM 与 IIC 相比，在 DEVICE ADDRESS 之后发送的第一个字节（DATA 的第一个字节）是 WORD ADDRESS（目标地址）

![[../../annex/I2C--读写EEPROM_image_66.png]]
Page Write，解决 Byte Write 的缺陷
![[../../annex/I2C--读写EEPROM_image_67.png]]
EEPROM 接收到时序之后，会自动把第一个数值存储到 WORD ADDRESS 起始地址，第二个数值存储到 WORD ADDRESS+1 地址，以此类推，会依次自己的地址自增。（突发写入，即给一次地址，写入多次数据）。（这种方式必须要连续写入，地址必须要是连续的，每次写入的字节数也有限制（8 个字节））

Current Address Read 当前地址的读取
![[../../annex/I2C--读写EEPROM_image_68.png]]
直接读取当前地址的内容，很少用，很难确认当前的地址是什么

Random Read 随机读取
![[../../annex/I2C--读写EEPROM_image_69.png]]
我们可以指定某一个字节去读取（IIC 的复合格式）
虽然最终的目的是想要读取数据，但是要先进行 WRITE 写操作，
需要主机告诉 EEPROM 现在需要读取的地址是哪里，这个地址其实是一个从机到主机的一个过程，这个过程就相当于是写入方向。将这个地址写入到 EEPROM。
所以 WRITE 必须要设置成 WRITE 方向（从主机到从机的方向）

![[../../annex/I2C--读写EEPROM_image_70.png]]
接着再次产生一个起始信号 START，然后设置读方向 READ

Sequential Read 顺序读取
![[../../annex/I2C--读写EEPROM_image_71.png]]
前面跟 Random Read 相同，但是在发送 DATA 后不是 NO ACK，而是 ACK（应答信号，由主机产生，因为现在是主机在接收数据），一直响应，接着给下一个数据，以此类推，直到主机不需要了，产生一个 NO ACK 信号，EEPROM 才会停止传输。同时由于主机产生了 NO ACK 信号，主机同时会成产生一个 STOP 信号。（起始信号和 STOP 信号都是由主机产生的）
顺序读取没有要求，因为 EEPROM 是 256 个字节，一直读到 256 都可以（0~255）。如果超出 256 个字节，超出最高地址之后会 roll over，读到 255 地址都还没有接收，还没有 NO ACK 的话，就会重新从 0 地址开始把数据返回给你（重新传输数据）。一般不会让他 roll over，防止产生错误内容



## P 50 代码详解-初始化 IIC

![[../../annex/I2C--读写EEPROM_image_72.png]]

![[../../annex/I2C--读写EEPROM_image_73.png]]
勾选自动补全

![[../../annex/I2C--读写EEPROM_image_74.png]]

```
可以通过路径指定方式来
#include "./led/bsp_led.h"
./代表当前目录（User）
```
![[../../annex/I2C--读写EEPROM_image_75.png]]


.o 文件是由同样名字的 C 文件编译之后生成的。

![[../../annex/I2C--读写EEPROM_image_76.png]]

![[../../annex/I2C--读写EEPROM_image_77.png]]
取消注释 Conf 文件中的 include "i2c.h"，否则初始化 I2C_InitTypeDef 会报错

![[../../annex/I2C--读写EEPROM_image_78.png]]
遇到不确定的引脚功能，看手册

EEPROM 的地址是 1010000，只要跟他的 7 位地址不一样的就可以了
![[../../annex/I2C--读写EEPROM_image_79.png]]
![[../../annex/I2C--读写EEPROM_image_80.png]]


###### 代码

```bsp_i2c.c
/**
  ******************************************************************************
  * @file    bsp_usart.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   重定向c库printf函数到usart端口
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火STM32 F103-指南者 开发板  
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */ 
	
#include "bsp_i2c.h"


 /**
  * @brief  I2C EEPROM 配置,工作参数配置
  * @param  无
  * @retval 无
  */
void I2C_EE_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;
	I2C_InitTypeDef	 I2C_InitStructure;

	// 打开串口GPIO的时钟
	EEPROM_I2C_APBxClkCmd(EEPROM_I2C_SCL_GPIO_CLK|EEPROM_I2C_SDA_GPIO_CLK, ENABLE);//利用或|运算来同时初始化
	//加一个或|，相当于把它们都初始化，宏定义的内容都是GPIOB，是一样的。同样一个数字去或运算，是不会改变的
	
	// 打开串口外设的时钟
	EEPROM_I2C_GPIO_APBxClkCmd(EEPROM_I2C_CLK, ENABLE);

	// 将IIC SCL的GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = EEPROM_I2C_SCL_GPIO_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;//需要将IIC总线高阻态，防止短路，要启用复用开漏
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(EEPROM_I2C_SCL_GPIO_PORT, &GPIO_InitStructure);

  // 将IIC SDA的GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = EEPROM_I2C_SDA_GPIO_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;
	GPIO_Init(EEPROM_I2C_SDA_GPIO_PORT, &GPIO_InitStructure);
	//省略了50Mhz等，实际上因为SCL和SDA的结构体成员的值跟前面SCL是一样的，所以删除掉不写也是可以的
	//SCL和SDA的Mode等的值是一样的，只要把不一样的值就进行赋值修改就可以了

	// 配置IIC的工作参数
	I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;//使能应答 
	I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;//使用7位地址模式
	I2C_InitStructure.I2C_ClockSpeed = EEPROM_I2C_BAUDRATE;//配置SCL时钟频率
	I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_2;
	I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;
	I2C_InitStructure.I2C_OwnAddress1 = STM32_I2C_OWN_ADDR;//这是STM32 IIC自身设备地址，只要是总线上唯一即可
	
	I2C_Init(EEPROM_I2C,&I2C_InitStructure);
	
	// 使能串口
	I2C_Cmd(EEPROM_I2C, ENABLE);	    
}


///*****************  发送一个字节 **********************/
//void Usart_SendByte( USART_TypeDef * pUSARTx, uint8_t ch)
//{
//	/* 发送一个字节数据到USART */
//	USART_SendData(pUSARTx,ch);
//		
//	/* 等待发送数据寄存器为空 */
//	while (USART_GetFlagStatus(pUSARTx, USART_FLAG_TXE) == RESET);	
//}

///****************** 发送8位的数组 ************************/
//void Usart_SendArray( USART_TypeDef * pUSARTx, uint8_t *array, uint16_t num)
//{
//  uint8_t i;
//	
//	for(i=0; i<num; i++)
//  {
//	    /* 发送一个字节数据到USART */
//	    Usart_SendByte(pUSARTx,array[i]);	
//  
//  }
//	/* 等待发送完成 */
//	while(USART_GetFlagStatus(pUSARTx,USART_FLAG_TC)==RESET);
//}

///*****************  发送字符串 **********************/
//void Usart_SendString( USART_TypeDef * pUSARTx, char *str)
//{
//	unsigned int k=0;
//  do 
//  {
//      Usart_SendByte( pUSARTx, *(str + k) );
//      k++;
//  } while(*(str + k)!='\0');
//  
//  /* 等待发送完成 */
//  while(USART_GetFlagStatus(pUSARTx,USART_FLAG_TC)==RESET)
//  {}
//}

///*****************  发送一个16位数 **********************/
//void Usart_SendHalfWord( USART_TypeDef * pUSARTx, uint16_t ch)
//{
//	uint8_t temp_h, temp_l;
//	
//	/* 取出高八位 */
//	temp_h = (ch&0XFF00)>>8;
//	/* 取出低八位 */
//	temp_l = ch&0XFF;
//	
//	/* 发送高八位 */
//	USART_SendData(pUSARTx,temp_h);	
//	while (USART_GetFlagStatus(pUSARTx, USART_FLAG_TXE) == RESET);
//	
//	/* 发送低八位 */
//	USART_SendData(pUSARTx,temp_l);	
//	while (USART_GetFlagStatus(pUSARTx, USART_FLAG_TXE) == RESET);	
//}

/////重定向c库函数printf到串口，重定向后可使用printf函数
//int fputc(int ch, FILE *f)
//{
//		/* 发送一个字节数据到串口 */
//		USART_SendData(DEBUG_USARTx, (uint8_t) ch);
//		
//		/* 等待发送完毕 */
//		while (USART_GetFlagStatus(DEBUG_USARTx, USART_FLAG_TXE) == RESET);		
//	
//		return (ch);
//}

/////重定向c库函数scanf到串口，重写向后可使用scanf、getchar等函数
//int fgetc(FILE *f)
//{
//		/* 等待串口输入数据 */
//		while (USART_GetFlagStatus(DEBUG_USARTx, USART_FLAG_RXNE) == RESET);

//		return (int)USART_ReceiveData(DEBUG_USARTx);
//}


```

```bsp_i2c.h
#ifndef __BSP_I2C_H
#define	__BSP_I2C_H


#include "stm32f10x.h"

/** 
* IIC EEPROM 的引脚定义

  */

#define STM32_I2C_OWN_ADDR							0x5f

// IIC1
#define  EEPROM_I2C	               	    I2C1
#define  EEPROM_I2C_CLK                	RCC_APB1Periph_I2C1
#define  EEPROM_I2C_APBxClkCmd          RCC_APB1PeriphClockCmd
#define  EEPROM_I2C_BAUDRATE            400000

// IIC GPIO 引脚宏定义
#define  EEPROM_I2C_SCL_GPIO_CLK       (RCC_APB2Periph_GPIOB)
#define  EEPROM_I2C_SDA_GPIO_CLK       (RCC_APB2Periph_GPIOB)

#define  EEPROM_I2C_GPIO_APBxClkCmd     RCC_APB2PeriphClockCmd
    
#define  EEPROM_I2C_SCL_GPIO_PORT       GPIOB
#define  EEPROM_I2C_SCL_GPIO_PIN        GPIO_Pin_6

#define  EEPROM_I2C_SDA_GPIO_PORT       GPIOB
#define  EEPROM_I2C_SDA_GPIO_PIN        GPIO_Pin_7


#endif /* __BSP_I2C_H */

```

```main.c
/**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   串口中断接收测试
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */ 
 
 
#include "stm32f10x.h"
#include "bsp_usart.h"

//1.初始化IIC相关的GPIO
//2.配置IIC外设的工作模式
//3.编写IIC写入EEPROM的Byte Write函数
//4.编写IIC读取EEPROM的Random Read函数
//5.使用Read函数及Write函数进行读写校验

//6.编写page write及seq read函数并校验

/**
  * @brief  主函数
  * @param  无
  * @retval 无
  */
int main(void)
{	
  /*初始化USART 配置模式为 115200 8-N-1，中断接收*/
  USART_Config();
	
	/* 发送一个字符串 */
	printf("这是一个IIC通讯实验\n");
	
  while(1)
	{	
		
	}	
}
/*********************************************END OF FILE**********************/

```





## P 51 代码详解-读写 EEPROM-上

![[../../annex/I2C--读写EEPROM_image_81.png]]
EEPROM 的设备地址（是固定的，为 1010000）、EEPROM 内部存储单元地址、DATA （数字）
后两个作为函数的参数，由于 EEPROM 是 256 个字节，所以地址最大是 255，直接使用 uint 8_t。
如果 STM 32 芯片用的 4 K、8 K、16 K 的，这些芯片可能的地址是超过 256 个的，可能要使用 uint 16_t 来表示。

![[../../annex/I2C--读写EEPROM_image_82.png]]

Random Read 和 Sequential Read 本身区别不大，直接写成一个函数。
先写 Random Read 再改成 Sequential Read

![[../../annex/I2C--读写EEPROM_image_83.png]]

![[../../annex/I2C--读写EEPROM_image_84.png]]
Debug，排查错误
![[../../annex/I2C--读写EEPROM_image_85.png]]
Debug->复位->设置断点->全速运行。
就会跑到断点来，断点处的蓝色箭头代表光标所在的地方，断点处的黄色箭头代表刚刚全速跑到的位置
全速运行旁边的红圈叉就是暂停键（跳到错误的地方）

![[../../annex/I2C--读写EEPROM_image_86.png]]
删除所有断点
为什么调试的时候正常，跑程序的时候不正常？

一个断点一个断点跑的时候是正常执行的，但是全速运行的时候有问题，是什么导致的？

EEPROM 正在产生一个内部时序，正在把数据写入它的存储单元，把刚刚发送给它的 0x55 写到它的 11 地址，所以内部时序它是不会响应我们的 Read 的起始信号的。
卡死在 I2C_CheckEvent。I2C_Send7bitAddress
因为没有响应，STM 32 认为一直 ERROR，卡死在这里，一直在等。但是当 EEPROM 写完数据了之后，可能可以产生一个应答信号，但是 STM 32 已经卡死在 I2C_CheckEvent 了，没有重新去发起
I2C_GenerateSTART 信号，所以就一直卡死。（EEPROM 没有接收到新的起始信号）。

![[../../annex/I2C--读写EEPROM_image_87.png]]

![[../../annex/I2C--读写EEPROM_image_88.png]]
写入数据的时候要有一个操作： ACKNOWLEDGE POLLING

![[../../annex/I2C--读写EEPROM_image_89.png]]
卡死在发送 7 位地址之后没响应
![[../../annex/I2C--读写EEPROM_image_90.png]]

弹幕：所以这句话会卡死，因为 EEPROM 写入需要时间而 STM32速度非常快，EEPROM 还没写完就开始写入7位地址

弹幕：应该是 stm32速度太快，数据发送或者接受还没有完成，语句就执行完了，然后代码就一只卡在用 while 语句检测事件的死循环里面
弹幕：当 IIC 把数据写给 EEPROM 之后就马上进行读指令了，但是 EERPOM 的内部时序还没有把 IIC 写给自己的数据储存好，所以 IIC 把自己之前写的数据当做指针去 EEPROM 寻址读出的时候就卡死了
弹幕：是在发送地址后，没收到应答，卡死在发送地址后那个循环（检测 EV6时间，我做实验测了）


###### 代码

```main.c
/**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   串口中断接收测试
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */ 
 
 
#include "stm32f10x.h"
#include "bsp_usart.h"
#include "./i2c/bsp_i2c.h"

//1.初始化IIC相关的GPIO
//2.配置IIC外设的工作模式
//3.编写IIC写入EEPROM的Byte Write函数
//4.编写IIC读取EEPROM的Random Read函数
//5.使用Read函数及Write函数进行读写校验

//6.编写page write及seq read函数并校验

uint8_t readData[10] = {0};
uint8_t writeData[8]={4,5,6,7,8,9,10,11};//注意，最多写8个

/**
  * @brief  主函数
  * @param  无
  * @retval 无
  */
int main(void)
{	
	uint8_t i=0;
  /*初始化USART 配置模式为 115200 8-N-1，中断接收*/
  USART_Config();
	
		/* 发送一个字符串 */
		printf("这是一个IIC通讯实验\n");
	
		//初始化IIC
		I2C_EE_Config();
		
		//写入一个字节
		EEPROM_Byte_Write(11,55);//0x55->55
		
		//等待写入操作完成
		EEPROM_WaitFortWriteEnd();
	
		//写入一个字节
		EEPROM_Byte_Write(12,52);//0x55->52
		
		//等待写入操作完成
		EEPROM_WaitFortWriteEnd();
	
		//页连续写入(最多8个字节)//addr%8 = 0;即为地址对齐
		EEPROM_Page_Write(0,writeData,8);//注意，页写入也要等待数据写入完成
		//注意，页写入的首地址必须要跟八字节地址对齐//从倒数开始，错位几个字节就几个数出错
	
		//等待写入操作完成
		EEPROM_WaitFortWriteEnd();
		
		//读取数据
		EEPROM_Read(0,readData,8);
		
		for(i=0;i<8;i++)
		{
		printf("%d ",readData[i]);
		}
		
		// printf("\r\n接收到的数据为:0x%x,0x%x\r\n",readData[0],readData[1]);

  while(1)
	{	
		
	}	
}
/*********************************************END OF FILE**********************/

```


```bsp_i2c.c
/**
  ******************************************************************************
  * @file    bsp_usart.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   重定向c库printf函数到usart端口
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火STM32 F103-指南者 开发板  
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */ 
	
#include "bsp_i2c.h"


 /**
  * @brief  I2C EEPROM 配置,工作参数配置
  * @param  无
  * @retval 无
  */
void I2C_EE_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;
	I2C_InitTypeDef	 I2C_InitStructure;

	// 打开串口GPIO的时钟
	EEPROM_I2C_APBxClkCmd(EEPROM_I2C_SCL_GPIO_CLK|EEPROM_I2C_SDA_GPIO_CLK, ENABLE);//利用或|运算来同时初始化
	//加一个或|，相当于把它们都初始化，宏定义的内容都是GPIOB，是一样的。同样一个数字去或运算，是不会改变的
	
	// 打开串口外设的时钟
	EEPROM_I2C_GPIO_APBxClkCmd(EEPROM_I2C_CLK, ENABLE);

	// 将IIC SCL的GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = EEPROM_I2C_SCL_GPIO_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;//需要将IIC总线高阻态，防止短路，要启用复用开漏
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(EEPROM_I2C_SCL_GPIO_PORT, &GPIO_InitStructure);

  // 将IIC SDA的GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = EEPROM_I2C_SDA_GPIO_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;
	GPIO_Init(EEPROM_I2C_SDA_GPIO_PORT, &GPIO_InitStructure);
	//省略了50Mhz等，实际上因为SCL和SDA的结构体成员的值跟前面SCL是一样的，所以删除掉不写也是可以的
	//SCL和SDA的Mode等的值是一样的，只要把不一样的值就进行赋值修改就可以了

	// 配置IIC的工作参数
	I2C_InitStructure.I2C_Ack = I2C_Ack_Enable;//使能应答 
	I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;//使用7位地址模式
	I2C_InitStructure.I2C_ClockSpeed = EEPROM_I2C_BAUDRATE;//配置SCL时钟频率
	I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_2;
	I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;
	I2C_InitStructure.I2C_OwnAddress1 = STM32_I2C_OWN_ADDR;//这是STM32 IIC自身设备地址，只要是总线上唯一即可
	
	I2C_Init(EEPROM_I2C,&I2C_InitStructure);
	
	// 使能串口
	I2C_Cmd(EEPROM_I2C, ENABLE);	    
}

//向EEPROM写入一个字节
void EEPROM_Byte_Write(uint8_t addr,uint8_t data)
{
	//产生起始信号
	I2C_GenerateSTART(EEPROM_I2C,ENABLE);
	
	//I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == SUCCESS;//不去使用getflagdata函数了//如果等于SUCCESS就正常发送出去了
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止
	//有弊端，如果失败了就卡死了。后面有超时处理：如果失败了会等待一段时间，如超过50ms还检测不到正常信号的话，就跳出这个程序，防止程序卡死
	
	//EV5事件被检测到,发送设备地址
	I2C_Send7bitAddress(EEPROM_I2C,EEPROM_ADDR,I2C_Direction_Transmitter);//实际上，输入一个8位的IIC地址，可以是读地址，也可以是写地址，库函数会根据第3个参数来给总线发送不一样的(会自己处理)
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	//EV6事件被检测到,发送要操作的存储单元地址
	I2C_SendData(EEPROM_I2C,addr);//发送的是一个数据，但是这个数据会被EEPROM认为是要写入的内存单元的地址

	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_BYTE_TRANSMITTING) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	//EV8事件被检测到,发送要存储的数据
	I2C_SendData(EEPROM_I2C,data);
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_BYTE_TRANSMITTED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	//数据传输完成
	I2C_GenerateSTOP(EEPROM_I2C,ENABLE);
}



//向EEPROM写入多个字节（页写入），每次写入不能超过8个字节
void EEPROM_Page_Write(uint8_t addr,uint8_t *data,uint8_t numByteToWrite)
{
	//产生起始信号
	I2C_GenerateSTART(EEPROM_I2C,ENABLE);
	
	//I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == SUCCESS;//不去使用getflagdata函数了//如果等于SUCCESS就正常发送出去了
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止
	//有弊端，如果失败了就卡死了。后面有超时处理：如果失败了会等待一段时间，如超过50ms还检测不到正常信号的话，就跳出这个程序，防止程序卡死
	
	//EV5事件被检测到,发送设备地址
	I2C_Send7bitAddress(EEPROM_I2C,EEPROM_ADDR,I2C_Direction_Transmitter);//实际上，输入一个8位的IIC地址，可以是读地址，也可以是写地址，库函数会根据第3个参数来给总线发送不一样的(会自己处理)
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	//EV6事件被检测到,发送要操作的存储单元地址
	I2C_SendData(EEPROM_I2C,addr);//发送的是一个数据，但是这个数据会被EEPROM认为是要写入的内存单元的地址

	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_BYTE_TRANSMITTING) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	while(numByteToWrite)
	{
		//EV8事件被检测到,发送要存储的数据
		I2C_SendData(EEPROM_I2C,*data);
		
		while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_BYTE_TRANSMITTED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

		data++;
		
		numByteToWrite--;
	}
	
	//数据传输完成
	I2C_GenerateSTOP(EEPROM_I2C,ENABLE);
}




//向EEPROM读取数据
void EEPROM_Read(uint8_t addr,uint8_t *data,uint8_t numByteToRead)//注意，要读取的数据一定要写成指针,因为函数作为输入参数的话，是不会被修改的，写成指针才能被修改
{
	//产生起始信号
	I2C_GenerateSTART(EEPROM_I2C,ENABLE);
	
	//I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == SUCCESS;//不去使用getflagdata函数了//如果等于SUCCESS就正常发送出去了
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止
	//有弊端，如果失败了就卡死了。后面有超时处理：如果失败了会等待一段时间，如超过50ms还检测不到正常信号的话，就跳出这个程序，防止程序卡死
	
	//EV5事件被检测到,发送设备地址
	I2C_Send7bitAddress(EEPROM_I2C,EEPROM_ADDR,I2C_Direction_Transmitter);//实际上，输入一个8位的IIC地址，可以是读地址，也可以是写地址，库函数会根据第3个参数来给总线发送不一样的(会自己处理)
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	//EV6事件被检测到,发送要操作的存储单元地址
	I2C_SendData(EEPROM_I2C,addr);//发送的是一个数据，但是这个数据会被EEPROM认为是要写入的内存单元的地址

	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_BYTE_TRANSMITTING) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	
	//第二次起始信号
		//产生起始信号
	I2C_GenerateSTART(EEPROM_I2C,ENABLE);
	
	//I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == SUCCESS;//不去使用getflagdata函数了//如果等于SUCCESS就正常发送出去了
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止
	
	//EV5事件被检测到,发送设备地址
	I2C_Send7bitAddress(EEPROM_I2C,EEPROM_ADDR,I2C_Direction_Receiver);//实际上，输入一个8位的IIC地址，可以是读地址，也可以是写地址，库函数会根据第3个参数来给总线发送不一样的(会自己处理)
	
	while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_RECEIVER_MODE_SELECTED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

	while(numByteToRead)
	{
		
		if(numByteToRead == 1)
		{
		
			//如果接收到的是最后一个字节
			//（先产生一个NO ACK信号）
			I2C_AcknowledgeConfig(EEPROM_I2C,DISABLE);
			
		}
		
		//EV7事件被检测到
		while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_BYTE_RECEIVED) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止

		//EV7事件被检测到，即数据寄存器有新的有效数据
		*data = I2C_ReceiveData(EEPROM_I2C);
		
		data++;
		
		numByteToRead--;
	}

	//数据接收完毕，产生STOP信号
 I2C_GenerateSTOP(EEPROM_I2C,ENABLE);
	
	//重新配置ACK使能，默认为响应，避免下次传输的时候出错
	I2C_AcknowledgeConfig(EEPROM_I2C,ENABLE);


	//数据传输完成
	I2C_GenerateSTOP(EEPROM_I2C,ENABLE);
}


//等待EEPROM内部时序完成
void EEPROM_WaitFortWriteEnd(void)
{
	do
	{
	//产生起始信号
	I2C_GenerateSTART(EEPROM_I2C,ENABLE);
	
	while(I2C_GetFlagStatus(EEPROM_I2C,I2C_FLAG_SB) == RESET);
	
	//while(I2C_CheckEvent(EEPROM_I2C,I2C_EVENT_MASTER_MODE_SELECT) == ERROR);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止
	//有弊端，如果失败了就卡死了。后面有超时处理：如果失败了会等待一段时间，如超过50ms还检测不到正常信号的话，就跳出这个程序，防止程序卡死
	//这里不能用I2C_CheckEvent，因为直接使用I2C_CheckEvent这些事件的时候，会检查很多个标志位。
		
	//EV5事件被检测到,发送设备地址
	I2C_Send7bitAddress(EEPROM_I2C,EEPROM_ADDR,I2C_Direction_Transmitter);//实际上，输入一个8位的IIC地址，可以是读地址，也可以是写地址，库函数会根据第3个参数来给总线发送不一样的(会自己处理)
	}
	while(I2C_GetFlagStatus(EEPROM_I2C,I2C_FLAG_ADDR) == RESET);//如果等于ERROR,就死循环，一直等，等到SUCCESS为止
	//用I2C_GetFlagStatus代替I2C_CheckEvent之后，就可以正常运行
	
	
	//EEPROM内部时序完成传输
	I2C_GenerateSTOP(EEPROM_I2C,ENABLE);

}



```


```bsp_i2c.h
#ifndef __BSP_I2C_H
#define	__BSP_I2C_H


#include "stm32f10x.h"

//EEPROM 设备地址（8位）
#define EEPROM_ADDR											0xA0

/** 
* IIC EEPROM 的引脚定义

  */

#define STM32_I2C_OWN_ADDR							0x5f

// IIC1
#define  EEPROM_I2C	               	    I2C1
#define  EEPROM_I2C_CLK                	RCC_APB1Periph_I2C1
#define  EEPROM_I2C_APBxClkCmd          RCC_APB1PeriphClockCmd
#define  EEPROM_I2C_BAUDRATE            400000

// IIC GPIO 引脚宏定义
#define  EEPROM_I2C_SCL_GPIO_CLK       (RCC_APB2Periph_GPIOB)
#define  EEPROM_I2C_SDA_GPIO_CLK       (RCC_APB2Periph_GPIOB)

#define  EEPROM_I2C_GPIO_APBxClkCmd     RCC_APB2PeriphClockCmd
    
#define  EEPROM_I2C_SCL_GPIO_PORT       GPIOB
#define  EEPROM_I2C_SCL_GPIO_PIN        GPIO_Pin_6

#define  EEPROM_I2C_SDA_GPIO_PORT       GPIOB
#define  EEPROM_I2C_SDA_GPIO_PIN        GPIO_Pin_7

void EEPROM_Byte_Write(uint8_t addr,uint8_t data);
void EEPROM_Read(uint8_t addr,uint8_t *data,uint8_t numByteToRead);
void I2C_EE_Config(void);
void EEPROM_WaitFortWriteEnd(void);
void EEPROM_Page_Write(uint8_t addr,uint8_t *data,uint8_t numByteToWrite);
	
#endif /* __BSP_I2C_H */

```



## P 52 代码详解-读写 EEPROM-下

![[../../annex/I2C--读写EEPROM_image_91.png]]
function 可以直接点击跳转到想看的那个函数开头

![[../../annex/I2C--读写EEPROM_image_92.png]]
F12 无法跳转去 OUPUT 中勾选 Browse Information 并编译文件

SDA 引脚具有输入和输出功能，SDA 线作为 SCL 通讯引脚，有时会接收数据，有时会发送数据，直接配置成开漏输出为什么没有问题呢？
![[../../annex/I2C--读写EEPROM_image_93.png]]
因为在 STM 32 中，开漏输出是可以同时作为输入和输出的，就是说配置成输出功能，它同时也是能够接收数据的。输出功能是不会影响输入功能的。

尝试改成推挽输出或复用输出，上一个程序还能不能正常的跑
弹幕：试过了  scl 推挽开漏都可以  sda 只能开漏

测试 I2C_TIMEOUT_UserCallback(4); 函数。
短接 PB6、PB7（SCL 和 SDA 短接）
注意：不要把地跟电源连了，如果不是把地跟电源连接起来，其他的无所谓（GPIO 随便联系起来）。不用担心板子会坏。
![[../../annex/I2C--读写EEPROM_image_94.png]]
短接 PB6 和 PB7，程序不能正常运行，在 void I2C_EE_WaitEepromStandbyState(void) 函数卡死（缺了判断标志位）。在 do，while 中加入 & 0x0002) &&(I2CTimeout>0)。就可以执行到 printf("iaaaaaaaaaaaaaa")了
![[../../annex/I2C--读写EEPROM_image_95.png]]
因为短接 PB6 和 PB7，程序肯定不能正常运行。
![[../../annex/I2C--读写EEPROM_image_96.png]]
有了这些机制，程序就不至于卡死，对于程序还有其他功能的话，这是一个非常重要的作用。



![[../../annex/I2C--读写EEPROM_image_97.png]]

![[../../annex/I2C--读写EEPROM_image_98.png]]

![[../../annex/I2C--读写EEPROM_image_99.png]]


###### 代码

```main.c
/**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   I2C EEPROM(AT24C02)测试，测试信息通过USART1打印在电脑的超级终端
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */
  
#include "stm32f10x.h"
#include "./led/bsp_led.h"
#include "./usart/bsp_usart.h"
#include "./i2c/bsp_i2c_ee.h"
#include <string.h>

#define  EEP_Firstpage      0x00
uint8_t I2c_Buf_Write[256];
uint8_t I2c_Buf_Read[256];
uint8_t I2C_Test(void);

/**
  * @brief  主函数
  * @param  无
  * @retval 无
  */
int main(void)
{ 
  LED_GPIO_Config();
  
  LED_BLUE;
  /* 串口初始化 */
	USART_Config();
	
	printf("\r\n 这是一个I2C外设(AT24C02)读写测试例程 \r\n");

	/* I2C 外设初(AT24C02)始化 */
	I2C_EE_Init();

	printf("\r\n 这是一个I2C外设(AT24C02)读写测试例程 \r\n");	
	 	 
  //EEPROM 读写测试
	if(I2C_Test() ==1)
	{
			LED_GREEN;
	}
	else
	{
			LED_RED;
	}
  
	EEPROM_DEBUG("这是一个调试信息");//EEPROM_DEBUG_ON配置为0的时候不起作用，调试的时候可以配置成1然后用来判断错误的地方
	
	printf("iaaaaaaaaaa");
	
  while (1)
  {      
  }
}

/**
  * @brief  I2C(AT24C02)读写测试
  * @param  无
  * @retval 正常返回1，异常返回0
  */
uint8_t I2C_Test(void)
{
	uint16_t i;

	printf("写入的数据\n\r");
    
	for ( i=0; i<=255; i++ ) //填充缓冲
  {   
    I2c_Buf_Write[i] = i;

    printf("0x%02X ", I2c_Buf_Write[i]);
    if(i%16 == 15)    //每隔16个数字做一次回车
        printf("\n\r");    
   }

  //将I2c_Buf_Write中顺序递增的数据写入EERPOM中 
	I2C_EE_BufferWrite( I2c_Buf_Write, EEP_Firstpage, 256);//解决了Page_write函数地址需要对齐的问题
  
  EEPROM_INFO("\n\r写成功\n\r");//EEPROM_INFO来源于Linux
   
   EEPROM_INFO("\n\r读出的数据\n\r");
  //将EEPROM读出数据顺序保持到I2c_Buf_Read中
	I2C_EE_BufferRead(I2c_Buf_Read, EEP_Firstpage, 256); 
   
  //将I2c_Buf_Read中的数据通过串口打印
	for (i=0; i<256; i++)
	{	
		if(I2c_Buf_Read[i] != I2c_Buf_Write[i])
		{
			EEPROM_ERROR("0x%02X ", I2c_Buf_Read[i]);
			EEPROM_ERROR("错误:I2C EEPROM写入与读出的数据不一致\n\r");
			return 0;
		}
    printf("0x%02X ", I2c_Buf_Read[i]);
    if(i%16 == 15)    
        printf("\n\r");
    
	}
  EEPROM_INFO("I2C(AT24C02)读写测试成功\n\r");
  
  return 1;
}
/*********************************************END OF FILE**********************/

```

```bsp_i2c_ee.c
/**
  ******************************************************************************
  * @file    bsp_i2c_ee.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   i2c EEPROM(AT24C02)应用函数bsp
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */ 

#include "./i2c/bsp_i2c_ee.h"
#include "./usart/bsp_usart.h"		




uint16_t EEPROM_ADDRESS;

static __IO uint32_t  I2CTimeout = I2CT_LONG_TIMEOUT;   


static uint32_t I2C_TIMEOUT_UserCallback(uint8_t errorCode);


/**
  * @brief  I2C I/O配置
  * @param  无
  * @retval 无
  */
static void I2C_GPIO_Config(void)
{
  GPIO_InitTypeDef  GPIO_InitStructure; 

	/* 使能与 I2C 有关的时钟 */
	EEPROM_I2C_APBxClock_FUN ( EEPROM_I2C_CLK, ENABLE );
	EEPROM_I2C_GPIO_APBxClock_FUN ( EEPROM_I2C_GPIO_CLK, ENABLE );
	
    
  /* I2C_SCL、I2C_SDA*/
  GPIO_InitStructure.GPIO_Pin = EEPROM_I2C_SCL_PIN;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;	       // 开漏输出
  GPIO_Init(EEPROM_I2C_SCL_PORT, &GPIO_InitStructure);
	
  GPIO_InitStructure.GPIO_Pin = EEPROM_I2C_SDA_PIN;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_OD;	       // 开漏输出
  GPIO_Init(EEPROM_I2C_SDA_PORT, &GPIO_InitStructure);	
	
	
}


/**
  * @brief  I2C 工作模式配置
  * @param  无
  * @retval 无
  */
static void I2C_Mode_Configu(void)
{
  I2C_InitTypeDef  I2C_InitStructure; 

  /* I2C 配置 */
  I2C_InitStructure.I2C_Mode = I2C_Mode_I2C;
	
	/* 高电平数据稳定，低电平数据变化 SCL 时钟线的占空比 */
  I2C_InitStructure.I2C_DutyCycle = I2C_DutyCycle_2;
	
  I2C_InitStructure.I2C_OwnAddress1 =I2Cx_OWN_ADDRESS7; 
  I2C_InitStructure.I2C_Ack = I2C_Ack_Enable ;
	 
	/* I2C的寻址模式 */
  I2C_InitStructure.I2C_AcknowledgedAddress = I2C_AcknowledgedAddress_7bit;
	
	/* 通信速率 */
  I2C_InitStructure.I2C_ClockSpeed = I2C_Speed;
  
	/* I2C 初始化 */
  I2C_Init(EEPROM_I2Cx, &I2C_InitStructure);
  
	/* 使能 I2C */
  I2C_Cmd(EEPROM_I2Cx, ENABLE);   
}


/**
  * @brief  I2C 外设(EEPROM)初始化
  * @param  无
  * @retval 无
  */
void I2C_EE_Init(void)
{

  I2C_GPIO_Config(); 
 
  I2C_Mode_Configu();

/* 根据头文件i2c_ee.h中的定义来选择EEPROM的设备地址 */
#ifdef EEPROM_Block0_ADDRESS
  /* 选择 EEPROM Block0 来写入 */
  EEPROM_ADDRESS = EEPROM_Block0_ADDRESS;
#endif

#ifdef EEPROM_Block1_ADDRESS  
	/* 选择 EEPROM Block1 来写入 */
  EEPROM_ADDRESS = EEPROM_Block1_ADDRESS;
#endif

#ifdef EEPROM_Block2_ADDRESS  
	/* 选择 EEPROM Block2 来写入 */
  EEPROM_ADDRESS = EEPROM_Block2_ADDRESS;
#endif

#ifdef EEPROM_Block3_ADDRESS  
	/* 选择 EEPROM Block3 来写入 */
  EEPROM_ADDRESS = EEPROM_Block3_ADDRESS;
#endif
}


/**
  * @brief   将缓冲区中的数据写到I2C EEPROM中
  * @param   
  *		@arg pBuffer:缓冲区指针
  *		@arg WriteAddr:写地址
  *     @arg NumByteToWrite:写的字节数
  * @retval  无
  */
void I2C_EE_BufferWrite(u8* pBuffer, u8 WriteAddr, u16 NumByteToWrite)
{
  u8 NumOfPage = 0, NumOfSingle = 0, Addr = 0, count = 0;

  Addr = WriteAddr % I2C_PageSize;// %8 的余数交给Addr//看超过8个字节没，超过了分两页来处理
  count = I2C_PageSize - Addr;//如果地址对齐是不需要count变量，这个用在else语句中
  NumOfPage =  NumByteToWrite / I2C_PageSize;//要分成多少个整数页，每个页由8个字节来写入
  NumOfSingle = NumByteToWrite % I2C_PageSize;//剩下的单页有多少个字节
 
  /* If WriteAddr is I2C_PageSize aligned  */
  if(Addr == 0) //根据Addr的值看是否地址对齐
  {
    /* If NumByteToWrite < I2C_PageSize */
    if(NumOfPage == 0) //单页。直接写入。
    {
      I2C_EE_PageWrite(pBuffer, WriteAddr, NumOfSingle);
      I2C_EE_WaitEepromStandbyState();//每次调用I2C_EE_PageWrite之后都需要调用I2C_EE_WaitEepromStandbyState来等待内部操作完成
    }
    /* If NumByteToWrite > I2C_PageSize */
    else  
    {
      while(NumOfPage--)//while循环，每次写入1页
      {
        I2C_EE_PageWrite(pBuffer, WriteAddr, I2C_PageSize); 
			I2C_EE_WaitEepromStandbyState();
        WriteAddr +=  I2C_PageSize;//第二次写入地址WriteAddr要加8个字节到下一页
        pBuffer += I2C_PageSize;//pBuffer是我们要写入的数据，指针也要+8偏移到后面使用
      }

      if(NumOfSingle!=0)//!=0，直接把剩余的字节写完
      {
        I2C_EE_PageWrite(pBuffer, WriteAddr, NumOfSingle);
        I2C_EE_WaitEepromStandbyState();
      }
    }
  }
  /* If WriteAddr is not I2C_PageSize aligned  */
  else //else分支，Addr!=0
  {
    /* If NumByteToWrite < I2C_PageSize */
    if(NumOfPage== 0) 
    {
      I2C_EE_PageWrite(pBuffer, WriteAddr, NumOfSingle);
      I2C_EE_WaitEepromStandbyState();
    }
    /* If NumByteToWrite > I2C_PageSize */
    else
    {
      NumByteToWrite -= count;
      NumOfPage =  NumByteToWrite / I2C_PageSize;//地址不对齐的时候，NumOfPage和NumOfSingle重新计算（覆盖之前的）
      NumOfSingle = NumByteToWrite % I2C_PageSize;	
      
      if(count != 0)//这一部分写入count个字节（红色部分）（count=7）
      {  
        I2C_EE_PageWrite(pBuffer, WriteAddr, count);
        I2C_EE_WaitEepromStandbyState();
        WriteAddr += count;
        pBuffer += count;
      } 
      
      while(NumOfPage--)//写入黄色的部分（第1页）
      {
        I2C_EE_PageWrite(pBuffer, WriteAddr, I2C_PageSize);
        I2C_EE_WaitEepromStandbyState();
        WriteAddr +=  I2C_PageSize;
        pBuffer += I2C_PageSize;  
      }
      if(NumOfSingle != 0)//写入绿色的部分（NumOfSingle=7）
      {
        I2C_EE_PageWrite(pBuffer, WriteAddr, NumOfSingle); 
        I2C_EE_WaitEepromStandbyState();
      }
    }
  }  
}


/**
  * @brief   写一个字节到I2C EEPROM中
  * @param   
  *		@arg pBuffer:缓冲区指针
  *		@arg WriteAddr:写地址 
  * @retval  无
  */
uint32_t I2C_EE_ByteWrite(u8* pBuffer, u8 WriteAddr) 
{
  /* Send STRAT condition */
  I2C_GenerateSTART(EEPROM_I2Cx, ENABLE);

  I2CTimeout = I2CT_FLAG_TIMEOUT;  
  /* Test on EV5 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_MODE_SELECT))  
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(0);
  } 
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Send EEPROM address for write */
  I2C_Send7bitAddress(EEPROM_I2Cx, EEPROM_ADDRESS, I2C_Direction_Transmitter);
  
  /* Test on EV6 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(1);
  }  
  /* Send the EEPROM's internal address to write to */
  I2C_SendData(EEPROM_I2Cx, WriteAddr);
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV8 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_BYTE_TRANSMITTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(2);
  } 
  
  /* Send the byte to be written */
  I2C_SendData(EEPROM_I2Cx, *pBuffer); 
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;  
  /* Test on EV8 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_BYTE_TRANSMITTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(3);
  } 
  
  /* Send STOP condition */
  I2C_GenerateSTOP(EEPROM_I2Cx, ENABLE);
  
  return 1;
}


/**
  * @brief   在EEPROM的一个写循环中可以写多个字节，但一次写入的字节数
  *          不能超过EEPROM页的大小，AT24C02每页有8个字节
  * @param   
  *		@arg pBuffer:缓冲区指针
  *		@arg WriteAddr:写地址
  *     @arg NumByteToWrite:写的字节数
  * @retval  无
  */
uint32_t I2C_EE_PageWrite(u8* pBuffer, u8 WriteAddr, u8 NumByteToWrite)
{
  I2CTimeout = I2CT_LONG_TIMEOUT;
	//防止总线BUSY忙的时候去通讯。
  while(I2C_GetFlagStatus(EEPROM_I2Cx, I2C_FLAG_BUSY))//while循环检测BUSY标志//==SET有效，说明总线是忙碌的   
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(4);//一开始I2CTimeout值为10000，不符合就重复执行while循环//减到0为止，return退出函数I2C_EE_PageWrite
  } //一万次检测BUSY位还等于SET的话，那么说明可能没法正常通讯，再等待下去STM32就卡死了。
		//I2C_TIMEOUT_UserCallback(4)函数实际上就是输出一段错误代码信息,这样很容易就可以确认哪一个标志位检测出错了(形参4就是出错位置的编号)
  
  /* Send START condition *///产生START信号
  I2C_GenerateSTART(EEPROM_I2Cx, ENABLE);
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV5 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_MODE_SELECT))  
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(5);
  } 
  
  /* Send EEPROM address for write */
  I2C_Send7bitAddress(EEPROM_I2Cx, EEPROM_ADDRESS, I2C_Direction_Transmitter);
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV6 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED))  
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(6);
  } 
  
  /* Send the EEPROM's internal address to write to */    
  I2C_SendData(EEPROM_I2Cx, WriteAddr);  //WriteAddr写入的EEPROM的存储地址

  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV8 and clear it */
  while(! I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_BYTE_TRANSMITTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(7);
  } 

  /* While there is data to be written *///要存储的数据
  while(NumByteToWrite--)  
  {
    /* Send the current byte */
    I2C_SendData(EEPROM_I2Cx, *pBuffer); 

    /* Point to the next byte to be written */
    pBuffer++; 
  
    I2CTimeout = I2CT_FLAG_TIMEOUT;

    /* Test on EV8 and clear it *///而且每次发送都会有一个超时检测
    while (!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_BYTE_TRANSMITTED))
    {
      if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(8);
    } 
  }

  /* Send STOP condition */
  I2C_GenerateSTOP(EEPROM_I2Cx, ENABLE);
  
  return 1;
}


/**
  * @brief   从EEPROM里面读取一块数据 
  * @param   
  *		@arg pBuffer:存放从EEPROM读取的数据的缓冲区指针
  *		@arg WriteAddr:接收数据的EEPROM的地址
  *     @arg NumByteToWrite:要从EEPROM读取的字节数
  * @retval  无
  */
uint32_t I2C_EE_BufferRead(u8* pBuffer, u8 ReadAddr, u16 NumByteToRead)//多了一个检查操作，超时机制
{  
  
  I2CTimeout = I2CT_LONG_TIMEOUT;
  
  //*((u8 *)0x4001080c) |=0x80; 
  while(I2C_GetFlagStatus(EEPROM_I2Cx, I2C_FLAG_BUSY))//检测总线是否为忙碌
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(9);
   }
  
  /* Send START condition */
  I2C_GenerateSTART(EEPROM_I2Cx, ENABLE);//如果不忙就产生起始信号
  //*((u8 *)0x4001080c) &=~0x80;
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV5 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_MODE_SELECT))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(10);
   }
  
  /* Send EEPROM address for write */
  I2C_Send7bitAddress(EEPROM_I2Cx, EEPROM_ADDRESS, I2C_Direction_Transmitter);//发送7位地址

  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV6 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_TRANSMITTER_MODE_SELECTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(11);
   }
    
  /* Clear EV6 by setting again the PE bit */
  I2C_Cmd(EEPROM_I2Cx, ENABLE);//写方向，写入存储地址

  /* Send the EEPROM's internal address to write to */
  I2C_SendData(EEPROM_I2Cx, ReadAddr);  //发送读取的存储器地址

   
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV8 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_BYTE_TRANSMITTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(12);
   }
    
  /* Send STRAT condition a second time */  
  I2C_GenerateSTART(EEPROM_I2Cx, ENABLE);//产生第二次起始信号//产生读方向
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV5 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_MODE_SELECT))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(13);
   }
    
  /* Send EEPROM address for read */
  I2C_Send7bitAddress(EEPROM_I2Cx, EEPROM_ADDRESS, I2C_Direction_Receiver);//读方向
  
  I2CTimeout = I2CT_FLAG_TIMEOUT;
  /* Test on EV6 and clear it */
  while(!I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_RECEIVER_MODE_SELECTED))
  {
    if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(14);
   }
  
  /* While there is data to be read */
  while(NumByteToRead)  
  {
    if(NumByteToRead == 1)
    {
      /* Disable Acknowledgement */
      I2C_AcknowledgeConfig(EEPROM_I2Cx, DISABLE);
      
      /* Send STOP Condition */
      I2C_GenerateSTOP(EEPROM_I2Cx, ENABLE);
    }

    /* Test on EV7 and clear it */    
    I2CTimeout = I2CT_LONG_TIMEOUT;
    
		while(I2C_CheckEvent(EEPROM_I2Cx, I2C_EVENT_MASTER_BYTE_RECEIVED)==0) //同样有while循环去检测标志是否正常，以及超时检测机制
		{
			if((I2CTimeout--) == 0) return I2C_TIMEOUT_UserCallback(3);
		} 
    {      
      /* Read a byte from the EEPROM */
      *pBuffer = I2C_ReceiveData(EEPROM_I2Cx);//读到的数据通过I2C_ReceiveData库函数把数据存储到*pBuffer，然后返回数据出去

      /* Point to the next location where the byte read will be saved */
      pBuffer++; 
      
      /* Decrement the read bytes counter */
      NumByteToRead--;        
    }   
  }

  /* Enable Acknowledgement to be ready for another reception */
  I2C_AcknowledgeConfig(EEPROM_I2Cx, ENABLE);
  
    return 1;
}


/**
  * @brief  Wait for EEPROM Standby state 
  * @param  无
  * @retval 无
  */
void I2C_EE_WaitEepromStandbyState(void) //检测EEPROM的响应，来判断它的内部写时序是否完成了     
{
  vu16 SR1_Tmp = 0;
	
    /* Test on EV7 and clear it */    
    I2CTimeout = I2CT_LONG_TIMEOUT;
	
  do
  {
		I2CTimeout--;
		
    /* Send START condition */
    I2C_GenerateSTART(EEPROM_I2Cx, ENABLE);
    /* Read I2C1 SR1 register */
    SR1_Tmp = I2C_ReadRegister(EEPROM_I2Cx, I2C_Register_SR1);
    /* Send EEPROM address for write */
    I2C_Send7bitAddress(EEPROM_I2Cx, EEPROM_ADDRESS, I2C_Direction_Transmitter);
  }while(!(I2C_ReadRegister(EEPROM_I2Cx,I2C_Register_SR1) & 0x0002) &&(I2CTimeout>0));
	//没有检测SB位，I2C_Register_SR1第二位是否为1，如果为1说明写完了。第二位SR1其实就是I2C_FLAG_ADDR的地址
  
  /* Clear AF flag *///清除标志
  I2C_ClearFlag(EEPROM_I2Cx, I2C_FLAG_AF);
    /* STOP condition */    
    I2C_GenerateSTOP(EEPROM_I2Cx, ENABLE); 
}




/**
  * @brief  Basic management of the timeout situation.
  * @param  errorCode：错误代码，可以用来定位是哪个环节出错.
  * @retval 返回0，表示IIC读取失败.
  */
static  uint32_t I2C_TIMEOUT_UserCallback(uint8_t errorCode)
{
  /* Block communication and all processes */
  EEPROM_ERROR("I2C 等待超时!errorCode = %d",errorCode);
  
  return 0;
}
/*********************************************END OF FILE**********************/


```

```bsp_i2c.ee.h
#ifndef __I2C_EE_H
#define	__I2C_EE_H


#include "stm32f10x.h"


/**************************I2C参数定义，I2C1或I2C2********************************/
#define             EEPROM_I2Cx                                I2C1
#define             EEPROM_I2C_APBxClock_FUN                   RCC_APB1PeriphClockCmd
#define             EEPROM_I2C_CLK                             RCC_APB1Periph_I2C1
#define             EEPROM_I2C_GPIO_APBxClock_FUN              RCC_APB2PeriphClockCmd
#define             EEPROM_I2C_GPIO_CLK                        RCC_APB2Periph_GPIOB     
#define             EEPROM_I2C_SCL_PORT                        GPIOB   
#define             EEPROM_I2C_SCL_PIN                         GPIO_Pin_6
#define             EEPROM_I2C_SDA_PORT                        GPIOB 
#define             EEPROM_I2C_SDA_PIN                         GPIO_Pin_7



/* STM32 I2C 快速模式 */
#define I2C_Speed              400000  //*

/* 这个地址只要与STM32外挂的I2C器件地址不一样即可 */
#define I2Cx_OWN_ADDRESS7      0X0A   

/* AT24C01/02每页有8个字节 */
#define I2C_PageSize           8

/* AT24C04/08A/16A每页有16个字节 */
//#define I2C_PageSize           16	

/*等待超时时间*/
#define I2CT_FLAG_TIMEOUT         ((uint32_t)0x1000)
#define I2CT_LONG_TIMEOUT         ((uint32_t)(10 * I2CT_FLAG_TIMEOUT))


/*信息输出*///方便调试，方便发布程序
#define EEPROM_DEBUG_ON         0	//调试的时候配置成1，整个程序就能输出各种各样的调试信息；但是配置成0后，整个工程这个语句涉及到的东西的调试信息都不会再输出出来了。

//调试的时候可能需要printf("111");但是调试完可能不需要这个东西输出了，就可以使用这种方式：
//EEPROM_INFO通常作为信息输出，EEPROM_ERROR用来输出错误，EEPROM_DEBUG用来做输出调试信息
#define EEPROM_INFO(fmt,arg...)           printf("<<-EEPROM-INFO->> "fmt"\n",##arg)	//fmt其实就是作为printf函数的一个输入。arg通过两个##连接起来(其实就是一个连接符)
#define EEPROM_ERROR(fmt,arg...)          printf("<<-EEPROM-ERROR->> "fmt"\n",##arg)
#define EEPROM_DEBUG(fmt,arg...)          do{\
                                          if(EEPROM_DEBUG_ON)\
                                          printf("<<-EEPROM-DEBUG->> [%s][%d]"fmt"\n",__FILE__,__LINE__, ##arg);\
                                          }while(0)	//为1输出调试信息，为0就不执行，相当于空操作
//__LINE__可以直接输出这个语句所在的行号,__LINE__就变成前面[%d]的%d的位置。如果还行知道在哪个文件就再加个[%s]

/* 
 * AT24C02 2kb = 2048bit = 2048/8 B = 256 B
 * 32 pages of 8 bytes each
 *
 * Device Address
 * 1 0 1 0 A2 A1 A0 R/W
 * 1 0 1 0 0  0  0  0 = 0XA0
 * 1 0 1 0 0  0  0  1 = 0XA1 
 */

/* EEPROM Addresses defines */
#define EEPROM_Block0_ADDRESS 0xA0   /* E2 = 0 */
//#define EEPROM_Block1_ADDRESS 0xA2 /* E2 = 0 */
//#define EEPROM_Block2_ADDRESS 0xA4 /* E2 = 0 */
//#define EEPROM_Block3_ADDRESS 0xA6 /* E2 = 0 */


void I2C_EE_Init(void);
void I2C_EE_BufferWrite(u8* pBuffer, u8 WriteAddr, u16 NumByteToWrite);
uint32_t I2C_EE_ByteWrite(u8* pBuffer, u8 WriteAddr);
uint32_t I2C_EE_PageWrite(u8* pBuffer, u8 WriteAddr, u8 NumByteToWrite);
uint32_t I2C_EE_BufferRead(u8* pBuffer, u8 ReadAddr, u16 NumByteToRead);
void I2C_EE_WaitEepromStandbyState(void);


#endif /* __I2C_EE_H */

```




## P 53 软件模拟 IIC 原理

软件模拟 IIC 的意思就是通过内核控制 SCL、SDA 两条总线的电平来产生起始信号、停止信号等来实现通讯。

硬件 IIC 就是为了给 CPU 腾出时间来的，为什么要搞软件模拟 IIC？
这主要是因为 STM 32 的硬件 IIC 用起来经常会出很多错（IIC 会卡死在某个环节）。用了其他中断或液晶屏初始化、FSMC 可能用了某些公用的引脚（当初始化 FSMC 时，虽然没有配置这个引脚，但是只要使能了 FSMC 外设--液晶屏的时候会用到的，就会影响到 IIC 的工作了）等的时候
![[../../annex/I2C--读写EEPROM_image_100.png]]

软件 IIC 跟硬件 IIC 的主要区别是它直接使用 CPU 去控制 GPIO 引脚（就像控制 LED 灯一样，控制它们的高低电平来实现这些信号），信号检测等寄存器全都没有用到了，通过内核直接去读取 GPIO 的电平来获取/传输数据。


![[../../annex/I2C--读写EEPROM_image_101.png]]

擦除其实是在 Flash 里的一个特性，EEPROM 其实是不需要这样擦除的。
这里所有擦除也是往里面写入 0 数据（往整个 EEPROM 写入数据 0 而已）

重点讲：ee_ReadBytes 和 ee_WriteBytes
跟 I2C_EE_BufferRead 和 I2C_EE_BufferWrite 功能是一样的，实现一样的功能，但是内部完全不一样了。

ReadBytes 它的起始信号在硬件里面是通过 GenerateSTART 来产生一个起始信号，但是在软件模拟 IIC 中是自己定义的 i2c_Start 函数（定义在 bsp_i2c_gpio 中）。

bsp_i2c_ee 实现上层的应用功能，bsp_i2c_gpio 实现 IIC 的基本逻辑

![[../../annex/I2C--读写EEPROM_image_102.png]]
GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_OD;
直接配置成普通开漏模式（不是复用的开漏模式），也就是可以直接控制 GPIO 的方式来控制输出高阻态还是低电平，而不是利用 IIC 外设来实现的。


![[../../annex/I2C--读写EEPROM_image_103.png]]
![[../../annex/I2C--读写EEPROM_image_104.png]]
把 SDA 和 SCL 都配置成高电平
i2c_Delay 延时函数是为了等待 GPIO 被拉上去，防止 STM 32 操作太快，GPIO 跟不上

![[../../annex/I2C--读写EEPROM_image_105.png]]
延时之后（确保 SDA 和 SCL 引脚成高电平状态之后），这时候把 SDA 引脚拉成 0 电平；SCL 没有操作，保存原来的高电平。
然后再延迟，确保信号产生完毕。
接着再 SCL 配置成 0 电平。

![[../../annex/I2C--读写EEPROM_image_106.png]]
这样就实现了信号逻辑。
通过直接控制 GPIO 的输出来实现了一个起始信号。

STOP 函数同理。
![[../../annex/I2C--读写EEPROM_image_107.png]]


i2c_ReadByte 函数
![[../../annex/I2C--读写EEPROM_image_108.png]]
一个 for 循环要读 8 位数据位（1 个字节），先定义一个 value 变量。在 for 循环的每次开头，value 变量会左移一位，左移一位是因为串行的，一位一位接收、处理。
每一位分解了之后在 for 循环中就是每一位的分解。
在其他的位置配置 SCL 为低电平，在 for 循环中配置成高电平（此时 SDA 中的数据有效），此时 SDA 引脚的数据是什么通过 EEPROM_I2C_SDA_READ 这个宏（通过 GPIO 引脚读它的 IDR 数据寄存器），读取到数据线的数据。
![[../../annex/I2C--读写EEPROM_image_109.png]]

不局限用于 Pin 6、Pin 7 外设. 只要是跟 EEPROM 连接起来就好了。

Delay 函数
![[../../annex/I2C--读写EEPROM_image_110.png]]
用 for 循环让 CPU 等待一段时间，再去进行下一步（经验而谈）。根据 CPU 的执行速度来进行调节，如果有逻辑分析仪，直接用逻辑分析仪的 IIC 协议分析就可以知道主频，知道时钟频率了。
此处是在 72 MHZ 的情况下配置的，STM 32 配置成其他时钟频率也是要注意的。
相当于配置 IIC 的波特率。


硬件 IIC 本质是库函数，是配置寄存器来实现时序的。软件模拟 IIC 直接控制 GPIO 的高低电平。


## P 54 使用 EEPROM 存储小数及整数

之前的几乎都是字节型的数据写入 EEPROM
整型、字符型等怎么存储？
跟存储字节型的数据没有本质的区别
![[../../annex/I2C--读写EEPROM_image_111.png]]

直接把数据按照字节型来存储。（本质没有区别，在 EEPROM 中的是 0、1）

为什么要使用标志位？
为什么不直接在 STM 32 中定义一个变量第一次执行的时候就写入，第二次执行时读取。为什么不直接使用 STM 32 内部的数据呢？
非要从 EEPROM 中读取值，赋值给变量？因为假如 STM 32 如果给开发板断电，存储在 STM 32 内存里边的变量全都丢失掉了。
![[../../annex/I2C--读写EEPROM_image_112.png]]








