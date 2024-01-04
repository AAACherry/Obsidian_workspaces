
## P 69 SRAM 芯片简介

![[../../annex/FSMC--扩展外部SRAM_image_1.png|目录]]

![[../../annex/FSMC--扩展外部SRAM_image_2.png|SRAM控制原理]]

程序本身的空间占用很大才扩展 nand flash。程序运行的时候使用到的很多的内存/变量都是放在内存里面的。

![[../../annex/FSMC--扩展外部SRAM_image_3.png|SRAM芯片外观]]
44 个引脚。

![[../../annex/FSMC--扩展外部SRAM_image_4.png|SRAM芯片内部功能框图]]
SRAM，易失性存储器。掉电数据丢失，但是运行速度非常快。
1、512K X 16存储器矩阵--具有 SRAM 性质的一些存储单元
2、地址译码器--把我们输入的一些地址对存储空间进行寻址
3、数据电路
4、列 I/O
5、控制电路


![[../../annex/FSMC--扩展外部SRAM_image_5.png|SRAM信号线]]
1、A 0~A 18 地址输入，用来访问指定的存储空间。
2、I/O 0-I/O 7 数据输入输出信号，低字节。I/O 可以理解为 SPI 的 MOSI 和 MISO 线，或 SDA 线。
3、I/8 0-I/O 15 数据输入输出信号，高字节
4、CS 2 和 CS 1# 片选信号线，CS 2 高电平有效，CS 1# 低电平有效。控制这个引脚并使能，芯片才能进行读写。
5、OE# 输出使能信号，低电平有效。（相对于 SRAM 而言，也就是说它能够对外返回数据）
6、WE# 写入使能，低电平有效。
7、UB# 数据掩码信号 Upper Byte，高位字节允许访问，低电平有效。
8、LB# 数据掩码信号 Lower Byte，低位字节允许访问，低电平有效。

```
#号相当于低电平有效。
```
SRAM 的控制比较简单，只要控制信号线使能了访问，从地址线输入要访问的地址，即可从 I/O 数据线写入或读出数据


![[../../annex/FSMC--扩展外部SRAM_image_6.png|SRAM存储矩阵]]

![[../../annex/FSMC--扩展外部SRAM_image_7.png|地址译码器、列I/O及I/O数据电路]]
A0~A18。9 根地址线一共可以表示 2<sup>19</sup>=2<sup>9</sup>X 1024=512 K 行存储单元。
所以一共能访问 512K X 16 bits =512K X 2 B （1 MB）大小的空间。
![[../../annex/FSMC--扩展外部SRAM_image_8.png]]
512 代表能够寻址 512 K 行，16 代表数据宽度（每一行能够保存多少个数据位）

一次访问两个字节（512 K 行，1 MB）
比如写入 0 地址的时候，实际上可以访问第 0 个字节以及第 1 个字节。（0 地址访问第 0、1 个字节，用 1 地址访问第 2、3 个字节）
地址线是针对一行地址寻址的，实际上一次访问 2 个字节。
那么我们可以通过 UB # 和 LB# 这两根线来指定。
U-upper，UB 线搞成高电平，就表示 I/O 8~I/O 15 中表示的内容是无效的。LB 线搞成低电平，就代表 I/O 0~I/O 7 数据线是有效的。
如果想同时修改两个字节，就将 UB 线和 LB 线都拉成低电平。
实际上，UB 线和 LB 线的作用也就在于此，就是它们是用来控制 I/O 口表示的数据是有效还是无效的。相当于是用来控制数据线的宽度。

控制电路部分
片选信号 CS 1 和 CS 2，是相反的。
OE 相当于是读使能
WE 相当于是写使能
UB 和 LB 也是防止误操作，只想操作一个字节的时候，防止 SRAM 把所有内容都拷贝进去。

SRAM 是不需要定时刷新的，这是和 SDRAM 的主要区别。



SRAM 是没有时钟线的，所以说其实是异步通讯，跟 IIC 和 SPI 不同。
![[../../annex/FSMC--扩展外部SRAM_image_9.png|SRAM读过程时序图]]
通过大家协调好的时间参数来协调通讯。
一般来说，在片选信号有效之后过了一定时间应当把读有效信号拉成低电平（使能有效信号）。

![[../../annex/FSMC--扩展外部SRAM_image_10.png]]
Taa 就是给出地址之后到数据出现（返回数据的过程）的一个时间。
TDOE 就是给出 OE 信号之后，数据信号输出的一个时间。

![[../../annex/FSMC--扩展外部SRAM_image_11.png|速度型号]]

![[../../annex/FSMC--扩展外部SRAM_image_12.png]]
根据这个图，t<sub>rc</sub>>55 ns 可以得知读取一个字节/两个字节的过程不能低于 55 ns。
T<sub>AA</sub><55 ns，没有最低值，我们可以控制 taa=33 ns，我们在 33 ns 后对数据进行采样。
T<sub>DOE</sub><25 ns，没有最低值要求，我们就可以确认，给出 OE 信号之后，T<sub>DOE</sub>不能超过 25 ns（在 25 ns 内读取 data 进行采样）。



![[../../annex/FSMC--扩展外部SRAM_image_13.png|SRAM写过程时序图]]
与 SRAM 读过程类似。
1、地址线一致（A 0~A 18 整个过程都是低电平有效）
2、片选。
T<sub>SA</sub>就有要求了。要过了 T<sub>SA</sub>时间后使能片选信号。
3、写使能
当给出写使能信号之后，过了 T<sub>PWE</sub>时间后，SRAM 会在这个时刻进行采样。
所以 STM 32 整个过程都保持数据线表示的信号有效。
SRAM 在中间时刻对数据进行采样。
![[../../annex/FSMC--扩展外部SRAM_image_14.png]]
数据出现之前到采样过程是 T<sub>SD</sub>，采样之后 T<sub>HD</sub>。

![[../../annex/FSMC--扩展外部SRAM_image_15.png|SRAM的读写流程]]

![[../../annex/FSMC--扩展外部SRAM_image_16.png|简单列举的时间参数表]]

![[../../annex/FSMC--扩展外部SRAM_image_17.png|SRAM时间参数表]]



#### B 站 AI 视频总结

如何使用 STM 32 芯片扩展内存，包括使用 SRAM 芯片和 FSMC 控制器，以及实验步骤。
00:01 介绍使用 STM 32 芯片扩展内存的方法
02:02 分成四个部分讲解 SDRAM 控制原理和使用
03:21  STM 32 芯片内部的 SRAM 相当于电脑内存，可扩展以满足程序需求

- 存储器矩阵、地址译码器、控制电路等半导体芯片的核心内容, 包括地址线、数据线、片选引脚等。
10:00 返回数据，涉及地址线、数据线和片选引脚
10:19 信号线分为地址线和数据线
13:50 STM 32 F 103 存储器矩阵不需要列地址，只需要输入行地址寻址

