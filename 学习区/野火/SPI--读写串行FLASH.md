
## P 55 SPI 协议介绍

### SPI 协议简介

IIC 是比较低速的，SPI 是高速的。
![[../../annex/SPI--读写串行FLASH_image_1.png]]
SPI 是全双工（可以同时接收和发送）的通信总线。用于板级（芯片）之间通讯较多。要求通讯速率较高的场合。（SPI 支持 25 M，45 M 等都有，本身 SPI 协议没有限制，但是受限于硬件设备）
IIC 是半双工的。最高时钟是 3.4 MHz（但是很多 IIC 的设备都最高只支持到 400 kHz）

#### SPI 物理层特点

![[../../annex/SPI--读写串行FLASH_image_2.png]]
实际上 SPI 设备只有 4 根线。
一般来说叫 CS 引脚（芯片/从设备选择信号线，也称为片选信号线），每个从设备都会有一根 CS 信号线独立接到主机的一个引脚。其他的线是公用的。

不是通过数据线来广播地址，而是通过每个设备独立的片选信号线来选择设备，片选信号线设为低电平就代表着该设备被选中，即 CS 线是用来区分各个从机的。作用：可以用来防止干扰，选中某个设备。

调为高电平就相当于意味结束信号。SPI 中直接用 CS 线拉低代表通讯的开始，CS 线由低电平变为高电平视为通讯的结束。


其他三根线的作用：SCK、MOSI、MISO
![[../../annex/SPI--读写串行FLASH_image_3.png]]
SCK （serial clock）线：时钟信号线，用于通讯数据同步。（使用时钟信号线来实现数据同步）
在 SCK 线的协调下，就规定了数据的有效性。
SPI 跟 IIC 不同，IIC 是在 SCK 的高电平下被采集。SPI 比较复杂，在边缘采样。既可能是高电平采样，也可能是低电平采样。
APB 1 总线的时钟就叫 PCLK 1，APB 2 总线的时钟就叫 PCLK 2。
STM 32 的 SPI 时钟频率最大为 f<sub>pclk</sub>÷2，通讯受限于低速设备。


![[../../annex/SPI--读写串行FLASH_image_4.png]]
MOSI 和 MISO 作为传输数据的。一根接收，一根发送，所以支持全双工。
MOSI 和 MISO 是 SPI 协议中专用的数据通讯（传输数据）的引脚。
MOSI（master output，slave input）：主设备输出/从设备输入引脚。
MISO（master input，slave output）：主设备输入/从设备输出引脚。

MOSI 和 MISO 也是串行的数据线。在一个时钟的协调下，只能传输一个数据位的信号。（传输一个字节就像 IIC 一样，传输完 8 个时钟就可以传输一个字节了）。

由于 MOSI 和 MISO 是同时在时钟线是同步的，在同样的时钟协调下，在发送数据的同时也可以接受数据，（MOSI 发送数据，MISO 接收数据）。


#### SPI 协议层

![[../../annex/SPI--读写串行FLASH_image_5.png]]
以主机为视角。
这里的触发代表数据变化，MOSI 和 MISO 是不稳定的，这个时候是不会采样的。
在采样过程中，数据都是稳定的，在这个时候 MOSI 和 MISO 所表达的数据（所表达的电平信号）就代表了一个数据位的数据。只要经过 8 个时钟，就能够把数据传输出去了。
MSBOUT 和 LSBOUT 串行通讯的时候，代表着高位字节先行还是低位字节先行。大多是规定是高位字节先行。
并不是规定了上升沿就变化，下降沿就采集，还需要配置的。

![[../../annex/SPI--读写串行FLASH_image_6.png]]

![[../../annex/SPI--读写串行FLASH_image_7.png]]

CPHA = 0 时。（图中绿色部分进行数据采样）奇数边沿采样
![[../../annex/SPI--读写串行FLASH_image_8.png]]
时钟极性 CPOL、时钟相位 CPHA 配置 SPI 协议是在哪个边缘触发或采样的。
时钟极性 CPOL：指 SPI 通讯设备处于空闲状态时，SCK 信号线的电平信号。（CS 信号线为高电平的时候就是空闲状态，在通讯开始之前，此时的 SCK 引脚的电平就表示时钟极性）
SCK 在空闲状态是低电平时，CPOL = 0，就说是时钟极性为 0。
SCK 在空闲状态是高电平时，CPOL = 1，就说是时钟极性为 1。

时钟相位 CPHA：指数据的采样时刻。
当 CPHA = 0 时，MOSI 或 MISO 数据线上的信号将会在 SCK 时钟线的 “奇数边沿” 被采样。
当 CPHA = 1 时，MOSI 或 MISO 数据线上的信号将会在 SCK 时钟线的 “偶数边沿” 被采样。
边沿是指上升沿/下降沿。

CPOL 时钟极性和 CPHA 时钟相位 配合才能确认到底是上升沿还是下降沿。

