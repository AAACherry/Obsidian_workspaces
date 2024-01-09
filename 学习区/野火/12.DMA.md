
## P 40 DMA 功能框图讲解和 DMA 初始化结构体讲解

### 01 DMA功能框图讲解

#### DMA 简介

![[../../annex/DMA_image_1.png]]
DMA 2 有 5 个通道，不是 7 个通道。
DMA 2 只存在大容量（Flash 在 256 k-512 k）、互联型（F 105、F 107 系列）的产品中。

Memory->Memory（Flash->SRAM）
Prefer->Memory （ADC）
M->P（串口发送）

通道可以理解成是搬运数据的管道

#### DMA 功能框图
![[../../annex/DMA_image_2.png]]

![[../../annex/DMA_image_3.png]]

![[../../annex/DMA_image_4.png]]

![[../../annex/DMA_image_5.png]]

![[../../annex/DMA_image_6.png]]

![[../../annex/DMA_image_7.png]]

![[../../annex/DMA_image_8.png]]

当使用 M->M 的时候，所有的控制器（DMA 1 和 DMA 2）所有的通道都是可以使用的。


###### 仲裁器
![[../../annex/DMA_image_9.png]]

![[../../annex/DMA_image_10.png]]

![[../../annex/DMA_image_11.png]]

比如发送串口 1 和 2（USART 1_TX、USART 2_TX）同时发送请求，通道 4 比通道 7 先发送（串口 1：USART 1_TX 先发送）

![[../../annex/DMA_image_12.png]]


### 02 DMA 相关库函数讲解

###### 初始化结构体
初始化结构体在固件库头文件中：stm32f10x_dma.h
![[../../annex/DMA_image_13.png]]

![[../../annex/DMA_image_14.png]]

每个外设的固件库函数里面都会有一个初始化结构体，编程的时候只需要往里面的各成员写数据，然后再调用外设初始化函数，就会把这些成员的值写到寄存器里面。

1、外设地址，DMA_CPAR
2、存储器地址，DMA_CMAR
前两个的值决定了数据从哪里来。
3、传输方向，DMA_CCR：DIR
第三个的值决定了要到哪里去。（M->M？M->P？P->M？）

外设地址通常从外设的数据寄存器中来。比如做 ADC 采集实验时，就从 ADC 采集数据寄存器中来。
![[../../annex/DMA_image_15.png]]
如果数据从 ADC_DR 寄存器中来，那么我们需要把 ADC_DR 数据寄存器的地址写到 DMA_CPAR 中去（32 位的）。

如果是从内部变量中来，就把数据寄存器的地址写到 DMA_CMAR 中去。
![[../../annex/DMA_image_16.png]]


![[../../annex/DMA_image_17.png]]

![[../../annex/DMA_image_18.png]]

M->M 有单独的一个位，（位 14，MEM 2 MEM）
![[../../annex/DMA_image_19.png]]



![[../../annex/DMA_image_20.png]]

![[../../annex/DMA_image_21.png]]

![[../../annex/DMA_image_22.png]]
一次可以传 2<sup>16</sup>=65536 个数据
传的数据的单位是什么由 4、外设数据宽度，DMA_CCRx：PSIZE 和 5、存储器数据宽度，DMA_CCRx：MSIZE
传的时候数据宽度要一样。
![[../../annex/DMA_image_23.png]]

![[../../annex/DMA_image_24.png]]

如果目标地址和源地址的数据宽度不一样
![[../../annex/DMA_image_25.png]]

源地址（SR）是 2 个字节，目标地址（DST）是 4 个字节时
![[../../annex/DMA_image_26.png]]
源地址（SR）是 4 个字节，目标地址（DST）是 2 个字节时
![[../../annex/DMA_image_27.png]]

![[../../annex/DMA_image_28.png]]



![[../../annex/DMA_image_29.png]]

![[../../annex/DMA_image_30.png]]

![[../../annex/DMA_image_31.png]]




![[../../annex/DMA_image_32.png]]

![[../../annex/DMA_image_33.png]]