- SRAM 的控制电路和时序, 包括地址线、片选信号、读写使能信号等, 以及数据传输的过程。
20:00 解释了 ub 和 Ib 电平的作用
22:41 介绍了控制电路的作用和配合时序图使用
26:05 讲解了 sram 的异步通讯方式和时序，需要协调好地址线和片选信号

- 数据信号的读取过程和时间要求包括 tlc、taa、tde 等参数对于写数据也有类似的要求。
30:00 数据信号输出保持一定时间
31:14 读取过程需要等待特定时间
34:32 读取访问过程必须不低于 55 纳秒

- 关于 SRAM 芯片的信号和时序细节, 以及使用 STM 32 控制 SRAM 芯片的相关内容。
40:02 必须超过 55 纳秒，然后看其他 t p w 1
40:36 ts d 至少为 25 纳秒，th d 最小值为零
42:34 调整程序时间参数，查看芯片手册，关注 55纳秘




## P 70 FSMC 功能框图详解

![[../../annex/FSMC--扩展外部SRAM_image_18.png]]

![[../../annex/FSMC--扩展外部SRAM_image_19.png]]

![[../../annex/FSMC--扩展外部SRAM_image_20.png|FSMC简介]]
FSMC 简介
STM 32 F 1 系列芯片使用 FSMC 外设来管理扩展的存储器，FSMC 是 Flexible **Static** Memory Controller 的缩写，译为**灵活的静态存储控制器**。它可以用于驱动包括 SRAM、NOR FLASH 以及 NAND FLSAH 类型的存储器，不能驱动如 SDRAM 这种动态的存储器而在 STM 32 F 429 系列的控制器中，它具有 FMC 外设，支持控制 SDRAM 存储器



![[../../annex/FSMC--扩展外部SRAM_image_21.png|FSMC功能框图]]

![[../../annex/FSMC--扩展外部SRAM_image_22.png]]

FSMC 是直接挂载在 AHB 总线上的。普通外设的挂载在 APB 1 、APB 2 上。
特别的功能：直接访问内存的地址就可以控制 SRAM 和 NAND FLASH

![[../../annex/FSMC--扩展外部SRAM_image_23.png|通讯引脚]]
NBL 对应 SRAM 的 LB 和 UB 线

N 等效于#，即低电平有效

SRAM 中的 CS 1# 和 CS 2 是互为相反的，是互补的一个引脚，实际上是同一个信号。
FSMC 中有 4 个引脚，不同的引脚对应 STM 32 内部不同的地址区域。


![[../../annex/FSMC--扩展外部SRAM_image_24.png|SRAM信号线]]


 ![[../../annex/FSMC--扩展外部SRAM_image_25.png|存储器控制器]]
上面不同类型的引脚是连接到 FSMC 内部对应的存储控制器中的。
对于 NOR/PSRAM/SRAM 这三类存储器，FSMC 使用了三种类型的寄存器用来控制它们的模式以及一些时序。
控制 SRAM 的有 FSMC BCR1/2/3/4、
控制寄存器 FSMC BTR1/2/3/4片选时序寄存器
以及 FSMC BWTR1/2/3/4写时序寄存器。

每种寄存器都有4个，分别对应于4个不同的存储区域，使用不同的访问区域的时候会对应不同的片选线。

各种寄存器介绍如下:
FSMC BCR 控制寄存器可配置要控制的存储器类型、数据线宽度以及信号有效极性能参数。
FMC BTR 时序寄存器用于配置 SRAM 访问时的各种时间延迟，如数据保持时间、地址保持时间等。
FMC BWTR 写时序寄存器与 FMC BTR 寄存器控制的参数类似，它专门用于控制写时序的时间参数。

4 个是一样的，是同类型的，只是控制的内容，控制的片选线/区域不一样；但是主要的功能是一样的。

配置 SRAM 一般用模式 A，NAND FLASH 用模式 B
![[../../annex/FSMC--扩展外部SRAM_image_26.png]]

写时序寄存器其实跟片选时序寄存器是类似的，基本一样。
![[../../annex/FSMC--扩展外部SRAM_image_27.png]]
为什么一样的寄存器要搞两个呢？
因为可以给我们配置 EXTMOD 扩展模式使能。
在 BCR 寄存器中配置它扩展模式使能后，可以让读时序和写时序有不同的时间参数。更加细致的区分读时序和写时序的一些延时参数。


![[../../annex/FSMC--扩展外部SRAM_image_28.png|时钟控制逻辑]]

因为 FSMC 是直接连在 AHB 总线上的，所以它的时钟信号是来源于 HCLK（默认 72 MHz）。
如果要控制同步时钟输出（如 CLK 引脚的输出时钟的话，就像 SPI 控制输出时钟）那么就是从 HCLK 分频得来的。
分配因子从这里（CLKDIV）配置
![[../../annex/FSMC--扩展外部SRAM_image_29.png]]
如果是使用 SRAM 异步的存储器。我们是不需要输出时钟的，所以这个配置就不起作用。
这个时钟的分频因子不起作用，只是说它配置之后, 时钟不会有输出。但是要明确：控制 SRAM 芯片的时候，使用异步通讯的时候，FSMC 片上外设的时钟也是来源于 HCLK 的，它的频率是 72 MHz。
有什么作用呢？
后面配置各种延迟的时候，比如数据保持时间、地址保持时间、地址建立时间，这些时间都是跟时序图相关的。这些的时间配置都是使用 HCLK 的周期的。我们可以配置它等于 n 个时钟周期，比如我们要让它延迟 55 ns，我们就可以算 55 ns=多少个时钟周期。这个时钟周期来源就是 HCLK（72 MHz），频率就是 1/72 MHz。
![[../../annex/FSMC--扩展外部SRAM_image_30.png]]


 ![[../../annex/FSMC--扩展外部SRAM_image_31.png|FSMC的地址映射]]
一般是使用外设，发送一个地址，告诉 EEPROM/SPI FLASH 我们要访问哪个地址，然后再进行读写操作。
对于 FSMC 是不一样的，对于 FSMC 来说是直接访问自己的内存的。FSMC 的一些地址空间都是会被映射的。
访问 SRAM 的时候直接访问 STM 32 内部的变量的地址就可以了。


![[../../annex/FSMC--扩展外部SRAM_image_32.png|FSMC地址映射]]
内核的地址映射
FSMC 控制器对内核的地址映射
把 1 GB 的空间又分成 4 块，每块 4 X 64 M 字节。Bank 1 作为 NOR FLASH 以及 PSRAM、SRAM 来作为使用的。Bank 2-3 作为 NAND FLASH 用。
访问的地址不在这些范围就无法访问对应的存储器。

映射有什么作用？
主要是为了实现一个功能。
比如我们可以使用 C 语言定义一个指针变量，让它去访问某个地址（如 6000 0000）。因为访问的地址是映射到 FSMC 的，而 FSMC 可能会外接到 SRAM/NAND FLASH/SD 等地方。那么当我们访问这个地址空间的时候，STM 32 默认，我们就是要使用 FSMC 来访问外部 RAM，那么就会去控制 FSMC 外设来产生后面的时序来对 SRAM 进行读写操作。

