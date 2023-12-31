

## P 74-75 显示器简介

![[../../annex/LCD-液晶显示_image_1.png]]

![[../../annex/LCD-液晶显示_image_2.png]]

显示器简介
显示器属于计算机的I/O设备，即输入输出设备。它是一种将特定电子信息输出到屏幕上再反射到人眼的显示工具。常见的有CRT显示器、液晶显示器、LED点阵显示器及OLED显示器。

液晶显示器
液晶显示器，简称 LCD (Liquid Crystal Display)，相对于上一代 CRT 显示器 (阴极射线管显示器)，LCD 显示器具有功耗低、体积小、承载的信息量大及不伤眼的优点，因而它成为了现在的主流电子显示设备，其中包括电视、电脑显示器、手机屏幕及各种嵌入式设备的显示器。

###### 液晶显示器
液晶是一种介于固体和液体之间的特殊物质，它是一种有机化合物，常态下呈液态，但是它的分子排列却和固体晶体一样非常规则，因此取名液晶。如果给液晶施加电场，会改变它的分子排列，从而改变光线的传播方向，配合偏振光片，它就具有控制光线透过率的作用，再配合彩色滤光片，改变加给液晶电压大小，就能改变某一颜色透光量的多少。

利用液晶控制的原理，做出可控红、绿、蓝光输出强度的显示结构，把三种显示结构组成一个显示单位，通过控制红绿蓝的强度，可以使该单位混合输出不同的色彩，这样的一个显示单位被称为像素。

注意液晶本身是不发光的，所以需要有一个背光灯提供光源，光线经过一系列处理过程才到输出，所以输出的光线强度是要比光源的强度低很多的，比较浪费能源 (当然，比 CRT 显示器还是节能多了)。而且这些处理过程会导致显示方向比较窄，也就是它的视角较小，从侧面看屏幕会看不清它的显示内容。另外，输出的色彩变换时，液晶分子转动也需要消耗一定的时间，导致屏幕的响应速度低。
![[../../annex/LCD-液晶显示_image_3.png]]


###### LED 显示器
LED 点阵彩色显示器的单个像素点内包含红绿蓝三色 LED 灯，显示原理类似实验板上的 LED 彩灯，通过控制红绿蓝颜色的强度进行混色，实现全彩颜色输出，多个像素点构成一个屏幕。由于每个像素点都是 LED 灯自发光的，所以在户外白天也显示得非常清晰，但由于 LED 灯体积较大，导致屏幕的像素密度低，所以它一般只适合用于广场上的巨型显示器。相对来说，单色的 LED 点阵显示器应用得更广泛，如公交车上的信息展示牌、店招等。
![[../../annex/LCD-液晶显示_image_4.png]]

###### OLED 显示器
新一代的 OLED 显示器与 LED 点阵彩色显示器的原理类似，但由于它采用的像素单元是“有机发光二极管”(Organic Light Emitting Diode)，所以像素密度比普通 LED 点阵显示器高得多

![[../../annex/LCD-液晶显示_image_5.png]]

OLED显示器不需要背光源、对比度高、轻薄、视角广及响应速度快等优点。待到生产工艺更加成熟时，必将取代现在液晶显示器的地位。
对于不同种类的显示器，其控制程序原理是类似的，都是以操作像素点为基础。
![[../../annex/LCD-液晶显示_image_6.png]]


###### 显示器的基本参数
不管是哪一种显示器，都有一定的参数用于描述它们的特性，各个参数介绍如下：

1、像素
像素是组成图像的最基本单元要素，显示器的像素指它成像最小的点，即前面讲解液晶原理中提到的一个显示单元。

2、分辨率
一些嵌入式设备的显示器常常以“行像素值 x 列像素值”表示屏幕的分辨率。如分辨率 800 x 480 表示该显示器的每一行有 800 个像素点，每一列有 480 个像素点，也可理解为有 800 列，480行。

3、色彩深度
色彩深度指显示器的每个像素点能表示多少种颜色，一般用“位”(bit)来表示。如单色屏的每个像素点能表示亮或灭两种状态 (即实际上能显示 2 种颜色)，用 1 个数据位就可以表示像素点的所有状态，所以它的色彩深度为 1 bit，其它常见的显示屏色深为 16 bit（RGB 565，16 个数据位，能够表达 2^16 种颜色）、24 bit（RGB 888，24 个数据位，能够表达 2^24 种颜色）。
RGB 888 -- 红绿蓝分量用 8 个数据位来表示 组成的格式

4、显示器尺寸 
显示器的大小一般以英寸表示，如 5 英寸、21 英寸、24 英寸等，这个长度是指屏幕对角线的长度，通过显示器的对角线长度及长宽比可确定显示器的实际长宽尺寸。

5、点距
点距指两个相邻像素点之间的距离，它会影响画质的细腻度及观看距离，相同尺寸的屏幕，若分辨率越高，则点距越小，画质越细腻。如现在有些手机的屏幕分辨率比电脑显示器的还大，这是手机屏幕点距小的原因；LED 点阵显示屏的点距一般都比较大，所以适合远距离观看。

###### 液晶控制原理
![[../../annex/LCD-液晶显示_image_7.png]]
完整的显示屏由液晶显示面板、电容触摸面板以及 PCB 底板构成。

液晶面板和 PCB 底板实际上是通过 FPC 接口连接的。

图中的触摸面板带有触摸控制芯片，该芯片处理触摸信号并通过引出的信号线与外部器件通讯面板中间是透明的，它贴在液晶面板上面，一起构成屏幕的主体，触摸面板与液晶面板引出的排线连接到PCB底板上。
根据实际需要，PCB底板上可能会带有“液晶控制器芯片”。因为控制液晶面板需要比较多的资源，所以大部分低级微控制器都不能直接控制液晶面板，需要额外配套一个专用液晶控制器来处理显示过程，外部微控制器只要把它希望显示的数据直接交给液晶控制器即可。而不带液晶控制器的PCB底板 ，只有小部分的电源管理电路，液晶面板的信号线与外部微控制器相连，直接控制。STM32F429系列的芯片不需要额外的液晶控制器，也就是说它把专用液晶控制器的功能集成到STM32F429芯片内部了，节约了额外的控制器成本。STM32F1系列的芯片控制液晶时，液晶屏需带有液晶控制器。

###### 液晶面板的控制信号
1、液晶面板的控制信号线 (不带液晶控制器)：
![[../../annex/LCD-液晶显示_image_8.png]]

2、RGB 信号线
RGB 信号线各有 8 根，分别用于表示液晶屏一个像素点的红、绿、蓝颜色分量。使用红绿蓝颜色分量来表示颜色是一种通用的做法，打开 Windows 系统自带的画板调色工具，可看到颜色的红绿蓝分量值，常见的颜色表示会在“RGB”后面附带各个颜色分量值的数据位数，如 RGB 565 表示红绿蓝的数据线数分别为 5、6、5 根，一共为 16 个数据位，可表示 216 种颜色；而这个液晶屏的种颜色分量的数据线都有 8 根，所以它支持 RGB 888 格式，一共 24 位数据线，可表示的颜色为224种。
![[../../annex/LCD-液晶显示_image_9.png]]

3、同步时钟信号 CLK
液晶屏与外部使用同步通讯方式，以 CLK 信号作为同步时钟，在同步时钟的驱动下，每个时钟传输一个像素点数据。

4、水平同步信号 HSYNC
水平同步信号 HSYNC (Horizontal Sync)用于表示液晶屏一行像素数据的传输结束，每传输完成液晶屏的一行像素数据时，HSYNC 会发生电平跳变，如分辨率为 800 x 480 的显示屏 (800 列，480 行)，传输一帧的图像 HSYNC 的电平会跳变 480次。

5、垂直同步信号 VSYNC
垂直同步信号 VSYNC (Vertical Sync)用于表示液晶屏一帧像素数据的传输结束，每传输完成一帧像素数据时，VSYNC 会发生电平跳变。其中“帧”是图像的单位，一幅图像称为一帧，在液晶屏中，一帧指一个完整屏液晶像素点。人们常常用“帧/秒”来表示液晶屏的刷新特性，即液晶屏每秒可以显示多少帧图像，如液晶屏以 60 帧/秒的速率运行时，VSYNC 每秒钟电平会跳变 60次。

刷新率，如：60 hz（60 帧每秒）。每秒从显存中把这些像素数据刷新 60 次，如果显示动态的画面就能显示出来。

6、数据使能信号 DE
数据使能信号 DE (Data Enable)用于表示数据的有效性，当 DE 信号线为高电平时，RGB 信号线表示的数据有效。

###### 液晶数据传输时序
向液晶屏传输一帧图像数据的时序：
![[../../annex/LCD-液晶显示_image_10.png]]

液晶屏显示的图像可看作一个矩形，液晶屏有一个显示指针，它指向将要显示的像素。显示指针的扫描方向方向从左到右、从上到下，一个像素点一个像素点地描绘图形。这些像素点的数据通过RGB数据线传输至液晶屏，它们在同步时钟CLK的驱动下一个一个地传输到液晶屏中，交给显示指针，传输完成一行时，水平同步信号HSYNC电平跳变一次，而传输完一帧时VSYNC电平跳变一次。
![[../../annex/LCD-液晶显示_image_11.png]]

液晶显示指针在行与行之间，帧与帧之间切换时需要延时，而且HSYNC及VSYNC信号本身也有宽度，这些时间参数说明见下表：
![[../../annex/LCD-液晶显示_image_12.png]]
在这些时间参数控制的区域，数据使能信号线“DE”都为低电平，RGB数据线的信号无效，当“DE”为高电平时，表示的数据有效，传输的数据会直接影响液晶屏的显示区域。