![[../../annex/DMA_image_34.png]]




## P 41 存储器到存储器传输代码讲解

![[../../annex/DMA_image_35.png]]

SRAM 中通常存变量，FLASH 中通常存 code

![[../../annex/DMA_image_36.png]]


![[../../annex/DMA_image_37.png]]

1、在 FLASH 中定义好要传输的数据，在 SRAM 中定义好用好接收 FLASH 数据的变量
2、初始化 DMA, 主要配置 DMA 初始化结构体
3、编写比较函数
4、编写 main 函数

![[../../annex/DMA_image_38.png]]

###### 代码

``` bsp_dma_mtm.c
#include "bsp_dma_mtm.h"


/* 定义aSRC_Const_Buffer数组作为DMA传输数据源
 * const关键字将aSRC_Const_Buffer数组变量定义为常量类型
 * 表示数据存储在内部的FLASH中
 */
 //Flash中存储的是常量（预先都初始化好的），加上const就把定义的数组里面的值放到Flash中了
const uint32_t aSRC_Const_Buffer[BUFFER_SIZE]= {
                                    0x01020304,0x05060708,0x090A0B0C,0x0D0E0F10,
                                    0x11121314,0x15161718,0x191A1B1C,0x1D1E1F20,
                                    0x21222324,0x25262728,0x292A2B2C,0x2D2E2F30,
                                    0x31323334,0x35363738,0x393A3B3C,0x3D3E3F40,
                                    0x41424344,0x45464748,0x494A4B4C,0x4D4E4F50,
                                    0x51525354,0x55565758,0x595A5B5C,0x5D5E5F60,
                                    0x61626364,0x65666768,0x696A6B6C,0x6D6E6F70,
                                    0x71727374,0x75767778,0x797A7B7C,0x7D7E7F80};//如果定义了>32个数据的话，后面的数据没有进行初始化数据的话，就会被全部初始化成0

/* 定义DMA传输目标存储器
 * 存储在内部的SRAM中																		
 */
uint32_t aDST_Buffer[BUFFER_SIZE];//源跟目标长度定义一致																		
							
//typedef struct
//{
//  uint32_t DMA_PeripheralBaseAddr;   // 外设地址
//  uint32_t DMA_MemoryBaseAddr;       // 存储器地址
//  uint32_t DMA_DIR;                  // 传输方向
//  uint32_t DMA_BufferSize;           // 传输数目
//  uint32_t DMA_PeripheralInc;        // 外设地址增量模式
//  uint32_t DMA_MemoryInc;            // 存储器地址增量模式
//  uint32_t DMA_PeripheralDataSize;   // 外设数据宽度
//  uint32_t DMA_MemoryDataSize;       // 存储器数据宽度
//  uint32_t DMA_Mode;                 // 模式选择
//  uint32_t DMA_Priority;             // 通道优先级
//  uint32_t DMA_M2M;                  // 存储器到存储器模式
//}DMA_InitTypeDef;																		

void MtM_DMA_Config(void)
{
	DMA_InitTypeDef DMA_InitStruct;
	
	RCC_AHBPeriphClockCmd(MTM_DMA_CLK,ENABLE);//M->M，随便选DMA
	
	// 解决数据从哪里来，到哪里去
	DMA_InitStruct.DMA_PeripheralBaseAddr = (uint32_t)aSRC_Const_Buffer;
	DMA_InitStruct.DMA_MemoryBaseAddr = (uint32_t)aDST_Buffer;
	DMA_InitStruct.DMA_DIR = DMA_DIR_PeripheralSRC;
	
	// 数据的单位是什么？
	DMA_InitStruct.DMA_BufferSize = BUFFER_SIZE;//一次要传输多少个数据
	DMA_InitStruct.DMA_PeripheralInc = DMA_PeripheralInc_Enable;
	DMA_InitStruct.DMA_PeripheralDataSize = DMA_PeripheralDataSize_Word;
	
	DMA_InitStruct.DMA_MemoryInc = DMA_MemoryInc_Enable;
	DMA_InitStruct.DMA_MemoryDataSize = DMA_MemoryDataSize_Word;
	
	DMA_InitStruct.DMA_Mode = DMA_Mode_Normal;
	DMA_InitStruct.DMA_Priority = DMA_Priority_High;
	DMA_InitStruct.DMA_M2M = DMA_M2M_Enable;
	
	DMA_ClearITPendingBit(MTM_DMA_FLAG_TC);
	
	DMA_Init(MTM_DMA_CHANNEL, &DMA_InitStruct);
	DMA_Cmd(MTM_DMA_CHANNEL,ENABLE );
}

/**
  * 判断指定长度的两个数据源是否完全相等，
  * 如果完全相等返回1，只要其中一对数据不相等返回0
  */
uint8_t Buffercmp(const uint32_t* pBuffer, 
                  uint32_t* pBuffer1, uint16_t BufferLength)
{
  /* 数据长度递减 */
  while(BufferLength--)
  {
    /* 判断两个数据源是否对应相等 */
    if(*pBuffer != *pBuffer1)
    {
      /* 对应数据源不相等马上退出函数，并返回0 */
      return 0;
    }
    /* 递增两个数据源的地址指针 */
    pBuffer++;
    pBuffer1++;
  }
  /* 完成判断并且对应数据相对 */
  return 1;  
}

					

```

