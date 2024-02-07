

弹幕：注意这里的 ARM complier 记得选对我因为没选对没编译成功导致代码中没有提示

弹幕：扳级支持包，相当于软件驱动硬件的一个桥梁

弹幕：LE 锁存置1，则输出 Q 随着输入 D 的变化而变化。LE 置0，则输出 Q 会保持 LE 置0之前时的状态，不会随着 D 的变化而变化








##### P 2 工程新建

FIle -- New Project -- STM32G431RB -- 选择 LQFP 64 封装 -- Start Project

![[../../annex/STMCubeMX_image_1.png]]


![[../../annex/STMCubeMX_image_2.png]]
System Core-RCC-开启高速时钟 HSE。 

![[../../annex/STMCubeMX_image_3.png]]
System Core--SYS--Debug--Serial Wire

![[../../annex/STMCubeMX_image_4.png]]
Clock Configuration--24MHz--选择 HSE 高速--PLLCLK--80 MHz

![[../../annex/STMCubeMX_image_5.png]]
选择 GPIO_Output

![[../../annex/STMCubeMX_image_6.png]]

System Core--GPIO--选中 Pin 8~Pin 15 勾选 modified。

![[../../annex/STMCubeMX_image_7.png]]

![[../../annex/STMCubeMX_image_8.png]]



一位学习者对手中开发板的介绍和配置方法。他整理了一份学习资料,包括产品手册、电路原理图等,通过这些资料可以了解开发板的基本信息和配置方法。视频详细介绍了开发板上的各个元件和接口,以及如何配置时钟和下载程序。同时,视频也提醒了一些需要注意的问题,如选择正确的调试器和避免使用中文文件名等。

- 开发板的基本情况和使用方法，包括电路原理图、调试器、信号发生器、LED 指示灯等。
00:25 开发板资料包括实验程序、官方历程和产品手册
01:41 开发板调试器使用 STM 32 芯片，通过 USB 连接电脑
03:23 开发板上的 LED 指示灯使用锁存器控制，避免与 LCD 屏冲突

- 如何使用 LED 和 LCD 屏, 以及按键和扩展接口等, 来进行控制和显示。
04:00 控制 Icd 屏，不控制 led 信号传输
04:41 按键信号通过 p p 0 p p 2 p 1 pa 0 传输
06:51 配置芯片型号，选择 usb ga 64封装

- 如何配置 STM 32 F 4 开发板上的时钟树和下载程序, 包括晶振频率和时钟配置等。
08:04 配置时钟树，选择正确的晶振频率
09:18 输入频率，选择 80，自动配对时钟
10:35 在 project manager 界面签名，不能有中文选择正确的编译器

- 如何在编译时注意文件夹和代码的位置，以及如何设置自动运行和调试器等。
12:00 代码必须放在 begin 和 end 中间，否则会被覆盖
13:46 将程序加进板载调试器后，需要设置自动运行选项
14:31 在项目目录中创建 bs 文件夹，等待下一次讲 led 时再填入内容


##### P 3 先来点个灯，LED 操作


弹幕：还可以点击魔法棒旁边的三色块，新建文件夹和添加.c 文件
弹幕：typedef unsigned char uchar; 大家别写错了
弹幕：左移8位是将原先的前8位都移出去
弹幕：最好用 typedef 重定义数据类型，define 只替换不会进行类型检查

![[../../annex/STMCubeMX_image_9.png]]

![[../../annex/STMCubeMX_image_10.png]]

弹幕：之后拉低电平防止引脚冲突导致的误写
弹幕：PD2高电平的一瞬间数据就已经过去了

![[../../annex/STMCubeMX_image_11.png]]


如何配置 LED 开发板的端口以及编写相关驱动程序。首先需要配置端口使其正常工作,然后在编写代码时需要注意参数类型和头文件包含等细节问题。通过修改引脚电平来控制 LED 的亮灭,可以实现不同的显示效果。最后,视频提供了实例来演示如何编写 LED 驱动程序,帮助读者更好地理解 LED 的控制方法。

- 如何配置 LED 的端口, 包括 PC 和 PD 2 等, 以及如何在工程中生成代码和编译。
00:01 配置端口，使 LED 能被点亮
02:50 修改 LED 的输出电平，使其能被点亮
03:48将所有模块集合到一起，实现视频演示的效果