初始化完 FSMC，然后使用指针来访问这些地址，就能获取到 SRAM 里面的内容了。实际上就相当于把外边的 SRAM 映射到了 STM 32 内部的地址，相当于 STM 32 预留了地址空间来给外部 SRAM 使用。当我们访问这些地址的时候，它就直接控制 FSMC 外设来去 SRAM 中找到对应的内容，然后对这些数据进行操作。在程序上，看起来是不需要进行任何操作，只需要对指针进行读写操作，对指针进行赋值/直接取指针然后就能读回来这些数据了。
所有这些时序都是由 FSMC 这个外设来产生的，不需要关注标志位。

![[../../annex/FSMC--扩展外部SRAM_image_33.png|FSMC地址映射(分成4个区)]]


![[../../annex/FSMC--扩展外部SRAM_image_34.png|FSMC地址映射(SRAM)]]
6000 0000 ~ 6 FFF FFFF
实际上又分成了 4 块。每一块 64 M 的空间
为什么这样分？
主要是为了方便连接不同的存储器芯片。
NE 的 4 根信号线：
NE 1 对应 0x6000 0000-0x63FF FFFF
NE 2 对应 0x0x6400 0000-0x67FF FFFF
NE 3 对应 0x0x6800 0000-0x6BFF FFFF
NE 4 对应 0x0x6C00 0000-0x6FFF FFFF
这样区分有一个好处：当我们访问一个范围内的空间的时候，NE 1 引脚就会输出片选使能了。
SRAM 芯片跟 STM 32 是连接到 NE 3 引脚的。所以实际上用到 0x0x6800 0000-0x6BFF FFFF      （共有 3FF FFFF (h)个地址=64 M）。


![[../../annex/FSMC--扩展外部SRAM_image_35.png|FSMC控制SRAM的读时序]]

T<sub>aa</sub>，地址访问周期
（ADDSET+1）HCLK 周期，地址的建立周期。ADDSET 就是配置经过 ADDSET+1 个时钟周期之后，读/写使能会变成有效

![[../../annex/FSMC--扩展外部SRAM_image_36.png|FSMC控制SRAM的时序]]

![[../../annex/FSMC--扩展外部SRAM_image_37.png|FSMC控制SRAM的写时序]]

发出写使能信号之后，DATAST+1 个时钟周期，这段时间必须要大于 40 ns，才能正常的把数据写入到 SRAM 中。
数据线由 FSMC 驱动的，也就是说写时序从 STM 32 把数据发送到 SRAM 上。读时序从 SRAM 传输数据给 STM 32。


#### B 站 AI 视频总结

STM 32 中用于控制 SRAM 的外设 FSMC，它可以控制多种存储器，包括 SDRAM、NOR Flash 和 NAND FIash 等。重点讲解了 FSMC 控制 SRAM 的部分，以及如何使用 FSMC 来控制 SDRAM。同时，视频还介绍了 FSMC 的通讯方式和引脚定义，以及它与其他存储器控制器之间的区别。最后，视频还提到了 FSMC 可以直接访问内存地址的功能，使其在控制 SRAM 时具有很大的优势。

- STM 32 中控制 SRAM 的外设 FSMC，它是用来控制多种存储器的，包括 SDRAM 和 NOR 闪存。
00:01 介绍 fsm c 外设的功能和控制 sram 的部分
01:00 Fs m c 可以控制多种存储器，包括 sram 和扩展存储器
02:28 fsmc 的名称来源于灵活的静态存储器控制器

- STM 32 存储器控制器和 SDRAM 的连接及地址信号线的使用方法。
10:00 Fsm c 的地址线最多可以有 26 根，n 一引脚控制片选信号
12:02 Fs m c 的 n 一引脚控制 sram 的读写地址，不同地址对应不同的片选线
14:37 Fsmc 使用三种类型的寄存器控制模式和时序，包括控制寄存器、读时序寄存器和写时序寄存器

- 时序参数和地址映射对于读写时序的重要性以及如何使用不同的配置参数来区分读写时序。
21:37 可以使用扩展模式来区分读写时序参数
20:02 使用 hclk 来计算 fs mc 的延迟时间
24:00 Fsmc 的地址映射是访问外部存储器的关键

- 芯片内部地址映射的细节, 包括不同存储器的分配和访问方式。
30:01 地址范围的映射和区分
31:22 不同存储器芯片的连接和地址映射 
35:14 64 兆字节的 sram 空间足以配置，没有用完

- STM 32 存储器操作时序包括地址建立周期、数据输出周期等并详细讲解了各种参数的含义和配置方法。
40:01 STM 32 的 SRAM 控制时序类似于 SRAM，完全对应
41:05 STM 32 的 SRAM 操作周期分为 ADD SET 和 DATA SET
46:13 STM 32 的地址信号线和数据信号线在采样时间上有细微差异

- 如何配置数据存储器 (SRM)的时序参数包括读时序和写时序, 以及注意事项。
50:00 讲解数据存储器时序寄存器的配置
51:52 配置写使能信号线为低电平有效
56:49 控制 FSMC 的读写时序，使数据能够正常传输

- 模式一和模式 a 的区别, 包括读时序和读词句的配置。同时, 还介绍了如何配置不同的模式。
01:00:01 模式一和模式 a 的区别，读时序和读词句的不司
01:00:55 只有四种模式可选，模式一需要先配置成模式 a 才能使用
01:02:44 初始化结构体和 sram 相关结构体的介绍，重点关注寄存器位



## P 71 FSMC 初始化结构体详解

![[../../annex/FSMC--扩展外部SRAM_image_38.png]]


#### 时序结构体 FSMC_NORSRAMTimingintTypeDef
![[../../annex/FSMC--扩展外部SRAM_image_39.png|FSMC时序结构体]]
写入 0 的时候相当于延时一个时钟周期，写入相当于延时 2 个时钟周期

![[../../annex/FSMC--扩展外部SRAM_image_40.png|AddressHoldTime/DataSetupTime]]
FSMC_AddressSetupTime 
本成员设置地址建立时间，它可以被设置为 0-0 xF 个 HCLK 周期数，按 STM 32 标准库的默认配置，HCLK 的时钟频率为 72 MHz，即一个 HCLK 周期为1/72微秒。
FSMC_AddressHoldTime 
本成员设置地址保持时间，它可以被设置为 0-0 xF 个 HCLK 周期数。
FSMC_DataSetupTime 
本成员设置数据建立时间，它可以被设置为 0-0 xF 个 HCLK 周期数。

![[../../annex/FSMC--扩展外部SRAM_image_41.png|BusTunAroundDuration]]
FSMC_BusTurnAroundDuration 
本成员设置总线转换周期，在 NOR FLASH 存储器中，地址线与数据线可以分时复用，总线转换周期就是指总线在这两种状态间切换需要的延时，防止冲突。控制其它存储器时这个参数无效，配置为 0 即可。