```bsp_dma_mtm.h
#ifndef _BSP_DMA_MTM_H
#define _BSP_DMA_MTM_H

#include "stm32f10x.h"

#define MTM_DMA_CLK				RCC_AHBPeriph_DMA1

#define MTM_DMA_CHANNEL		DMA1_Channel6

#define MTM_DMA_FLAG_TC		DMA1_IT_TC6

// 要发送的数据大小
#define BUFFER_SIZE     32

void MtM_DMA_Config(void);

uint8_t Buffercmp(const uint32_t* pBuffer, 
                  uint32_t* pBuffer1, uint16_t BufferLength);

#endif /*_BSP_DMA_MTM_H*/




```


```main.c
/**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   测试led
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
#include "bsp_led.h"
#include "bsp_dma_mtm.h"

extern const uint32_t aSRC_Const_Buffer[BUFFER_SIZE];
extern uint32_t aDST_Buffer[BUFFER_SIZE];

#define SOFT_DELAY Delay(0x0FFFFF);

void Delay(__IO u32 nCount); 

/**
  * @brief  主函数
  * @param  无  
  * @retval 无
  */
int main(void)
{	
	uint8_t status = 0;
	
	/* LED 端口初始化 */
	LED_GPIO_Config();
	LED_YELLOW;
	Delay(0xFFFFFF);
	
	MtM_DMA_Config();
	
	while( DMA_GetFlagStatus(MTM_DMA_FLAG_TC) == RESET );//如果等于RESET的话就表示还没有传输成功//严谨一点，加上while判断成数据是否传输成功
	
	status = Buffercmp(aSRC_Const_Buffer, aDST_Buffer, BUFFER_SIZE);
	
	if( status == 0)
	{
		LED_RED;
	}
	else
	{
		LED_GREEN;
	}
	
	while (1)
	{

	}
}

void Delay(__IO uint32_t nCount)	 //简单的延时函数
{
	for(; nCount != 0; nCount--);
}
/*********************************************END OF FILE**********************/

```



## P 42 存储器到外设代码讲解

![[../../annex/DMA_image_39.png]]

![[../../annex/DMA_image_40.png]]

![[../../annex/DMA_image_41.png]]
数据发送寄存器，想通过串口发送出去的数据都会放在这里


弹幕：为什么存储器向外设传输数据，用 TX，而不是 RX 接收？

弹幕：串口向 DMA 控制器发送请求，DMA 收到请求后，将数据从内存搬运到串口的数据寄存器，从而串口将 DR 中的数据发送给上位机

弹幕：我理解这时候 DMA 初始化数据已经发送到串口了，后面这个 TX 只是验证发回来的数据，不知道是不是这样

弹幕：这里是串口向 mda 发送请求，请求 mda 将数据发送到串口，懂？？？？

