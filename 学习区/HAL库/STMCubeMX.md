

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

![[../../annex/STMCubeMX_image_17.png]]

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

![[../../annex/STMCubeMX_image_18.png]]

![[../../annex/STMCubeMX_image_19.png]]


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

![[../../annex/STMCubeMX_image_20.png]]
互补 PWM，正常高则地，正常低则高。（在三相控制、Bug boss 电路模型中常见）

分屏系数和重装载值确定：
时钟 80 M，要得到 100 Hz 的。目的要将这个数经过两次相除得到 100
为了方便，把重装载值设置为 100。然后把分频系数设置为 8000（分频系数为 0~65535）.（记得-1）

还有一个关于 PWM 生成的相关参数，Pulse--相当于占空比（可以在程序当中进行调节）

![[../../annex/STMCubeMX_image_21.png]]


第二个要 200 Hz
![[../../annex/STMCubeMX_image_22.png]]

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
![[../../annex/STMCubeMX_image_23.png]]

配置 PA 15、PB 4，同样选择 CH 1 通道。

```
弹幕：为什么在这个时候 TIM3可以使用了？
弹幕：tim3不是用作中断了吗？
弹幕：是的，所以会有冲突，可以把按键的定时器改成未使用的
```

![[../../annex/STMCubeMX_image_24.png]]
作为输入模式用。


```
弹幕：为什么 TIM3之前已经用来定时器中断了，现在还能用来输入捕获啊
弹幕：当时用 tim3 的时候是随便用的现在用 tim3 是题目要求的所以现在用 tim3 的话那之前的按键就失灵了我们只要换个没用的定时器来中断就行了
弹幕：UPonenote没有压感，可能是笔驱动。和软件的驱动相冲突。可以把笔的驱动在任务管理器中完全停掉。
```


![[../../annex/STMCubeMX_image_25.png]]

这次程序同样要写到 interrupt 中，因为同样属于中断，写在一起避免重复建文件

按键失效问题：换一个暂时用不到的定时器来作为按键的中断。

![[../../annex/STMCubeMX_image_26.png]]

![[../../annex/STMCubeMX_image_27.png]]

配置：另外找一个通道（channel 2），配置成间接模式（input capture indirect mode）。
![[../../annex/STMCubeMX_image_28.png]]
下面我们要直接的去测上升沿，间接的去测下降沿。即可实现测量占空比的功能。

弹幕：这里为什么不是高电平时间除以总时间而是除以低电平时间搞不懂

![[../../annex/STMCubeMX_image_29.png]]
![[../../annex/STMCubeMX_image_30.png]]

![[../../annex/STMCubeMX_image_31.png]]

![[../../annex/STMCubeMX_image_32.png]]

![[../../annex/STMCubeMX_image_33.png]]

![[../../annex/STMCubeMX_image_34.png]]


![[../../annex/STMCubeMX_image_35.png]]

弹幕：兄弟们切屏有上一个屏幕的残留怎么办
弹幕：在你的 main.c 里的确认按键按下的函数（up 写的 key_proc 函数）里面加上 LCD_Clear(Black);

###### 代码

```interrupt.c
#include "interrupt.h"

struct keys key[4]={0,0,0};//有4个按键，声明成结构体数组
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)//中断回调函数
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


uint ccrl_val1 = 0,ccrl_val2 = 0;
uint frq1 = 0,frq2 = 0;

//然后写中断回调函数
void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)
{
		//首先要判断这个中断是来自哪个定时器，因为用到了TIM2和TIM3。至于判断，参照上面按键的判断方法
	if(htim->Instance == TIM2)	
	{
		ccrl_val1 = HAL_TIM_ReadCapturedValue(htim,TIM_CHANNEL_1);//把定时器的计时值读到一个变量中
		//通过这个可以把定时器锁定到TIM2中，读到它的计时值
		__HAL_TIM_SetCounter(htim,0);//读到之后让计时值清零
		frq1 = (80000000/80)/ccrl_val1;//÷分频系数80再÷计时值
		HAL_TIM_IC_Start(htim,TIM_CHANNEL_1);
	}
	
	if(htim->Instance == TIM3)	
	{
		ccrl_val2 = HAL_TIM_ReadCapturedValue(htim,TIM_CHANNEL_1);//把定时器的计时值读到一个变量中
		//通过这个可以把定时器锁定到TIM2中，读到它的计时值
		__HAL_TIM_SetCounter(htim,0);//读到之后让计时值清零
		frq2 = (80000000/80)/ccrl_val2;//÷分频系数80再÷计时值
		HAL_TIM_IC_Start(htim,TIM_CHANNEL_1);
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
void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim);


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
extern uint frq1,frq2;//就是计算得到的两个频率值
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
  MX_TIM2_Init();
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
	
	HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_1);//频率测量捕获定时器开启
	HAL_TIM_IC_Start_IT(&htim3,TIM_CHANNEL_1);	

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
	
		sprintf(text,"      FRQ1=%d",frq1);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"      FRQ2=%d",frq2);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
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
extern uint frq1,frq2;//就是计算得到的两个频率值

	HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_1);//频率测量捕获定时器开启
	HAL_TIM_IC_Start_IT(&htim3,TIM_CHANNEL_1);	

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
	
		sprintf(text,"      FRQ1=%d",frq1);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"      FRQ2=%d",frq2);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
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

```