###### 显存
液晶屏中的每个像素点都是数据，在实际应用中需要把每个像素点的数据缓存起来，再传输给液晶屏，这种存储显示数据的存储器被称为显存。显存一般至少要能存储液晶屏的一帧显示数据，如分辨率为800x480的液晶屏，使用RGB888格式显示，它的一帧显示数据大小为：3x800x480=1152000字节；若使用RGB565格式显示，一帧显示数据大小为：2x800x480=768000字节。

电脑中这些数据直接存储在电脑中的内存条中，就是说使用内存条划出一块来作为存储显示数据的一个内容，因为是用集成显卡的，是 CPU 集成显卡的。（也有独立显卡）

这个芯片叫液晶控制器，加控制器的主要原因是该 stm 32 系列的驱动性能不足，所以必须要接一个这样的液晶控制器来控制液晶屏幕。自带液晶控制器的屏幕，内部包含了显存（至少有显存空间），然后要把自己的缓存的数据显存里面的数据控制传输到液晶面板上（能够实现时序等，不停的把自己显存空间的内容刷新到屏幕，控制器的功能也在于此）。STM 32 就只需要负责协调和控制跟液晶控制器的通讯（把数据缓存到液晶控制器的显存中，不需要管刷新、HWP 等等）
Stm32f429 芯片不需要额外的液晶控制器，因为 f429 内部在 stm 32 芯片内部集成了。
![[../../annex/LCD-液晶显示_image_13.png]]

电阻触摸屏和电容触摸屏


## P 76--3.2 寸液晶屏介绍

![[../../annex/LCD-液晶显示_image_14.png]]


###### 3.2 寸电阻触摸屏实物
![[../../annex/LCD-液晶显示_image_15.png]]

图中的标号③部分是液晶屏幕的整体，通过引出的排针接入到实验板上可对它进行控制，它分为标号①的液晶触摸面板和标号②的 PCB 底板两部分。

标号①处的液晶触摸面板由液晶屏和触摸屏组成，屏幕表面的灰色线框即为电阻触摸屏的信号线，触摸屏的下方即为液晶面板，在它的内部包含了一个型号为 ILI9341的液晶控制器芯片(由于集成度高，所以图中无法看见)，该液晶控制器使用8080接口与单片机通讯，图中液晶面板引出的 FPC 信号线即8080接口(RGB 接口已在内部直接与 ILI9341相连)，且控制器中包含有显存，单片机把要显示的数据通过引出的8080接口发送到液晶控制器，这些数据会被存储到它内部的显存中，然后液晶控制器不断把显存的内容刷新到液晶面板，显示内容。

标号②处的是PCB底板，它主要包含了一个电阻触摸屏的控制器XPT2046，电阻触摸屏控制器实质上是一个ADC芯片，通过检测电压值来计算触摸坐标。PCB底板与液晶触摸面板通过FPC排线座连接，然后引出到排针，方便与实验板的排母连接。


###### ILI 9341 液晶控制器简介
![[../../annex/LCD-液晶显示_image_16.png]]

该芯片最主核心部分是位于中间的 GRAM(Graphics RAM)，它就是显存。GRAM 中每个存储单元都对应着液晶面板的一个像素点。它右侧的各种模块共同作用把 GRAM 存储单元的数据转化成液晶面板的控制信号，使像素点呈现特定的颜色，而像素点组合起来则成为一幅完整的图像。

框图的左上角为 ILI9341的主要控制信号线和配置引脚，根据其不同状态设置可以使芯片工作在不同的模式，如每个像素点的位数是6、16还是18位；可配置使用 SPI 接口、8080接口还是 RGB 接口与 MCU 进行通讯。MCU 通过 SPI、8080接口或 RGB 接口与 ILI9341进行通讯，从而访问它的控制寄存器(CR)、地址计数器(AC)、及 GRAM。

在GRAM的左侧还有一个LED控制器(LED Controller)。LCD为非发光性的显示装置，它需要借助背光源才能达到显示功能，LED控制器就是用来控制液晶屏中的LED背光源。

GRAM (显存)，把像素数据存储在这里
黄色部分是常用的（8080 接口）。而通常情况下 RGB 用 16 位的颜色深度（RGB 565 的格式）。18 位的话每个像素点 >2 字节、<3 字节，很浪费空间。
IM【3：0】实际上液晶生产厂家直接就固化成知己诶使用 8080 接口了，所以说这个屏幕是没法改成支持 SPI 模式的。跟 STM 32 通讯是比较方便的，直接使用 FSMC 模拟是比较通用的一种方式。
![[../../annex/LCD-液晶显示_image_17.png]]

###### 液晶屏的信号线及 8080 时序
除了黄色的一些线都是 8080 相关的数据线了（不包括 LCD_BK 背光、LCD_RESET 复位的红色和蓝色都是 8080 的控制线）。红色的就是数据线，蓝色的就是控制线控制信号。
![[../../annex/LCD-液晶显示_image_18.png]]
ILI 9341 控制器根据自身的 IM【3:0】信号线电平决定它与 MCU 的通讯方式，它本身支持 SPI 及 8080 通讯方式，本示例中液晶屏的 ILI 9341 控制器在出厂前就已经按固定配置好 (内部已连接硬件电路)，它被配置为通过 8080 接口通讯，使用 16 根数据线的 RGB 565 格式。内部硬件电路连接完，剩下的其它信号线被引出到 FPC 排线，最后该排线由 PCB 底板引出到排针，排针再与实验板上的 STM 32 芯片连接，引出的排针信号线如下图：

![[../../annex/LCD-液晶显示_image_19.png]]
这些引出的信号线即8080通讯接口，带X的表示低电平有效，STM32通过该接口与ILI9341芯片进行通讯，实现对液晶屏的控制。通讯的内容主要包括命令和显存数据，显存数据即各个像素点的RGB565内容；命令是指对ILI9341的控制指令，MCU可通过8080接口发送命令编码控制ILI9341的工作方式，例如复位指令、设置光标指令、睡眠模式指令等等，具体的指令在《ILI9341.pdf》数据手册均有详细说明。

LCD_DB【15: 0】--D【15: 0】就是作为 RGB 565 格式的像素数据的信号
没有引出 CLK 引脚，所以说我们不是采用同步通讯的，使用异步通讯。
X 代表低电平有效
LCD_RS -- D/CX，这个引脚作为命令和数据的选择端，发送命令的时候就搞成低电平，通过 D【15: 0】D0 到 D15 发送一个信号，信号表示 2Ah。同时 CX 引脚搞成低电平，9341 就知道想发送的这个是命令代码，当成命令来解释。再通过数据线发送几个参数给他，这个时候参数要用数据来表示。

2Ah--0x2A，2A 的 16 进制，命令号就是 2A,它就是一个命令，然后下面发送了命令 Command 之后，可以发送一些命令相关的参数。
![[../../annex/LCD-液晶显示_image_20.png]]


###### 向 ILI 9341 写命令的时序图：
![[../../annex/LCD-液晶显示_image_21.png]]

写命令时序由片选信号 CSX 拉低开始，对数据/命令选择信号线 D/CX 也置低电平表示写入的是命令地址(可理解为命令编码，如软件复位命令：0x01)，以写信号 WRX 为低，读信号 RDX 为高表示数据传输方向为写入，同时，在数据线 D【17:0】(或 D【15:0】)输出命令地址，在第二个传输阶段传送的是命令的参数，所以 D/CX 要置高电平，表示写入的是命令数据，命令数据是某些指令带有的参数，如复位指令编码为0x01，它后面可以带一个参数，该参数表示多少秒后复位(实际的复位命令不含参数，此处只是为了讲解指令编码与参数的区别)。

当需要把像素数据写入 GRAM 时，过程很类似，把片选信号 CSX 拉低后，再把数据/命令选择信号线 D/CX 置为高电平，这时由 D【17:0】传输的数据则会被 ILI9341保存至它的 GRAM 中。


RESX 复位信号为高电平，表示芯片不处于复位状态，处于正常通讯的状态。
D/CX 数据命令选择信号。
WRX 写使能。
RDX 读使能。
D【17: 0】作为数据/命令的输入。
ILI 9341 -- D【17: 0】(LCD to Host) 这根线就表达从 LCD 到 STM 32 的一个数据。写时序里面，由于数据是从 STM 32 往液晶屏的，所以说写时序的时候数据是不从这里（这根线是没有信号的，这根线不是实际存在的，是一种表示）
主机 -- D【17: 0】(Host to LCD) ，实际上 STM 32 把命令和命令的数据传给 9341 芯片了，表示这么一个方向，这两根线是不存在的。

第一阶段：
首先片选使能，像 SPI Flash 一样，把它拉成低电平，就选中了。选中之后开始通讯。
复位引脚不能把它搞成低电平，不然芯片就在复位了。
命令选择端搞成低电平，发送的是命令。
因为是写命令，所以写使能拉成低电平（有效）。
那么就把数据通过 D【17: 0】配置 D/CX 引脚发送到 ILI 9341 芯片上，当命令来处理。
![[../../annex/LCD-液晶显示_image_22.png]]
第二阶段：
当数据来处理。
在 D/CX 处不一样。拉成高电平，再把数据参数发送出去。


读时序
![[../../annex/LCD-液晶显示_image_23.png]]

![[../../annex/LCD-液晶显示_image_24.png]]

Memory Write 函数
前面还省略了一些内容，比如设置光标，要在屏幕的哪个点显示。
![[../../annex/LCD-液晶显示_image_25.png]]