- 如何在电路中配置 LED 驱动, 以及如何通过代码控制 LED 的亮灭。
05:43 配置开发板，添加头文件和路径
08:57 在 main. H 中添加代码，定义变量
10:21 使用 GPIO 设置引脚电平，让所有 LED 熄灭

- 如何通过修改 gpo 状态来控制 led 灯的亮灭, 并给出了具体的操作代码。
11:25 讲解如何控制 LED 灯的开关
12:12 解释如何控制特定代的 LED 灯
16:30 讲解如何初始化 LED 灯并验证代码是否正确

- LED 灯的控制方法及其在蓝桥杯大赛中的应用, 同时分享了真题的下载方式。
17:03 学习蓝桥杯真题最有效最快速的办法
20:18 蓝桥杯官网有历届真题，搜索即可找到
22:17 录制视频过程中遇到各种问题，希望观众批评指正

###### 代码
```led.h
#ifndef _LED_H_
#define _LED_H_

#include "main.h"

//#define uchar unsigned char
typedef unsigned char uchar;//最好用 typedef 重定义数据类型，define 只替换不会进行类型检查




void LED_Disp(uchar dsLED);



#endif /*_LED_H_*/


```

```led.c
#include "led.h"

void LED_Disp(uchar dsLED)
{
	HAL_GPIO_WritePin(GPIOC,GPIO_PIN_All,GPIO_PIN_SET);//把GPIOC的全部引脚置高电平
	HAL_GPIO_WritePin(GPIOC,dsLED<<8,GPIO_PIN_RESET);//置低电平
	HAL_GPIO_WritePin(GPIOD,GPIO_PIN_2,GPIO_PIN_SET);
	HAL_GPIO_WritePin(GPIOD,GPIO_PIN_2,GPIO_PIN_RESET);//把锁存器打开，让数据过去

}

```


```main.c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "gpio.h"
#include "led.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#define uchar unsigned char
#define uint unsigned int

//typedef unsigned char uchar;


/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */
	LED_Disp(0x00);//LED的初始化,让所有LED都熄灭
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
		LED_Disp(0x01);
		HAL_Delay(500);
		LED_Disp(0x00);
		HAL_Delay(500);
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1);
  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLM = RCC_PLLM_DIV3;
  RCC_OscInitStruct.PLL.PLLN = 20;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/

```

```main.c改
/* USER CODE BEGIN Includes */
#define uchar unsigned char
#define uint unsigned int

//typedef unsigned char uchar;


/* USER CODE END Includes */

  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
		LED_Disp(0x01);
		HAL_Delay(500);
		LED_Disp(0x00);
		HAL_Delay(500);
  }
  /* USER CODE END 3 */
}
```



##### P 4 LCD 液晶显示

蓝桥杯官方给我们提供的库函数中有完整的 IO 配置初始化过程，省略了我们在 CubeMX 中的操作

弹幕：gpio.h 是 cubemx 配置的时候自动添加的例如 led 的引脚配置就在 gpio。h 里

sprint 函数的功能：把一个字符串打印到一个字符数组的里面

![[../../annex/STMCubeMX_image_12.png]]

###### 代码
```main.c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
//typedef unsigned char uchar;
/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  /* USER CODE BEGIN 2 */
	LED_Disp(0x00);//LED的初始化,让所有LED都熄灭
	LCD_Init();//LCD初始化
	
	LCD_Clear(Black);
	LCD_SetBackColor(Black);
	LCD_SetTextColor(White);
	
//	LCD_DisplayStringLine(Line0, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line1, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line2, (uint8_t *)"      LCD Test      ");
//	LCD_DisplayStringLine(Line3, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line4, (uint8_t *)"                    ");
//	
//	LCD_SetBackColor(White);
//	LCD_SetTextColor(Blue);

//	LCD_DisplayStringLine(Line5, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line6, (uint8_t *)"       HAL LIB      ");
//	LCD_DisplayStringLine(Line7, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line8, (uint8_t *)"         @80        ");
//	LCD_DisplayStringLine(Line9, (uint8_t *)"                    ");
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
		LED_Disp(0x01);
		HAL_Delay(500);
		LED_Disp(0x00);
		HAL_Delay(500);
		
		char text[30];
		uint i=5;
		sprintf(text,"CNBR:%d",i);//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line9, (uint8_t *)text);

  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1);
  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLM = RCC_PLLM_DIV3;
  RCC_OscInitStruct.PLL.PLLN = 20;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/


```