##### P 9 模数转换 ADC

硬件有一个滑动变阻器，通过一个跳接帽连接到 IO 口上。
![[../../annex/STMCubeMX_image_36.png]]

![[../../annex/STMCubeMX_image_37.png]]

![[../../annex/STMCubeMX_image_38.png]]

创建文件不能直接写 adc. C，会冲突。

![[../../annex/STMCubeMX_image_39.png]]

V1：如果显示有白块，转化成英文再输入“:”


###### 代码

```bsp_adc.c
#include "bsp_adc.h"

//读取ADC值的函数，5行代码即可
double getADC(ADC_HandleTypeDef *pin)
{
	uint adc;
	HAL_ADC_Start(pin);//内置函数，开启ADC
	//下一步把读取的ADC值读取回来，存到adc变量中
	adc = HAL_ADC_GetValue(pin);
	return adc*3.3/4096;//读取完后进行一个简单的运算就可以把值返回到主程序中去了
	//为什么是*3.3？因为ADC的实际原理就是把一个读取到的电压值均分一个等份，然后看3.3v（电源电压）占的比例。
	//为什么是4096？主要是它是一个12位精度（ADC 12bit resolution）即4096（2^12=4096）
	//这个值（adc）实际上就是一个分数的关系，通过这样一个运算就得到了一个具体的电压值
}

```

```bsp_adc.h
#ifndef _BSP_ADC_H_
#define _BSP_ADC_H_

#include "main.h"

typedef unsigned int uint;

double getADC(ADC_HandleTypeDef *pin);




#endif /*_BSP_ADC_H_*/

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
#include "adc.h"
#include "tim.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#include "bsp_adc.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
extern uint frq1,frq2;//就是计算得到的两个频率值
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
  MX_TIM2_Init();
  MX_ADC1_Init();
  MX_ADC2_Init();
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
	
	HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_1);//频率测量捕获定时器开启
	HAL_TIM_IC_Start_IT(&htim3,TIM_CHANNEL_1);	

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
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

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
  /** Initializes the peripherals clocks
  */
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_ADC12;
  PeriphClkInit.Adc12ClockSelection = RCC_ADC12CLKSOURCE_SYSCLK;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
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
	
		sprintf(text,"      FRQ1=%d",frq1);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"      FRQ2=%d",frq2);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
		sprintf(text,"      V1:=%.2f    ",getADC(&hadc1));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line6, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		sprintf(text,"      V2:=%.2f    ",getADC(&hadc2));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line7, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		//V1：如果显示有白块，转化成英文再输入:
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

```main,c改
#include "bsp_adc.h"

		sprintf(text,"      V1:=%.2f    ",getADC(&hadc1));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line6, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		sprintf(text,"      V2:=%.2f    ",getADC(&hadc2));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line7, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		//V1：如果显示有白块，转化成英文再输入:
```



##### P 10 IIC 通信--eeprom 读写

![[../../annex/STMCubeMX_image_40.png]]
24C02 是一个存储芯片（eeprom），4017T 是一个数字电位器

用 GPIO_Output，比赛的时候官方回提供一个 IIC 的库，这个库可以直接用，这个库中用的是软件 IIC

![[../../annex/STMCubeMX_image_41.png]]
在 IIC 上可以挂无数个芯片，然后通过地址来识别对应叫到哪个芯片然后进行通信。

首先要对官方库进行移植（iic_hal. C）基本上已经写好了，我们只须添加读写的函数即可。

![[../../annex/STMCubeMX_image_42.png]]
1 K/2 K 是容量。（01 就是 1 K，02 就是 2 K，以此类推）
02，所以地址是 1010 A2 A1 A0 R/W。
只需要看第一行。在最初始状态是不确定的
![[../../annex/STMCubeMX_image_43.png]]

![[../../annex/STMCubeMX_image_44.png]]
E1 E2 E3 对应 A0 A1 A2。接地，相当于是 0。

读 1，写 0。
为什么有 3 个不确定是位？比如有 8 个 EEPROM 芯片, 用以区分不同的芯片。可以同时来控制 8 个相同的芯片，因为 IIC 总线是可以复用的，一个总线上可以挂很多个芯片

弹幕：AT24C02地址的组成，由固定位（1010）和可编程位（就是自己定义，但原理图上都已经接地了其实就是000）以及最后的读写位决定
![[../../annex/STMCubeMX_image_45.png]]


![[../../annex/STMCubeMX_image_46.png]]

弹幕：但芯片手册上的是需要 SendNotAck，希望 up 解答一下
弹幕：WaitAck()和 SendNotAck()都行
弹幕：但芯片手册上的是需要 SendNotAck，希望 up 解答一下
弹幕：大佬们 uint 不是占4字节，32位吗？求解答
弹幕：好像是因为 keil 编译器的原因，long int 就是32位
弹幕：int 的定义不一样，Keil 里就是16位

弹幕：读的是低八位，读的时候把高八位右移八位，显示的时候肯定要往左移
弹幕：左移：高位丢弃，低位补0



![[../../annex/STMCubeMX_image_47.png]]

![[../../annex/STMCubeMX_image_48.png]]

![[../../annex/STMCubeMX_image_49.png]]

![[../../annex/STMCubeMX_image_50.png]]

![[../../annex/STMCubeMX_image_51.png]]

![[../../annex/STMCubeMX_image_52.png]]

![[../../annex/STMCubeMX_image_53.png]]


###### 代码

```i2c_hal.c
/*
  程序说明: CT117E-M4嵌入式竞赛板GPIO模拟I2C总线驱动程序
  软件环境: MDK-ARM HAL库
  硬件环境: CT117E-M4嵌入式竞赛板
  日    期: 2020-3-1
*/