CPHA = 1 时。（图中绿色部分进行数据采样）偶数边沿采样
![[../../annex/SPI--读写串行FLASH_image_9.png]]

此处，16 个时钟边沿，即 8 个时钟，所以说传输的是 8 个数据位（不是硬性规定）。

#### 总结
![[../../annex/SPI--读写串行FLASH_image_10.png]]
CPOL 时钟极性和 CPHA 时钟相位：相同上升沿、相异下降沿。



## P 56 SPI 框图及通讯过程

#### 功能框图
![[../../annex/SPI--读写串行FLASH_image_11.png]]

![[../../annex/SPI--读写串行FLASH_image_12.png]]
SPI 1 挂载在 APB 2 上，最大时钟频率位 36 MHz。
SPI 2 等挂载在 APB 1 上，最大时钟频率为 18 MHz。

STM32F10X 的数据帧长度为 8 位或 16 位，即一次传输的数据位。
可以设置高位先行还是低位先行，支持双线全双工。可以进行双线单向传输（提高传输速率），单线传输（节省数据线）

![[../../annex/SPI--读写串行FLASH_image_13.png]]
波特率发生器，控制 SCK 引脚，产生时钟输出。SPI 协议也是只有主机才能够产生时钟，所以波特率发生器就是 STM 32 作为主机的时候使用的。作为从机，波特率发生器就从外面接收时钟，作为主机就对外发送时钟。
跟 IIC 类似，数据接收进来后，都是通过移位寄存器进行数据的传输发送。
整体控制逻辑：
CR 寄存器（control 控制寄存器，CR 2 是配置 DMA 等有关的），
SR 寄存器（status 状态寄存器），
BR 寄存器（控制波特率），
CRL 寄存器（CRL 1 和 CRL 2 分别配置不同的功能）.


##### 通讯引脚
![[../../annex/SPI--读写串行FLASH_image_14.png]]

SPI 3 所用的引脚用在了 SWDK 下载接口。（引脚是复用的）用起来会比较麻烦。所以下载程序的时候可能要按着复位键，然后再点 SMDK 下载按钮才能够把程序下载进去。
就是说，让芯片处于复位状态，它就不会占用这个 SPI 3 引脚而导致下载不进去。

一般不会直接用 NSS 线，可以不直接使用硬件控制逻辑，是可以直接产生 SPI 时序。直接控制 NSS 引脚，在发送数据的时候产生起始信号或结束信号，直接由硬件产生。
但实际上一般不会直接用 NSS 线。一般像软件模拟 IIC 一样类似的方法，用软件控制这个引脚。
NSS 引脚用软件控制，随便选择一个普通的 GPIO，使用推挽输出的功能，控制它要开始通讯的时候把它拉成低电平，结束的时候改成高电平。
其他的引脚就交给硬件来控制。

##### 时钟控制逻辑
![[../../annex/SPI--读写串行FLASH_image_15.png]]
波特率发送器根据 SPI_CR 1 寄存器的 BR 0~2（3 个位）来设置的
![[../../annex/SPI--读写串行FLASH_image_16.png]]
三个数据位，一共可以配置成 000~111（8 种配置）。
配置成 0 的时候，是二分频，即至少是二分频。SP 1 挂载在 APB 2 上，最高频率是 72/2 = 36 MHz。
如果是 SP 2 和 SP 3，挂载在 APB 1 上，最高频率是 36/2 = 18 MHz。（STM 32 给我们可以随便配置时钟，就是为了兼容低频设备）

这个时钟配置是 STM 32 作为通讯主机才有效的，如果作为从机就失效。


##### 数据控制逻辑
![[../../annex/SPI--读写串行FLASH_image_17.png]]
实际上是有两个移位寄存器，一个用来接收，一个用来发送。
读/写 DR 寄存器，都是同一个。但是在写的时候，会把这些数据放到发送缓冲区里，如果是读的时候，就会把接收缓冲区里的内容读取到内存里面。
虽然功能框图中移位寄存器只有一个，因为是同时接收和发送，所以内部结构是由两个的，这个图只是为了简化我们的理解，只画成了一个，实际上是有两个的。

![[../../annex/SPI--读写串行FLASH_image_18.png]]


##### 整体控制逻辑

![[../../annex/SPI--读写串行FLASH_image_19.png]]
整体控制逻辑负责协调整个 SPI 外设，控制逻辑的工作模式根据 “控制寄存器(CR1/CR2)” 的参数而改变，基本的控制参数包括前面提到的 SPI 模式、波特率、LSB 先行、主从模式、单双向模式等等。

在外设工作时，控制逻辑会根据外设的工作状态修改“状态寄存器(SR)”，只要读取状态寄存器相关的寄存器位，就可以了解 SPI 的工作状态了。除此之外，控制逻辑还根据要求，负责控制产生 SPI 中断信号、DMA 请求及控制 NSS 信号线。