弹幕：应该是 DMA 把数据给串口，然后串口发出

弹幕：串口向 CPU 发送 DMA 请求，CPU 收到请求后，指示 DMA 将数据从 mem 内存搬运到串口的数据寄存器，从而串口将 DR 中的数据发送给上位机

弹幕：这里的 TX 和 RX 是以串口为视角的



![[../../annex/DMA_image_42.png]]
高容量的、超值型的 stm32f10x 才能用这个串口五
![[../../annex/DMA_image_43.png]]
指南者和霸道是 f103 系列的，不能用。只有 f100 系列才能用串口五


```bsp_dma_mtp.c
#include "bsp_dma_mtp.h"

uint8_t SendBuff[SENDBUFF_SIZE];

/**
  * @brief  USART GPIO 配置,工作参数配置
  * @param  无
  * @retval 无
  */
void USART_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;
	USART_InitTypeDef USART_InitStructure;

	// 打开串口GPIO的时钟
	DEBUG_USART_GPIO_APBxClkCmd(DEBUG_USART_GPIO_CLK, ENABLE);
	
	// 打开串口外设的时钟
	DEBUG_USART_APBxClkCmd(DEBUG_USART_CLK, ENABLE);

	// 将USART Tx的GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = DEBUG_USART_TX_GPIO_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(DEBUG_USART_TX_GPIO_PORT, &GPIO_InitStructure);

  // 将USART Rx的GPIO配置为浮空输入模式
	GPIO_InitStructure.GPIO_Pin = DEBUG_USART_RX_GPIO_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;
	GPIO_Init(DEBUG_USART_RX_GPIO_PORT, &GPIO_InitStructure);
	
	// 配置串口的工作参数
	// 配置波特率
	USART_InitStructure.USART_BaudRate = DEBUG_USART_BAUDRATE;
	// 配置 针数据字长
	USART_InitStructure.USART_WordLength = USART_WordLength_8b;
	// 配置停止位
	USART_InitStructure.USART_StopBits = USART_StopBits_1;
	// 配置校验位
	USART_InitStructure.USART_Parity = USART_Parity_No ;
	// 配置硬件流控制
	USART_InitStructure.USART_HardwareFlowControl = USART_HardwareFlowControl_None;
	// 配置工作模式，收发一起
	USART_InitStructure.USART_Mode = USART_Mode_Rx | USART_Mode_Tx;
	// 完成串口的初始化配置
	USART_Init(DEBUG_USARTx, &USART_InitStructure);	
	
	// 使能串口
	USART_Cmd(DEBUG_USARTx, ENABLE);	    
}


///重定向c库函数printf到串口，重定向后可使用printf函数
int fputc(int ch, FILE *f)
{
		/* 发送一个字节数据到串口 */
		USART_SendData(DEBUG_USARTx, (uint8_t) ch);
		
		/* 等待发送完毕 */
		while (USART_GetFlagStatus(DEBUG_USARTx, USART_FLAG_TXE) == RESET);		
	
		return (ch);
}


//Memory-> P(USART-DR)
void USARTx_DMA_Config(void)
{
	DMA_InitTypeDef DMA_InitStruct;
	
	RCC_AHBPeriphClockCmd(USART_TX_DMA_CLK,ENABLE);//M->M，随便选DMA
	
	// 解决数据从哪里来，到哪里去
	DMA_InitStruct.DMA_PeripheralBaseAddr = (uint32_t)USART_DR_ADDRESS;
	DMA_InitStruct.DMA_MemoryBaseAddr = (uint32_t)SendBuff;
	DMA_InitStruct.DMA_DIR = DMA_DIR_PeripheralDST;
	
	// 数据的单位是什么？
	DMA_InitStruct.DMA_BufferSize = SENDBUFF_SIZE;//一次要传输多少个数据
	DMA_InitStruct.DMA_PeripheralInc = DMA_PeripheralInc_Disable;//外设的地址指针要不要递增？只有一个串口数据寄存器，所以不用递增，Disable。
	DMA_InitStruct.DMA_PeripheralDataSize = DMA_MemoryDataSize_Byte;
	
	DMA_InitStruct.DMA_MemoryInc = DMA_MemoryInc_Enable;
	DMA_InitStruct.DMA_MemoryDataSize = DMA_MemoryDataSize_Byte;
	
	DMA_InitStruct.DMA_Mode = DMA_Mode_Normal;
	DMA_InitStruct.DMA_Priority = DMA_Priority_High;
	DMA_InitStruct.DMA_M2M = DMA_M2M_Disable;
	
	//DMA_ClearITPendingBit(MTM_DMA_FLAG_TC);
	
	DMA_Init(USART_TX_DMA_CHANNEL, &DMA_InitStruct);
	
	DMA_ClearFlag(USART_TX_DMA_FLAG_TC);//清除传输完成标志位
	DMA_Cmd(USART_TX_DMA_CHANNEL,ENABLE );
}






```