#include "i2c_hal.h"//#include "i2c - hal.h"

#define DELAY_TIME	20

/**
  * @brief SDA线输入模式配置
  * @param None
  * @retval None
  */
void SDA_Input_Mode()
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    GPIO_InitStructure.Pin = GPIO_PIN_7;
    GPIO_InitStructure.Mode = GPIO_MODE_INPUT;
    GPIO_InitStructure.Pull = GPIO_PULLUP;
    GPIO_InitStructure.Speed = GPIO_SPEED_FREQ_HIGH;
    HAL_GPIO_Init(GPIOB, &GPIO_InitStructure);
}

/**
  * @brief SDA线输出模式配置
  * @param None
  * @retval None
  */
void SDA_Output_Mode()
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    GPIO_InitStructure.Pin = GPIO_PIN_7;
    GPIO_InitStructure.Mode = GPIO_MODE_OUTPUT_OD;
    GPIO_InitStructure.Pull = GPIO_NOPULL;
    GPIO_InitStructure.Speed = GPIO_SPEED_FREQ_HIGH;
    HAL_GPIO_Init(GPIOB, &GPIO_InitStructure);
}

/**
  * @brief SDA线输出一个位
  * @param val 输出的数据
  * @retval None
  */
void SDA_Output( uint16_t val )
{
    if ( val )
    {
        GPIOB->BSRR |= GPIO_PIN_7;
    }
    else
    {
        GPIOB->BRR |= GPIO_PIN_7;
    }
}

/**
  * @brief SCL线输出一个位
  * @param val 输出的数据
  * @retval None
  */
void SCL_Output( uint16_t val )
{
    if ( val )
    {
        GPIOB->BSRR |= GPIO_PIN_6;
    }
    else
    {
        GPIOB->BRR |= GPIO_PIN_6;
    }
}

/**
  * @brief SDA输入一位
  * @param None
  * @retval GPIO读入一位
  */
uint8_t SDA_Input(void)
{
	if(HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_7) == GPIO_PIN_SET){
		return 1;
	}else{
		return 0;
	}
}


/**
  * @brief I2C的短暂延时
  * @param None
  * @retval None
  */
static void delay1(unsigned int n)
{
    uint32_t i;
    for ( i = 0; i < n; ++i);
}

/**
  * @brief I2C起始信号
  * @param None
  * @retval None
  */
void I2CStart(void)
{
    SDA_Output(1);
    delay1(DELAY_TIME);
    SCL_Output(1);
    delay1(DELAY_TIME);
    SDA_Output(0);
    delay1(DELAY_TIME);
    SCL_Output(0);
    delay1(DELAY_TIME);
}

/**
  * @brief I2C结束信号
  * @param None
  * @retval None
  */
void I2CStop(void)
{
    SCL_Output(0);
    delay1(DELAY_TIME);
    SDA_Output(0);
    delay1(DELAY_TIME);
    SCL_Output(1);
    delay1(DELAY_TIME);
    SDA_Output(1);
    delay1(DELAY_TIME);

}

/**
  * @brief I2C等待确认信号
  * @param None
  * @retval None
  */
unsigned char I2CWaitAck(void)
{
    unsigned short cErrTime = 5;
    SDA_Input_Mode();
    delay1(DELAY_TIME);
    SCL_Output(1);
    delay1(DELAY_TIME);
    while(SDA_Input())
    {
        cErrTime--;
        delay1(DELAY_TIME);
        if (0 == cErrTime)
        {
            SDA_Output_Mode();
            I2CStop();
            return ERROR;
        }
    }
    SDA_Output_Mode();
    SCL_Output(0);
    delay1(DELAY_TIME);
    return SUCCESS;
}

/**
  * @brief I2C发送确认信号
  * @param None
  * @retval None
  */