实际应用中，一般不使用 STM32 SPI 外设的标准 NSS 信号线，而是更简单地使用普通的 GPIO，软件控制它的电平输出，从而产生通讯起始和停止信号。

![[../../annex/SPI--读写串行FLASH_image_20.png]]
双线双向模式：标准的 SPI 协议，MOSI 进行发送，MISO 进行接收。
单线双向模式：虽然说是双向，但是位 14 又配置只能接收/只能发送，实际上也是一个单工，一般来说就不使用了。

CRCEN 位 13：可以用来配置 CRC 校验，防止数据通信的时候发生错误。（CRC 校验也很少使用，因为一般很少 SPI 设备是支持这个 CRC 设备校验的）

DFF 位 11：用来配置传输的数据帧是 8 位或者 16 位，一般 SPI 用 8 位。

RXONLY 位 10：配置双线双向模式，可以配置成全双工（MOSI 和 MISO 正常的，一个接收、一个发送）。如果配置成 1：就是两根线都作为接收方式，两根线同时接收数据，接收的快一点，一个时钟传输两个数据位，一般也很少用。

SSM 位 9：配置 NSS 线是使用软件模式还是硬件模式。
为什么推荐使用软件配置？
因为可以随便选择很多个 GPIO 引脚，如果有很多个从机，就需要很多的 NSS 线，而用硬件 IIC 的话引脚数少（一般只有 1 根 NSS 引脚的），使用软件配置的话，就可以随便使用普通的 GPIO 作为 NSS 引脚了。
使用软件配置的时候，主要是把这个 NSS 引脚配置成推挽输出功能，然后启用这个软件从设备管理这个寄存器位就可以使用了。

LSBFIRST 位 7：控制高位先行或者低位先行（从高位开始读取还是从低位开始读取）

SPE 位 6：配置 SPI 使能。

位 5:3：配置波特率

MSTR 位 2：作为主设备或从设备

CPOL 位 1：配置时钟极性，即空闲时，SCK 的电平高还是低

CPHA 位 0：配置时钟相位，配置奇/偶数边沿 采样


SRI_CR 2 寄存器主要是一些中断信号、使能中断等。



#### STM 32 作为主机时的通讯过程
![[../../annex/SPI--读写串行FLASH_image_21.png]]
CS 线没有画出。
图中的标志位其实就是 SPI 相关寄存器的数据位
注意：MOSI 和 MISO 是同时进行的。双线全双工的意义就在于此，同一根时钟的同步下，一根线作为发送，另一根线作为接收。
首先，软件向 SPI_DR 写入 0xF1，
0xF1 写入到发送缓冲区后，TXE 标志就置 0（发送到数据缓冲区后，发送缓冲区非空，TXE表示 0）
DR 中的数据很快就会被搬运到数据移位寄存器中，很快就会变空, 所以 TXE 标志很快就置 1。搬到移位寄存器后，就一位一位的把 0xF1 通过 MOSI 引脚发送出去了。
接着下一个数据 0xF2 的时候，往 DR 寄存器写入第二个字节0xF2的时候，因为 0xF1 还没有通过 MOSI 发送完毕，所以 0xF2 就一直放在 DR 寄存器中暂存，还没有转移到移位寄存器中。
过了 8 个时钟，把第一个字节 0xF1 一位一位的发送完了，再把第二个字节 0xF2 搬到移位寄存器中，TXE 再次置 1。
以此类推发送数据。

BSY 标志其实是与 CS 引脚是反过来的，作用：检测总线忙碌就不去进行干扰。

MISO 接收同理（是同时进行的）
从外边接收到数据 0xA1，接收完 8 个数据位了，接受缓冲区非空，就会把 RXNE 标志置 1。
为 1 的时候就可以读取接收缓冲区（读取同样一个 DR 寄存器）了。

接收的时候并不是只检测 RXNE 标志位就可以了，因为如果想要接收数据的时候，由于没有控制它写入数据，SCK 引脚是不会产生时钟的。所以就算只是接收数据，也要往发送缓冲区写入数据，才能触发 SCK 引脚产生时钟。只有产生时钟了，外边的 flash 才能够正常的在时钟的同步下把数据传给 STM 32。
SCK 需要驱动，所以也是需要发送数据的。而 flash 可以忽略这些数据。


##### 通讯过程 --文字描述：
![[../../annex/SPI--读写串行FLASH_image_22.png]]
* 控制 NSS 信号线，产生起始信号 (图中没有画出)；
* 把要发送的数据写入到“数据寄存器 DR”中，该数据会被存储到发送缓冲区；
- 通讯开始，SCK 时钟开始运行。MOSI 把发送缓冲区中的数据一位一位地传输出去；MISO 则把数据一位一位地存储进接收缓冲区中；
- 当发送完一帧数据的时候，“状态寄存器 SR”中的“TXE 标志位”会被置 1，表示传输完一帧，发送缓冲区已空；类似地，当接收完一帧数据的时候，“RXNE 标志位”会被置 1，表示传输完一帧，接收缓冲区非空；
- 等待到“TXE 标志位”为 1 时，若还要继续发送数据，则再次往“数据寄存器 DR”写入数据即可；等待到“RXNE 标志位”为 1 时，通过读取“数据寄存器 DR”可以获取接收缓冲区中的内容。