![[../../annex/FSMC--扩展外部SRAM_image_42.png|CLKDivision]]
FSMC_CLKDivision 
本成员用于设置时钟分频，它以 HCLK 时钟作为输入，经过 FSMC_CLKDivision 分频后输出到 FSMC_CLK 引脚作为通讯使用的同步时钟。控制其它异步通讯的存储器时这个参数无效，配置为0即可。

![[../../annex/FSMC--扩展外部SRAM_image_43.png|DataLatency]]
FSMC_DataLatency 
本成员设置数据保持时间，它表示在读取第一个数据之前要等待的周期数，该周期指同步时钟的周期，本参数仅用于同步 NOR FLASH 类型的存储器，控制其它类型的存储器时，本参数无效。

![[../../annex/FSMC--扩展外部SRAM_image_44.png|AccessMode]]
FSMC_AccessMode 
本成员设置存储器访问模式，不同的模式下 FSMC 访问存储器地址时引脚输出的时序不一样，可选 FSMC_AccessMode_A/B/C/D 模式。一般来说控制 SRAM 时使用 A 模式。

这个FSMC_NORSRAMTimingInitTypeDef 时序结构体配置的延时参数，将作为下一节的FSMC SRAM初始化结构体的一个成员。


其实 FSMC 扩展外部 SRAM （异步）主要只用到 ADDSET、DATAST、AccessMode

#### 初始化结构体 FSMC_NORSRAMintTypeDef
![[../../annex/FSMC--扩展外部SRAM_image_45.png]]

![[../../annex/FSMC--扩展外部SRAM_image_46.png]]

![[../../annex/FSMC--扩展外部SRAM_image_47.png|FSMC_Bank]]
FSMC_Bank
本成员用于选择 FSMC 映射的存储区域，它的可选参数以及相应的内核地址映射范围见上面的表格

![[../../annex/FSMC--扩展外部SRAM_image_48.png|FSMC_DataAddressMux]]
FSMC_DataAddressMux
本成员用于设置地址总线与数据总线是否复用 (FSMC_DataAddressMux_Enable /Disable)，在控制 NOR FLASH 时，可以地址总线与数据总线可以分时复用，以减少使用 STM 32 信号线的数量。

![[../../annex/FSMC--扩展外部SRAM_image_49.png|FSMC_MemoryType]]
FSMC_MemoryType
本成员用于设置**要控制的存储器类型**，它支持控制的存储器类型为 SRAM、PSRAM 以及 NOR FLASH (FSMC_MemoryType_SRAM/PSRAM/NOR)。

FSMC_MemoryDataWidth
本成员用于设置要控制的存储器的**数据宽度**，可选择设置成 8 或 16 位 (FSMC_MemoryDataWidth_8 b /16 b)。

FSMC_BurstAccessMode 
本成员用于设置是否使用突发访问模式 (FSMC_BurstAccessMode_Enable/Disable)，突发访问模式是指发送**一个地址后连续访问多个数据**，非突发模式下每访问一个数据都需要输入一个地址，仅在控制**同步类型的存储器时才能使用**突发模式。

FSMC_AsynchronousWait
本成员用于设置**是否使能在同步传输时使用的等待信号** (FSMC_AsynchronousWait_Enable/Disable)，在控制同步类型的 NOR 或 PSRAM 时，存储器可以使用 FSMC_NWAIT 引脚通知 STM32需要等待。

FSMC_WaitSignalPolarity 
本成员用于**设置等待信号的有效极性，即要求等待时，使用高电平还是低电平** (FSMC_WaitSignalPolarity_High/Low)。

FSMC_WrapMode 
本成员用于设置**是否支持把非对齐的 AHB 突发操作分割成 2 次线性操作** (FSMC_WrapMode_Enable/Disable)，该配置仅在突发模式下有效。

FSMC_WaitSignalActive
本成员用于**配置在突发传输模式时，决定存储器是在等待状态之前的一个数据周期有效还是在等待状态期间有效** (FSMC_WaitSignalActive_BeforeWaitState/DuringWaitState)。

FSMC_WriteOperation 
这个成员用于**设置是否<font color="#ff0000">写</font>使能** (FSMC_WriteOperation_ Enable /Disable)，禁止写使能的话 FSMC 只能从存储器中读取数据，不能写入。

FSMC_WaitSignal
本成员用于设置当存储器牌突发传输模式时，**是否允许通过 NWAIT 信号插入等待状态** (FSMC_WaitSignal_Enable/Disable)。

FSMC_ExtendedMode
本成员用于设置**是否使用扩展模式** (FSMC_ExtendedMode_Enable/Disable)，在非扩展模式下，对存储器读写的时序都只使用 FSMC_BCR 寄存器中的配置，即下面的 FSMC_ReadWriteTimingStruct 结构体成员；在扩展模式下，**对存储器的读写时序可以分开配置**，读时序使用 FSMC_BCR 寄存器，写时序使用 FSMC_BWTR 寄存器的配置，即下面的 FSMC_WriteTimingStruct 结构体。

FSMC_ReadWriteTimingStruct
本成员是一个**指针**，赋值时使用上一小节中讲解的时序结构体 FSMC_NORSRAMInitTypeDef 设置，**当不使用扩展模式时，读写时序都使用本成员的参数配置**。

FSMC_WriteTimingStruct
同样地，本成员也是一个**时序结构体的指针**，只有**当使用扩展模式时，本配置才有效**，它是**写操作**使用的时序。


![[../../annex/FSMC--扩展外部SRAM_image_50.png|需要关注配置的结构体成员]]


#### B 站 AI 视频总结

与 FSMC 相关的结构体和库函数包括 Nine Fresh、PC 卡 No Fresh 和 S RAM 等存储器的控制。其中 S RAM 的初始化函数为 fmc_nsrm 初始化结构体为 fs_mc_nsrm_t。详细讲解了 FSMC 时序结构体包括地址建立时间和地址保持时间等参数的配置。最后, 以延时 20 纳秒为例, 演示了如何通过写入地址建立时间来控制 S RAM 的读写操作。

- 关于 fs mcsram 相关的结构体及其使用方法，包括地址建立时间等参数的配置。
00:04 介绍 FSMC 的相关结构体和库函数
02:34 讲解 FSMC 时序结构体和相关配置函数
06:18 解释 FSMC 地址建立时间的概念和单位，并举例说明如何配置延时时间

- 在不同模式下, 地址保持时间、数据保持时间、总线转换周期的配置方法以及时钟分频因子的作用。
09:03 详细讲解时间配置，后面类似。
11:49 设置总线转换周期，只有在 no flash 存储器使用时有效。
15:55 选择访问模式，使用模式 A 即可。

- 关于 fmcsram 初始化结构体的参数配置包括读写时序、存储器类型、地址数据复用等。
18:04 讲解了 fmcsram 的初始化结构体成员及参数
19:55 分析了 s 2 初始化结构体，讲解了如何配置 bcr 寄存器
23:49 讲解了 fcm data address mu x 结构体，介绍了地址线和数据线复用的情况