void I2CSendAck(void)
{
    SDA_Output(0);
    delay1(DELAY_TIME);
    delay1(DELAY_TIME);
    SCL_Output(1);
    delay1(DELAY_TIME);
    SCL_Output(0);
    delay1(DELAY_TIME);

}

/**
  * @brief I2C发送非确认信号
  * @param None
  * @retval None
  */
void I2CSendNotAck(void)
{
    SDA_Output(1);
    delay1(DELAY_TIME);
    delay1(DELAY_TIME);
    SCL_Output(1);
    delay1(DELAY_TIME);
    SCL_Output(0);
    delay1(DELAY_TIME);

}

/**
  * @brief I2C发送一个字节
  * @param cSendByte 需要发送的字节
  * @retval None
  */
void I2CSendByte(unsigned char cSendByte)
{
    unsigned char  i = 8;
    while (i--)
    {
        SCL_Output(0);
        delay1(DELAY_TIME);
        SDA_Output(cSendByte & 0x80);
        delay1(DELAY_TIME);
        cSendByte += cSendByte;
        delay1(DELAY_TIME);
        SCL_Output(1);
        delay1(DELAY_TIME);
    }
    SCL_Output(0);
    delay1(DELAY_TIME);
}

/**
  * @brief I2C接收一个字节
  * @param None
  * @retval 接收到的字节
  */
unsigned char I2CReceiveByte(void)
{
    unsigned char i = 8;
    unsigned char cR_Byte = 0;
    SDA_Input_Mode();
    while (i--)
    {
        cR_Byte += cR_Byte;
        SCL_Output(0);
        delay1(DELAY_TIME);
        delay1(DELAY_TIME);
        SCL_Output(1);
        delay1(DELAY_TIME);
        cR_Byte |=  SDA_Input();
    }
    SCL_Output(0);
    delay1(DELAY_TIME);
    SDA_Output_Mode();
    return cR_Byte;
}

//
void I2CInit(void)
{
    GPIO_InitTypeDef GPIO_InitStructure = {0};

    GPIO_InitStructure.Pin = GPIO_PIN_7 | GPIO_PIN_6;
    GPIO_InitStructure.Mode = GPIO_MODE_OUTPUT_PP;
    GPIO_InitStructure.Pull = GPIO_PULLUP;
    GPIO_InitStructure.Speed = GPIO_SPEED_FREQ_HIGH;
    HAL_GPIO_Init(GPIOB, &GPIO_InitStructure);
}


uchar eeprom_read(uchar addr)//传参，它的地址
{
	unsigned char dat;
	I2CStart();//第一步，开启
	I2CSendByte(0xa0);//第二步，联系芯片(把数据发送出去),传参地址应该是0xa0（先放1个写,因为要先联系芯片，并告诉它要读哪个引脚,告诉它的过程就相当于写）
	I2CWaitAck();//写完之后，等待应答
	I2CSendByte(addr);//发送要读取的地址,要发的是addr地址,这是一个参数，到时候调用的时候传递过来了
	I2CWaitAck();//发完之后再来一个等待
	I2CStop();//IIC可以停止歇歇
	
	I2CStart();//停止完再打开
	I2CSendByte(0xa1);//读
	I2CWaitAck();//发送完后，继续等待
	dat = I2CReceiveByte();//等待完就可以接收了
	I2CWaitAck();
	I2CStop();//读取完毕，停止下来
	
	return dat;//把结果返回
}

void eeprom_write(uchar addr,uchar dat)//写函数，没有返回值//这个数据必须是8位数，芯片的要求
{
	I2CStart();//开启
	I2CSendByte(0xa0);//联系芯片(把数据发送出去),传参地址应该是0xa0（先放1个写,因为要先联系芯片，并告诉它要读哪个引脚,告诉它的过程就相当于写）
	I2CWaitAck();//联系完后，等待应答
	I2CSendByte(addr);//发送要写入的地址,要写的是addr地址,这是一个参数，到时候调用的时候传递过来了
	I2CWaitAck();//等待应答
	
	I2CSendByte(dat);//直接发送给芯片，不停止了
	I2CWaitAck();//发完之后再来个等待
	I2CStop();
}


```

```i2c_hal.h
#ifndef __I2C_HAL_H
#define __I2C_HAL_H

#include "stm32g4xx_hal.h"

void I2CStart(void);
void I2CStop(void);
unsigned char I2CWaitAck(void);
void I2CSendAck(void);
void I2CSendNotAck(void);
void I2CSendByte(unsigned char cSendByte);
unsigned char I2CReceiveByte(void);
void I2CInit(void);

typedef unsigned char uchar;

uchar eeprom_read(uchar addr);
void eeprom_write(uchar addr,uchar dat);