假如使能了 TXE 或 RXNE 中断，TXE 或 RXNE 置 1 时会产生 SPI 中断信号，进入同一个中断服务函数，到 SPI 中断服务程序后，可通过检查寄存器位来了解是哪一个事件，再分别进行处理。也可以使用 DMA 方式来收发“数据寄存器 DR”中的数据。



## P 57 初始化结构体讲解

![[../../annex/SPI--读写串行FLASH_image_23.png]]

有很多是带有 I²S 的，说明这些函数跟 IIS 是共用的。

![[../../annex/SPI--读写串行FLASH_image_24.png]]
跟其它外设一样，STM 32 标准库提供了 SPI 初始化结构体及初始化函数来配置 SPI 外设。初始化结构体及函数定义在库文件“stm 32 f 10 x_spi. H”及“stm 32 f 10 x_spi. C”中，编程时我们可以结合这两个文件内的注释使用或参考库帮助文档。

![[../../annex/SPI--读写串行FLASH_image_25.png]]

![[../../annex/SPI--读写串行FLASH_image_26.png]]

SPI_Direction
 本成员设置 SPI 的通讯方向，可设置为双线全双工(<font color="#00b050">SPI_Direction_2Lines_FullDuplex</font>)，双线只接收(SPI_Direction_2Lines_RxOnly)，单线只接收(SPI_Direction_1Line_Rx)、单线只发送模式(SPI_Direction_1Line_Tx)。


SPI_Mode
 本成员设置 SPI 工作在主机模式(SPI_Mode_Master)或从机模式(SPI_Mode_Slave  )，这两个模式的<font color="#00b0f0">最大区别为 SPI 的 SCK 信号线的时序</font>，SCK 的时序是由通讯中的主机产生的。若被配置为从机模式，STM32的 SPI 外设将接受外来的 SCK 信号。

SPI_DataSize
 本成员可以选择 SPI 通讯的数据帧大小是为8位(SPI_DataSize_8b)还是16位(SPI_DataSize_16b)。
 此处说的 8 位和 16 位只是配置这个数据一帧的长度。

SPI_CPOL 和 SPI_CPHA
 这两个成员配置 SPI 的时钟极性 CPOL 和时钟相位 CPHA，这两个配置影响到 SPI 的通讯模式，
 时钟极性 CPOL 成员，可设置为高电平 (SPI_CPOL_High)或低电平 (SPI_CPOL_Low )。
 时钟相位 CPHA 则可以设置为 SPI_CPHA_1 Edge (在 SCK 的奇数边沿采集数据) 或 SPI_CPHA_2 Edge (在 SCK 的偶数边沿采集数据) 。

SPI_NSS
 本成员配置 NSS 引脚的使用模式，可以选择为硬件模式 (SPI_NSS_Hard )与软件模式 (<font color="#00b050">SPI_NSS_Soft</font>  )，在硬件模式中的 SPI 片选信号由 SPI 硬件自动产生，而软件模式则需要亲自把相应的 GPIO 端口拉高或置低产生非片选和片选信号。
 实际中软件模式应用比较多。
 到时候配置软件模式，初始化的时候，只要把 CLK、MISO、MOSI 这三根线配置成复用推挽输出/输入模式，NSS 就配置成普通的推挽输出。
 ![[../../annex/SPI--读写串行FLASH_image_27.png]]

SPI_BaudRatePrescaler
 本成员设置波特率分频因子，分频后的时钟即为 SPI 的 SCK 信号线的时钟频率。这个成员参数可设置为 fpclk 的 2、4、6、8、16、32、64、128、256分频。
 ![[../../annex/SPI--读写串行FLASH_image_28.png]]

SPI_FirstBit
 所有串行的通讯协议都会有 MSB 先行 (高位数据在前)还是 LSB 先行 (低位数据在前)的问题，而 STM 32 的 SPI 模块可以通过这个结构体成员，对该特性编程控制。

SPI_CRCPolynomial
 这是 SPI 的 CRC 校验中的多项式，若我们使用 CRC 校验时，就使用这个成员的参数 (多项式)，来计算 CRC 的值。

<font color="#ff0000">配置完这些结构体成员后，要调用 SPI_Init 函数把这些参数写入到寄存器中，实现 SPI 的初始化，然后调用 SPI_Cmd 来使能 SPI 外设</font>。




## P 58 Flash 芯片介绍