## P 77 使用 FSMC 模拟 8080 时序

##### 使用 STM 32 的 FSMC 模拟 8080 接口时序

ILI9341的8080通讯接口时序可以由 STM32 使用普通 I/O 接口进行模拟，但这样效率太低，STM32提供了一种特别的控制方法——使用 FSMC 接口实现8080时序。

如果 STM 32 芯片是 100 引脚以下，是没法用 FSMC 来模拟控制液晶屏。引脚太少，连 FSMC 控制 NOR FLASH 的方式都不支持。100 引脚不支持控制 SRAM，144 引脚才支持控制 SRAM，但是 100 引脚是支持控制类似 NOR FLASH 的，支持 FSMC。

在前面的《FSMC—扩展外部 SRAM》章节中了解到 STM32的 FSMC 外设可以用于控制扩展的外部存储器，而 MCU 对液晶屏的操作实际上就是把显示数据写入到显存中，与控制存储器非常类似，且8080接口的通讯时序完全可以使用 FSMC 外设产生，因而非常适合使用 FSMC 控制液晶屏（把液晶屏当成是 NOR FLASH 来控制）。

###### FSMC 简介
![[../../annex/LCD-液晶显示_image_26.png]]

控制 LCD 时，适合使用 FSMC 的 NOR \ PSRAM 模式，它与前面使用 FSMC 控制 SRAM 的稍有不同，控制 SRAM 时使用的是模式 A，而控制 LCD 时使用的是与 NOR FLASH 一样的模式 B（NOR FLASH 的功能），所以我们重点分析框图中 NOR FLASH 控制信号线部分。
![[../../annex/LCD-液晶显示_image_27.png]]

在控制 LCD 时，使用的是类似异步、地址与数据线独立的 NOR FLASH 控制方式，所以实际上 CLK、NWAIT、NADV 引脚（因为此处地址线和数据线是分开使用的，地址线和数据线复用时才使用的）并没有使用到。

（低电平有效）

NE【x】 引脚--片选引脚，NE 引脚一共有 4 根，使用不同的控制的时候会映射，代表着地址的映射，STM 32 内部地址的映射是不一样的。

###### FSMC NOR/PSRAM 中的模式 B 的写时序如下图：
![[../../annex/LCD-液晶显示_image_28.png]]
根据 STM 32 对寻址空间的地址映射，地址 0 x 6000 0000 ~0 x 9 FFF FFFF 是映射到外部存储器的，而其中的 0 x 6000 0000 ~0 x 6 FFF FFFF 则是分配给 NOR FLASH、PSRAM 这类可直接寻址的器件。

写时序（从 STM 32 往存储器方向，所以读使能 NOE 是高电平，是没效的；而写使能 NWE 写使能低电平，有效）

读时序要比写时序多两个 HCLK（在 (DATAST+1) HCLK 时钟周期后面还有 2 个 HCLK，作为结束，来完成读操作）


当 FSMC 外设被配置成正常工作，并且外部接了 NOR FLASH 时，若向0x60000000地址写入数据如0xABCD，FSMC 会自动在各信号线上产生相应的电平信号，写入数据。FSMC 会控制片选信号 NE1选择相应的 NOR 芯片，然后使用地址线 A25:0】输出0x60000000，在 NWE 写使能信号线上发出低电平的写使能信号，而要写入的数据信号0xABCD 则从数据线 D【15:0】输出，然后数据就被保存到 NOR FLASH 中

###### 用 FSMC 模拟 8080时序
![[../../annex/LCD-液晶显示_image_29.png]]
（右图的黄色部分等效左边的一整幅）

对比 FSMC NOR/PSRAM 中的模式 B 时序与 ILI9341 液晶控制器芯片使用的 8080 时序可发现，这两个时序是十分相似的(除了 FSMC 的地址线 A 和 8080的 D/CX 线，可以说是完全一样)

![[../../annex/LCD-液晶显示_image_30.png]]

对于 FSMC 和 8080接口，前四种信号线都是完全一样的，仅仅是 FSMC 的地址信号线 A【25:0】与8080的数据/命令选择线 D/CX 有区别。而对于 D/CX 线，它为高电平的时候表示数值，为低电平的时候表示命令，如果能使用 FSMC 的 A 地址线根据不同的情况产生对应的电平，那么就完全可以使用 FSMC 来产生8080接口需要的时序了。

为了模拟出 8080 时序，我们可以把 FSMC 的 A0地址线(也可以使用其它 A1/A2 等地址线)与 ILI9341 芯片 8080 接口的 D/CX 信号线连接，那么当 A0 为高电平时(即 D/CX 为高电平)，数据线 D【15:0】的信号会被 ILI9341 理解为数值，若 A0 为低电平时(即 D/CX 为低电平)，传输的信号则会被理解为命令。

（A【25: 0】有 26 根线，控制其中一根地址线，让他根据我们的发送命令/数据的需要，来在不同的时候产生不同的电平状态。发送命令低电平，发送数据高电平）


由于 FSMC 会自动产生地址信号，当使用 FSMC 向0x6xxx xxx1、0x6xxx xxx3、0x6xxx xxx5…这些奇数地址写入数据时，地址最低位的值均为1，所以它会控制地址线 A0(D/CX)输出高电平，那么这时通过数据线传输的信号会被理解为数值；若向0x6xxx xxx0 、0x6xxx xxx2、0x6xxx xxx4…这些偶数地址写入数据时，地址最低位的值均为0，所以它会控制地址线 A0(D/CX)输出低电平，因此这时通过数据线传输的信号会被理解为命令，如下表：
![[../../annex/LCD-液晶显示_image_31.png]]

有了这个基础，只要配置好 FSMC 外设，然后在代码中利用指针变量，向不同的地址单元写入数据，就能够由 FSMC 模拟出的8080接口向 ILI9341写入控制命令或 GRAM 的数据了。

<font color="#ff0000">注意：在实际控制时，以上地址计算方式还不完整，还需要注意 HADDR 内部地址与 FSMC 地址信号线的转换，关于这部分内容在代码讲解时再详细举例说明</font>。

跟 SRAM 不同，SRAM 有 UB、LB 作为掩码，可以直接以访问 8 位、以 16 位的方式来访问。
但是 NOR FLASH 实际上是没有 SRAM 的这种机制。NOR FLASH 是 16 位就只能以 16 位的方式去访问。而用 16 位的方式来访问的时候要注意 STM 32 内部地址的访问，STM 32 在内部实际上还有一个地址线叫 HADDR（AHB 地址线上的一个地址，字节地址），访问的时候会有地址对齐问题（NOR FLASH 是 16 位的，是 2 个字节的地址）。

![[../../annex/LCD-液晶显示_image_32.png]]
如果液晶屏是 8 位的话，那么 STM 32 内部的 HADDR 跟 FSMC_A【25:0】对应相连。访问第 0 个字节就是访问 NOR FLASH 的第 0 个字节。
但是如果外接的是 16 位的 NOR FLASH 的话，我们要访问 1 地址才相当于访问 NOR FLASH 的第 0 地址。
![[../../annex/LCD-液晶显示_image_33.png]]
内部 STM 32 （HADDR）会进行移位，不用管。
HADDR 的第 1 根数据线跟 NOR FLASH 的第 0 根数据线是连接的，达到移位的目的。
所以说比如在我们要控制 A 0 地址为输出高电平的时候，实际上要访问 0010 地址的时候才能正常的把 FSMC 的 A0 地址线产生一个达到我们高低电平的目的。也就是说，想要控制 A 0 地址线，在控制的结果要左移一位，相当于控制的时候要左移一位。


## P 78 FSMC 相关结构体介绍
跟 SRAM 基本一致，访问模式换成模式 B。
##### NOR FLASH 时序结构体
与控制SRAM时一样，控制FSMC使用NOR FLASH存储器时主要是配置时序寄存器以及控制寄存器，利用ST标准库的时序结构体以及初始化结构体可以很方便地写入参数。
![[../../annex/LCD-液晶显示_image_34.png]]
最重要的是 AddressSetupTime、DataSetupTime、AccessMode，设置 ADDSET、DATAST、设置访问模式。

##### FSMC 时序结构体
![[../../annex/LCD-液晶显示_image_35.png]]

FSMC_AddressSetupTime 
本成员设置地址建立时间，它可以被设置为 0-0 xF 个 HCLK 周期数，按 STM 32 标准库的默认配置，HCLK 的时钟频率为 72 MHz，即一个 HCLK 周期为1/72微秒。（HCLK 不是一定是 72 M，HCLK 跟 STM 32 的主频是一致的，主频是 36 M 就是 36 M，主频是 72 M 就是 72 M。这里配置 n 个 HCLK 时钟，所以用 n X 1/72 微秒，就可以得出延时多少时间）

FSMC_AddressHoldTime 
本成员设置地址保持时间，它可以被设置为 0-0 xF 个 HCLK 周期数。

FSMC_DataSetupTime 
本成员设置数据建立时间，它可以被设置为 0-0 xF 个 HCLK 周期数。

FSMC_BusTurnAroundDuration 
本成员设置总线转换周期，在 NOR FLASH 存储器中，地址线与数据线可以分时复用，总线转换周期就是指总线在这两种状态间切换需要的延时，防止冲突。控制其它存储器时这个参数无效，配置为 0 即可。

FSMC_CLKDivision 
本成员用于设置时钟分频，它以 HCLK 时钟作为输入，经过 FSMC_CLKDivision 分频后输出到 FSMC_CLK 引脚作为通讯使用的同步时钟。控制其它异步通讯的存储器时这个参数无效，配置为0即可。