#endif

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
#include "adc.h"
#include "tim.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#include "bsp_adc.h"
#include "i2c_hal.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
extern uint frq1,frq2;//就是计算得到的两个频率值
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
  MX_TIM2_Init();
  MX_ADC1_Init();
  MX_ADC2_Init();
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
	
	HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_1);//频率测量捕获定时器开启
	HAL_TIM_IC_Start_IT(&htim3,TIM_CHANNEL_1);	

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
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

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
  /** Initializes the peripherals clocks
  */
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_ADC12;
  PeriphClkInit.Adc12ClockSelection = RCC_ADC12CLKSOURCE_SYSCLK;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
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
		
		if(key[3].single_flag == 1)//按键被按下，存储数据到EEPROM的两个区域中
		{
			uchar frq_h = frq1>>8;//frq频率是16位的数（uint型），但是EEPROM一个位只能存8位的数，所以要拆成高8位和低8位
			uchar frq_l = frq1&0xff;//拆成高8位和低8位
			eeprom_write(1,frq_h);//存高8位到1的位置
			HAL_Delay(10);//注意，要加一个延迟//延迟10ms。因为写入是需要时间的，不加延迟可能写不进去
			eeprom_write(2,frq_l);//存低8位到2的位置
			key[3].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	
		sprintf(text,"      FRQ1=%d",frq1);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"      FRQ2=%d",frq2);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
		sprintf(text,"      V1:=%.2f    ",getADC(&hadc1));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line6, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		sprintf(text,"      V2:=%.2f    ",getADC(&hadc2));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line7, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		//V1：如果显示有白块，转化成英文再输入:
		
		//把存进去的数再显示出来
		//先需要把读出的数据进行一个处理
		uint eep_temp = (eeprom_read(1)<<8)+eeprom_read(2);//先读取第一位（高8位），高8位操作右移8位
		sprintf(text,"      FRQ_eep=%d    ",eep_temp);//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line8, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
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

```main,c改
#include "i2c_hal.h"

		if(key[3].single_flag == 1)//按键被按下，存储数据到EEPROM的两个区域中
		{
			uchar frq_h = frq1>>8;//frq频率是16位的数（uint型），但是EEPROM一个位只能存8位的数，所以要拆成高8位和低8位
			uchar frq_l = frq1&0xff;//拆成高8位和低8位
			eeprom_write(1,frq_h);//存高8位到1的位置
			HAL_Delay(10);//注意，要加一个延迟//延迟10ms。因为写入是需要时间的，不加延迟可能写不进去
			eeprom_write(2,frq_l);//存低8位到2的位置
			key[3].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}

		//把存进去的数再显示出来
		//先需要把读出的数据进行一个处理
		uint eep_temp = (eeprom_read(1)<<8)+eeprom_read(2);//先读取第一位（高8位），高8位操作右移8位
		sprintf(text,"      FRQ_eep=%d    ",eep_temp);//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line8, (uint8_t *)text);//第3行被占用了，给它改显示到第8行
```



##### P 11 UART 串口通信

与 IIC 类似，也是跟外设通信的方式，串口更偏向于上位机（比如电脑）这样的设备去通信。
IIC 则侧重于和一些芯片外设去通信。

![[../../annex/STMCubeMX_image_54.png]]
串口跟开发板的连接方式，连接到了调试器上，通过调试器（DAP Link）一起转发给电脑

如果不配置 LCD 的话，会默认把端口弄到 PC 4 (TX)、PC 5 (RX)，要手动改成 PA 9 (TX) 和 PA 10（RX）
![[../../annex/STMCubeMX_image_55.png]]

![[../../annex/STMCubeMX_image_56.png]]

![[../../annex/STMCubeMX_image_57.png]]
具体看题目要求

![[../../annex/Pasted image 20240208165247.png]]
打开接收中断


除了接收，更重要的是要把它解析出来。

![[../../annex/Pasted image 20240208170932.png]]
接收位数一定要填 1，每次中断只能接收 1 个字符

弹幕：这个 IT 函数的意思是打开中断，用中断去接受，不是在中断里面使用
弹幕：中断回调是接收了多少个数据，就进入中断回调，填5代表每接收5个数据回调一次
弹幕：这里为什么 rx_poiner 不需要赋予初值0啊
弹幕：全局变量自动初始化位0吧

![[../../annex/Pasted image 20240208172913.png]]

![[../../annex/Pasted image 20240208180800.png]]
CNBR:A392:200202120000







###### 代码--发送