![[../../annex/SPI--读写串行FLASH_image_29.png]]

W25Q64BV。
64 表明了芯片的容量。（64 M-bit，64/8 = 8 M 字节（一个字节等于 8 个数据位））
Serial flash memory 串行 flash 存储器，flash 分 nond flash 和 nor flash（支持 XIP 的）

引脚的封装。
![[../../annex/SPI--读写串行FLASH_image_30.png]]
括号中的（IO<sub>0~3</sub>）是干什么的？这几个引脚可以作为第二功能，SPI 有 MOSI 和 MISO 双工通讯，但是有时为了加快传输速度，flash 芯片可以进入一个快速模式（Quad Output）。使用四个引脚来同时进行通讯一次，一个时钟就传输 4 个数据位。
这种特殊模式其实在 STM 32 中是不支持的，因为 STM 32 只支持标准的 SPI 协议。
![[../../annex/SPI--读写串行FLASH_image_31.png]]

![[../../annex/SPI--读写串行FLASH_image_32.png]]

Flash 和 EEPROM 到底有什么不同？
![[../../annex/SPI--读写串行FLASH_image_33.png]]

一共有 128 个块，每个块 64 kb。每个块是分开来的，而 EEPROM 是不分开的。Flash 的每个块又进一步的细分。分成 0~15 sector（扇区），每个扇区 4 kb（4 X 16=64 kb）。
为什么这么分？
主要是因为 Flash 的存储特性：
1、在写入数据之前必须先擦除。
2、擦除时会把数据位全置 1。擦除的操作就是把所有的数据位改成 1
3、写入数据时只能把为 1 的数据位改成 0
4、擦除时必须按最小单位（一般为一个扇区）来擦除。
EEPROM 中可以一个字节一个字节的擦除，而对于 Flash，只能以扇区为单位进行擦除 （4096 字节，4 kb）

Norflash 可以以一个字节写入，这也是为什么可以支持 XIP（直接放程序进行执行，方便内核去执行指令）
Nandflash 可以以一个块或扇区位单位进行读写（存储空间存储密度大一点）


![[../../annex/SPI--读写串行FLASH_image_34.png]]
带括号的表示这些数据是从 flash 传输给 STM 32 （控制器）的
没有带括号的这些内容就是由控制器传输给 flash
括号中的值表示返回的是该数据位的寄存器的值

![[../../annex/SPI--读写串行FLASH_image_35.png]]
规定了一些特殊的时序，flash 内部进行协调，如：约定好了接收到第一个字节为 0x05 就认为是读取状态寄存器的命令。然后就会根据约定返回寄存器的值。
如果没有结束通讯，会不断的返回状态寄存器的值，STM 32 接收到第 0 位的字节后，读取到了最低位，就能够知道 BUSY 位是否为 1 了，进行确认 Flash 是否忙碌。

![[../../annex/SPI--读写串行FLASH_image_36.png]]
Sector Erase，每一次会擦除 4 KB，该命令的代码是 0x20。向 Flash 芯片发送代码 0x20，后面的三个字节是地址。
Flash 的地址一共由 24 位来表示，所以需要通过 3 个字节来传输要擦除的地址。
将 24 位的地址分成 3 个字节发送给 Flash。
Flash 接收到接收到代码和地址后就会对相应的存储空间进行擦除。
擦除之后又可以读取状态寄存器来了解是否擦除完毕。如果擦除完成就可以向它写入数据。

![[../../annex/SPI--读写串行FLASH_image_37.png]]
想要写它写入数据的命令：Page Program（页写入），代码是 0x02。
同样的，跟擦除类似，需要先给它一个三个字节的 24 位地址。
发送了地址之后，再向它写入数据。
STM 32 向 Flash 写入数据，应该是没有括号的。这个数据是 STM 32 传过去的。

![[../../annex/SPI--读写串行FLASH_image_38.png]]
读取数据过程同理。

![[../../annex/SPI--读写串行FLASH_image_39.png]]
这些是 Flash 的 ID 号，dummy 其实就是空白字节。
STM 32 可以发送命令代码之后，再随便发几个无关的数据，dummy 可以是任意数据。
然后在第 4 个字节再发送 0x00
在第 5 个字节 flash 就会返回 MF0~7、ID0~7 (返回下图中的几个值)
![[../../annex/SPI--读写串行FLASH_image_40.png]]
ID 号的作用：特别在开机检测的时候，一般是不知道 Flash 芯片是否正常连接的。
可以利用 ID 号，通过读取 ID 号和它固定的 EF 或 4017 的值是否相等来确认 Flash 芯片是否正常工作。
看 STM 32 和 Flash 是否能够正常连接、正常通讯。


![[../../annex/SPI--读写串行FLASH_image_41.png]]
Power -down，进行进入低功耗模式。