FSMC_DataLatency 
本成员设置数据保持时间，它表示在读取第一个数据之前要等待的周期数，该周期指同步时钟的周期，本参数仅用于同步 NOR FLASH 类型的存储器，控制其它类型的存储器时，本参数无效。

FSMC_AccessMode 
本成员设置存储器访问模式，不同的模式下 FSMC 访问存储器地址时引脚输出的时序不一样，可选 FSMC_AccessMode_A/B/C/D 模式。一般来说控制异步 NOR FLASH 时使用 B 模式。

这个FSMC_NORSRAMTimingInitTypeDef 时序结构体配置的延时参数，将作为下一节的FSMC SRAM初始化结构体的一个成员。


##### FSMC 的 NOR FLASH 初始化结构体
FSMC初始化结构体，除最后两个成员是上一小节讲解的时序配置外，其它结构体成员的配置都对应到FSMC_BCR中的寄存器位。
![[../../annex/LCD-液晶显示_image_36.png]]


![[../../annex/LCD-液晶显示_image_37.png]]

![[../../annex/LCD-液晶显示_image_38.png]]

FSMC_Bank
本成员用于选择 FSMC 映射的存储区域，它的可选参数以及相应的内核地址映射范围见上面的表格

FSMC_DataAddressMux
本成员用于设置地址总线与数据总线是否复用 (FSMC_DataAddressMux_Enable /Disable)，在控制 NOR FLASH 时，可以地址总线与数据总线可以分时复用，以减少使用 STM 32 信号线的数量。

FSMC_MemoryType
本成员用于设置要控制的存储器类型，它支持控制的存储器类型为 SRAM、PSRAM 以及 NOR FLASH (FSMC_MemoryType_SRAM/PSRAM/NOR)。

FSMC_MemoryDataWidth
本成员用于设置要控制的存储器的数据宽度，可选择设置成 8 或 16 位 (FSMC_MemoryDataWidth_8 b /16 b)。

FSMC_BurstAccessMode 
本成员用于设置是否使用突发访问模式 (FSMC_BurstAccessMode_Enable/Disable)，突发访问模式是指发送一个地址后连续访问多个数据，非突发模式下每访问一个数据都需要输入一个地址，仅在控制同步类型的存储器时才能使用突发模式。

FSMC_AsynchronousWait
本成员用于设置是否使能在同步传输时使用的等待信号 (FSMC_AsynchronousWait_Enable/Disable)，在控制同步类型的 NOR 或 PSRAM 时，存储器可以使用 FSMC_NWAIT 引脚通知 STM32需要等待。

FSMC_WaitSignalPolarity 
本成员用于设置等待信号的有效极性，即要求等待时，使用高电平还是低电平 (FSMC_WaitSignalPolarity_High/Low)。

FSMC_WrapMode 
本成员用于设置是否支持把非对齐的 AHB 突发操作分割成 2 次线性操作 (FSMC_WrapMode_Enable/Disable)，该配置仅在突发模式下有效。

FSMC_WaitSignalActive
本成员用于配置在突发传输模式时，决定存储器是在等待状态之前的一个数据周期有效还是在等待状态期间有效 (FSMC_WaitSignalActive_BeforeWaitState/DuringWaitState)。

FSMC_WriteOperation 
这个成员用于设置是否写使能 (FSMC_WriteOperation_ Enable /Disable)，禁止写使能的话 FSMC 只能从存储器中读取数据，不能写入。

FSMC_WaitSignal
本成员用于设置当存储器牌突发传输模式时，是否允许通过 NWAIT 信号插入等待状态 (FSMC_WaitSignal_Enable/Disable)。

FSMC_ExtendedMode
本成员用于设置是否使用扩展模式 (FSMC_ExtendedMode_Enable/Disable)，在非扩展模式下，对存储器读写的时序都只使用 FSMC_BCR 寄存器中的配置，即下面的 FSMC_ReadWriteTimingStruct 结构体成员；在扩展模式下，对存储器的读写时序可以分开配置，读时序使用 FSMC_BCR 寄存器，写时序使用 FSMC_BWTR 寄存器的配置，即下面的 FSMC_WriteTimingStruct 结构体。

FSMC_ReadWriteTimingStruct
本成员是一个指针，赋值时使用上一小节中讲解的时序结构体 FSMC_NORSRAMInitTypeDef 设置，当不使用扩展模式时，读写时序都使用本成员的参数配置。

FSMC_WriteTimingStruct
同样地，本成员也是一个时序结构体的指针，只有当使用扩展模式时，本配置才有效，它是写操作使用的时序。


## P 79 液晶控制代码讲解1

![[../../annex/LCD-液晶显示_image_39.png]]

检查知道连接线没问题的话，板子可能卡死了, 跑到某个地方导致没法控制它
![[../../annex/LCD-液晶显示_image_40.png]]
按着板子的复位键再点软件的下载 Download，再释放板子的复位键就可以下载进去了

初始化 GPIO 端口 
(配置打开所有的时钟，
配置 GPIO-BK 背光、RST 复位的模式为推挽模式，
配置 GPIO-CS、RD、WR、DC 的模式为复用推挽模式，
配置 FSMC 相对应的数据线,FSMC-D0~D15)
```
void ILI9341_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;

	// 打开GPIO的时钟 //或语句，把这些端口连接起来
	RCC_APB2PeriphClockCmd(ILI9341_CS_CLK|ILI9341_RST_CLK|ILI9341_BK_CLK|
					ILI9341_RD_CLK|ILI9341_WR_CLK|ILI9341_DC_CLK|ILI9341_D0_CLK|ILI9341_D1_CLK|
					ILI9341_D2_CLK|ILI9341_D3_CLK|ILI9341_D4_CLK|ILI9341_D5_CLK|ILI9341_D6_CLK|
					ILI9341_D7_CLK|ILI9341_D8_CLK|ILI9341_D9_CLK|ILI9341_D10_CLK|ILI9341_D11_CLK|
					ILI9341_D12_CLK|ILI9341_D13_CLK|ILI9341_D14_CLK|ILI9341_D15_CLK, ENABLE);
	
	// 将GPIO配置为推挽模式
	GPIO_InitStructure.GPIO_Pin = ILI9341_BK_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(ILI9341_BK_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_RST_PIN;//模式和速度一样，可以删掉
	GPIO_Init(ILI9341_RST_PORT, &GPIO_InitStructure);
	
	// 将GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = ILI9341_CS_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(ILI9341_CS_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_RD_PIN;
	GPIO_Init(ILI9341_RD_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_WR_PIN;
	GPIO_Init(ILI9341_WR_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_DC_PIN;
	GPIO_Init(ILI9341_DC_PORT, &GPIO_InitStructure);

		/* 配置FSMC相对应的数据线,FSMC-D0~D15 */	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_InitStructure.GPIO_Mode =  GPIO_Mode_AF_PP;
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D0_PIN;
	GPIO_Init ( ILI9341_D0_PORT, & GPIO_InitStructure );

	GPIO_InitStructure.GPIO_Pin = ILI9341_D1_PIN;
	GPIO_Init ( ILI9341_D1_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D2_PIN;
	GPIO_Init ( ILI9341_D2_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D3_PIN;
	GPIO_Init ( ILI9341_D3_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D4_PIN;
	GPIO_Init ( ILI9341_D4_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D5_PIN;
	GPIO_Init ( ILI9341_D5_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D6_PIN;
	GPIO_Init ( ILI9341_D6_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D7_PIN;
	GPIO_Init ( ILI9341_D7_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D8_PIN;
	GPIO_Init ( ILI9341_D8_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D9_PIN;
	GPIO_Init ( ILI9341_D9_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D10_PIN;
	GPIO_Init ( ILI9341_D10_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D11_PIN;
	GPIO_Init ( ILI9341_D11_PORT, & GPIO_InitStructure );

	GPIO_InitStructure.GPIO_Pin = ILI9341_D12_PIN;
	GPIO_Init ( ILI9341_D12_PORT, & GPIO_InitStructure );	
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D13_PIN;
	GPIO_Init ( ILI9341_D13_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D14_PIN;
	GPIO_Init ( ILI9341_D14_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D15_PIN;
	GPIO_Init ( ILI9341_D15_PORT, & GPIO_InitStructure );

	
}
```