同步通讯中 SDRAM 的配置包括同步、突发模式、扩展模式等. 同时讲解了等待信号和写使能等配置。
27:08 配置同步通讯的 noflash 里边的突发模式
29:10 配置等待信号在等待前有效还是等待期间有效
32:35 配置写突发 ribus，不需要使用




## P 72 代码讲解--初始化 SRAM

![[../../annex/FSMC--扩展外部SRAM_image_51.png|霸道原理图]]
指南者没法做。

时序结构体作为一个指针写到初始化结构体中

写时序的要求
![[../../annex/FSMC--扩展外部SRAM_image_52.png]]
ADDSET = 0
DATASET = 2（DATASET 配置成 1 不能正常工作，<40 ns）
1、 ADDSET+1+DATAST+1 >55 ns  ---- 0+1+2+1 = 55.2 ns>55 ns
2、DATAST+1 >40 ns  ---- 2+1 = 41.4 ns >40 ns
3、ADDSET+1 >0 ns  ---- 0+1=13.8 ns >0
![[../../annex/FSMC--扩展外部SRAM_image_53.png]]

读时序的要求
![[../../annex/FSMC--扩展外部SRAM_image_54.png]]
ADDSET = 0
DATASET = 1
1、 ADDSET+1+DATAST+1 +2 >55 ns ----0+1+1+1+2 = 69 ns >55 ns
2、DATAST+1 >25 ns  ---- 1+1 = 27.6 ns >25 ns
3、ADDSET+1 >0 ns  ---- 0+1=13.8 ns >0

1 个 HCLK 时钟周期
T = 1/72 MHz = 1.38* 10<sup>-8</sup> s=13.8 ns


##### 代码