```bsp_dma_mtp.h
#ifndef _BSP_MDA_MTP_H
#define _BSP_MDA_MTP_H

#include "stm32f10x.h"
#include <stdio.h>

// 串口工作参数宏定义
#define  DEBUG_USARTx                   USART1
#define  DEBUG_USART_CLK                RCC_APB2Periph_USART1
#define  DEBUG_USART_APBxClkCmd         RCC_APB2PeriphClockCmd
#define  DEBUG_USART_BAUDRATE           115200

// USART GPIO 引脚宏定义
#define  DEBUG_USART_GPIO_CLK           (RCC_APB2Periph_GPIOA)
#define  DEBUG_USART_GPIO_APBxClkCmd    RCC_APB2PeriphClockCmd
    
#define  DEBUG_USART_TX_GPIO_PORT       GPIOA   
#define  DEBUG_USART_TX_GPIO_PIN        GPIO_Pin_9
#define  DEBUG_USART_RX_GPIO_PORT       GPIOA
#define  DEBUG_USART_RX_GPIO_PIN        GPIO_Pin_10

#define  USART_TX_DMA_CLK 							RCC_AHBPeriph_DMA1							

#define  USART_TX_DMA_CHANNEL						DMA1_Channel4	//channel是4，不是14

#define  USART_TX_DMA_FLAG_TC						DMA1_FLAG_TC4

// 外设寄存器地址
#define  USART_DR_ADDRESS        (USART1_BASE+0x04)
//外设的地址是DR寄存器的地址，偏移是DR寄存器的地址相对于 USART_BASE那个地址定义的，这个 只是 表达形式 的 问题
//串口1的基地址，加上偏移地址，是这个寄存器的绝对地址。就是GPIOA的DA寄存器，要GPIOA的基地址加上偏移地址

#define SENDBUFF_SIZE							5000	//可以传（计数的值）是16位的，2^16=65535（5000没有超出范围）


void USARTx_DMA_Config(void);
void USART_Config(void);


#endif /*_BSP_MDA_MTP_H*/



```

```main.c
/**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   测试led
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
#include "bsp_led.h"
#include "bsp_dma_mtp.h"

extern uint8_t SendBuff[SENDBUFF_SIZE];

#define SOFT_DELAY Delay(0x0FFFFF);

void Delay(__IO u32 nCount); 

/**
  * @brief  主函数
  * @param  无  
  * @retval 无
  */
int main(void)
{	
	uint16_t i=0;
	/* LED 端口初始化 */
	LED_GPIO_Config();	
	
	USART_Config();

	for( ; i<SENDBUFF_SIZE; i++)
	{
		SendBuff[i] = 'P';
	}
	
	USARTx_DMA_Config();

	USART_DMACmd(DEBUG_USARTx,USART_DMAReq_Tx,ENABLE);
	
	while (1)
	{
		LED1_TOGGLE
    Delay(0xFFFFF);
	}
}

void Delay(__IO uint32_t nCount)	 //简单的延时函数
{
	for(; nCount != 0; nCount--);
}
/*********************************************END OF FILE**********************/

```































