初始化 FSMC 模拟 8080 时序的端口
（初始化结构体、读写时序的结构体。）
主要修改模式选择 B、 Bank 区域以及时间（ADDSET、DATAST、HCLK）
```
void ILI9341_FSMC_Config(void)
{
	FSMC_NORSRAMInitTypeDef  FSMC_NORSRAMInitStructure;//初始化结构体
	FSMC_NORSRAMTimingInitTypeDef  readWriteTiming;//读写时序的结构体
	//主要要还改模式选择为B、FSMC选择的Bank
		
	/*使能FSMC外设时钟*/
	RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC,ENABLE);

	//地址建立时间（ADDSET）为1个HCLK 2/72M=28ns
	readWriteTiming.FSMC_AddressSetupTime = 0x01;	
	//数据保持时间（DATAST）+ 1个HCLK 5/72M=70ns
	readWriteTiming.FSMC_DataSetupTime = 0x04;		 

	
	//地址保持时间（ADDHLD）模式B未用到
	readWriteTiming.FSMC_AddressHoldTime = 0x00;	 
	
	//设置总线转换周期，仅用于复用模式的NOR操作
	readWriteTiming.FSMC_BusTurnAroundDuration = 0x00;
	
	//设置时钟分频，仅用于同步类型的存储器
	readWriteTiming.FSMC_CLKDivision = 0x00;	

	//数据保持时间，仅用于NOR
	readWriteTiming.FSMC_DataLatency = 0x00;		
	
	//选择匹配SRAM的模式
	readWriteTiming.FSMC_AccessMode = FSMC_AccessMode_B;	 
    

	// 选择FSMC映射的存储区域： Bank1 NORSRAMx
	FSMC_NORSRAMInitStructure.FSMC_Bank = FSMC_BANK_NORSRAMx; 
	
	//设置地址总线与数据总线是否复用，仅用于NOR
	FSMC_NORSRAMInitStructure.FSMC_DataAddressMux = FSMC_DataAddressMux_Disable; 
	
	//设置要控制的存储器类型：NOR类型
	FSMC_NORSRAMInitStructure.FSMC_MemoryType =FSMC_MemoryType_NOR;   
	
	//存储器数据宽度：16位
	FSMC_NORSRAMInitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_16b; 
	
	//设置是否使用突发访问模式，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_BurstAccessMode =FSMC_BurstAccessMode_Disable;
	
	//设置是否使能等待信号，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_AsynchronousWait=FSMC_AsynchronousWait_Disable;
	
	//设置等待信号的有效极性，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WaitSignalPolarity = FSMC_WaitSignalPolarity_Low;
	
	//设置是否支持把非对齐的突发操作，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WrapMode = FSMC_WrapMode_Disable; 
	
	//设置等待信号插入的时间，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WaitSignalActive = FSMC_WaitSignalActive_BeforeWaitState;
	
	//存储器写使能 
	FSMC_NORSRAMInitStructure.FSMC_WriteOperation = FSMC_WriteOperation_Enable;
	
	//不使用等待信号
	FSMC_NORSRAMInitStructure.FSMC_WaitSignal = FSMC_WaitSignal_Disable;  		
	
	// 不使用扩展模式，读写使用相同的时序
	FSMC_NORSRAMInitStructure.FSMC_ExtendedMode = FSMC_ExtendedMode_Disable; 
	
	//突发写操作
	FSMC_NORSRAMInitStructure.FSMC_WriteBurst = FSMC_WriteBurst_Disable;  
	
	//读写时序配置
	FSMC_NORSRAMInitStructure.FSMC_ReadWriteTimingStruct = &readWriteTiming;
	
	//读写同样时序，使用扩展模式时这个配置才有效
	FSMC_NORSRAMInitStructure.FSMC_WriteTimingStruct = &readWriteTiming; 

	FSMC_NORSRAMInit(&FSMC_NORSRAMInitStructure);  //初始化FSMC配置

	FSMC_NORSRAMCmd(FSMC_BANK_NORSRAMx, ENABLE);  // 使能BANK										  
	
}

```


#### 代码
```bsp_ili9341_lcd.c
 /**
  ******************************************************************************
  * @file    bsp_xxx.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   LCD 驱动
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */
  
#include "./lcd/bsp_ili9341_lcd.h"

void ILI9341_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;

	// 打开GPIO的时钟 //或语句，把这些端口连接起来
	RCC_APB2PeriphClockCmd(ILI9341_CS_CLK|ILI9341_RST_CLK|ILI9341_BK_CLK|
					ILI9341_RD_CLK|ILI9341_WR_CLK|ILI9341_DC_CLK|ILI9341_D0_CLK|ILI9341_D1_CLK|
					ILI9341_D2_CLK|ILI9341_D3_CLK|ILI9341_D4_CLK|ILI9341_D5_CLK|ILI9341_D6_CLK|
					ILI9341_D7_CLK|ILI9341_D8_CLK|ILI9341_D9_CLK|ILI9341_D10_CLK|ILI9341_D11_CLK|
					ILI9341_D12_CLK|ILI9341_D13_CLK|ILI9341_D14_CLK|ILI9341_D15_CLK, ENABLE);
	
	// 将GPIO配置为推挽模式
	GPIO_InitStructure.GPIO_Pin = ILI9341_BK_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(ILI9341_BK_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_RST_PIN;//模式和速度一样，可以删掉
	GPIO_Init(ILI9341_RST_PORT, &GPIO_InitStructure);
	
	// 将GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = ILI9341_CS_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(ILI9341_CS_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_RD_PIN;
	GPIO_Init(ILI9341_RD_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_WR_PIN;
	GPIO_Init(ILI9341_WR_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_DC_PIN;
	GPIO_Init(ILI9341_DC_PORT, &GPIO_InitStructure);

		/* 配置FSMC相对应的数据线,FSMC-D0~D15 */	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_InitStructure.GPIO_Mode =  GPIO_Mode_AF_PP;
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D0_PIN;
	GPIO_Init ( ILI9341_D0_PORT, & GPIO_InitStructure );

	GPIO_InitStructure.GPIO_Pin = ILI9341_D1_PIN;
	GPIO_Init ( ILI9341_D1_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D2_PIN;
	GPIO_Init ( ILI9341_D2_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D3_PIN;
	GPIO_Init ( ILI9341_D3_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D4_PIN;
	GPIO_Init ( ILI9341_D4_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D5_PIN;
	GPIO_Init ( ILI9341_D5_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D6_PIN;
	GPIO_Init ( ILI9341_D6_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D7_PIN;
	GPIO_Init ( ILI9341_D7_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D8_PIN;
	GPIO_Init ( ILI9341_D8_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D9_PIN;
	GPIO_Init ( ILI9341_D9_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D10_PIN;
	GPIO_Init ( ILI9341_D10_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D11_PIN;
	GPIO_Init ( ILI9341_D11_PORT, & GPIO_InitStructure );

	GPIO_InitStructure.GPIO_Pin = ILI9341_D12_PIN;
	GPIO_Init ( ILI9341_D12_PORT, & GPIO_InitStructure );	
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D13_PIN;
	GPIO_Init ( ILI9341_D13_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D14_PIN;
	GPIO_Init ( ILI9341_D14_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D15_PIN;
	GPIO_Init ( ILI9341_D15_PORT, & GPIO_InitStructure );

	
}

void ILI9341_FSMC_Config(void)
{
	FSMC_NORSRAMInitTypeDef  FSMC_NORSRAMInitStructure;//初始化结构体
	FSMC_NORSRAMTimingInitTypeDef  readWriteTiming;//读写时序的结构体
	//主要要还改模式选择为B、FSMC选择的Bank
		
	/*使能FSMC外设时钟*/
	RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC,ENABLE);

	//地址建立时间（ADDSET）为1个HCLK 2/72M=28ns
	readWriteTiming.FSMC_AddressSetupTime = 0x01;	
	//数据保持时间（DATAST）+ 1个HCLK 5/72M=70ns
	readWriteTiming.FSMC_DataSetupTime = 0x04;		 

	
	//地址保持时间（ADDHLD）模式B未用到
	readWriteTiming.FSMC_AddressHoldTime = 0x00;	 
	
	//设置总线转换周期，仅用于复用模式的NOR操作
	readWriteTiming.FSMC_BusTurnAroundDuration = 0x00;
	
	//设置时钟分频，仅用于同步类型的存储器
	readWriteTiming.FSMC_CLKDivision = 0x00;	

	//数据保持时间，仅用于NOR
	readWriteTiming.FSMC_DataLatency = 0x00;		
	
	//选择匹配SRAM的模式
	readWriteTiming.FSMC_AccessMode = FSMC_AccessMode_B;	 
    

	// 选择FSMC映射的存储区域： Bank1 NORSRAMx
	FSMC_NORSRAMInitStructure.FSMC_Bank = FSMC_BANK_NORSRAMx; 
	
	//设置地址总线与数据总线是否复用，仅用于NOR
	FSMC_NORSRAMInitStructure.FSMC_DataAddressMux = FSMC_DataAddressMux_Disable; 
	
	//设置要控制的存储器类型：NOR类型
	FSMC_NORSRAMInitStructure.FSMC_MemoryType =FSMC_MemoryType_NOR;   
	
	//存储器数据宽度：16位
	FSMC_NORSRAMInitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_16b; 
	
	//设置是否使用突发访问模式，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_BurstAccessMode =FSMC_BurstAccessMode_Disable;
	
	//设置是否使能等待信号，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_AsynchronousWait=FSMC_AsynchronousWait_Disable;
	
	//设置等待信号的有效极性，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WaitSignalPolarity = FSMC_WaitSignalPolarity_Low;
	
	//设置是否支持把非对齐的突发操作，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WrapMode = FSMC_WrapMode_Disable; 
	
	//设置等待信号插入的时间，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WaitSignalActive = FSMC_WaitSignalActive_BeforeWaitState;
	
	//存储器写使能 
	FSMC_NORSRAMInitStructure.FSMC_WriteOperation = FSMC_WriteOperation_Enable;
	
	//不使用等待信号
	FSMC_NORSRAMInitStructure.FSMC_WaitSignal = FSMC_WaitSignal_Disable;  		
	
	// 不使用扩展模式，读写使用相同的时序
	FSMC_NORSRAMInitStructure.FSMC_ExtendedMode = FSMC_ExtendedMode_Disable; 
	
	//突发写操作
	FSMC_NORSRAMInitStructure.FSMC_WriteBurst = FSMC_WriteBurst_Disable;  
	
	//读写时序配置
	FSMC_NORSRAMInitStructure.FSMC_ReadWriteTimingStruct = &readWriteTiming;
	
	//读写同样时序，使用扩展模式时这个配置才有效
	FSMC_NORSRAMInitStructure.FSMC_WriteTimingStruct = &readWriteTiming; 

	FSMC_NORSRAMInit(&FSMC_NORSRAMInitStructure);  //初始化FSMC配置

	FSMC_NORSRAMCmd(FSMC_BANK_NORSRAMx, ENABLE);  // 使能BANK										  

	
	
	
}


/*********************************************END OF FILE**********************/

```