```bsp_sram.c
 /**
  ******************************************************************************
  * @file    bsp_xxx.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   SRAM 底层应用函数bsp 
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */
  
#include "./sram/bsp_sram.h"

/**
  * @brief  初始化控制SRAM的IO
  * @param  无
  * @retval 无
  */
static void SRAM_GPIO_Config(void)
{
	GPIO_InitTypeDef  GPIO_InitStructure;
 
  /* 使能SRAM相关的GPIO时钟 */

                         /*地址信号线*/
  RCC_APB2PeriphClockCmd(FSMC_A0_GPIO_CLK | FSMC_A1_GPIO_CLK | FSMC_A2_GPIO_CLK | 
                         FSMC_A3_GPIO_CLK | FSMC_A4_GPIO_CLK | FSMC_A5_GPIO_CLK |
                         FSMC_A6_GPIO_CLK | FSMC_A7_GPIO_CLK | FSMC_A8_GPIO_CLK |
                         FSMC_A9_GPIO_CLK | FSMC_A10_GPIO_CLK| FSMC_A11_GPIO_CLK| 
												 FSMC_A12_GPIO_CLK| FSMC_A13_GPIO_CLK|FSMC_A14_GPIO_CLK|
												 FSMC_A15_GPIO_CLK|FSMC_A16_GPIO_CLK|FSMC_A17_GPIO_CLK|FSMC_A18_GPIO_CLK|
                         /*数据信号线*/
                         FSMC_D0_GPIO_CLK | FSMC_D1_GPIO_CLK | FSMC_D2_GPIO_CLK | 
                         FSMC_D3_GPIO_CLK | FSMC_D4_GPIO_CLK | FSMC_D5_GPIO_CLK |
                         FSMC_D6_GPIO_CLK | FSMC_D7_GPIO_CLK | FSMC_D8_GPIO_CLK |
                         FSMC_D9_GPIO_CLK | FSMC_D10_GPIO_CLK| FSMC_D11_GPIO_CLK|
                         FSMC_D12_GPIO_CLK| FSMC_D13_GPIO_CLK| FSMC_D14_GPIO_CLK|
                         FSMC_D15_GPIO_CLK|  
                         /*控制信号线*/
                         FSMC_CS_GPIO_CLK  | FSMC_WE_GPIO_CLK | FSMC_OE_GPIO_CLK |
                         FSMC_UDQM_GPIO_CLK|FSMC_LDQM_GPIO_CLK, ENABLE);	//	UDQM--UB,LBQM--LB
												 


	 /*-- GPIO 配置 -----------------------------------------------------*/

  /* 通用 GPIO 配置 */
  GPIO_InitStructure.GPIO_Mode  = GPIO_Mode_AF_PP;       //配置为复用功能
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;     
  
  /*A地址信号线 针对引脚配置*/
  GPIO_InitStructure.GPIO_Pin = FSMC_A0_GPIO_PIN; 
  GPIO_Init(FSMC_A0_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A1_GPIO_PIN; 
  GPIO_Init(FSMC_A1_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A2_GPIO_PIN; 
  GPIO_Init(FSMC_A2_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A3_GPIO_PIN; 
  GPIO_Init(FSMC_A3_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A4_GPIO_PIN; 
  GPIO_Init(FSMC_A4_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A5_GPIO_PIN; 
  GPIO_Init(FSMC_A5_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A6_GPIO_PIN; 
  GPIO_Init(FSMC_A6_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A7_GPIO_PIN; 
  GPIO_Init(FSMC_A7_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A8_GPIO_PIN; 
  GPIO_Init(FSMC_A8_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A9_GPIO_PIN; 
  GPIO_Init(FSMC_A9_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A10_GPIO_PIN; 
  GPIO_Init(FSMC_A10_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A11_GPIO_PIN; 
  GPIO_Init(FSMC_A11_GPIO_PORT, &GPIO_InitStructure);
	
	GPIO_InitStructure.GPIO_Pin = FSMC_A12_GPIO_PIN; 
  GPIO_Init(FSMC_A12_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A13_GPIO_PIN; 
  GPIO_Init(FSMC_A13_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A14_GPIO_PIN; 
  GPIO_Init(FSMC_A14_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A15_GPIO_PIN; 
  GPIO_Init(FSMC_A15_GPIO_PORT, &GPIO_InitStructure);	
	
	GPIO_InitStructure.GPIO_Pin = FSMC_A16_GPIO_PIN; 
  GPIO_Init(FSMC_A16_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A17_GPIO_PIN; 
  GPIO_Init(FSMC_A17_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_A18_GPIO_PIN; 
  GPIO_Init(FSMC_A18_GPIO_PORT, &GPIO_InitStructure);
    
  /*DQ数据信号线 针对引脚配置*/
  GPIO_InitStructure.GPIO_Pin = FSMC_D0_GPIO_PIN; 
  GPIO_Init(FSMC_D0_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D1_GPIO_PIN; 
  GPIO_Init(FSMC_D1_GPIO_PORT, &GPIO_InitStructure);
    
  GPIO_InitStructure.GPIO_Pin = FSMC_D2_GPIO_PIN; 
  GPIO_Init(FSMC_D2_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D3_GPIO_PIN; 
  GPIO_Init(FSMC_D3_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D4_GPIO_PIN; 
  GPIO_Init(FSMC_D4_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D5_GPIO_PIN; 
  GPIO_Init(FSMC_D5_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D6_GPIO_PIN; 
  GPIO_Init(FSMC_D6_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D7_GPIO_PIN; 
  GPIO_Init(FSMC_D7_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D8_GPIO_PIN; 
  GPIO_Init(FSMC_D8_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D9_GPIO_PIN; 
  GPIO_Init(FSMC_D9_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D10_GPIO_PIN; 
  GPIO_Init(FSMC_D10_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D11_GPIO_PIN; 
  GPIO_Init(FSMC_D11_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D12_GPIO_PIN; 
  GPIO_Init(FSMC_D12_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D13_GPIO_PIN; 
  GPIO_Init(FSMC_D13_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D14_GPIO_PIN; 
  GPIO_Init(FSMC_D14_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_D15_GPIO_PIN; 
  GPIO_Init(FSMC_D15_GPIO_PORT, &GPIO_InitStructure);
  
  /*控制信号线*/
  GPIO_InitStructure.GPIO_Pin = FSMC_CS_GPIO_PIN; 
  GPIO_Init(FSMC_CS_GPIO_PORT, &GPIO_InitStructure);
    
  GPIO_InitStructure.GPIO_Pin = FSMC_WE_GPIO_PIN; 
  GPIO_Init(FSMC_WE_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_OE_GPIO_PIN; 
  GPIO_Init(FSMC_OE_GPIO_PORT, &GPIO_InitStructure);    
  
  GPIO_InitStructure.GPIO_Pin = FSMC_UDQM_GPIO_PIN; 
  GPIO_Init(FSMC_UDQM_GPIO_PORT, &GPIO_InitStructure);
  
  GPIO_InitStructure.GPIO_Pin = FSMC_LDQM_GPIO_PIN; 
  GPIO_Init(FSMC_LDQM_GPIO_PORT, &GPIO_InitStructure);	
}			

//写时序的要求
//ADDSET = 0
//DATASET = 2（DATASET 配置成 1 不能正常工作，<40 ns）
//1、 ADDSET+1+DATAST+1 >55 ns  ---- 0+1+2+1 = 55.2 ns>55 ns
//2、DATAST+1 >40 ns  ---- 2+1 = 41.4 ns >40 ns
//3、ADDSET+1 >0 ns  ---- 0+1=13.8 ns >0

//读时序的要求
//ADDSET = 0
//DATASET = 1
//1、 ADDSET+1+DATAST+1 +2 >55 ns ----0+1+1+1+2 = 69 ns >55 ns
//2、DATAST+1 >25 ns  ---- 1+1 = 27.6 ns >25 ns
//3、ADDSET+1 >0 ns  ---- 0+1=13.8 ns >0

//1 个 HCLK 时钟周期
//T = 1/72 MHz = 1.38* 10^-8 s=13.8 ns

//初始化FSMC模式
static void FSMC_ModeConfig(void)
{
	//读时序
	//时序结构体
	FSMC_NORSRAMTimingInitTypeDef readTimInitStruct;
	FSMC_NORSRAMTimingInitTypeDef writeTimInitStruct;

	//初始化结构体
	FSMC_NORSRAMInitTypeDef SRAMInitStruct;
	
	readTimInitStruct.FSMC_AccessMode = FSMC_AccessMode_A;
	readTimInitStruct.FSMC_AddressHoldTime = 0;//SRAM没用到
	readTimInitStruct.FSMC_AddressSetupTime = 0;//理论值
	readTimInitStruct.FSMC_BusTurnAroundDuration = 0;//SRAM没用到
	readTimInitStruct.FSMC_CLKDivision = 	0;//SRAM没用到
	readTimInitStruct.FSMC_DataLatency = 0;//SRAM没用到
	readTimInitStruct.FSMC_DataSetupTime = 2; //2是经验值,理论值为1,后面再测试
	
	//写时序
	writeTimInitStruct.FSMC_AccessMode = FSMC_AccessMode_A;
	writeTimInitStruct.FSMC_AddressHoldTime = 0;//SRAM没用到
	writeTimInitStruct.FSMC_AddressSetupTime = 0;//理论值
	writeTimInitStruct.FSMC_BusTurnAroundDuration = 0;//SRAM没用到
	writeTimInitStruct.FSMC_CLKDivision = 	0;//SRAM没用到
	writeTimInitStruct.FSMC_DataLatency = 0;//SRAM没用到
	writeTimInitStruct.FSMC_DataSetupTime = 2; //2是经验值,理论值为1,后面再测试
	
	
	SRAMInitStruct.FSMC_Bank = FSMC_Bank1_NORSRAM3;
	SRAMInitStruct.FSMC_ExtendedMode = FSMC_ExtendedMode_Enable;
	SRAMInitStruct.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_16b;
	SRAMInitStruct.FSMC_MemoryType = FSMC_MemoryType_SRAM;
	
	//SRAM没用到
	SRAMInitStruct.FSMC_AsynchronousWait = FSMC_AsynchronousWait_Disable;
	SRAMInitStruct.FSMC_BurstAccessMode = FSMC_BurstAccessMode_Disable;
	SRAMInitStruct.FSMC_DataAddressMux = FSMC_DataAddressMux_Disable;
	SRAMInitStruct.FSMC_WaitSignal = FSMC_WaitSignal_Disable;
	SRAMInitStruct.FSMC_WaitSignalActive = FSMC_WaitSignalActive_BeforeWaitState;
	SRAMInitStruct.FSMC_WaitSignalPolarity = FSMC_WaitSignalPolarity_Low;
	SRAMInitStruct.FSMC_WrapMode = FSMC_WrapMode_Disable;
	SRAMInitStruct.FSMC_WriteBurst = FSMC_WriteBurst_Disable;
	SRAMInitStruct.FSMC_WriteOperation = FSMC_WriteOperation_Enable;
	
	//读写时序结构体
	SRAMInitStruct.FSMC_ReadWriteTimingStruct = &readTimInitStruct;
	SRAMInitStruct.FSMC_WriteTimingStruct = &writeTimInitStruct;//FSMC_ExtendedMode配置成Enable时有效
	
	//把配置写入到寄存器
	FSMC_NORSRAMInit(&SRAMInitStruct);
	//使能FSMC
	FSMC_NORSRAMCmd(FSMC_Bank1_NORSRAM3,ENABLE);
	
}


//SRAM初始化
void SRAME_Init(void)
{
	SRAM_GPIO_Config();
	FSMC_ModeConfig();
}



   
/*********************************************END OF FILE**********************/

```

```main.c
 /**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   华邦 8M串行flash测试，并将测试信息通过串口1在电脑的超级终端中打印出来
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
#include "./usart/bsp_usart.h"
#include "./led/bsp_led.h"

/*
 * 函数名：main
 * 描述  ：主函数
 * 输入  ：无
 * 输出  ：无
 * 提示  ：不要乱盖PC0跳帽！！
 */
int main(void)
{ 	
	LED_GPIO_Config();
	LED_BLUE;
	
	/* 配置串口为：115200 8-N-1 */
	USART_Config();
	printf("\r\n 这是一个8Mbyte串行flash(W25Q64)实验 \r\n");

	
	while(1);  
}



/*********************************************END OF FILE**********************/

```