```main,c改
#include "string.h"

		char temp[20];//把频率上显示的频率frq1发送出去
		sprintf(temp,"frq=%d\r\n",frq1);//与LCD类型，也用sprintf
		HAL_UART_Transmit(&huart1,(uint8_t *)temp,strlen(temp),50);//需要添加库string.h
		//参数1：串口几，使能了串口1填串口1；参数2：要发送的数据，先强制转换；参数3：要发送的程度，可以通过函数来得出；参数4：超时时间

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
#include "adc.h"
#include "tim.h"
#include "usart.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#include "bsp_adc.h"
#include "i2c_hal.h"
#include "string.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
extern uint frq1,frq2;//就是计算得到的两个频率值
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
  MX_TIM2_Init();
  MX_ADC1_Init();
  MX_ADC2_Init();
  MX_USART1_UART_Init();
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
	
	HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_1);//频率测量捕获定时器开启
	HAL_TIM_IC_Start_IT(&htim3,TIM_CHANNEL_1);	

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
	
//		char temp[20];//把频率上显示的频率frq1发送出去
//		sprintf(temp,"frq=%d\r\n",frq1);//与LCD类型，也用sprintf
//		HAL_UART_Transmit(&huart1,(uint8_t *)temp,strlen(temp),50);//需要添加库string.h
//		//参数1：串口几，使能了串口1填串口1；参数2：要发送的数据，先强制转换；参数3：要发送的程度，可以通过函数来得出；参数4：超时时间
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
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

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
  /** Initializes the peripherals clocks
  */
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART1|RCC_PERIPHCLK_ADC12;
  PeriphClkInit.Usart1ClockSelection = RCC_USART1CLKSOURCE_PCLK2;
  PeriphClkInit.Adc12ClockSelection = RCC_ADC12CLKSOURCE_SYSCLK;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
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
		
		if(key[3].single_flag == 1)//按键被按下，存储数据到EEPROM的两个区域中
		{
			uchar frq_h = frq1>>8;//frq频率是16位的数（uint型），但是EEPROM一个位只能存8位的数，所以要拆成高8位和低8位
			uchar frq_l = frq1&0xff;//拆成高8位和低8位
			eeprom_write(1,frq_h);//存高8位到1的位置
			HAL_Delay(10);//注意，要加一个延迟//延迟10ms。因为写入是需要时间的，不加延迟可能写不进去
			eeprom_write(2,frq_l);//存低8位到2的位置
			key[3].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	
		sprintf(text,"      FRQ1=%d",frq1);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"      FRQ2=%d",frq2);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
		sprintf(text,"      V1:=%.2f    ",getADC(&hadc1));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line6, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		sprintf(text,"      V2:=%.2f    ",getADC(&hadc2));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line7, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		//V1：如果显示有白块，转化成英文再输入:
		
		//把存进去的数再显示出来
		//先需要把读出的数据进行一个处理
		uint eep_temp = (eeprom_read(1)<<8)+eeprom_read(2);//先读取第一位（高8位），高8位操作右移8位
		sprintf(text,"      FRQ_eep=%d    ",eep_temp);//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line8, (uint8_t *)text);//第3行被占用了，给它改显示到第8行
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



###### 代码--接收

```interrupt.c
#include "interrupt.h"
#include "usart.h"

struct keys key[4]={0,0,0};//有4个按键，声明成结构体数组
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)//中断回调函数
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


uint ccrl_val1 = 0,ccrl_val2 = 0;
uint frq1 = 0,frq2 = 0;

//然后写中断回调函数
void HAL_TIM_IC_CaptureCallback(TIM_HandleTypeDef *htim)
{
		//首先要判断这个中断是来自哪个定时器，因为用到了TIM2和TIM3。至于判断，参照上面按键的判断方法
	if(htim->Instance == TIM2)	
	{
		ccrl_val1 = HAL_TIM_ReadCapturedValue(htim,TIM_CHANNEL_1);//把定时器的计时值读到一个变量中
		//通过这个可以把定时器锁定到TIM2中，读到它的计时值
		__HAL_TIM_SetCounter(htim,0);//读到之后让计时值清零
		frq1 = (80000000/80)/ccrl_val1;//÷分频系数80再÷计时值
		HAL_TIM_IC_Start(htim,TIM_CHANNEL_1);
	}
	
	if(htim->Instance == TIM3)	
	{
		ccrl_val2 = HAL_TIM_ReadCapturedValue(htim,TIM_CHANNEL_1);//把定时器的计时值读到一个变量中
		//通过这个可以把定时器锁定到TIM2中，读到它的计时值
		__HAL_TIM_SetCounter(htim,0);//读到之后让计时值清零
		frq2 = (80000000/80)/ccrl_val2;//÷分频系数80再÷计时值
		HAL_TIM_IC_Start(htim,TIM_CHANNEL_1);
	}
}

//接收
char rxdata[30];//最终处理好的数据放在这里
uint8_t rxdat;//每次接收给一个变量存储
uchar rx_pointer;//确定写到哪个位置了,一位一位的接收，直接写程序就能看出来
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
	rxdata[rx_pointer++]=rxdat;//把上一次的数据存储一下，放在这个数组中//每次接收只能接收1位，每进一次 中断就把上一次的先存到数组中，然后一个一个拼起来成完整的字符串
	//rx_pointer指示处理到第几位了，先用后加
	HAL_UART_Receive_IT(&huart1,&rxdat,1);//中断模式下接收，所以Receive_IT//引用头文件usart.h
	//参数2放的是指针变量，取地址；参数3：接收的位数，必须填成1
}

```

```interruipt.c改
#include "usart.h"