```main.c改
	LED_Disp(0x00);//LED的初始化,让所有LED都熄灭
	LCD_Init();//LCD初始化
	
	LCD_Clear(Black);
	LCD_SetBackColor(Black);
	LCD_SetTextColor(White);
	
	
	char text[30];
	uint i=5;
	sprintf(text,"CNBR:%d",i);//第二个参数是我们要打印的内容
	LCD_DisplayStringLine(Line9, (uint8_t *)text);
```


##### P 5 定时器-按键单击

弹幕：应该是79才是工作频率1MHz

100 hz-10 ms 中断一次
![[../../annex/STMCubeMX_image_13.png]]
80,000,000÷80=1,000,000
1,000,000,÷10000=100--10 ms
需要减 1 才是准确的。（从 0 开始，所以要 -1）
![[../../annex/STMCubeMX_image_14.png]]
弹幕：频率分之一是周期

![[../../annex/STMCubeMX_image_15.png]]

![[../../annex/STMCubeMX_image_16.png]]
写中断回调函数

弹幕：delay 会阻塞进程
弹幕：这里 key 初始化是不是少了一个按键不是四个？  0 0 0 0
```
弹幕：并不是，Key[4]表示创造了4个这样的结构体每个结构体默认是对应的那3个其实010应该更合理
```
弹幕：默认给四个0了，写一个0也可以
弹幕：少一个零不要紧，会自动补上
弹幕：没有少0勒，就是这样的。结构体里只有三个参数当然只能赋值三个0啊。key4是因为有四个按键。每个按键都配有 struct 里的三个参数

弹幕：那 break 不会跳出吗？
弹幕：就是需要 break 跳出，这样就可以10ms 后再进入循环
弹幕：消抖用的一直都是延时，up 这个也是因为定时器配置已经算好了时间，while 空循环只是一个截止.....

![[../../annex/Pasted image 20240207163430.png]]

弹幕：为什么定义用 keys  key【】
弹幕：到这觉得用外部中断更好
弹幕：你们看看 LCD_Init 还在不在
弹幕：因为 CPU 在执行延时函数，你把 led 的去掉就好了

弹幕：问：为什么没有 case3呢（有答案戳我一下，多谢）答：case0判断按钮有没有按下，case1消抖功能，case2判断按钮已经松开，所以不需要 case3

弹幕：用 HAL_Delay 会影响主进程
弹幕：HAL_Delay 是延时函数，不具备有定时器的实时性，简单来说就是跟主函数一起运行的功能
弹幕：有外接上拉电阻，可以不用配置上拉
弹幕：上拉是设置按键按下低电平有效为默认状态
弹幕：没必要上拉，外部已经上拉了。
弹幕：如果用 Delay 来消抖的话，代码会停在 delay 函数里面，一个还行，如果是多个的话，程序运行会卡
弹幕：中断服务函数是 IRQ 这个是回调
弹幕：外部中断只能配3个按键所以不用外部中断


###### 代码

```interrupt.c
#include "interrupt.h"

struct keys key[4]={0,0,0};//有4个按键，声明成结构体数组
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
	if(htim->Instance == TIM3)
	{
		key[0].key_sta = HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_0);//板载第一个按键
		key[1].key_sta = HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_1);//板载第一个按键
		key[2].key_sta = HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_2);//板载第一个按键
		key[3].key_sta = HAL_GPIO_ReadPin(GPIOA,GPIO_PIN_0);//板载第一个按键
		
		for(int i=0;i<4;i++)//for循环，轮询按键
		{
			switch(key[i].judge_sta)
			{
				case 0:
				{
					if(key[i].key_sta == 0) key[i].judge_sta = 1;//存储到的数为0的话则按键按下
				}
				break;
				case 1:
				{
						if(key[i].key_sta == 0) //消抖过程
						{
							key[i].judge_sta = 2;//如果10ms后又判断。还是0，则证明按键真的被按下了
							key[i].single_flag = 1;
						}
						else
						{
							key[i].judge_sta = 0;
						}
				}
				break;
				case 2:
				{
					if(key[i].key_sta == 1)//按键松开
					{
						key[i].judge_sta = 0;//清零
					}
				}
				break;
			}
		}
	}
}


```