```bsp_sram.h
#ifndef __SRAM_H
#define __SRAM_H

#include "stm32f10x.h"


  
/*A地址信号线*/    
#define FSMC_A0_GPIO_PORT        GPIOF
#define FSMC_A0_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A0_GPIO_PIN         GPIO_Pin_0

#define FSMC_A1_GPIO_PORT        GPIOF
#define FSMC_A1_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A1_GPIO_PIN         GPIO_Pin_1

#define FSMC_A2_GPIO_PORT        GPIOF
#define FSMC_A2_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A2_GPIO_PIN         GPIO_Pin_2

#define FSMC_A3_GPIO_PORT        GPIOF
#define FSMC_A3_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A3_GPIO_PIN         GPIO_Pin_3

#define FSMC_A4_GPIO_PORT        GPIOF
#define FSMC_A4_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A4_GPIO_PIN         GPIO_Pin_4

#define FSMC_A5_GPIO_PORT        GPIOF
#define FSMC_A5_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A5_GPIO_PIN         GPIO_Pin_5

#define FSMC_A6_GPIO_PORT        GPIOF
#define FSMC_A6_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A6_GPIO_PIN         GPIO_Pin_12

#define FSMC_A7_GPIO_PORT        GPIOF
#define FSMC_A7_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A7_GPIO_PIN         GPIO_Pin_13

#define FSMC_A8_GPIO_PORT        GPIOF
#define FSMC_A8_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A8_GPIO_PIN         GPIO_Pin_14

#define FSMC_A9_GPIO_PORT        GPIOF
#define FSMC_A9_GPIO_CLK         RCC_APB2Periph_GPIOF
#define FSMC_A9_GPIO_PIN         GPIO_Pin_15

#define FSMC_A10_GPIO_PORT        GPIOG
#define FSMC_A10_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_A10_GPIO_PIN         GPIO_Pin_0

#define FSMC_A11_GPIO_PORT        GPIOG
#define FSMC_A11_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_A11_GPIO_PIN         GPIO_Pin_1

#define FSMC_A12_GPIO_PORT        GPIOG
#define FSMC_A12_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_A12_GPIO_PIN         GPIO_Pin_2

#define FSMC_A13_GPIO_PORT        GPIOG
#define FSMC_A13_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_A13_GPIO_PIN         GPIO_Pin_3

#define FSMC_A14_GPIO_PORT        GPIOG
#define FSMC_A14_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_A14_GPIO_PIN         GPIO_Pin_4

#define FSMC_A15_GPIO_PORT        GPIOG
#define FSMC_A15_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_A15_GPIO_PIN         GPIO_Pin_5

#define FSMC_A16_GPIO_PORT        GPIOD
#define FSMC_A16_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_A16_GPIO_PIN         GPIO_Pin_11

#define FSMC_A17_GPIO_PORT        GPIOD
#define FSMC_A17_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_A17_GPIO_PIN         GPIO_Pin_12

#define FSMC_A18_GPIO_PORT        GPIOD
#define FSMC_A18_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_A18_GPIO_PIN         GPIO_Pin_13

/*D 数据信号线*/
#define FSMC_D0_GPIO_PORT        GPIOD
#define FSMC_D0_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D0_GPIO_PIN         GPIO_Pin_14

#define FSMC_D1_GPIO_PORT        GPIOD
#define FSMC_D1_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D1_GPIO_PIN         GPIO_Pin_15

#define FSMC_D2_GPIO_PORT        GPIOD
#define FSMC_D2_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D2_GPIO_PIN         GPIO_Pin_0

#define FSMC_D3_GPIO_PORT        GPIOD
#define FSMC_D3_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D3_GPIO_PIN         GPIO_Pin_1

#define FSMC_D4_GPIO_PORT        GPIOE
#define FSMC_D4_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D4_GPIO_PIN         GPIO_Pin_7

#define FSMC_D5_GPIO_PORT        GPIOE
#define FSMC_D5_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D5_GPIO_PIN         GPIO_Pin_8

#define FSMC_D6_GPIO_PORT        GPIOE
#define FSMC_D6_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D6_GPIO_PIN         GPIO_Pin_9

#define FSMC_D7_GPIO_PORT        GPIOE
#define FSMC_D7_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D7_GPIO_PIN         GPIO_Pin_10

#define FSMC_D8_GPIO_PORT        GPIOE
#define FSMC_D8_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D8_GPIO_PIN         GPIO_Pin_11

#define FSMC_D9_GPIO_PORT        GPIOE
#define FSMC_D9_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D9_GPIO_PIN         GPIO_Pin_12

#define FSMC_D10_GPIO_PORT        GPIOE
#define FSMC_D10_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D10_GPIO_PIN         GPIO_Pin_13

#define FSMC_D11_GPIO_PORT        GPIOE
#define FSMC_D11_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D11_GPIO_PIN         GPIO_Pin_14

#define FSMC_D12_GPIO_PORT        GPIOE
#define FSMC_D12_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_D12_GPIO_PIN         GPIO_Pin_15

#define FSMC_D13_GPIO_PORT        GPIOD
#define FSMC_D13_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D13_GPIO_PIN         GPIO_Pin_8

#define FSMC_D14_GPIO_PORT        GPIOD
#define FSMC_D14_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D14_GPIO_PIN         GPIO_Pin_9

#define FSMC_D15_GPIO_PORT        GPIOD
#define FSMC_D15_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_D15_GPIO_PIN         GPIO_Pin_10


/*控制信号线*/  
/*CS片选*/
/*NE3 ,对应的基地址0x68000000*/
#define FSMC_CS_GPIO_PORT        GPIOG
#define FSMC_CS_GPIO_CLK         RCC_APB2Periph_GPIOG
#define FSMC_CS_GPIO_PIN         GPIO_Pin_10

/*WE写使能*/
#define FSMC_WE_GPIO_PORT        GPIOD
#define FSMC_WE_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_WE_GPIO_PIN         GPIO_Pin_5

/*OE读使能*/
#define FSMC_OE_GPIO_PORT        GPIOD
#define FSMC_OE_GPIO_CLK         RCC_APB2Periph_GPIOD
#define FSMC_OE_GPIO_PIN         GPIO_Pin_4


/*LB数据掩码*/
#define FSMC_UDQM_GPIO_PORT        GPIOE
#define FSMC_UDQM_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_UDQM_GPIO_PIN         GPIO_Pin_1

/*UB数据掩码*/
#define FSMC_LDQM_GPIO_PORT        GPIOE
#define FSMC_LDQM_GPIO_CLK         RCC_APB2Periph_GPIOE
#define FSMC_LDQM_GPIO_PIN         GPIO_Pin_0



//SRAM初始化
void SRAME_Init(void);




#endif /* __SRAM_H */


```