//接收
char rxdata[30];//最终处理好的数据放在这里
uint8_t rxdat;//每次接收给一个变量存储
uchar rx_pointer;//确定写到哪个位置了,一位一位的接收，直接写程序就能看出来
void HAL_UART_RxCpltCallback(UART_HandleTypeDef *huart)
{
	rxdata[rx_pointer++]=rxdat;//把上一次的数据存储一下，放在这个数组中//每次接收只能接收1位，每进一次 中断就把上一次的先存到数组中，然后一个一个拼起来成完整的字符串
	//rx_pointer指示处理到第几位了，先用后加
	HAL_UART_Receive_IT(&huart1,&rxdat,1);//中断模式下接收，所以Receive_IT//引用头文件usart.h
	//参数2放的是指针变量，取地址；参数3：接收的位数，必须填成1
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
#include "adc.h"
#include "tim.h"
#include "usart.h"
#include "gpio.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
#include "led.h"
#include "lcd.h"
#include "stdio.h"
#include "interrupt.h"
#include "bsp_adc.h"
#include "i2c_hal.h"
#include "string.h"
#define uchar unsigned char
#define uint unsigned int
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */
extern struct keys key[];
extern uint frq1,frq2;//就是计算得到的两个频率值
uchar view = 0;
void key_proc(void);
void disp_proc(void);

uchar pa6_duty = 10;//定义两个变量，分别是两个通道PWM的占空比
uchar pa7_duty = 10;

extern char rxdata[30];
extern uint8_t rxdat;
extern uchar rx_pointer;

//此处只能存储一辆车的信息，存储多辆车改为结构体即可
char car_type[5];//第一个变量，车辆类型
char car_data[5];
char car_time[13];
void uart_rx_proc(void);


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
  MX_TIM2_Init();
  MX_ADC1_Init();
  MX_ADC2_Init();
  MX_USART1_UART_Init();
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
	
	HAL_TIM_IC_Start_IT(&htim2,TIM_CHANNEL_1);//频率测量捕获定时器开启
	HAL_TIM_IC_Start_IT(&htim3,TIM_CHANNEL_1);	
	
	//加一个关于串口的初始化--把中断打开
	HAL_UART_Receive_IT(&huart1,&rxdat,1);//这个语句的作用也是开一次中断
	//这样相当于串口接收初始化就已经完成了
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
	
//		char temp[20];//把频率上显示的频率frq1发送出去
//		sprintf(temp,"frq=%d\r\n",frq1);//与LCD类型，也用sprintf
//		HAL_UART_Transmit(&huart1,(uint8_t *)temp,strlen(temp),50);//需要添加库string.h
//		//参数1：串口几，使能了串口1填串口1；参数2：要发送的数据，先强制转换；参数3：要发送的程度，可以通过函数来得出；参数4：超时时间
		
		if(rx_pointer!=0)//防止接收不完整
		{
			int temp=rx_pointer;
			HAL_Delay(1);
			if(temp==rx_pointer)uart_rx_proc();
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
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

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
  /** Initializes the peripherals clocks
  */
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART1|RCC_PERIPHCLK_ADC12;
  PeriphClkInit.Usart1ClockSelection = RCC_USART1CLKSOURCE_PCLK2;
  PeriphClkInit.Adc12ClockSelection = RCC_ADC12CLKSOURCE_SYSCLK;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
}

/* USER CODE BEGIN 4 */
void key_proc(void)//按键按下的过程
{
		if(key[0].single_flag == 1)//按键被按下
		{
			view = view++;//3个界面了，!view->view++
			if(view==3)	view=0;//清空，回到第一个界面
			LCD_Clear(Black);//清屏
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
		
		if(key[3].single_flag == 1)//按键被按下，存储数据到EEPROM的两个区域中
		{
			uchar frq_h = frq1>>8;//frq频率是16位的数（uint型），但是EEPROM一个位只能存8位的数，所以要拆成高8位和低8位
			uchar frq_l = frq1&0xff;//拆成高8位和低8位
			eeprom_write(1,frq_h);//存高8位到1的位置
			HAL_Delay(10);//注意，要加一个延迟//延迟10ms。因为写入是需要时间的，不加延迟可能写不进去
			eeprom_write(2,frq_l);//存低8位到2的位置
			key[3].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	
	if(view == 0)
	{	
		char text[30];
		sprintf(text,"      Data");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
	
		sprintf(text,"      FRQ1=%d",frq1);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"      FRQ2=%d",frq2);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
		sprintf(text,"      V1:=%.2f    ",getADC(&hadc1));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line6, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		sprintf(text,"      V2:=%.2f    ",getADC(&hadc2));//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line7, (uint8_t *)text);//第3行被占用了，给它改显示到第6行
		//V1：如果显示有白块，转化成英文再输入:
		
		//把存进去的数再显示出来
		//先需要把读出的数据进行一个处理
		uint eep_temp = (eeprom_read(1)<<8)+eeprom_read(2);//先读取第一位（高8位），高8位操作右移8位
		sprintf(text,"      FRQ_eep=%d    ",eep_temp);//直接调用写的ADC读取函数//传递的参数就是ADC几，刚刚CubeMX中配置的
		LCD_DisplayStringLine(Line8, (uint8_t *)text);//第3行被占用了，给它改显示到第8行
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
		
		//清理界面残留
//		LCD_DisplayStringLine(Line3,(uint8_t *)text);
//		LCD_DisplayStringLine(Line5,(uint8_t *)text);		
	}
	
	if(view == 2)//接收到的放到新的界面,界面2中
	{
		char text[30];
		sprintf(text,"      Car_msg   ");//message显示到第一行
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
		sprintf(text,"Car_type=%s   ",car_type);//第二行开始把接收到的变量拆分成3个,分别打印出来
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"Car_data=%s   ",car_data);
		LCD_DisplayStringLine(Line3, (uint8_t *)text);
		sprintf(text,"Car_time=%s   ",car_time);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
	}
}	

//单独写个函数来处理接收部分
void uart_rx_proc(void)//主要就是处理接收的内容
{
	//第一步，先判断有没有接收到东西。因为接收是上位机发东西，具体什么时候接收到东西，单片机是不确定的
	if(rx_pointer>0)//如果指针不在0的位置，就是接收到了,<=0就是没接收到
	{
		if(rx_pointer==22)//证明接收成功
		{
			sscanf(rxdata,"%4s:%4s:%12s",car_type,car_data,car_time);//就处理一下接收到的东西//原始字符串，主要作用就是拆分//格式化过程
			//把三个拆分部分分别存储到不同变量中
		}
		else
		{
			char temp[20];
			sprintf(temp,"Error");
			HAL_UART_Transmit(&huart1,(uint8_t *)temp,strlen(temp),50);
		}
		rx_pointer=0;//指针归位，准备接收下一次数据
		memset(rxdata,0,30);//同时把这个变量给清空
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

```main,c改
#include "string.h"

extern char rxdata[30];
extern uint8_t rxdat;
extern uchar rx_pointer;

//此处只能存储一辆车的信息，存储多辆车改为结构体即可
char car_type[5];//第一个变量，车辆类型
char car_data[5];
char car_time[13];


	//加一个关于串口的初始化--把中断打开
	HAL_UART_Receive_IT(&huart1,&rxdat,1);//这个语句的作用也是开一次中断
	//这样相当于串口接收初始化就已经完成了

		if(rx_pointer!=0)//防止接收不完整
		{
			int temp=rx_pointer;
			HAL_Delay(1);
			if(temp==rx_pointer)uart_rx_proc();
		}

void key_proc(void)//按键按下的过程
{
		if(key[0].single_flag == 1)//按键被按下
		{
			view = view++;//3个界面了，!view->view++
			if(view==3)	view=0;//清空，回到第一个界面
			LCD_Clear(Black);//清屏
			key[0].single_flag = 0;//做完以后标志位清零,避免被重复执行
		}
}

void disp_proc(void)//显示界面的过程
{
	if(view == 1)
	{
		char text[30];
		sprintf(text,"      Para");//第二个参数是我们要打印的内容
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
		sprintf(text,"    PA6:%d%%",pa6_duty);
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"    PA7:%d%%",pa7_duty);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
		//清理界面残留
//		LCD_DisplayStringLine(Line3,(uint8_t *)text);
//		LCD_DisplayStringLine(Line5,(uint8_t *)text);		
	}
	
	if(view == 2)//接收到的放到新的界面,界面2中
	{
		char text[30];
		sprintf(text,"      Car_msg   ");//message显示到第一行
		LCD_DisplayStringLine(Line1, (uint8_t *)text);
		sprintf(text,"Car_type=%s   ",car_type);//第二行开始把接收到的变量拆分成3个,分别打印出来
		LCD_DisplayStringLine(Line2, (uint8_t *)text);
		sprintf(text,"Car_data=%s   ",car_data);
		LCD_DisplayStringLine(Line3, (uint8_t *)text);
		sprintf(text,"Car_time=%s   ",car_time);
		LCD_DisplayStringLine(Line4, (uint8_t *)text);
		
	}
}	

//单独写个函数来处理接收部分
void uart_rx_proc(void)//主要就是处理接收的内容
{
	//第一步，先判断有没有接收到东西。因为接收是上位机发东西，具体什么时候接收到东西，单片机是不确定的
	if(rx_pointer>0)//如果指针不在0的位置，就是接收到了,<=0就是没接收到
	{
		if(rx_pointer==22)//证明接收成功
		{
			sscanf(rxdata,"%4s:%4s:%12s",car_type,car_data,car_time);//就处理一下接收到的东西//原始字符串，主要作用就是拆分//格式化过程
			//把三个拆分部分分别存储到不同变量中
		}
		else
		{
			char temp[20];
			sprintf(temp,"Error");
			HAL_UART_Transmit(&huart1,(uint8_t *)temp,strlen(temp),50);
		}
		rx_pointer=0;//指针归位，准备接收下一次数据
		memset(rxdata,0,30);//同时把这个变量给清空
	}
}
```


##### 14

![[../../annex/Pasted image 20240208232658.png]]























































































































