![[../../annex/SPI--读写串行FLASH_image_42.png]]
Release Power down or HPM / Device ID，唤醒命令，重新进入正常的状态。Flash 也会返回一个 ID 号。



![[../../annex/SPI--读写串行FLASH_image_43.png]]
Read 读取时序图
传回地址是通过 DO 引脚（flash 的 output 引脚，即 MISO 引脚）的

![[../../annex/SPI--读写串行FLASH_image_44.png]]
Page program 写入时序图
一次可以写入 1~256 个字节
一般会严格要求自己的输入参数是严格等于 sector（扇区）的基地址的，保证一擦除 4096 字节就刚好是该扇区
![[../../annex/SPI--读写串行FLASH_image_45.png]]



#### B 站 AI 视频总结
使用 SPI 来读写板子上的 Fres 芯片的相关知识，包括 Fres 芯片的型号、存储空间、存储速度等特性，以及芯片的引脚说明和使用方法。同时，视频提醒在写程序之前需要了解好存储器的使用方法和连接方式，以及按照自己配套板子的原理图来写程序。此外，视频还介绍了 Fres 与 SPI 的对应关系，以及 SPI 协议中的一些引脚定义

SPI 和 SPI 相关的一些初始化结构体和库函数以及使用 SPI 读写 Fresh 芯片的方法和原理。

00:03 STM3和 STM32的 SPI 相关初始化和库函数介绍
01:00 学习使用 SPIFresh 存储器，包括读写和连接
04:49了解 Fres 和 STM32内部 Fres 的性质，以及 Fres 存储器的引脚定义

Fresh 芯片作为 SPI 闪存的一种特性和其存储空间的分块特性。

10:01 STM32F103支持快速模式的 SPI 通讯，提高传输速度
12:28 Fresh 存储器的写入和读取是通过控制逻辑和寄存器实现的
15:23 Fresh 存储器将 128 个块分成 16 个扇区，每个扇区是 4 KB

闪存芯片 Fresh 的存储特性包括擦除、写入和读取数据时的限制和要求

20:01 讲解 fresh 存储特性，擦除和写入只能改唯一数据位
22:46 擦除必须按最小单位进行，fresh 最小单位为扇区
25:53 flash 存储存储器必须先擦除再写入，而 nofresh 可以一个字节一个字节的写入

如何通过发送 SPI 命令来读取或写入 Flash 芯片中的状态寄存器,并详细讲解了命令表和时序图。

30:01  只需知道内部时序正在进行或已完成即可
30:28 状态寄存器值通过 SPI 接口发送命令获取
32:54 通过 SPI 协议读取状态寄存器，了解其值

STM 32 与 FLASH 之间的通信操作, 包括写入、擦除、读取等操作, 以及相关的命令和时序。

40:00 讲解了写入和擦除操作的过程和时序图
42:24 介绍了读取操作和读取状态寄存器的方法
45:03 讲解了使用 ID 号确认 fresh 芯片正常工作的方法和命令

擦除、写入和读取数据的过程, 以及需要注意的细节包括地址对齐、擦除单位等。

50:04 擦除空间需按最小单位进行，发送相应扇区的最低位地址
52:20 擦除前需确认输入参数严格等于 sector 的激励值
54:50 下一节介绍报道和指南者原理图



## P 59 代码讲解-初始化 SPI

 ![[../../annex/SPI--读写串行FLASH_image_46.png]]
![[../../annex/SPI--读写串行FLASH_image_47.png]]


###### 代码