#### B 站 AI 视频总结
STM 32 F 103 开发板的基本原理和指南者的使用问题。讲解了 SAM 和 HFM 的连接方式, 以及如何使用不同的芯片来控制 SRAM。同时视频还详细介绍了各个引脚的功能和连接方式并提到了在使用 STM 32 内部指针访问地址时需要将片选引脚连接到 FMC 的 N 13 引脚以使 SRAM 正常工作。视频最后给出了相关的代码和资料下载链接,供读者参考

- 如何准备写程序需要查看原理图, 了解连接说明, 以及芯片引脚匹配等问题。
00:01 准备写程序前的原理图查看
02:41 STM 32 VET 6 芯片与指南者芯片的区别
05:25 STM 32 F 103 CZ 的硬件资源分配

- FMCA 0 对应的地址以及 FMCCDO 等引脚的连接方式并讲解了控制引脚 W 1 和片选引脚 C1的用途
05:30 地址分配和控制引脚连接
08:28 SRAM 驱动程序测试

- 如何访问和操作 SRAM 包括使用指针访问、读写数据等并提供了相关工程的实现方式
11:05 通过指针访问和修改数据
11:25 使用 16 位指针和 32 位指针写入数据
15:10 编写程序的基本步骤和 sram 的读写操作

- 如何初始化 GPIO 并将其配置为复用输出功能, 以及相关引脚的定义和配置。
17:48 硬件引脚宏定义和 GPIO 初始化
22:16 复用输出功能配置和时钟使能SMBUS 功能和配置
22:22 如何配置 FM 7 外设的引脚以及如何初始化 FSMC 模式和时序结构体。

- 如何配置 FM7外设的引脚以及如何初始化 FSMC 模式和时序结构体。
22:41 SFRAM 配置: 讲解了如何配置 SFRAM，包括输入输出引脚和 SRAM 的配置。
26:15 GPIO 模式初始化: 介绍了如何初始化 GPIO 模式，包括上拉模式和非上拉模式。
27:18 讲解了时序结构体和初始化结构体，包括如何初始化各个参数

- 如何配置 FMAC 的时序包括地址和数据的设置以及采样时间等, 以实现正常数据写入。
27:52 配置 STM 32 的时序，使用了结构体和中断服务函数。
32:07  SDRAM 的写时序要求，包括地址信号和数据信号的有效时间。
33:29 数据采集的时序要求，包括地址信号、数据信号和读使能信号的有效时间。

- 关于写和读时序的要求, 包括 a d ds et、data s t、采样时刻等. 以及如何满足这些要求。
34:43 写使能和读使能的时序要求
35:41 读写时序要求和数据有效性
39:59 时序要求和数据采集

- 在配置数据寄存器时. 需要注意不能写入零否则会延时两个时钟周期。
40:36 寄存器值写入与时钟周期关系
44:21 配置数据存储器时间要求与技巧
46:41 STM 32 F 103 数据存储器配置与作用

- 如何配置时序结构体和初始化结构体以满足读写时序要求并进行了详细的计算和说明。
46:48 配置显示器时序: 讲解了如何配置显示器的数据传输时序，包括地址时间、数据设置时间等
51:10 配置时序结构体: 介绍了如何配置时序结构体，包括分屏、数据延迟等
52:55 初始化 STM 32: 介绍了如何初始化 STM 32，包括配置时钟、设置堆栈大小等。

- 如何配置 ADC 控制器包括选择寄存器组、地址复用、读写时序等. 并提供了程序示例。
53:22 配置 STM 32 的 Bank 和引脚配置
55:25 使用 Extend Mode 和 Read/Write Timing
59:35 配置寄存器和时序指针

- 如何使用库函数初始化 STM 32 的 SRAM 并注意了一些配置细节和读写时序的问题。
01:00:45 延展模式和内存类型有效性配置
01:04:11 SRAM 的初始化和配置
01:05:58 测试延展模式和内存类型有效性



## P 73 代码讲解--读写 SRAM

直接使用指针的方式对 STM 32 内存地址进行访问，访问内存地址的时候 FSMC 会自动激活，就会产生时序来访问 SRAM，对 SRAM 的内容进行读写。


```main.c
 /**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   SRAM测试
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
#include "./usart/bsp_usart.h"
#include "./led/bsp_led.h"
#include "./sram/bsp_sram.h"

//1.使用指针对SRAM进行读写
//2.使用绝对地址的方式来访问SRAM

//uint8_t testValue;//由编译器分配地址，没有指定时直接分配到0x20000028，，这个地址范围是STM32内部的SRAM
//使用__attribute__ 定义变量时，需要定义为全局变量
uint8_t testValue __attribute__ ((at (SRAM_BASE_ADDR+0x40)));//让这个全局变量指定向某一个地址，这个地址刚好是SRAM的基地址，访问这个变量的时候就能直接访问SRAM了
//本身变量的地址是由MDK编译器分配的，但是通过语法可以指定它指向某一个地址


/*
 * 函数名：main
 * 描述  ：主函数
 * 输入  ：无
 * 输出  ：无
 * 提示  ：不要乱盖PC0跳帽！！
 */
int main(void)
{ 
	
//	uint8_t testValue __attribute__ ((at (SRAM_BASE_ADDR+0x40)));//如果定义为局部变量，它的地址还是在内部SRAM的范围
	//为什么不能在局部变量的时候使用？是因为局部变量使用的是Stack栈区的空间，栈区空间默认分配到内部SRAM的。
	
	uint8_t *p;
	uint16_t *p16;
	float *pf;
//	uint8_t temp;
	
	LED_GPIO_Config();
	LED_BLUE;
	
	/* 配置串口为：115200 8-N-1 */
	USART_Config();
	printf("\r\n 这是一个SRAM测试实验 \r\n");
	SRAME_Init();
	
	//把数据强制转化成指针
	p = (uint8_t *)SRAM_BASE_ADDR;
	
	*p = 0xAB;
	
//	temp = *p;
	
	printf("\r\n读出的数据内容为： 0x%x\r\n",*p);
	
	//转换成16位的指针，一次可以访问2个字节的内容
	p16 = (uint16_t *)(SRAM_BASE_ADDR+10);
	*p16 = 0xcdef;
	printf("\r\n读出的数据内容为： 0x%x\r\n",*p16);
	
	//类似的可以写32位(如float)的指针
	pf = (float *)(SRAM_BASE_ADDR+20);
	*pf = 56.35;
	printf("\r\n读出的数据内容为： 0x%.2f\r\n",*pf);
	
	testValue = 50;
	
	printf("testValue = %d,testValue addr = 0x%x",testValue,(unsigned int)&testValue);

	while(1);  
}



/*********************************************END OF FILE**********************/

```



#### B 站 AI 视频总结

如何通过指针访问 STM32的 SRAM 进行读写测试。首先需要定义 SRAM 的基地址和大小，然后通过指针访问地址来实现对 SRAM 的读写操作。通过配置 FSMC，FMAC 会产生时序来访间 SRAM，并将数据写入 SRAM 中。通过读取地址可以获取写入的数据内容。视频中演示了如何进行读写测试，并提到了可能出现的问题和解决方法。















































