```bsp_ili9341_lcd.h
#ifndef __ILI9341_LCD_H
#define __ILI9341_LCD_H

#include "stm32f10x.h"
#include <stdio.h>

#define USE_ZNZ 1

/*LCD控制信号****************************/
#ifdef	USE_ZNZ

	#define			 FSMC_BANK_NORSRAMx				FSMC_Bank1_NORSRAM1
	
	#define      ILI9341_CS_CLK           RCC_APB2Periph_GPIOD	//初始化GPIO的时钟，所有GPIO时钟都是在APB2上的，所以都使用APB2即可
	#define      ILI9341_CS_PORT          GPIOD	//PORT的话就是引脚端口号
	#define      ILI9341_CS_PIN           GPIO_Pin_7

	#define      ILI9341_RST_CLK           RCC_APB2Periph_GPIOE	
	#define      ILI9341_RST_PORT          GPIOE	
	#define      ILI9341_RST_PIN           GPIO_Pin_1

	#define      ILI9341_BK_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_BK_PORT          GPIOD	
	#define      ILI9341_BK_PIN           GPIO_Pin_12

	#define      ILI9341_RD_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_RD_PORT          GPIOD	
	#define      ILI9341_RD_PIN           GPIO_Pin_4

	#define      ILI9341_WR_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_WR_PORT          GPIOD	
	#define      ILI9341_WR_PIN           GPIO_Pin_5

	#define      ILI9341_DC_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_DC_PORT          GPIOD	
	#define      ILI9341_DC_PIN           GPIO_Pin_11

#else

	#define			 FSMC_BANK_NORSRAMx				FSMC_Bank1_NORSRAM4

	#define      ILI9341_CS_CLK           RCC_APB2Periph_GPIOG	
	#define      ILI9341_CS_PORT          GPIOG	
	#define      ILI9341_CS_PIN           GPIO_Pin_12

	#define      ILI9341_RST_CLK           RCC_APB2Periph_GPIOG	
	#define      ILI9341_RST_PORT          GPIOG	
	#define      ILI9341_RST_PIN           GPIO_Pin_11

	#define      ILI9341_BK_CLK           RCC_APB2Periph_GPIOG	
	#define      ILI9341_BK_PORT          GPIOG	
	#define      ILI9341_BK_PIN           GPIO_Pin_6

	#define      ILI9341_RD_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_RD_PORT          GPIOD	
	#define      ILI9341_RD_PIN           GPIO_Pin_4

	#define      ILI9341_WR_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_WR_PORT          GPIOD	
	#define      ILI9341_WR_PIN           GPIO_Pin_5

	#define      ILI9341_DC_CLK           RCC_APB2Periph_GPIOE	//RS
	#define      ILI9341_DC_PORT          GPIOE	
	#define      ILI9341_DC_PIN           GPIO_Pin_2
	
#endif

//数据线(16根)
#define      ILI9341_D0_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D0_PORT               GPIOD
#define      ILI9341_D0_PIN                GPIO_Pin_14

#define      ILI9341_D1_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D1_PORT               GPIOD
#define      ILI9341_D1_PIN                GPIO_Pin_15

#define      ILI9341_D2_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D2_PORT               GPIOD
#define      ILI9341_D2_PIN                GPIO_Pin_0

#define      ILI9341_D3_CLK                RCC_APB2Periph_GPIOD  
#define      ILI9341_D3_PORT               GPIOD
#define      ILI9341_D3_PIN                GPIO_Pin_1

#define      ILI9341_D4_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D4_PORT               GPIOE
#define      ILI9341_D4_PIN                GPIO_Pin_7

#define      ILI9341_D5_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D5_PORT               GPIOE
#define      ILI9341_D5_PIN                GPIO_Pin_8

#define      ILI9341_D6_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D6_PORT               GPIOE
#define      ILI9341_D6_PIN                GPIO_Pin_9

#define      ILI9341_D7_CLK                RCC_APB2Periph_GPIOE  
#define      ILI9341_D7_PORT               GPIOE
#define      ILI9341_D7_PIN                GPIO_Pin_10

#define      ILI9341_D8_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D8_PORT               GPIOE
#define      ILI9341_D8_PIN                GPIO_Pin_11

#define      ILI9341_D9_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D9_PORT               GPIOE
#define      ILI9341_D9_PIN                GPIO_Pin_12

#define      ILI9341_D10_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D10_PORT               GPIOE
#define      ILI9341_D10_PIN                GPIO_Pin_13

#define      ILI9341_D11_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D11_PORT               GPIOE
#define      ILI9341_D11_PIN                GPIO_Pin_14

#define      ILI9341_D12_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D12_PORT               GPIOE
#define      ILI9341_D12_PIN                GPIO_Pin_15

#define      ILI9341_D13_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D13_PORT               GPIOD
#define      ILI9341_D13_PIN                GPIO_Pin_8

#define      ILI9341_D14_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D14_PORT               GPIOD
#define      ILI9341_D14_PIN                GPIO_Pin_9

#define      ILI9341_D15_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D15_PORT               GPIOD
#define      ILI9341_D15_PIN                GPIO_Pin_10


/*SPI接口定义-结尾****************************/

/*等待超时时间*/
#define SPIT_FLAG_TIMEOUT         ((uint32_t)0x1000)
#define SPIT_LONG_TIMEOUT         ((uint32_t)(10 * SPIT_FLAG_TIMEOUT))

/*信息输出*/
#define FLASH_DEBUG_ON         1

#define FLASH_INFO(fmt,arg...)           printf("<<-FLASH-INFO->> "fmt"\n",##arg)
#define FLASH_ERROR(fmt,arg...)          printf("<<-FLASH-ERROR->> "fmt"\n",##arg)
#define FLASH_DEBUG(fmt,arg...)          do{\
                                          if(FLASH_DEBUG_ON)\
                                          printf("<<-FLASH-DEBUG->> [%d]"fmt"\n",__LINE__, ##arg);\
                                          }while(0)

																					
#endif /* __ILI9341_LCD_H */


```



---

## P 80 液晶控制代码讲解2

初始化完 GPIO、FSMC 的模式，在这里就可以用 FSMC 以 NOR FLASH 的格式去对外访问外部的存储器，但是我们这个程序是用来控制液晶屏的。


```
#define  	   ILI9341_CMD_ADDR		 		  (__IO uint16_t *)(0x60000000)
#define 		 ILI9341_DATA_ADDR				(__IOuint16_t *)(0x60020000)

__IO

volatile uint 16_t temp;//正常使用 volatile 这个变量的方式。
volatile 定义之后就说明让编译器不要去优化这个变量，编译器会有优化操作。（优化了从内存复制到寄存器的操作，节省CPU取内存的一个访问操作）

比如 temp 变量，赋值为 10 的话，它可能会把 tem 变量加载到内核中的某些寄存器，CPU 做运算的时候会把这些变量从内存中搬运到寄存器中，这个过程相当于是复制。
如果没有对这个变量进行修改，比如 CPU 对这个变量复制到寄存器中后，我们没有语句去修改这个变量的值，比如说 temp++、temp+10 之类的操作。
CPU就会知道这个变量没有变化，编译器有汇编指令会告诉CPU说后面就不用再从内存里面（SRAM）中读取这个值。直接用寄存器中缓冲的这个值。

但是此处就会有问题，我们控制LCD，希望调用这个函数之后，FSMC产生一个时序，发送一个地址去访问那个地址，必须要去访问那个存储器的。而编译器觉得我们根本没有改过内存中的地址（只进行了读取操作,没有赋值之类的操作，编译器就进行了优化，直接从寄存器中读取这个值，实际上都不控制FSMC产生时序）导致不加 __IO 这个前缀，有可能FSMC根本就不操作（没有接收到CPU的指令）不去产生内存访问。
内核可能直接就从寄存器中获取到这个值了，所以要加 __IO 看是否正常

printf("\r\n0x0C命令返回值测试:0x%x",Read_Pixel_Format());
//返回结果0x0c(1100)不正常，第一位无论如何都应该是0
//修改背光BK和复位键RST启用后返回全是0，依然有问题。__IO的问题，是一个关键字。

加上 __IO 前缀，实际上就是加了volatile这个前缀
```