```bsp_spi_flash.c
/**
  ******************************************************************************
  * @file    bsp_i2c_ee.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   SPI-FLASH驱动
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */ 

#include "./flash/bsp_spi_flash.h"
#include "./usart/bsp_usart.h"		

static __IO uint32_t SPITimeout = SPIT_LONG_TIMEOUT;

static uint32_t SPI_TIMEOUT_UserCallback(uint8_t errorCode);


/**
  * @brief  SPI I/O配置
  * @param  无
  * @retval 无
  */
static void SPI_GPIO_Config(void)
{
  GPIO_InitTypeDef  GPIO_InitStructure; 

	/* 使能与 SPI 有关的时钟 */
	FLASH_SPI_APBxClock_FUN ( FLASH_SPI_CLK, ENABLE );
	FLASH_SPI_GPIO_APBxClock_FUN ( FLASH_SPI_GPIO_CLK, ENABLE );
	
    
  /* MISO、MOSI、SCK*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_SCK_PIN;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;	        
  GPIO_Init(FLASH_SPI_SCK_PORT, &GPIO_InitStructure);

  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_MOSO_PIN;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;	        
  GPIO_Init(FLASH_SPI_MOSI_PORT, &GPIO_InitStructure);
	
	GPIO_InitStructure.GPIO_Pin = FLASH_SPI_MISO_PIN;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_IN_FLOATING;	        
  GPIO_Init(FLASH_SPI_MISO_PORT, &GPIO_InitStructure);
	
	//初始化CS引脚，使用软件控制，所以直接设置成推挽输出
	GPIO_InitStructure.GPIO_Pin = FLASH_SPI_CS_PIN;
  GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;	        
  GPIO_Init(FLASH_SPI_CS_PORT, &GPIO_InitStructure);
	
	FLASH_SPI_CS_HIGH;
	
}


/**
  * @brief  SPI 工作模式配置
  * @param  无
  * @retval 无
  */
static void SPI_Mode_Config(void)
{
  SPI_InitTypeDef  SPI_InitStructure.; 

	SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_2;
	//SPI 使用模式3
	SPI_InitStructure.SPI_CPHA = SPI_CPHA_2Edge;
	SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;
	SPI_InitStructure.SPI_CRCPolynomial = 0;//不适用CRC功能，数值随便写
	SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
	SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;//双线全双工
	SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;//MSB先行
	SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
	SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;
	
	SPI_Init(FLASH_SPIx,&SPI_InitStructure);	//写入配置到寄存器
	
	SPI_Cmd(FLASH_SPIx,ENABLE);	//使能SPI
	
}



/**
  * @brief  SPI 初始化
  * @param  无
  * @retval 无
  */
void SPI_FLASH_Init(void)
{

	SPI_GPIO_Config();
	SPI_Mode_Config();
	
//  I2C_GPIO_Config(); 
// 
//  I2C_Mode_Configu();

}


/**
  * @brief  Basic management of the timeout situation.
  * @param  errorCode：错误代码，可以用来定位是哪个环节出错.
  * @retval 返回0，表示SPI读取失败.
  */
static  uint32_t SPI_TIMEOUT_UserCallback(uint8_t errorCode)
{
  /* Block communication and all processes */
  FLASH_ERROR("I2C 等待超时!errorCode = %d",errorCode);
  
  return 0;
}
/*********************************************END OF FILE**********************/


```


```bsp_spi_flash.h
#ifndef __SPI_FLASH_H
#define	__SPI_FLASH_H


#include "stm32f10x.h"

//如果使用了霸道开发板，就把宏配置成1，指南者配置成0
#define USE_BD		1

/**************************I2C参数定义，I2C1或I2C2********************************/
#define             FLASH_SPIx                                SPI1
#define             FLASH_SPI_APBxClock_FUN                   RCC_APB2PeriphClockCmd
#define             FLASH_SPI_CLK                             RCC_APB2Periph_SPI1
#define             FLASH_SPI_GPIO_APBxClock_FUN              RCC_APB2PeriphClockCmd



#define             FLASH_SPI_SCK_PORT                        GPIOA   
#define             FLASH_SPI_SCK_PIN                         GPIO_Pin_5

#define             FLASH_SPI_MOSI_PORT                       GPIOA 
#define             FLASH_SPI_MOSO_PIN                        GPIO_Pin_7

#define             FLASH_SPI_MISO_PORT                       GPIOA 
#define             FLASH_SPI_MISO_PIN                        GPIO_Pin_6

#define             FLASH_SPI_CS_PORT                         GPIOA 
#define             FLASH_SPI_CS_PIN                          GPIO_Pin_4

#if (USE_BD == 1)
	#define             FLASH_SPI_GPIO_CLK                        RCC_APB2Periph_GPIOA
	
	#define             FLASH_SPI_CS_PORT                         GPIOA 
	#define             FLASH_SPI_CS_PIN                          GPIO_Pin_4
#else
	#define             FLASH_SPI_GPIO_CLK                       (RCC_APB2Periph_GPIOA|RCC_APB2Periph_GPIOC)

	#define             FLASH_SPI_CS_PORT                         GPIOC 
	#define             FLASH_SPI_CS_PIN                          GPIO_Pin_0
#endif


//CS引脚配置
#define FLASH_SPI_CS_HIGH			GPIO_SetBits(FLASH_SPI_CS_PORT,FLASH_SPI_CS_PIN);
#define FLASH_SPI_CS_LOW			GPIO_ResetBits(FLASH_SPI_CS_PORT,FLASH_SPI_CS_PIN);


/*等待超时时间*/
#define SPIT_FLAG_TIMEOUT 								((uint32_t)0x1000)	
#define SPIT_LONG_TIMEOUT 								((uint32_t)(10 * SPIT_LONG_TIMEOUT))			

/*信息输出*///方便调试，方便发布程序
#define FLASH_DEBUG_ON         0	//调试的时候配置成1，整个程序就能输出各种各样的调试信息；但是配置成0后，整个工程这个语句涉及到的东西的调试信息都不会再输出出来了。

//调试的时候可能需要printf("111");但是调试完可能不需要这个东西输出了，就可以使用这种方式：
//FLASH_INFO通常作为信息输出，FLASH_ERROR用来输出错误，FLASH_DEBUG用来做输出调试信息
#define FLASH_INFO(fmt,arg...)           printf("<<-FLASH-INFO->> "fmt"\n",##arg)	//fmt其实就是作为printf函数的一个输入。arg通过两个##连接起来(其实就是一个连接符)
#define FLASH_ERROR(fmt,arg...)          printf("<<-FLASH-ERROR->> "fmt"\n",##arg)
#define FLASH_DEBUG(fmt,arg...)          do{\
                                          if(FLASH_DEBUG_ON)\
                                          printf("<<-FLASH-DEBUG->> [%s][%d]"fmt"\n",__FILE__,__LINE__, ##arg);\
                                          }while(0)	//为1输出调试信息，为0就不执行，相当于空操作
//__LINE__可以直接输出这个语句所在的行号,__LINE__就变成前面[%d]的%d的位置。如果还行知道在哪个文件就再加个[%s]

													
																					
void SPI_FLASH_Init(void);																					
																					

#endif /* __SPI_FLASH_H */

```