```interrupt.h
#ifndef _INTERRUPT_H_
#define _INTERRUPT_H_

#include "main.h"
#include "stdbool.h"

typedef unsigned char uchar;

struct keys
{
	uchar judge_sta;//判断进行到哪一步了
	bool key_sta;//用bool类型是为了节约空间（只用0和1的状态）//识别按键被按下，记录为0
	bool single_flag;//被确认按键被按下则为1
};





void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim);


#endif /*_INTERRUPT_H_*/


```

```main.c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "tim.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
//typedef unsigned char uchar;
/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM3_Init();
  /* USER CODE BEGIN 2 */
	LED_Disp(0x00);//LED的初始化,让所有LED都熄灭
	LCD_Init();//LCD初始化
	
	LCD_Clear(Black);
	LCD_SetBackColor(Black);
	LCD_SetTextColor(White);
	
//	LCD_DisplayStringLine(Line0, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line1, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line2, (uint8_t *)"      LCD Test      ");
//	LCD_DisplayStringLine(Line3, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line4, (uint8_t *)"                    ");
//	
//	LCD_SetBackColor(White);
//	LCD_SetTextColor(Blue);

//	LCD_DisplayStringLine(Line5, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line6, (uint8_t *)"       HAL LIB      ");
//	LCD_DisplayStringLine(Line7, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line8, (uint8_t *)"         @80        ");
//	LCD_DisplayStringLine(Line9, (uint8_t *)"                    ");

	HAL_TIM_Base_Start_IT(&htim3);//打开定时器3
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
		LED_Disp(0x01);
		HAL_Delay(500);
		LED_Disp(0x00);
		HAL_Delay(500);
		
		char text[30];
		uint i=5;
		sprintf(text,"CNBR:%d",i);//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line9, (uint8_t *)text);
		
		if(key[0].single_flag == 1)//按键被按下
		{
			sprintf(text,"  key0down     ");
			LCD_DisplayStringLine(Line8, (uint8_t *)text);
			
			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
		if(key[1].single_flag == 1)//按键被按下
		{
			sprintf(text,"  key1down     ");
			LCD_DisplayStringLine(Line8, (uint8_t *)text);
			
			key[1].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}

  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1);
  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLM = RCC_PLLM_DIV3;
  RCC_OscInitStruct.PLL.PLLN = 20;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/

```

```main.c改
#include "interrupt.h"

extern struct keys key[];

HAL_TIM_Base_Start_IT(&htim3);//打开定时器3
	
	if(key[0].single_flag == 1)//按键被按下
	{
		sprintf(text,"  key0down     ");
		LCD_DisplayStringLine(Line8, (uint8_t *)text);
		
		key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
	}
	if(key[1].single_flag == 1)//按键被按下
	{
		sprintf(text,"  key1down     ");
		LCD_DisplayStringLine(Line8, (uint8_t *)text);
		
		key[1].single_flag = 0;//做完以后标志位清零,避免被重复执行
	}

```




##### P 6 定时器-长按键、双击

弹幕：记得在 key_sta == 1的 if 末尾把 key_time 归零

![[../../annex/Pasted image 20240207172405.png]]


###### 代码