##### 代码
```bsp_ili9341_lcd.c
 /**
  ******************************************************************************
  * @file    bsp_xxx.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   LCD 驱动
  ******************************************************************************
  * @attention
  *
  * 实验平台:野火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :https://fire-stm32.taobao.com
  *
  ******************************************************************************
  */
  
#include "./lcd/bsp_ili9341_lcd.h"

void ILI9341_GPIO_Config(void)
{
	GPIO_InitTypeDef GPIO_InitStructure;

	// 打开GPIO的时钟 //或语句，把这些端口连接起来
	RCC_APB2PeriphClockCmd(ILI9341_CS_CLK|ILI9341_RST_CLK|ILI9341_BK_CLK|
					ILI9341_RD_CLK|ILI9341_WR_CLK|ILI9341_DC_CLK|ILI9341_D0_CLK|ILI9341_D1_CLK|
					ILI9341_D2_CLK|ILI9341_D3_CLK|ILI9341_D4_CLK|ILI9341_D5_CLK|ILI9341_D6_CLK|
					ILI9341_D7_CLK|ILI9341_D8_CLK|ILI9341_D9_CLK|ILI9341_D10_CLK|ILI9341_D11_CLK|
					ILI9341_D12_CLK|ILI9341_D13_CLK|ILI9341_D14_CLK|ILI9341_D15_CLK, ENABLE);
	
	// 将GPIO配置为推挽模式
	GPIO_InitStructure.GPIO_Pin = ILI9341_BK_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(ILI9341_BK_PORT, &GPIO_InitStructure);
	//只是初始化了背光和复位键的引脚，但是没有配置高低电平，都是默认值
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_RST_PIN;//模式和速度一样，可以删掉
	GPIO_Init(ILI9341_RST_PORT, &GPIO_InitStructure);
	
	// 将GPIO配置为推挽复用模式
	GPIO_InitStructure.GPIO_Pin = ILI9341_CS_PIN;
	GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_Init(ILI9341_CS_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_RD_PIN;
	GPIO_Init(ILI9341_RD_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_WR_PIN;
	GPIO_Init(ILI9341_WR_PORT, &GPIO_InitStructure);

	GPIO_InitStructure.GPIO_Pin = ILI9341_DC_PIN;
	GPIO_Init(ILI9341_DC_PORT, &GPIO_InitStructure);

		/* 配置FSMC相对应的数据线,FSMC-D0~D15 */	
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
	GPIO_InitStructure.GPIO_Mode =  GPIO_Mode_AF_PP;
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D0_PIN;
	GPIO_Init ( ILI9341_D0_PORT, & GPIO_InitStructure );

	GPIO_InitStructure.GPIO_Pin = ILI9341_D1_PIN;
	GPIO_Init ( ILI9341_D1_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D2_PIN;
	GPIO_Init ( ILI9341_D2_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D3_PIN;
	GPIO_Init ( ILI9341_D3_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D4_PIN;
	GPIO_Init ( ILI9341_D4_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D5_PIN;
	GPIO_Init ( ILI9341_D5_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D6_PIN;
	GPIO_Init ( ILI9341_D6_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D7_PIN;
	GPIO_Init ( ILI9341_D7_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D8_PIN;
	GPIO_Init ( ILI9341_D8_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D9_PIN;
	GPIO_Init ( ILI9341_D9_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D10_PIN;
	GPIO_Init ( ILI9341_D10_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D11_PIN;
	GPIO_Init ( ILI9341_D11_PORT, & GPIO_InitStructure );

	GPIO_InitStructure.GPIO_Pin = ILI9341_D12_PIN;
	GPIO_Init ( ILI9341_D12_PORT, & GPIO_InitStructure );	
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D13_PIN;
	GPIO_Init ( ILI9341_D13_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D14_PIN;
	GPIO_Init ( ILI9341_D14_PORT, & GPIO_InitStructure );
	
	GPIO_InitStructure.GPIO_Pin = ILI9341_D15_PIN;
	GPIO_Init ( ILI9341_D15_PORT, & GPIO_InitStructure );

	
}

void ILI9341_FSMC_Config(void)
{
	FSMC_NORSRAMInitTypeDef  FSMC_NORSRAMInitStructure;//初始化结构体
	FSMC_NORSRAMTimingInitTypeDef  readWriteTiming;//读写时序的结构体
	//主要要还改模式选择为B、FSMC选择的Bank
		
	/*使能FSMC外设时钟*/
	RCC_AHBPeriphClockCmd(RCC_AHBPeriph_FSMC,ENABLE);

	//地址建立时间（ADDSET）为1个HCLK 2/72M=28ns
	readWriteTiming.FSMC_AddressSetupTime = 0x01;	
	//数据保持时间（DATAST）+ 1个HCLK 5/72M=70ns
	readWriteTiming.FSMC_DataSetupTime = 0x04;		 

	
	//地址保持时间（ADDHLD）模式B未用到
	readWriteTiming.FSMC_AddressHoldTime = 0x00;	 
	
	//设置总线转换周期，仅用于复用模式的NOR操作
	readWriteTiming.FSMC_BusTurnAroundDuration = 0x00;
	
	//设置时钟分频，仅用于同步类型的存储器
	readWriteTiming.FSMC_CLKDivision = 0x00;	

	//数据保持时间，仅用于NOR
	readWriteTiming.FSMC_DataLatency = 0x00;		
	
	//选择匹配SRAM的模式
	readWriteTiming.FSMC_AccessMode = FSMC_AccessMode_B;	 
    

	// 选择FSMC映射的存储区域： Bank1 NORSRAMx
	FSMC_NORSRAMInitStructure.FSMC_Bank = FSMC_BANK_NORSRAMx; 
	
	//设置地址总线与数据总线是否复用，仅用于NOR
	FSMC_NORSRAMInitStructure.FSMC_DataAddressMux = FSMC_DataAddressMux_Disable; 
	
	//设置要控制的存储器类型：NOR类型
	FSMC_NORSRAMInitStructure.FSMC_MemoryType =FSMC_MemoryType_NOR;   
	
	//存储器数据宽度：16位
	FSMC_NORSRAMInitStructure.FSMC_MemoryDataWidth = FSMC_MemoryDataWidth_16b; 
	
	//设置是否使用突发访问模式，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_BurstAccessMode =FSMC_BurstAccessMode_Disable;
	
	//设置是否使能等待信号，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_AsynchronousWait=FSMC_AsynchronousWait_Disable;
	
	//设置等待信号的有效极性，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WaitSignalPolarity = FSMC_WaitSignalPolarity_Low;
	
	//设置是否支持把非对齐的突发操作，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WrapMode = FSMC_WrapMode_Disable; 
	
	//设置等待信号插入的时间，仅用于同步类型的存储器
	FSMC_NORSRAMInitStructure.FSMC_WaitSignalActive = FSMC_WaitSignalActive_BeforeWaitState;
	
	//存储器写使能 
	FSMC_NORSRAMInitStructure.FSMC_WriteOperation = FSMC_WriteOperation_Enable;
	
	//不使用等待信号
	FSMC_NORSRAMInitStructure.FSMC_WaitSignal = FSMC_WaitSignal_Disable;  		
	
	// 不使用扩展模式，读写使用相同的时序
	FSMC_NORSRAMInitStructure.FSMC_ExtendedMode = FSMC_ExtendedMode_Disable; 
	
	//突发写操作
	FSMC_NORSRAMInitStructure.FSMC_WriteBurst = FSMC_WriteBurst_Disable;  
	
	//读写时序配置
	FSMC_NORSRAMInitStructure.FSMC_ReadWriteTimingStruct = &readWriteTiming;
	
	//读写同样时序，使用扩展模式时这个配置才有效
	FSMC_NORSRAMInitStructure.FSMC_WriteTimingStruct = &readWriteTiming; 

	FSMC_NORSRAMInit(&FSMC_NORSRAMInitStructure);  //初始化FSMC配置

	FSMC_NORSRAMCmd(FSMC_BANK_NORSRAMx, ENABLE);  // 使能BANK										  

	
}

/**
 * @brief  ILI9341背光LED控制
 * @param  enumState ：决定是否使能背光LED
  *   该参数为以下值之一：
  *     @arg ENABLE :使能背光LED
  *     @arg DISABLE :禁用背光LED
 * @retval 无
 */
void ILI9341_BackLed_Control ( FunctionalState enumState )
{
	if ( enumState )
		GPIO_ResetBits ( ILI9341_BK_PORT, ILI9341_BK_PIN );	
	else
		GPIO_SetBits ( ILI9341_BK_PORT, ILI9341_BK_PIN );
		
}

/**
  * @brief  用于 ILI9341 简单延时函数
  * @param  nCount ：延时计数值
  * @retval 无
  */	
static void ILI9341_Delay ( __IO uint32_t nCount )
{
  for ( ; nCount != 0; nCount -- );
	
}

/**
 * @brief  ILI9341 软件复位
 * @param  无
 * @retval 无
 */
void ILI9341_Rst ( void )
{			
	GPIO_ResetBits ( ILI9341_RST_PORT, ILI9341_RST_PIN );	 //低电平复位

	ILI9341_Delay ( 0xAFF ); 					   

	GPIO_SetBits ( ILI9341_RST_PORT, ILI9341_RST_PIN );		 	 

	ILI9341_Delay ( 0xAFF ); 	
	
}

//将两个初始化封装起来
void ILI9341_Init(void)
{
	ILI9341_GPIO_Config();
	ILI9341_FSMC_Config();
	
	ILI9341_BackLed_Control(ENABLE);
	ILI9341_Rst();
}

//使用内联函数
/**
  * @brief  向ILI9341写入命令
  * @param  usCmd :要写入的命令（表寄存器地址）
  * @retval 无
  */	
__inline void ILI9341_Write_Cmd ( uint16_t usCmd )
{
	 *ILI9341_CMD_ADDR = usCmd;	
}


/**
  * @brief  向ILI9341写入数据
  * @param  usData :要写入的数据
  * @retval 无
  */	
__inline void ILI9341_Write_Data ( uint16_t usData )
{
	* ILI9341_DATA_ADDR = usData;
	
}


/**
  * @brief  从ILI9341读取数据
  * @param  无
  * @retval 读取到的数据
  */	
__inline uint16_t ILI9341_Read_Data ( void )
{
	return ( *ILI9341_DATA_ADDR );
	
}

uint16_t Read_Pixel_Format(void)
{
	ILI9341_Write_Cmd(0x0C);//发送0Ch命令，发送了什么函数就返回了什么
	ILI9341_Read_Data();//然后要读取数据，返回的第一个参数是无效的，但是也要读取
	
	return ILI9341_Read_Data();//读取第二个参数需要返回值（8个数据位有效的D0-D7）
	//返回值还是uint8_t改成uint16_t，与上面的函数保持一致，避免格式转化问题
	
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
  * @brief   LCD显示实验
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
#include "./lcd/bsp_ili9341_lcd.h"

extern uint16_t Read_Pixel_Format(void);
//volatile uint16_t temp;//正常使用volatile这个变量的方式。
//volatile定义之后就说明让编译器不要去优化这个变量，编译器会有优化操作。

//#define  	   ILI9341_CMD_ADDR		 		  (uint16_t *)(0x60000000)
//#define 		 ILI9341_DATA_ADDR				(uint16_t *)(0x60020000)

/*
 * 函数名：main
 * 描述  ：主函数
 * 输入  ：无
 * 输出  ：无
 * 提示  ：不要乱盖PC0跳帽！！
 */
int main(void)
{ 	
	//霸道的要将基地址改成0x6C000000，地址偏移量16改成23(引脚为A23)
	
//	uint32_t temp = 0x60000000;//SRAM,NE1引脚的基地址
//	uint16_t lcd_read_temp;
	
	ILI9341_Init();
	
	LED_GPIO_Config();
	LED_BLUE;
	
	/* 配置串口为：115200 8-N-1 */
	USART_Config();
	printf("\r\n 这是一个LCD实验 \r\n");
	
	printf("\r\n0x0C命令返回值测试:0x%x",Read_Pixel_Format());
	//1.返回结果0x0c(1100)不正常，第一位无论如何都应该是0
	//2.修改背光BK和复位键RST启用后返回全是0，依然有问题。__IO的问题，是一个关键字。
	//3.加了__IO （volatile）前缀后就恢复正常了(返回值0x06)。
	//即0110，代表18个数据位(一个像素点用18个数据位来表示)。现在表示的还是18位，是因为是默认值，后面还要写一些参数改成16位
	
//	//向LCD发送命令
//	*ILI9341_CMD_ADDR = 0xABCD;
//	//等价于 *(uint16_t *)(0x60000000) = 0xABCD;
//	
//	//向LCD发送数据
//	*ILI9341_DATA_ADDR = 0x1234;
//	//等价于 *(uint16_t *)(0x60020000) = 0x1234;
//	
//	//从液晶屏读取数据
//	lcd_read_temp = *ILI9341_DATA_ADDR;
//	lcd_read_temp = *(uint16_t *)(0x60020000);

//	temp = temp | (1<<(16+1));	//第A16根引脚致高电平，访问的是数据//地址算完之后要检查是否还处于SRAM1的地址范围

//	printf("\r\nA16为高电平，地址=0x%x",temp);

//	temp = 0x60000000;
//	
//	temp &= ~(1<<(16+1));//地址要左移一位，地址对齐问题
//	
//	printf("\r\nA16为低电平，地址=0x%x",temp);

	
	
	while(1);  
}


void Delay(__IO uint32_t nCount)
{
  for(; nCount != 0; nCount--);
}
/*********************************************END OF FILE**********************/

```