#### B 站 AI 视频总结

在写程序之前，需要先了解芯片的引脚连接和原理图。同时，讲解了 SPI 通讯接口的原理和使用方法，以及如何在不同电路中写出兼容性好的程序。视频还介绍了指南者和霸道开发版的不同之处，以及如何使用串口调试助手进行调试。最后，还介绍了 Fres 芯片的存储器规划和程序的使用方法。

- 如何通过原理图来了解芯片的连接方式, 以及如何写一个兼容多电路的程序。
00:05 学习原理图，了解芯片连接
01:21  搜索数据手册，了解 SPI 引脚连接
04:40 配置 SPI 引脚，设置推网输出，兼容不同电路程序

- 如何使用 SPI Flash 来进行读写操作及测试并讲解了相关的头文件和程序修改。
10:01 通过读取 id 号来检测芯片是否正常
10:58 演示如何在不同板子上编写程序
13:34 如何写一个 spi flash 的驱动程序来测试

- 如何使用不同的 SPI 时钟和 GPIO 引脚来操作 iPhone 的 SPI 接口, 并详细讲解了各个步骤。
20:00 分成三个，共有三个 spi，分别是接在 app r、app e、app a 上。
21:46 可以选择使用 spi 一时钟作为输入参数
24:27 可以根据不同的硬件切换不同的红或者不同的函数

- 如何初始化 GPIO 和 SPI 模式包括配置引脚和时钟等步骤并提供了一些编程技巧。
30:02 初始化宏和硬件，初始化 gpio 和 spi 模式
31:41 删除其他函数，添加初始化 gpio 和 spi 时钟的函数
36:12 在 gpio 章节中找到 spi 复用功能映射，配置成主模式和推广复用模式

- 如何使用库函数控制 SPI 接口以及配置工作模式。通过软件控制方式可以实现对数据的读写操作。
40:01 介绍霸道和指南者的使用情况
40:38 配置银角引脚和控制模式的方法
43:49 初始化 spi 工作模式的步骤和注意事项

- SPI 的双线全双工模式以及初始化和测试方法包括使用软件 SPI 等。
50:01 选择双线全双工，避免使用 research rey 或一条线。
51:11  选择软件 SPI 控制，方便添加多个设备
53:48 初始化完成后，要开始测试程序是否正常。




## P 60 代码讲解--初始化 SPI

发送数据后直接检测 TXE 标志位是不准确的，因为发送数据后，缓冲区立马就空了。因为数据被移位到数据移位寄存器中了。读取这个标志可以重新往它写入数据，但是无法确认数据是否发送完毕。而什么时候发送完可以检测 RXNE 标志位，因为 SPI 是同步接收的（发送和接收是同步的）。所以当我们检测 RXNE 标志位时，就可以知道 8 个数据位均已通过移位寄存器（通过数据线）发送出去了。
![[../../annex/SPI--读写串行FLASH_image_48.png|通讯过程图]]


DI 线发送给 flash 的数据 flash 会忽略这些数据。
而 STM32 又能从 DO 线接收数据。
![[../../annex/SPI--读写串行FLASH_image_49.png]]
所以函数同时具有发送和接收的功能。
而且控制 SPI 发送一个字节，才能够产生一个时序来接收数据
假如纯粹写一个接收数据的函数，不写写入语句，是没有时序的，没有时序就没法接收数据。
（时钟是由主机产生的，但是没有发送数据的时候 STM 32 是不会产生时钟的）











#### B 站 AI 视频总结

如何通过读取 Fresh 的 ID 号来验证硬件连接和初始化模式是否正常。通过编写一个读取 ID 号的函数实现对 Fresh 的 SPI 通讯的驱动。在函数中,首先需要检测标志位,确保发送缓冲区为空然后向数据寄存器写入要发送的数据最后等待发送完毕的标志。此外,还介绍了发送多个字节的方法和检测标志位的方法。
















