```interrupt.c
#include "interrupt.h"

struct keys key[4]={0,0,0};//有4个按键，声明成结构体数组
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
	if(htim->Instance == TIM3)
	{
		key[0].key_sta = HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_0);//板载第一个按键
		key[1].key_sta = HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_1);//板载第一个按键
		key[2].key_sta = HAL_GPIO_ReadPin(GPIOB,GPIO_PIN_2);//板载第一个按键
		key[3].key_sta = HAL_GPIO_ReadPin(GPIOA,GPIO_PIN_0);//板载第一个按键
		
		for(int i=0;i<4;i++)//for循环，轮询按键
		{
			switch(key[i].judge_sta)
			{
				case 0:
				{
					if(key[i].key_sta == 0) 
					{
						key[i].judge_sta = 1;
						key[i].key_time = 0;//时间清零
					}
				}
				break;
				case 1:
				{
						if(key[i].key_sta == 0) //消抖过程,不能判断短按键，会跟长按键冲突
						{
							key[i].judge_sta = 2;//如果10ms后又判断。还是0，则证明按键真的被按下了
						}
						else
						{
							key[i].judge_sta = 0;
						}
				}
				break;
				case 2:
				{
					if(key[i].key_sta == 1)//按键松开
					{
						key[i].judge_sta = 0;//清零
						if(key[i].key_time<70)//<700ms,10ms加一次
						{
							key[i].single_flag = 1;//为短按
						}
						key[i].key_time = 0;
					}
					else //在没有松开的一大段时间里，让时间+
					{
						key[i].key_time++;//确定究竟被按多长时间
						if(key[i].key_time>70)//>700ms,10ms加一次
						{
							key[i].long_flag = 1;//为长按
						}
						key[i].key_time = 0;
					}
				}
				break;
			}
		}
	}
}


```

```interrupt.h
#ifndef _INTERRUPT_H_
#define _INTERRUPT_H_

#include "main.h"
#include "stdbool.h"

typedef unsigned char uchar;
typedef unsigned int uint;

struct keys
{
	uchar judge_sta;//判断进行到哪一步了
	bool key_sta;//用bool类型是为了节约空间（只用0和1的状态）//识别按键被按下，记录为0
	bool single_flag;//被确认按键被按下则为1
	bool long_flag;
	uint key_time;
};


void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim);


#endif /*_INTERRUPT_H_*/

```

```main.c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "tim.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
uchar view = 0;
void key_proc(void);
void disp_proc(void);

//typedef unsigned char uchar;
/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM3_Init();
  /* USER CODE BEGIN 2 */
	LED_Disp(0x00);//LED的初始化,让所有LED都熄灭
	LCD_Init();//LCD初始化
	
	LCD_Clear(Black);
	LCD_SetBackColor(Black);
	LCD_SetTextColor(White);
	
//	LCD_DisplayStringLine(Line0, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line1, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line2, (uint8_t *)"      LCD Test      ");
//	LCD_DisplayStringLine(Line3, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line4, (uint8_t *)"                    ");
//	
//	LCD_SetBackColor(White);
//	LCD_SetTextColor(Blue);

//	LCD_DisplayStringLine(Line5, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line6, (uint8_t *)"       HAL LIB      ");
//	LCD_DisplayStringLine(Line7, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line8, (uint8_t *)"         @80        ");
//	LCD_DisplayStringLine(Line9, (uint8_t *)"                    ");

	HAL_TIM_Base_Start_IT(&htim3);//打开定时器3
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
//		LED_Disp(0x01);
//		HAL_Delay(500);
//		LED_Disp(0x00);
//		HAL_Delay(500);
		
//		char text[30];
//		uint i=5;
//		sprintf(text,"CNBR:%d",i);//第二个参数是我们要打印的内容
//		LCD_DisplayStringLine(Line9, (uint8_t *)text);
//		
//		if(key[0].single_flag == 1)//按键被按下
//		{
//			sprintf(text,"  key0down     ");
//			LCD_DisplayStringLine(Line8, (uint8_t *)text);
//			
//			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
//		}
//		if(key[1].single_flag == 1)//按键被按下
//		{
//			sprintf(text,"  key1down     ");
//			LCD_DisplayStringLine(Line8, (uint8_t *)text);
//			
//			key[1].single_flag = 0;//做完以后标志位清零,避免被重复执行
//		}
		
		key_proc();//先判断按键
		disp_proc();//判断完显示

  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1);
  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLM = RCC_PLLM_DIV3;
  RCC_OscInitStruct.PLL.PLLN = 20;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */
void key_proc(void)//按键按下的过程
{
		if(key[0].single_flag == 1)//按键被按下
		{
			view = !view;
			LCD_Clear(Black);
			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	}
	if(view == 1)
	{
		char text[30];
		sprintf(text,"      Para");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	}
}	
/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/

```