```bsp_ili9341_lcd.h
#ifndef __ILI9341_LCD_H
#define __ILI9341_LCD_H

#include "stm32f10x.h"
#include <stdio.h>

#define USE_ZNZ 1

/*LCD控制信号****************************/
#ifdef	USE_ZNZ

	#define			 FSMC_BANK_NORSRAMx				FSMC_Bank1_NORSRAM1
	
	#define  	   ILI9341_CMD_ADDR		 		  (__IO uint16_t *)(0x60000000)
	#define 		 ILI9341_DATA_ADDR				(__IO uint16_t *)(0x60020000)

	
	#define      ILI9341_CS_CLK           RCC_APB2Periph_GPIOD	//初始化GPIO的时钟，所有GPIO时钟都是在APB2上的，所以都使用APB2即可
	#define      ILI9341_CS_PORT          GPIOD	//PORT的话就是引脚端口号
	#define      ILI9341_CS_PIN           GPIO_Pin_7

	#define      ILI9341_RST_CLK           RCC_APB2Periph_GPIOE	
	#define      ILI9341_RST_PORT          GPIOE	
	#define      ILI9341_RST_PIN           GPIO_Pin_1

	#define      ILI9341_BK_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_BK_PORT          GPIOD	
	#define      ILI9341_BK_PIN           GPIO_Pin_12

	#define      ILI9341_RD_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_RD_PORT          GPIOD	
	#define      ILI9341_RD_PIN           GPIO_Pin_4

	#define      ILI9341_WR_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_WR_PORT          GPIOD	
	#define      ILI9341_WR_PIN           GPIO_Pin_5

	#define      ILI9341_DC_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_DC_PORT          GPIOD	
	#define      ILI9341_DC_PIN           GPIO_Pin_11
	

#else

	#define			 FSMC_BANK_NORSRAMx				FSMC_Bank1_NORSRAM4
	
	#define 		 ILI9341_CMD_ADDR					(__IO uint16_t *)(0x6c000000)
	#define 		 ILI9341_DATA_ADDR				(__IO uint16_t *)(0x6d000000)


	#define      ILI9341_CS_CLK           RCC_APB2Periph_GPIOG	
	#define      ILI9341_CS_PORT          GPIOG	
	#define      ILI9341_CS_PIN           GPIO_Pin_12

	#define      ILI9341_RST_CLK           RCC_APB2Periph_GPIOG	
	#define      ILI9341_RST_PORT          GPIOG	
	#define      ILI9341_RST_PIN           GPIO_Pin_11

	#define      ILI9341_BK_CLK           RCC_APB2Periph_GPIOG	
	#define      ILI9341_BK_PORT          GPIOG	
	#define      ILI9341_BK_PIN           GPIO_Pin_6

	#define      ILI9341_RD_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_RD_PORT          GPIOD	
	#define      ILI9341_RD_PIN           GPIO_Pin_4

	#define      ILI9341_WR_CLK           RCC_APB2Periph_GPIOD	
	#define      ILI9341_WR_PORT          GPIOD	
	#define      ILI9341_WR_PIN           GPIO_Pin_5

	#define      ILI9341_DC_CLK           RCC_APB2Periph_GPIOE	//RS
	#define      ILI9341_DC_PORT          GPIOE	
	#define      ILI9341_DC_PIN           GPIO_Pin_2
	
#endif

//数据线(16根)
#define      ILI9341_D0_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D0_PORT               GPIOD
#define      ILI9341_D0_PIN                GPIO_Pin_14

#define      ILI9341_D1_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D1_PORT               GPIOD
#define      ILI9341_D1_PIN                GPIO_Pin_15

#define      ILI9341_D2_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D2_PORT               GPIOD
#define      ILI9341_D2_PIN                GPIO_Pin_0

#define      ILI9341_D3_CLK                RCC_APB2Periph_GPIOD  
#define      ILI9341_D3_PORT               GPIOD
#define      ILI9341_D3_PIN                GPIO_Pin_1

#define      ILI9341_D4_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D4_PORT               GPIOE
#define      ILI9341_D4_PIN                GPIO_Pin_7

#define      ILI9341_D5_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D5_PORT               GPIOE
#define      ILI9341_D5_PIN                GPIO_Pin_8

#define      ILI9341_D6_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D6_PORT               GPIOE
#define      ILI9341_D6_PIN                GPIO_Pin_9

#define      ILI9341_D7_CLK                RCC_APB2Periph_GPIOE  
#define      ILI9341_D7_PORT               GPIOE
#define      ILI9341_D7_PIN                GPIO_Pin_10

#define      ILI9341_D8_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D8_PORT               GPIOE
#define      ILI9341_D8_PIN                GPIO_Pin_11

#define      ILI9341_D9_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D9_PORT               GPIOE
#define      ILI9341_D9_PIN                GPIO_Pin_12

#define      ILI9341_D10_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D10_PORT               GPIOE
#define      ILI9341_D10_PIN                GPIO_Pin_13

#define      ILI9341_D11_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D11_PORT               GPIOE
#define      ILI9341_D11_PIN                GPIO_Pin_14

#define      ILI9341_D12_CLK                RCC_APB2Periph_GPIOE   
#define      ILI9341_D12_PORT               GPIOE
#define      ILI9341_D12_PIN                GPIO_Pin_15

#define      ILI9341_D13_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D13_PORT               GPIOD
#define      ILI9341_D13_PIN                GPIO_Pin_8

#define      ILI9341_D14_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D14_PORT               GPIOD
#define      ILI9341_D14_PIN                GPIO_Pin_9

#define      ILI9341_D15_CLK                RCC_APB2Periph_GPIOD   
#define      ILI9341_D15_PORT               GPIOD
#define      ILI9341_D15_PIN                GPIO_Pin_10


/*SPI接口定义-结尾****************************/

/*等待超时时间*/
#define SPIT_FLAG_TIMEOUT         ((uint32_t)0x1000)
#define SPIT_LONG_TIMEOUT         ((uint32_t)(10 * SPIT_FLAG_TIMEOUT))

/*信息输出*/
#define FLASH_DEBUG_ON         1

#define FLASH_INFO(fmt,arg...)           printf("<<-FLASH-INFO->> "fmt"\n",##arg)
#define FLASH_ERROR(fmt,arg...)          printf("<<-FLASH-ERROR->> "fmt"\n",##arg)
#define FLASH_DEBUG(fmt,arg...)          do{\
                                          if(FLASH_DEBUG_ON)\
                                          printf("<<-FLASH-DEBUG->> [%d]"fmt"\n",__LINE__, ##arg);\
                                          }while(0)


																					
void ILI9341_Init(void);



																					
																					
#endif /* __ILI9341_LCD_H */


```





