```main.c改
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#define uchar unsigned char
#define uint unsigned int

extern struct keys key[];
uchar view = 0;
void key_proc(void);
void disp_proc(void);

		key_proc();//先判断按键
		disp_proc();//判断完显示

void key_proc(void)//按键按下的过程
{
		if(key[0].single_flag == 1)//按键被按下
		{
			view = !view;
			LCD_Clear(Black);
			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	}
	if(view == 1)
	{
		char text[30];
		sprintf(text,"      Para");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	}
}	


```




##### P 7 定时器-PWM 输出

BKIN 不能用，只能选 CH 1 的，上一节用 TIM3 用作按键定时器，选 TIM 16 的 CH 1 。带 N 的输出互补 PWM。

![[../../annex/Pasted image 20240207175709.png]]
互补 PWM，正常高则地，正常低则高。（在三相控制、Bug boss 电路模型中常见）

分屏系数和重装载值确定：
时钟 80 M，要得到 100 Hz 的。目的要将这个数经过两次相除得到 100
为了方便，把重装载值设置为 100。然后把分频系数设置为 8000（分频系数为 0~65535）.（记得-1）

还有一个关于 PWM 生成的相关参数，Pulse--相当于占空比（可以在程序当中进行调节）

![[../../annex/Pasted image 20240207180337.png]]


第二个要 200 Hz
![[../../annex/Pasted image 20240207180618.png]]

弹幕：前面闪烁的，可以在按键函数那里加 LCD_Clear
```
弹幕：如果需要显示%的话记得在 LCD 显示那个写成%d%%
```


###### 代码

```main.c
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2024 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */
/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "tim.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
uchar view = 0;
void key_proc(void);
void disp_proc(void);

uchar pa6_duty = 10;//定义两个变量，分别是两个通道PWM的占空比
uchar pa7_duty = 10;

//typedef unsigned char uchar;
/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_TIM3_Init();
  MX_TIM16_Init();
  MX_TIM17_Init();
  /* USER CODE BEGIN 2 */
	LED_Disp(0x00);//LED的初始化,让所有LED都熄灭
	LCD_Init();//LCD初始化
	
	LCD_Clear(Black);
	LCD_SetBackColor(Black);
	LCD_SetTextColor(White);
	
//	LCD_DisplayStringLine(Line0, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line1, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line2, (uint8_t *)"      LCD Test      ");
//	LCD_DisplayStringLine(Line3, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line4, (uint8_t *)"                    ");
//	
//	LCD_SetBackColor(White);
//	LCD_SetTextColor(Blue);

//	LCD_DisplayStringLine(Line5, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line6, (uint8_t *)"       HAL LIB      ");
//	LCD_DisplayStringLine(Line7, (uint8_t *)"                    ");
//	LCD_DisplayStringLine(Line8, (uint8_t *)"         @80        ");
//	LCD_DisplayStringLine(Line9, (uint8_t *)"                    ");

	HAL_TIM_Base_Start_IT(&htim3);//打开定时器3
	
	HAL_TIM_PWM_Start(&htim16,TIM_CHANNEL_1);//第一个参数是使用哪个定时器,第二个参数是通道几
	HAL_TIM_PWM_Start(&htim17,TIM_CHANNEL_1);//两个定时器的PWM输出的开启

  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
//		LED_Disp(0x01);
//		HAL_Delay(500);
//		LED_Disp(0x00);
//		HAL_Delay(500);
		
//		char text[30];
//		uint i=5;
//		sprintf(text,"CNBR:%d",i);//第二个参数是我们要打印的内容
//		LCD_DisplayStringLine(Line9, (uint8_t *)text);
//		
//		if(key[0].single_flag == 1)//按键被按下
//		{
//			sprintf(text,"  key0down     ");
//			LCD_DisplayStringLine(Line8, (uint8_t *)text);
//			
//			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
//		}
//		if(key[1].single_flag == 1)//按键被按下
//		{
//			sprintf(text,"  key1down     ");
//			LCD_DisplayStringLine(Line8, (uint8_t *)text);
//			
//			key[1].single_flag = 0;//做完以后标志位清零,避免被重复执行
//		}
		
		key_proc();//先判断按键
		disp_proc();//判断完显示

  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};

  /** Configure the main internal regulator output voltage
  */
  HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1);
  /** Initializes the RCC Oscillators according to the specified parameters
  * in the RCC_OscInitTypeDef structure.
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSE;
  RCC_OscInitStruct.HSEState = RCC_HSE_ON;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSE;
  RCC_OscInitStruct.PLL.PLLM = RCC_PLLM_DIV3;
  RCC_OscInitStruct.PLL.PLLN = 20;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV2;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB buses clocks
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_2) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */
void key_proc(void)//按键按下的过程
{
		if(key[0].single_flag == 1)//按键被按下
		{
			view = !view;
			LCD_Clear(Black);
			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
		
		if(key[1].single_flag == 1)//按键被按下
		{
			LCD_Clear(Black);
			pa6_duty += 10;
			if(pa6_duty>=100) pa6_duty = 10;
			__HAL_TIM_SetCompare(&htim16,TIM_CHANNEL_1,pa6_duty);
			key[1].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
		if(key[2].single_flag == 1)//按键被按下
		{
			LCD_Clear(Black);
			pa7_duty += 10;
			if(pa7_duty>=100) pa7_duty = 10;
			__HAL_TIM_SetCompare(&htim17,TIM_CHANNEL_1,pa7_duty);
			key[2].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	}
	if(view == 1)
	{
		char text[30];
		sprintf(text,"      Para");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
		sprintf(text,"    PA6:%d%%",pa6_duty);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"    PA7:%d%%",pa7_duty);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
	}
}	
/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */
  __disable_irq();
  while (1)
  {
  }
  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     ex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/

```

```main.c改

uchar pa6_duty = 10;//定义两个变量，分别是两个通道PWM的占空比
uchar pa7_duty = 10;

	HAL_TIM_PWM_Start(&htim16,TIM_CHANNEL_1);//第一个参数是使用哪个定时器,第二个参数是通道几
	HAL_TIM_PWM_Start(&htim17,TIM_CHANNEL_1);//两个定时器的PWM输出的开启

		key_proc();//先判断按键
		disp_proc();//判断完显示

void key_proc(void)//按键按下的过程
{
		if(key[0].single_flag == 1)//按键被按下
		{
			view = !view;
			LCD_Clear(Black);
			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
		
		if(key[1].single_flag == 1)//按键被按下
		{
			LCD_Clear(Black);
			pa6_duty += 10;
			if(pa6_duty>=100) pa6_duty = 10;
			__HAL_TIM_SetCompare(&htim16,TIM_CHANNEL_1,pa6_duty);
			key[1].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
		if(key[2].single_flag == 1)//按键被按下
		{
			LCD_Clear(Black);
			pa7_duty += 10;
			if(pa7_duty>=100) pa7_duty = 10;
			__HAL_TIM_SetCompare(&htim17,TIM_CHANNEL_1,pa7_duty);
			key[2].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	}
	if(view == 1)
	{
		char text[30];
		sprintf(text,"      Para");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
		sprintf(text,"    PA6:%d%%",pa6_duty);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"    PA7:%d%%",pa7_duty);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
	}
```



##### P 8 定时器-输入捕获（频率、占空比测量）

捕获计数器的用法、检测频率和占空比，弥补没有示波器的尴尬情况
开发板上有一个信号发生器，555 芯片，可以输出一个频率可调的方波信号。分别通过跳线帽接到芯片上的 PA 15 和 PB 4 引脚。
![[../../annex/Pasted image 20240207190043.png]]

配置 PA 15、PB 4，同样选择 CH 1 通道。

```
弹幕：为什么在这个时候 TIM3可以使用了？
弹幕：tim3不是用作中断了吗？
弹幕：是的，所以会有冲突，可以把按键的定时器改成未使用的
```

![[../../annex/Pasted image 20240207191022.png]]
作为输入模式用。


```
弹幕：为什么 TIM3之前已经用来定时器中断了，现在还能用来输入捕获啊
弹幕：当时用 tim3 的时候是随便用的现在用 tim3 是题目要求的所以现在用 tim3 的话那之前的按键就失灵了我们只要换个没用的定时器来中断就行了
弹幕：UPonenote没有压感，可能是笔驱动。和软件的驱动相冲突。可以把笔的驱动在任务管理器中完全停掉。
```


![[../../annex/Pasted image 20240207191349.png]]

这次程序同样要写到 interrupt 中，因为同样属于中断，写在一起避免重复建文件


















































































































































































































