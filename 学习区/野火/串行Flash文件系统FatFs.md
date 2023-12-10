
把文件系统移植到 STM 32 上，将 flash 芯片格式化，使用 fat 文件系统。
不需要像前面用直接存储的方式存储数据，而是像电脑一样用文件的格式来存储数据。
## P 62 文件系统简介

#### 文件系统的概念
![[../../annex/串行Flash文件系统FatFs_image_1.png]]

![[../../annex/串行Flash文件系统FatFs_image_2.png]]
![[../../annex/串行Flash文件系统FatFs_image_3.png]]

##### 磁盘的物理结构
![[../../annex/串行Flash文件系统FatFs_image_4.png]]


![[../../annex/串行Flash文件系统FatFs_image_5.png]]

NTFS 文件系统没有那么多的碎片，支持单个文件超过 4 GB
以前的 FAT 格式的文件系统是不支持一个文件超过 4 GB 的

文件系统的好处：可以移植到不同的存储介质上（不需要管是什么底层硬件）

从 nand flash 复制一个文件到机械硬盘都是可以、兼容的。

![[../../annex/串行Flash文件系统FatFs_image_6.png]]


##### 磁盘分区表
![[../../annex/串行Flash文件系统FatFs_image_7.png]]

![[../../annex/串行Flash文件系统FatFs_image_8.png]]
逻辑地址->物理地址，然后进行存储


文件系统的意义：
1、包括存储介质上的管理系统
2、读取管理系统的代码

#### 文件系统是如何工作的？

##### 文件目录
![[../../annex/串行Flash文件系统FatFs_image_9.png|文件系统的空间示意图]]
簇，可以理解为是某个扇区。一个簇相当于等于多个扇区

| 文件分配表 |  目录   |   A.TXT    |    B.TXT    |    C.TXT    |             |
|:----------:|:-------:|:----------:|:-----------:|:-----------:|:-----------:|
|  第 0 簇   | 第 1 簇 | 第 2~11 簇 | 第 12~65 簇 | 第 66~86 簇 | 第 87~99 簇 |

![[../../annex/串行Flash文件系统FatFs_image_10.png|目录示意图]]

![[../../annex/串行Flash文件系统FatFs_image_11.png]]

![[../../annex/串行Flash文件系统FatFs_image_12.png]]
开始地址被隐藏了，因为我们不需要知道

文件目录是记录了文件存储的扇区位置（某个文件存储在哪个扇区）

##### 文件分配表
![[../../annex/串行Flash文件系统FatFs_image_13.png|文件分配表]]

可能有文件是不连续的（不是连着 2~11 都是文件 A 的）
文件分配表是记录了这个扇区存储了哪一些内容，这个内容的下一部分在哪里。
文件分配表登记了某个文件的开头，以及它每个扇区之后的一个连续空间应该从哪里开始。

##### 空间示意图
![[../../annex/串行Flash文件系统FatFs_image_14.png]]
删除 B.TXT 文件后，磁盘的该部分空间就空余出来了。文件系统就会管理：这个空白空间可以写入新的数据，就把 D.TXT 写到这部分的空白空间。一直写到 65 簇，可能会发现空间不够用，（12~65 簇写完了不够，而 66 簇已经写了 C.TXT 了，不能把 C.TXT 的空间覆盖掉）。所以，在 C.TXT （找到新的空白空间）后写了 D.TXT 文件剩余的内容。

文件目录只表示一些连续的信息。文件分配表就专门用来处理这些不连续的空间。
![[../../annex/串行Flash文件系统FatFs_image_15.png]]
通过文件分配表就实现了一个连接

以什么格式来解读数据
在 windows 上用后缀名来区分


#### B 站 AI 视频总结

在嵌入式系统中使用串行 FAT 文件系统进行数据记录的方法。视频首先介绍了文件系统的概念和常用的 FAT 文件系统，然后讲解了如何将 FAT 文件系统移植到 STM 32 中进行实验。视频还介绍了如何使用文件系统进行数据记录，并提到了在使用文件系统时可能遇到的问题，如难以记录有效数据的位置、难以确定存储介质的剩余空间和不明确应该用什么格式来解读数据

串行 fresh 文件系统、嵌入式中常用的 f at fs 文件系统以及代码移植到 STM32的实验
00:01 文件系统概念与移植
02:26 存储数据的过程与格式转换
04:55 存储数据的位置与读取

如何管理存储空间、有效数据位置和数据格式等问题可以借鉴文件系统的思路。
06:20 数据解码问题: 如何解决数据解码问题，如何记录有效数据的位置。
09:32 文件系统: 文件系统的作用和实现方式，如何有效地管理存储空间。
10:32 硬盘和存储空间: 硬盘的类型和存储空间的管理方式，不同类型的存储空间的使用方法。

磁盘的物理结构和文件系统的作用, 以及不同文件系统的种类和使用。
10:45 文件系统和存储数据的方式
14:26 磁盘上的数据存储和管理
15:57 文件系统的作用和原理

文件系统的作用和移植方法以及在不同存储介质上的应用实现文件操作和管理功能。
16:24 文件系统的作用和移植方法
17:16 文件系统的存储介质: 各种存储介质的支持。
20:53 常见的文件系统: 常见的文件系统和其功能

硬盘格式化和文件系统的基本知识包括文件分配表和目录表的作用及读写过程
21:22 文件系统格式化: 讲解了不同文件系统格式的使用，以及在磁盘中建立文件的过程。
24:03 文件读取过程: 详细介绍了文件读取的过程包括目录表和磁盘分区表的作用。
25:08 分区表: 介绍了分区表的作用，以及在编程中如何使用分区表来管理文件系统。

文件系统的工作过程, 包括存储和读取文件时的逻辑地址转换为物理地址的过程
26:00 文件系统的存储过程: 在存储数据或文件时会先建立一个文件索引，然后写入新文件时会在索引里记录文件的物理地址，存储到目录里边。
30:24 解读文件系统: 文件系统的一些代码也叫做文件系统，它实际上是解读文件格式和数据的时候用到的代码。

文件系统的目录和文件分配表, 以及如何将逻辑地址转成物理地址, 存储文件等内容。
34:17 文件系统的目录和文件分配表
30:46 文件名和文件存储位置的关系
35:41 文件管理和权限控制

文件系统的目录和文件分配表的作用, 以及如何通过它们来管理磁盘上的文件。
36:11 文件系统中的目录和文件分配表
39:16 文件分配表的作用和结构
40:45 b.txt 文件的存储和读取过程

文件分配表的作用, 以及在处理不连续空间时的方法, 以实现文件系统的管理
41:00 文件分配表的作用和目录管理
44:53 处理不连续空间的方法和硬盘驱动器的结构
45:30 文件分配表的作用和硬盘驱动器组织

文件系统中的目录表和文件分配表的作用以及如何通过它们来读取文件。
45:41 文件系统的基本概念和结构，包括文件分配表和目录表的作用。
49:03 文件系统在存储介质上面的信息结构，以及如何通过目录表和文件分配表来获取数据。
50:20 建立文件系统的概念，如何存储数据到存储介质，以及数据在存储介质上如何映射。

地址的概念包括物理地址和地址的了解。下一节将介绍代码。
50:47 物理地址和数据传输
50:54 结束语

## P 63 FatFs 文件系统简介

![[../../annex/串行Flash文件系统FatFs_image_16.png|C语言中的文件操作]]
![[../../annex/串行Flash文件系统FatFs_image_17.png]]

STM 32 不能用上述的 C 语言文件操作，因为我们没有实现 C 语言的这些文件操作的一些接口（ windows 和 linux 系统本身已支持，所以可以直接使用）
我们需要自己重新实现 C 语言的这些函数，然后把它重定向到我们的 flash 芯片上，才能够事项对 flash 等进行读写。Stm 32 无法一直 windows 和 linux，所以就需要移植文件系统。

![[../../annex/串行Flash文件系统FatFs_image_18.png|FATFS文件系统简介]]
FATFS 文件系统的源码属于读取文件管理系统信息的代码

FATFS 文件系统的一个作用：
我们要对不同介质（flash、EEPROM、SD 卡等）进行读写的时候，我们都可以把 FATFS 文件系统移植到上面。然后使用 FATFS 文件系统以文件的格式去读写 flash 等介质。

FATFS 文件系统的好处：
占用空间小，所以就不考虑像以前一样把 C 语言标准函数 print 重定向到串口。文件系统就不重定向这些了。

FATXX 主要区别是文件容量大小（单个文件大小的最大空间，FAT 每个文件最大支持 2 GB、FAT 32 最大应该是支持 4 GB）

移植 FATFS 到 STM 32 上，然后再把 Flash 进行格式化，格式化后就能像电脑使用磁盘一样，在存储介质上建立文件，然后通过以文件格式的形式对 flash 进行存储/读取数据。

注意：不是说直接就支持这些存储介质，我们需要向 FATFS 的代码提供一些关于底层接口的函数，像 printf 重定向一样，需要我们底层自己实现的。

##### FATFS 目录结构
![[../../annex/串行Flash文件系统FatFs_image_19.png|FatFs源码目录结构]]

FATFS 文件系统帮我们实现了 fopen、fclose 以及这些 C 语言标准的一些函数。
![[../../annex/串行Flash文件系统FatFs_image_20.png]]

![[../../annex/串行Flash文件系统FatFs_image_21.png]]
但是它要求我们要自己实现一些函数。
主要是为了方便我们，如果想要对 EEPROM 进行读写的话，我们要返回 EEPROM 的状态，文件系统的上层某一些函数可能需要知道底层介质状态，就要调用 disk_status 函数，disk_status 就需要我们自己的 EEPROM 去实现。
![[../../annex/串行Flash文件系统FatFs_image_22.png]]

![[../../annex/串行Flash文件系统FatFs_image_23.png]]
又比如 f_write 函数可能最底层就需要调用 disk_write 函数，而 disk_write 函数就需要我们去实现对存储介质进行存储数据。


![[../../annex/串行Flash文件系统FatFs_image_24.png]]

![[../../annex/串行Flash文件系统FatFs_image_25.png]]

###### Integer 文件
![[../../annex/串行Flash文件系统FatFs_image_26.png]]
打开 integer，内容是为了防止编译平台不一样（C 语言中，int 型 8051 芯片是 16 位的，在 stm 32 是 32 位的，通过这种格式定义就可以方便屏蔽编译器的差异）

###### Diskio .c 文件
![[../../annex/串行Flash文件系统FatFs_image_27.png]]
独立于底层介质的操作函数。移植 FATFS 文件系统的时候只需要改这个文件，然后给它提供各种接口。

###### ff .c 核心文件
![[../../annex/串行Flash文件系统FatFs_image_28.png]]
ff.c 实现了文件系统和底层的逻辑转换，而逻辑底层操作又封装成 diskio.c 等
这个文件实际上是把各种逻辑转换，转换之后得到了一个最底层的操作，就交给 diskio.c 文件里面的各种函数接口去操作。
![[../../annex/串行Flash文件系统FatFs_image_29.png]]
其实就属于 fatfs module（中间层），跟底层的操作就通过底层操作接口来对我们的存储介质进行操作

###### cc . 936 文件
![[../../annex/串行Flash文件系统FatFs_image_30.png]]
其实就是简体中文的编码页，通过 unicode 转 GB 2312 。Unicode 是国际通用的（包含全球字符的编码，转成像 ASCII 码一样的编码）编码。OEM 就是代码页。936 代表简体中文的代码页。
其实就是字符转换表。

![[../../annex/串行Flash文件系统FatFs_image_31.png]]

![[../../annex/串行Flash文件系统FatFs_image_32.png]]


#### B 站 AI 视频总结

文件系统的基本概念和代码表现形式，以 fat fs 文件系统为例，讲解了使用 c 语言的文件操作函数对存储介质里边存储的一些信息结构进行解读的方法。同时强调了这些操作是依赖于平台的，需要根据具体平台来实现。对于没有移植 Windows 和 Linux 的 STM 32 平台，需要自己重新实现 c 语言的文件操作函数并重定向到 fresh 芯片上

文件系统的基本概念和 fat 文件系统在存储介质上的表现, 以及文件系统代码的表现形式。
00:26 介绍了 fat 32 文件系统的代码表现形式，包括 fopen、fread、fwrite 等函数的使用。
01:43 打开文件: 讲解了如何使用 fopen 函数打开文件，并以读取模式打开文件。
05:06 操作文件: 介绍了如何使用 fread、fwrite 等函数对文件进行操作，并强调了在不同平台上函数的使用差异

如何移植文件系统并使用 f a t fs 文件系统实现对不同存储介质的读写操作。

06:27 实现读取文件系统: 介绍了如何通过编写代码实现对 Fresh 芯片或 SD 卡的读写操作。
08:40 Fatfs 文件系统: 详细介绍了 Fatfs 文件系统包括其作用、优点和使用方法。
11:38 读写操作: 提供了基本的读写操作函数，并说明了如何在 Fatfs 文件系统中使用。

Fatfs 文件系统该系统可以方便地对文件进行读写操作, 具有小巧易移植等特点。
12:18 文件系统操作: 介绍了如何使用 fatfs 文件系统进行文件的读写操作。
15:57 应用函数: 介绍了 fatfs 文件系统中的应用函数，包括 fopen、fread 等。
16:50 移植和使用: 介绍了如何移植和使用 fatfs 文件系统，并给出了相关代码。

文件系统层、底层设备控制层和存储介质之间的关系以及如何使用 API 实现文件操作
19:19 文件系统层: 把一个大问题分成小问题，实现文件系统的操作。
22:20 实现文件系统: 实现 fopen、fclose 等函数，要求自己实现，方便操作 rom。
23:47 读取数据: 使用 fread 函数，经过各种转换，最终调用到 diread 函数。

文件系统的读写操作和底层存储介质的访问, 以及相关的源代码和目录结构。
25:17 文件系统读写操作: 文件系统的读写操作，包括打开文件、读写数据、关闭文件等操作
25:51 文件系统代码介绍: 文件系统代码的目录结构和功能介绍，包括帮助文档和源代码。
29:28 文件作用和包含文件: 文件的作用和包含文件的概述，包括 integ. H、stm 32 f 10 x. H、stm 32 f 10 xdevice. H 等文件

如何使用一种格式定义来屏蔽编译器的差异, 以及如何实现底层驱动函数。
30:43 数据类型定义和驱动函数实现
34:14 存储介质初始化和数据读取
35:59 逻辑地址转换为物理地址

·ioct 和 fat time 等函数的作用以及 discio. H 和 f fc 文件的作用和实现方法。
36:31 文件系统相关函数和功能介绍
40:35 fatfs 文件系统及驱动开发
41:47 文件系统操作及注册文件观察器

文件系统中的 fatfs 文件系统的主体代码以及如何通过配置文件来裁剪代码。
42:15 文件系统的主体代码和配置文件
46:19 配置文件的作用和裁剪代码
47:54 移植文件系统的过程和注意事项

文件系统的各个文件和选项, 以及如何支持中文和英文介绍了代码页的概念和转换。
48:01 文件系统介绍: 文件系统的核心文件和选项文件，以及如何支持中文和英文
48:37 字符编码: Unicode 和 OEM 代码页的概念，以及如何使用 CC 936 和 932 文件支持中文和日文字符。
52:30 移植步骤: 如何阅读和理解源代码，以及移植的步骤和注意事项。

Fatfs 文件系统的分层结构和主要文件的作用, 以及如何进行移植和应用。
53:14 文件系统实现和使用: 推荐使用业界反映较好的免费文件系统。
57:21 移植文件系统可以方便编程和理解文件系统的来龙去脉。
57:54 总结和实践建议: 总结文件系统的作用，并提醒实践中要多思考和尝试。



## P 64 代码讲解-FatFs 文件系统移植


![[../../annex/串行Flash文件系统FatFs_image_33.png]]

![[../../annex/串行Flash文件系统FatFs_image_34.png]]
主要是要实现 diskio.c 文件的这几个函数

把这些文件都添加到工程目录中去
![[../../annex/串行Flash文件系统FatFs_image_35.png]]
Manage project items
![[../../annex/串行Flash文件系统FatFs_image_36.png]]
添加文件
![[../../annex/串行Flash文件系统FatFs_image_37.png]]
记得添加头文件路径
![[../../annex/串行Flash文件系统FatFs_image_38.png]]

##### Disk_Ioctl 函数
补充
![[../../annex/串行Flash文件系统FatFs_image_39.png]]
Ioctl 函数，没有实现这个函数也是能用的。
但是可能会出现一些问题：比如要格式化 flash 是没法用电脑进行格式化的。
以前板子只对 SD 卡实现文件系统的时候，是没有移植 ioctl 函数的，因为 SD 卡都是直接使用 USB 读卡器在电脑格式化后，再插入到板子上。所以以前不需要实现 ioctl 函数以实现格式化。
![[../../annex/串行Flash文件系统FatFs_image_40.png]]
但是 flash 是固定在板子上的，我们没法使用什么设备对 flash 在电脑上进行格式化。移植文件系统的时候必须要实现格式化，因为对 flash 进行格式化后，里面会存在一些信息结构，才能对 flash 以文件格式进行读写。

Cmd 命令号给底层，告诉我比如 flash 的容量多大。就向 disk_ioctl 发送一个命令请求，请求文件大小。又比如想知道每个扇区多大 512 还是 4096。
所有的请求都通过 cmd 这个参数传给我们的底层，我们底层要根据 switch case 来对不同的命令实现不同的返回值。

Buff，既可能是输入也可能是输出。不能通过 DRESULT 这个返回值返回 flash 容量大小等，因为这个返回值可能是不够大的，因为这个返回值只是一个普通的 result。Buff 返回值是通过一个指针，上层通过这个指针获取到我们告诉它的一些参数，比如把 4096 传进来。

具体有什么命令？
![[../../annex/串行Flash文件系统FatFs_image_41.png]]
![[../../annex/串行Flash文件系统FatFs_image_42.png]]
要实习什么命令主要看右图 PPT 即可
![[../../annex/串行Flash文件系统FatFs_image_43.png]]

CTRL_SYNC 用来确认数据是写入到文件系统中。这个是同步功能。
也就是说，文件系统很多时候其实会缓存一些数据。比如写入文件的时候，加了几个字符，实际上这几个字符本身是存储在内存上的。它不会立即把这些数据保存到我们的硬盘。
（放到内存，掉电数据丢失。而实际上，文件系统的数据是保存在硬盘上的）
当点击保存才会把数据从内存存储在硬盘中。
作用：节省时间。
文件系统每次都是操作一个扇区，如果只写入几个字符就进行存储数据，又要以扇区为单位写入一次。
而且存储器（磁盘）的工作是相对内存而言是很慢的，所以文件系统一般会先把数据缓存到内存中，当我们点了保存后才会把数据保存在硬盘中。
![[../../annex/串行Flash文件系统FatFs_image_44.png]]
而我们每次写入数据的时候都是直接往 flash 直接写入内容的，而不是写到缓存中的。所以这个命令也不需要实现。

![[../../annex/串行Flash文件系统FatFs_image_45.png]]
不是通过 DRSULT 返回，返回的是状态。而是通过 buff 指针
![[../../annex/串行Flash文件系统FatFs_image_46.png]]


![[../../annex/串行Flash文件系统FatFs_image_47.png]]
要求 DWORD 的第 25-31 位返回 1980 年到现在这个时间的时间差。
21-24 位返回月数...

##### 代码

```diskio.c
/*-----------------------------------------------------------------------*/
/* Low level disk I/O module skeleton for FatFs     (C)ChaN, 2014        */
/*-----------------------------------------------------------------------*/
/* If a working storage control module is available, it should be        */
/* attached to the FatFs via a glue function rather than modifying it.   */
/* This is an example of glue functions to attach various exsisting      */
/* storage control modules to the FatFs module with a defined API.       */
/*-----------------------------------------------------------------------*/

#include "diskio.h"		/* FatFs lower layer API */
#include "./flash/bsp_spi_flash.h"
//#include "usbdisk.h"	/* Example: Header file of existing USB MSD control module */
//#include "atadrive.h"	/* Example: Header file of existing ATA harddisk control module */
//#include "sdcard.h"		/* Example: Header file of existing MMC/SDC contorl module */

/* Definitions of physical drive number for each drive */
//#define ATA		0	/* Example: Map ATA harddisk to physical drive 0 */
//#define MMC		1	/* Example: Map MMC/SD card to physical drive 1 */
//#define USB		2	/* Example: Map USB MSD to physical drive 2 */

#define SD_CARD 	0
#define SPI_FLASH 1


/*-----------------------------------------------------------------------*/
/* Get Drive Status                                                      */
/*-----------------------------------------------------------------------*/

DSTATUS disk_status (
	BYTE pdrv		/* Physical drive nmuber to identify the drive */
)
{
	DSTATUS stat;
//	int result;//没用到

	switch (pdrv) {
	case SD_CARD :
//		result = ATA_disk_status();

		// translate the reslut code here

		return stat;

	case SPI_FLASH :
		
		if (SPI_FLASH_ReadID() == sFLASH_ID)
		{
			//状态正常
			stat = 0;//为什么这么写，不写return 0;
			//因为加一个写保护(0000 0100)检测，可以再加stat |=STA_PROTECT。
			//为什么加|=呢，因为可能不仅是不存在，可能进行了保护。
			//最后再来返回stat，上层的ff.c就可以检测到好几个标志
		}
		else
		{
			//状态不正常
			stat = STA_NOINIT;
		}
		
//		//写保护检测
//		
//		stat |= STA_PROTECT;//用|= 。注意：没有写检测protect的函数，此处只是简单的示范一下
		
		return stat;

	}
	return STA_NOINIT;
}



/*-----------------------------------------------------------------------*/
/* Inidialize a Drive                                                    */
/*-----------------------------------------------------------------------*/

DSTATUS disk_initialize (
	BYTE pdrv				/* Physical drive nmuber to identify the drive */
)
{
	DSTATUS stat;
//	int result;

	switch (pdrv) {
	case SD_CARD :
//		result = ATA_disk_initialize();

		// translate the reslut code here

		return stat;

	case SPI_FLASH :
		
		SPI_FLASH_Init();
//		result = MMC_disk_initialize();
	
	return disk_status(SPI_FLASH);
	
//	return stat;//不需要return stat了。因为	return disk_status(SPI_FLASH)中已经有了
	}
	return STA_NOINIT;
}



/*-----------------------------------------------------------------------*/
/* Read Sector(s)                                                        */
/*-----------------------------------------------------------------------*/

DRESULT disk_read (
	BYTE pdrv,		/* Physical drive nmuber to identify the drive */
	BYTE *buff,		/* Data buffer to store read data */
	DWORD sector,	/* Sector address in LBA */
	UINT count		/* Number of sectors to read */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :
		// translate the arguments here

//		result = ATA_disk_read(buff, sector, count);

		// translate the reslut code here

		return res;

	case SPI_FLASH ://SPI_FLASH -- pdrv
	
		SPI_FLASH_BufferRead(buff,sector*4096,count*4096);//void 没有返回值,//默认返回都是正常的
	//第二个参数要根据扇区号算出地址，*4096（每个扇区4096个字节）
	//为什么要输出扇区号。文件系统上层其实都是通过最小操作单位都是扇区来操作，而不是字节
		res = RES_OK;//默认返回都是正常的
	
		return res;
	}

	return RES_PARERR;
}



/*-----------------------------------------------------------------------*/
/* Write Sector(s)                                                       */
/*-----------------------------------------------------------------------*/

#if _USE_WRITE
DRESULT disk_write (
	BYTE pdrv,			/* Physical drive nmuber to identify the drive */
	const BYTE *buff,	/* Data to be written */
	DWORD sector,		/* Sector address in LBA */
	UINT count			/* Number of sectors to write */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :
		// translate the arguments here

//		result = ATA_disk_write(buff, sector, count);

		// translate the reslut code here

		return res;

	case SPI_FLASH :
		
		SPI_FLASH_BufferWrite((u8 *)buff,sector*4096,count*4096);
	//此处buff报错:指针类型不一致。要求输入的指针是u8，前面还是个const常量。需要把它转换过来，这里直接进行了强转
	
		res = RES_OK;//默认返回都是正常的//严谨来说应该加入读取SPI_FLASH的存储器状态/写保护状态来返回一些详细信息的
	
		return res;
	
	}

	return RES_PARERR;
}
#endif


/*-----------------------------------------------------------------------*/
/* Miscellaneous Functions                                               */
/*-----------------------------------------------------------------------*/

#if _USE_IOCTL
DRESULT disk_ioctl (
	BYTE pdrv,		/* Physical drive nmuber (0..) */
	BYTE cmd,		/* Control code */
	void *buff		/* Buffer to send/receive control data */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :

		// Process of the command for the ATA drive

		return res;

	case SPI_FLASH :

	switch(cmd)
	{
		//返回扇区个数
		case GET_SECTOR_COUNT:
					*(DWORD*)buff = 2048;//128*16=2048个扇区//报错error：空指针，空指针是没法进行操作的。所以我们需要给它一个类型
		break;
	
		//返回每个扇区的大小
		case GET_SECTOR_SIZE:
					*(WORD*)buff = 4096;
		break;
	
		//返回擦除扇区的最小个数(单位为扇区)。每次擦除4096个字节（一个扇区）
		case GET_BLOCK_SIZE:
					*(WORD*)buff = 1;//注意，不要写成了4096
		break;
	
	}
		// Process of the command for the MMC/SD card

		res = RES_OK;
		return res;
	}

	return RES_PARERR;
}
#endif

//返回时间//通过DWORD32个数据位返回一个时间
DWORD get_fattime(void)
{
	return 0;
}







```

```diskio.h
/*-----------------------------------------------------------------------/
/  Low level disk interface modlue include file   (C)ChaN, 2014          /
/-----------------------------------------------------------------------*/

#ifndef _DISKIO_DEFINED
#define _DISKIO_DEFINED

#ifdef __cplusplus
extern "C" {
#endif

#define _USE_WRITE	1	/* 1: Enable disk_write function */
#define _USE_IOCTL	1	/* 1: Enable disk_ioctl fucntion */

#include "integer.h"


/* Status of Disk Functions */
typedef BYTE	DSTATUS;

/* Results of Disk Functions */
typedef enum {
	RES_OK = 0,		/* 0: Successful */
	RES_ERROR,		/* 1: R/W Error */
	RES_WRPRT,		/* 2: Write Protected */
	RES_NOTRDY,		/* 3: Not Ready */
	RES_PARERR		/* 4: Invalid Parameter */
} DRESULT;


/*---------------------------------------*/
/* Prototypes for disk control functions */


DSTATUS disk_initialize (BYTE pdrv);
DSTATUS disk_status (BYTE pdrv);
DRESULT disk_read (BYTE pdrv, BYTE* buff, DWORD sector, UINT count);
DRESULT disk_write (BYTE pdrv, const BYTE* buff, DWORD sector, UINT count);
DRESULT disk_ioctl (BYTE pdrv, BYTE cmd, void* buff);


/* Disk Status Bits (DSTATUS) */

#define STA_NOINIT		0x01	/* Drive not initialized */
#define STA_NODISK		0x02	/* No medium in the drive */
#define STA_PROTECT		0x04	/* Write protected */


/* Command code for disk_ioctrl fucntion */

/* Generic command (Used by FatFs) */
#define CTRL_SYNC			0	/* Complete pending write process (needed at _FS_READONLY == 0) */
#define GET_SECTOR_COUNT	1	/* Get media size (needed at _USE_MKFS == 1) */
#define GET_SECTOR_SIZE		2	/* Get sector size (needed at _MAX_SS != _MIN_SS) */
#define GET_BLOCK_SIZE		3	/* Get erase block size (needed at _USE_MKFS == 1) */
#define CTRL_TRIM			4	/* Inform device that the data on the block of sectors is no longer used (needed at _USE_TRIM == 1) */

/* Generic command (Not used by FatFs) */
#define CTRL_POWER			5	/* Get/Set power status */
#define CTRL_LOCK			6	/* Lock/Unlock media removal */
#define CTRL_EJECT			7	/* Eject media */
#define CTRL_FORMAT			8	/* Create physical format on the media */

/* MMC/SDC specific ioctl command */
#define MMC_GET_TYPE		10	/* Get card type */
#define MMC_GET_CSD			11	/* Get CSD */
#define MMC_GET_CID			12	/* Get CID */
#define MMC_GET_OCR			13	/* Get OCR */
#define MMC_GET_SDSTAT		14	/* Get SD status */

/* ATA/CF specific ioctl command */
#define ATA_GET_REV			20	/* Get F/W revision */
#define ATA_GET_MODEL		21	/* Get model name */
#define ATA_GET_SN			22	/* Get serial number */

#ifdef __cplusplus
}
#endif

#endif

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
#include "./flash/bsp_spi_flash.h"


typedef enum { FAILED = 0, PASSED = !FAILED} TestStatus;

/* 获取缓冲区的长度 */
#define TxBufferSize1   (countof(TxBuffer1) - 1)
#define RxBufferSize1   (countof(TxBuffer1) - 1)
#define countof(a)      (sizeof(a) / sizeof(*(a)))
#define  BufferSize (countof(Tx_Buffer)-1)

#define  FLASH_WriteAddress     0x00000
#define  FLASH_ReadAddress      FLASH_WriteAddress
#define  FLASH_SectorToErase    FLASH_WriteAddress

     

/* 发送缓冲区初始化 */
uint8_t Tx_Buffer[] = "感谢您选用野火stm32开发板\r\n";
uint8_t Rx_Buffer[BufferSize];

__IO uint32_t DeviceID = 0;
__IO uint32_t FlashID = 0;
__IO TestStatus TransferStatus1 = FAILED;

// 函数原型声明
void Delay(__IO uint32_t nCount);
TestStatus Buffercmp(uint8_t* pBuffer1,uint8_t* pBuffer2, uint16_t BufferLength);

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
	printf("\r\n 使用指南者底板时 左上角排针位置 不要将PC0盖有跳帽 防止影响PC0做SPIFLASH片选脚 \r\n");

	/* 8M串行flash W25Q64初始化 */
	SPI_FLASH_Init();
	
	/* 获取 Flash Device ID */
	DeviceID = SPI_FLASH_ReadDeviceID();	
	Delay( 200 );
	
	/* 获取 SPI Flash ID */
	FlashID = SPI_FLASH_ReadID();	
	printf("\r\n FlashID is 0x%X,\
	Manufacturer Device ID is 0x%X\r\n", FlashID, DeviceID);
	
	/* 检验 SPI Flash ID */
	if (FlashID == sFLASH_ID)
	{	
		printf("\r\n 检测到串行flash W25Q64 !\r\n");
		
		/* 擦除将要写入的 SPI FLASH 扇区，FLASH写入前要先擦除 */
		// 这里擦除4K，即一个扇区，擦除的最小单位是扇区
		SPI_FLASH_SectorErase(FLASH_SectorToErase);	 	 
		
		/* 将发送缓冲区的数据写到flash中 */
		// 这里写一页，一页的大小为256个字节
		SPI_FLASH_BufferWrite(Tx_Buffer, FLASH_WriteAddress, BufferSize);		
		printf("\r\n 写入的数据为：%s \r\t", Tx_Buffer);
		
		/* 将刚刚写入的数据读出来放到接收缓冲区中 */
		SPI_FLASH_BufferRead(Rx_Buffer, FLASH_ReadAddress, BufferSize);
		printf("\r\n 读出的数据为：%s \r\n", Rx_Buffer);
		
		/* 检查写入的数据与读出的数据是否相等 */
		TransferStatus1 = Buffercmp(Tx_Buffer, Rx_Buffer, BufferSize);
		
		if( PASSED == TransferStatus1 )
		{ 
			LED_GREEN;
			printf("\r\n 8M串行flash(W25Q64)测试成功!\n\r");
		}
		else
		{        
			LED_RED;
			printf("\r\n 8M串行flash(W25Q64)测试失败!\n\r");
		}
	}// if (FlashID == sFLASH_ID)
	else// if (FlashID == sFLASH_ID)
	{ 
		LED_RED;
		printf("\r\n 获取不到 W25Q64 ID!\n\r");
	}
	
	while(1);  
}

/*
 * 函数名：Buffercmp
 * 描述  ：比较两个缓冲区中的数据是否相等
 * 输入  ：-pBuffer1     src缓冲区指针
 *         -pBuffer2     dst缓冲区指针
 *         -BufferLength 缓冲区长度
 * 输出  ：无
 * 返回  ：-PASSED pBuffer1 等于   pBuffer2
 *         -FAILED pBuffer1 不同于 pBuffer2
 */
TestStatus Buffercmp(uint8_t* pBuffer1, uint8_t* pBuffer2, uint16_t BufferLength)
{
  while(BufferLength--)
  {
    if(*pBuffer1 != *pBuffer2)
    {
      return FAILED;
    }

    pBuffer1++;
    pBuffer2++;
  }
  return PASSED;
}

void Delay(__IO uint32_t nCount)
{
  for(; nCount != 0; nCount--);
}
/*********************************************END OF FILE**********************/

```

```bsp_spi_flash.c
 /**
  ******************************************************************************
  * @file    bsp_xxx.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   spi flash 底层应用函数bsp 
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

static __IO uint32_t  SPITimeout = SPIT_LONG_TIMEOUT;    
static uint16_t SPI_TIMEOUT_UserCallback(uint8_t errorCode);

/**
  * @brief  SPI_FLASH初始化
  * @param  无
  * @retval 无
  */
void SPI_FLASH_Init(void)
{
  SPI_InitTypeDef  SPI_InitStructure;
  GPIO_InitTypeDef GPIO_InitStructure;
	
	/* 使能SPI时钟 */
	FLASH_SPI_APBxClock_FUN ( FLASH_SPI_CLK, ENABLE );
	
	/* 使能SPI引脚相关的时钟 */
 	FLASH_SPI_CS_APBxClock_FUN ( FLASH_SPI_CS_CLK|FLASH_SPI_SCK_CLK|
																	FLASH_SPI_MISO_PIN|FLASH_SPI_MOSI_PIN, ENABLE );
	
  /* 配置SPI的 CS引脚，普通IO即可 */
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_CS_PIN;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_Init(FLASH_SPI_CS_PORT, &GPIO_InitStructure);
	
  /* 配置SPI的 SCK引脚*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_SCK_PIN;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
  GPIO_Init(FLASH_SPI_SCK_PORT, &GPIO_InitStructure);

  /* 配置SPI的 MISO引脚*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_MISO_PIN;
  GPIO_Init(FLASH_SPI_MISO_PORT, &GPIO_InitStructure);

  /* 配置SPI的 MOSI引脚*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_MOSI_PIN;
  GPIO_Init(FLASH_SPI_MOSI_PORT, &GPIO_InitStructure);

  /* 停止信号 FLASH: CS引脚高电平*/
  SPI_FLASH_CS_HIGH();

  /* SPI 模式配置 */
  // FLASH芯片 支持SPI模式0及模式3，据此设置CPOL CPHA
  SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;
  SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
  SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
  SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;
  SPI_InitStructure.SPI_CPHA = SPI_CPHA_2Edge;
  SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;
  SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_4;
  SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;
  SPI_InitStructure.SPI_CRCPolynomial = 7;
  SPI_Init(FLASH_SPIx , &SPI_InitStructure);

  /* 使能 SPI  */
  SPI_Cmd(FLASH_SPIx , ENABLE);
	
}
 /**
  * @brief  擦除FLASH扇区
  * @param  SectorAddr：要擦除的扇区地址
  * @retval 无
  */
void SPI_FLASH_SectorErase(u32 SectorAddr)
{
  /* 发送FLASH写使能命令 */
  SPI_FLASH_WriteEnable();
  SPI_FLASH_WaitForWriteEnd();
  /* 擦除扇区 */
  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();
  /* 发送扇区擦除指令*/
  SPI_FLASH_SendByte(W25X_SectorErase);
  /*发送擦除扇区地址的高位*/
  SPI_FLASH_SendByte((SectorAddr & 0xFF0000) >> 16);
  /* 发送擦除扇区地址的中位 */
  SPI_FLASH_SendByte((SectorAddr & 0xFF00) >> 8);
  /* 发送擦除扇区地址的低位 */
  SPI_FLASH_SendByte(SectorAddr & 0xFF);
  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();
  /* 等待擦除完毕*/
  SPI_FLASH_WaitForWriteEnd();
}

 /**
  * @brief  擦除FLASH扇区，整片擦除
  * @param  无
  * @retval 无
  */
void SPI_FLASH_BulkErase(void)
{
  /* 发送FLASH写使能命令 */
  SPI_FLASH_WriteEnable();

  /* 整块 Erase */
  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();
  /* 发送整块擦除指令*/
  SPI_FLASH_SendByte(W25X_ChipErase);
  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();

  /* 等待擦除完毕*/
  SPI_FLASH_WaitForWriteEnd();
}

 /**
  * @brief  对FLASH按页写入数据，调用本函数写入数据前需要先擦除扇区
  * @param	pBuffer，要写入数据的指针
  * @param WriteAddr，写入地址
  * @param  NumByteToWrite，写入数据长度，必须小于等于SPI_FLASH_PerWritePageSize
  * @retval 无
  */
void SPI_FLASH_PageWrite(u8* pBuffer, u32 WriteAddr, u16 NumByteToWrite)
{
  /* 发送FLASH写使能命令 */
  SPI_FLASH_WriteEnable();

  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();
  /* 写页写指令*/
  SPI_FLASH_SendByte(W25X_PageProgram);
  /*发送写地址的高位*/
  SPI_FLASH_SendByte((WriteAddr & 0xFF0000) >> 16);
  /*发送写地址的中位*/
  SPI_FLASH_SendByte((WriteAddr & 0xFF00) >> 8);
  /*发送写地址的低位*/
  SPI_FLASH_SendByte(WriteAddr & 0xFF);

  if(NumByteToWrite > SPI_FLASH_PerWritePageSize)
  {
     NumByteToWrite = SPI_FLASH_PerWritePageSize;
     FLASH_ERROR("SPI_FLASH_PageWrite too large!"); 
  }

  /* 写入数据*/
  while (NumByteToWrite--)
  {
    /* 发送当前要写入的字节数据 */
    SPI_FLASH_SendByte(*pBuffer);
    /* 指向下一字节数据 */
    pBuffer++;
  }

  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();

  /* 等待写入完毕*/
  SPI_FLASH_WaitForWriteEnd();
}

 /**
  * @brief  对FLASH写入数据，调用本函数写入数据前需要先擦除扇区
  * @param	pBuffer，要写入数据的指针
  * @param  WriteAddr，写入地址
  * @param  NumByteToWrite，写入数据长度
  * @retval 无
  */
void SPI_FLASH_BufferWrite(u8* pBuffer, u32 WriteAddr, u16 NumByteToWrite)
{
  u8 NumOfPage = 0, NumOfSingle = 0, Addr = 0, count = 0, temp = 0;
	
	/*mod运算求余，若writeAddr是SPI_FLASH_PageSize整数倍，运算结果Addr值为0*/
  Addr = WriteAddr % SPI_FLASH_PageSize;
	
	/*差count个数据值，刚好可以对齐到页地址*/
  count = SPI_FLASH_PageSize - Addr;
	/*计算出要写多少整数页*/
  NumOfPage =  NumByteToWrite / SPI_FLASH_PageSize;
	/*mod运算求余，计算出剩余不满一页的字节数*/
  NumOfSingle = NumByteToWrite % SPI_FLASH_PageSize;
	
	/* Addr=0,则WriteAddr 刚好按页对齐 aligned  */
  if (Addr == 0)
  {
		/* NumByteToWrite < SPI_FLASH_PageSize */
    if (NumOfPage == 0) 
    {
      SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumByteToWrite);
    }
    else /* NumByteToWrite > SPI_FLASH_PageSize */
    { 
			/*先把整数页都写了*/
      while (NumOfPage--)
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, SPI_FLASH_PageSize);
        WriteAddr +=  SPI_FLASH_PageSize;
        pBuffer += SPI_FLASH_PageSize;
      }
			/*若有多余的不满一页的数据，把它写完*/
      SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumOfSingle);
    }
  }
	/* 若地址与 SPI_FLASH_PageSize 不对齐  */
  else 
  {
		/* NumByteToWrite < SPI_FLASH_PageSize */
    if (NumOfPage == 0)
    {
			/*当前页剩余的count个位置比NumOfSingle小，一页写不完*/
      if (NumOfSingle > count) 
      {
        temp = NumOfSingle - count;
				/*先写满当前页*/
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, count);
				
        WriteAddr +=  count;
        pBuffer += count;
				/*再写剩余的数据*/
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, temp);
      }
      else /*当前页剩余的count个位置能写完NumOfSingle个数据*/
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumByteToWrite);
      }
    }
    else /* NumByteToWrite > SPI_FLASH_PageSize */
    {
			/*地址不对齐多出的count分开处理，不加入这个运算*/
      NumByteToWrite -= count;
      NumOfPage =  NumByteToWrite / SPI_FLASH_PageSize;
      NumOfSingle = NumByteToWrite % SPI_FLASH_PageSize;
			
			/* 先写完count个数据，为的是让下一次要写的地址对齐 */
      SPI_FLASH_PageWrite(pBuffer, WriteAddr, count);
			
			/* 接下来就重复地址对齐的情况 */
      WriteAddr +=  count;
      pBuffer += count;
			/*把整数页都写了*/
      while (NumOfPage--)
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, SPI_FLASH_PageSize);
        WriteAddr +=  SPI_FLASH_PageSize;
        pBuffer += SPI_FLASH_PageSize;
      }
			/*若有多余的不满一页的数据，把它写完*/
      if (NumOfSingle != 0)
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumOfSingle);
      }
    }
  }
}

 /**
  * @brief  读取FLASH数据
  * @param 	pBuffer，存储读出数据的指针
  * @param   ReadAddr，读取地址
  * @param   NumByteToRead，读取数据长度
  * @retval 无
  */
void SPI_FLASH_BufferRead(u8* pBuffer, u32 ReadAddr, u16 NumByteToRead)
{
  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();

  /* 发送 读 指令 */
  SPI_FLASH_SendByte(W25X_ReadData);

  /* 发送 读 地址高位 */
  SPI_FLASH_SendByte((ReadAddr & 0xFF0000) >> 16);
  /* 发送 读 地址中位 */
  SPI_FLASH_SendByte((ReadAddr& 0xFF00) >> 8);
  /* 发送 读 地址低位 */
  SPI_FLASH_SendByte(ReadAddr & 0xFF);
	
	/* 读取数据 */
  while (NumByteToRead--) /* while there is data to be read */
  {
    /* 读取一个字节*/
    *pBuffer = SPI_FLASH_SendByte(Dummy_Byte);
    /* 指向下一个字节缓冲区 */
    pBuffer++;
  }

  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();
}

 /**
  * @brief  读取FLASH ID
  * @param 	无
  * @retval FLASH ID
  */
u32 SPI_FLASH_ReadID(void)
{
  u32 Temp = 0, Temp0 = 0, Temp1 = 0, Temp2 = 0;

  /* 开始通讯：CS低电平 */
  SPI_FLASH_CS_LOW();

  /* 发送JEDEC指令，读取ID */
  SPI_FLASH_SendByte(W25X_JedecDeviceID);

  /* 读取一个字节数据 */
  Temp0 = SPI_FLASH_SendByte(Dummy_Byte);

  /* 读取一个字节数据 */
  Temp1 = SPI_FLASH_SendByte(Dummy_Byte);

  /* 读取一个字节数据 */
  Temp2 = SPI_FLASH_SendByte(Dummy_Byte);

 /* 停止通讯：CS高电平 */
  SPI_FLASH_CS_HIGH();

  /*把数据组合起来，作为函数的返回值*/
	Temp = (Temp0 << 16) | (Temp1 << 8) | Temp2;

  return Temp;
}
 /**
  * @brief  读取FLASH Device ID
  * @param 	无
  * @retval FLASH Device ID
  */
u32 SPI_FLASH_ReadDeviceID(void)
{
  u32 Temp = 0;

  /* Select the FLASH: Chip Select low */
  SPI_FLASH_CS_LOW();

  /* Send "RDID " instruction */
  SPI_FLASH_SendByte(W25X_DeviceID);
  SPI_FLASH_SendByte(Dummy_Byte);
  SPI_FLASH_SendByte(Dummy_Byte);
  SPI_FLASH_SendByte(Dummy_Byte);
  
  /* Read a byte from the FLASH */
  Temp = SPI_FLASH_SendByte(Dummy_Byte);

  /* Deselect the FLASH: Chip Select high */
  SPI_FLASH_CS_HIGH();

  return Temp;
}
/*******************************************************************************
* Function Name  : SPI_FLASH_StartReadSequence
* Description    : Initiates a read data byte (READ) sequence from the Flash.
*                  This is done by driving the /CS line low to select the device,
*                  then the READ instruction is transmitted followed by 3 bytes
*                  address. This function exit and keep the /CS line low, so the
*                  Flash still being selected. With this technique the whole
*                  content of the Flash is read with a single READ instruction.
* Input          : - ReadAddr : FLASH's internal address to read from.
* Output         : None
* Return         : None
*******************************************************************************/
void SPI_FLASH_StartReadSequence(u32 ReadAddr)
{
  /* Select the FLASH: Chip Select low */
  SPI_FLASH_CS_LOW();

  /* Send "Read from Memory " instruction */
  SPI_FLASH_SendByte(W25X_ReadData);

  /* Send the 24-bit address of the address to read from -----------------------*/
  /* Send ReadAddr high nibble address byte */
  SPI_FLASH_SendByte((ReadAddr & 0xFF0000) >> 16);
  /* Send ReadAddr medium nibble address byte */
  SPI_FLASH_SendByte((ReadAddr& 0xFF00) >> 8);
  /* Send ReadAddr low nibble address byte */
  SPI_FLASH_SendByte(ReadAddr & 0xFF);
}


 /**
  * @brief  使用SPI读取一个字节的数据
  * @param  无
  * @retval 返回接收到的数据
  */
u8 SPI_FLASH_ReadByte(void)
{
  return (SPI_FLASH_SendByte(Dummy_Byte));
}

 /**
  * @brief  使用SPI发送一个字节的数据
  * @param  byte：要发送的数据
  * @retval 返回接收到的数据
  */
u8 SPI_FLASH_SendByte(u8 byte)
{
	 SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待发送缓冲区为空，TXE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_TXE) == RESET)
	{
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(0);
   }

  /* 写入数据寄存器，把要写入的数据写入发送缓冲区 */
  SPI_I2S_SendData(FLASH_SPIx , byte);

	SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待接收缓冲区非空，RXNE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_RXNE) == RESET)
  {
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(1);
   }

  /* 读取数据寄存器，获取接收缓冲区数据 */
  return SPI_I2S_ReceiveData(FLASH_SPIx );
}

 /**
  * @brief  使用SPI发送两个字节的数据
  * @param  byte：要发送的数据
  * @retval 返回接收到的数据
  */
u16 SPI_FLASH_SendHalfWord(u16 HalfWord)
{
	  SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待发送缓冲区为空，TXE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_TXE) == RESET)
	{
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(2);
   }
	
  /* 写入数据寄存器，把要写入的数据写入发送缓冲区 */
  SPI_I2S_SendData(FLASH_SPIx , HalfWord);

	 SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待接收缓冲区非空，RXNE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_RXNE) == RESET)
	 {
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(3);
   }
  /* 读取数据寄存器，获取接收缓冲区数据 */
  return SPI_I2S_ReceiveData(FLASH_SPIx );
}

 /**
  * @brief  向FLASH发送 写使能 命令
  * @param  none
  * @retval none
  */
void SPI_FLASH_WriteEnable(void)
{
  /* 通讯开始：CS低 */
  SPI_FLASH_CS_LOW();

  /* 发送写使能命令*/
  SPI_FLASH_SendByte(W25X_WriteEnable);

  /*通讯结束：CS高 */
  SPI_FLASH_CS_HIGH();
}

/* WIP(busy)标志，FLASH内部正在写入 */
#define WIP_Flag                  0x01

 /**
  * @brief  等待WIP(BUSY)标志被置0，即等待到FLASH内部数据写入完毕
  * @param  none
  * @retval none
  */
void SPI_FLASH_WaitForWriteEnd(void)
{
  u8 FLASH_Status = 0;

  /* 选择 FLASH: CS 低 */
  SPI_FLASH_CS_LOW();

  /* 发送 读状态寄存器 命令 */
  SPI_FLASH_SendByte(W25X_ReadStatusReg);

  /* 若FLASH忙碌，则等待 */
  do
  {
		/* 读取FLASH芯片的状态寄存器 */
    FLASH_Status = SPI_FLASH_SendByte(Dummy_Byte);	 
  }
  while ((FLASH_Status & WIP_Flag) == SET);  /* 正在写入标志 */

  /* 停止信号  FLASH: CS 高 */
  SPI_FLASH_CS_HIGH();
}


//进入掉电模式
void SPI_Flash_PowerDown(void)   
{ 
  /* 通讯开始：CS低 */
  SPI_FLASH_CS_LOW();

  /* 发送 掉电 命令 */
  SPI_FLASH_SendByte(W25X_PowerDown);

  /*通讯结束：CS高 */
  SPI_FLASH_CS_HIGH();
}   

//唤醒
void SPI_Flash_WAKEUP(void)   
{
  /*选择 FLASH: CS 低 */
  SPI_FLASH_CS_LOW();

  /* 发送 上电 命令 */
  SPI_FLASH_SendByte(W25X_ReleasePowerDown);

   /* 停止信号 FLASH: CS 高 */
  SPI_FLASH_CS_HIGH();
}   
   

/**
  * @brief  等待超时回调函数
  * @param  None.
  * @retval None.
  */
static  uint16_t SPI_TIMEOUT_UserCallback(uint8_t errorCode)
{
  /* 等待超时后的处理,输出错误信息 */
  FLASH_ERROR("SPI 等待超时!errorCode = %d",errorCode);
  return 0;
}
   
/*********************************************END OF FILE**********************/

```


#### B 站 AI 视频总结

如何将 f a t fs 文件系统的代码移植到 s t m32上，并利用这些代码对 fresh 进行读写和数据存储。通过对接 disk io.c 文件提供的函数接口，实现了对底层的操作，使文件系统的移植变得更加方便通用。同时，视频也详细介绍了如何添加文件系统中的文件和头文件，以及如何解决编译过程中出现的问题。最后，视频还提到了官方提供的范例程序，以及如何为其进行修改。



## P 65 代码讲解-FATFS 文件系统配置

![[../../annex/串行Flash文件系统FatFs_image_48.png]]
挂载。挂载文件系统。相当于在整个工程代码中运行文件系统的代码，就可以产生系统盘了。通过挂载初始化整个文件系统，它会读取我们存储介质上的一些信息，实现创建信息结构，方便后面调用 f_open 等函数。（这个函数就是使用文件系统之前必须要调用的，且需要挂载的）

##### F_mount 函数的参数
参数：
FATFS 类型是很大的，所以通常自定义在全局变量里面。
第二个是路径（存储器的路径）。路径就像是 C 盘、D 盘.... 。在 FATFS 文件系统里面是直接用数字来表示的。如果用 0 来表示的话就是 SD 卡，用 1 来表示的话就是 SPI_Flash
![[../../annex/串行Flash文件系统FatFs_image_49.png]]

定义了 FATFS* fs 后，其实是以指针形式输入到这个函数里面的。输入的时候，如果这个指针是非空的话（不是 null）f_mount 函数就会挂载文件系统。如果输入的是空指针，就会取消挂载文件系统。

![[../../annex/串行Flash文件系统FatFs_image_50.png]]
这个结构体是非常大的。因为有一个 win 数组，其实就相当于一个缓存。是非常大的，512（后面配置成 4096，更大了）
![[../../annex/串行Flash文件系统FatFs_image_51.png]]

我们知道，我们默认的启动文件配置，栈空间是 400（1024 个字节）
![[../../annex/串行Flash文件系统FatFs_image_52.png]]
如果把 FATFS 结构体定义到 main 函数内部的话，就变成栈空间。而栈空间是会溢出的。所以一般我们习惯直接定义为全局变量。
每个存储设备在挂载的时候都需要一个这样的信息结构这个结构体来存储文件系统的信息。
一个 flash 定义一个 FATFS 全局变量结构体。如果加了个 SD 卡，则需再定义一个。

![[../../annex/串行Flash文件系统FatFs_image_53.png]]
FRESULT。返回值结构体，用来查看 f_mount 的执行情况。

##### VOLUMES 改为 2
![[../../annex/串行Flash文件系统FatFs_image_54.png]]
默认情况下，使用存储设备 1 个。现在用了两个存储设备（SD 和 Flash）。所以就会找不到这个设备，所以会报错：FR_INVALID_DRIVE (11)，将 ffconf. H 文件中的 VOLUMES 的值改为 2 即可。
![[../../annex/串行Flash文件系统FatFs_image_55.png]]



##### 数据溢出
![[../../annex/串行Flash文件系统FatFs_image_56.png]]
卡死在 f_mount 函数之后
![[../../annex/串行Flash文件系统FatFs_image_57.png]]
这种问题大部分都是由于数据溢出或者 while 循环死掉了。（在 mount 函数中） while 循环死掉了，或者在 it. C 出现了 hardfault error。

```
用串口输出代替调试

小技巧：判断是不是 stm32f10x_it.C 文件的 void HardFault_Handler (void)函数部分出现问题。可以在该函数内部加 printf ("\r\n hardfault error");
并添加头文件 #include "./usart/bsp_usart.h"
就可以知道是不是这里出现问题。就不需要使用那个断点调试了。

经常有 hardfault error 首先要想到是不是内存溢出了。
```


![[../../annex/串行Flash文件系统FatFs_image_58.png]]
主要就是 MAX_SS != MIN_SS 时，我们需要配置 GET_SECTOR_SIZE ，需要配置 4096
![[../../annex/串行Flash文件系统FatFs_image_59.png]]
现在配置 4096，实际上 MAX_SS 相当于是作为 fsObject（FATFS 结构体）有一个数组，相当于作为缓冲区
![[../../annex/串行Flash文件系统FatFs_image_60.png]]

![[../../annex/串行Flash文件系统FatFs_image_61.png]]
现在要读取一个扇区为 4096 个字节的内容的话，如果这里只定义了 512 个字节，读取数据就很容易出错，数据就溢出了。（把 4096 个字节放到 512 字节的数组中溢出了）
所以我们可以通过把 ffconf. H 文件中的 MAX_SS 配置成跟我们的扇区大小一致，就不会出现这种报错情况（挂载失败）。
最小的 512，最大是 4096. 其实 512 个字节用在 SD 卡比较多，FLASH 中一般就配置 4096 个字节。

##### FR_NO_FILESYSTEM=13
![[../../annex/串行Flash文件系统FatFs_image_62.png]]
现在的问题不是卡死，res = 13
FR_NO_FILESYSTEM（13）。这个设备上不存在文件系统
在一个存储设备上，要是有文件系统的话，是会带有相应的文件系统里边的一些信息结构
现在代码查找 SPI FLASH 说没有文件系统。这是因为我们对 FLASH 还没有进行格式化。
格式化后在 flash 上存储一些信息结构，后面我们才能使用文件格式来创建。
所以现在的问题是要对它进行格式化。（f_mkfs 函数）

![[../../annex/串行Flash文件系统FatFs_image_63.png]]
调用 f_mkfs 函数就是格式化了，就会把存储设备上的所有内容都删掉。（只执行一次）

![[../../annex/串行Flash文件系统FatFs_image_64.png]]
报错：声明了，但是找不到定义。
因为这个文件系统为了裁剪大小，一般来说是默认不支持这个格式化的。
我们要使用格式化时，需要把宏设置成 1（ffconf. H 文件中_USE_MKFS 改为 1）
![[../../annex/串行Flash文件系统FatFs_image_65.png]]
![[../../annex/串行Flash文件系统FatFs_image_66.png]]

还是报错 FR_NO_FILESYSTEM=13
![[../../annex/串行Flash文件系统FatFs_image_67.png]]
测试情况
![[../../annex/串行Flash文件系统FatFs_image_68.png]]
说是格式化正常，但是再次复位的时候还依然告诉我们不存在这个文件系统。

```
排查：
查看 ffconf. H 文件中是否有关于文件系统的宏没有定义好
看看接口初始化有没有问题 
SPI_FLASH 有没有使能  SPI_FLASH_WriteEnable (); （写入的时候没有的话就写入不正常了）
读取是正常的，但是 flash 缺了擦除过程。写入的时候直接写入，但是 SPI_FLASH_BufferWrite 函数不包含擦除，所以现在其实是漏了一个擦除过程。
因此，如果调用 f_mkfs 函数，它要向我们的存储介质写入信息结构的时候，它自己以为写完成了，但实际上没有擦除 flash ，所以就会导致格式化一直不成功。
所以我们需要加一个 SPI_FLASH_SectorErase ，要确保它有一些 SPI_FLASH_WaitForWriteEnd
```

![[../../annex/串行Flash文件系统FatFs_image_69.png]]


格式化后要取消挂载再重新挂载文件系统
flash 已经存在了格式了（存在文件系统了），所以不执行这里
![[../../annex/串行Flash文件系统FatFs_image_70.png]]

```
flash已经存在了格式了（存在文件系统了），所以不执行这里。我们可以把文件系统的格式取消掉

如何取消：现在我们是在flash里面建立文件系统，但是我们重新执行一下程序，我们的例程会破坏掉我们的文件系统，因为它是直接写入的（没有带文件系统的程序）

重新打开未写文件系统的keil文件并编译）

直接烧进来会往第一个扇区擦除数据。如果擦除第一个扇区的数据，基本上这个文件系统就被破坏掉了。然后再来下载格式化（本文件），相当于重新执行一次格式化

```

重新格式化后：
![[../../annex/串行Flash文件系统FatFs_image_71.png]]
第一次挂载不成功（f_mount res=13）
然后执行 f_mkfs 格式化（f_mkfs res=0）
然后第二次挂载，成功了（second f_mount res=0）


也可以，如果自己想要重新格式化文件系统的话，把 f_mkfs 放到 if 语句外面，就可以每次运行的时候直接格式化了。但是要注意，每次格式化后，数据都会丢失。



![[../../annex/串行Flash文件系统FatFs_image_72.png]]

![[../../annex/串行Flash文件系统FatFs_image_73.png]]

![[../../annex/串行Flash文件系统FatFs_image_74.png]]
能够输出到此处说明写入正常，有判断语句，如果写入不正常是不会读取的。
![[../../annex/串行Flash文件系统FatFs_image_75.png]]
Printf 看看 bw 和 br，看有多少个字节被正常写入/读取。
写入是 21，读取是 0。说明写入是正常的，读取出现问题。
因为现在是没有关闭这个文件，相当于光标来到文件末，我们再想读取数据，读到文件的结尾就是读取到后面的数据（空白的）。
所以在读取之前要把光标重新放到文件的开头再去读。

![[../../annex/串行Flash文件系统FatFs_image_76.png]]
F_lseek 配置光标。
注意，可能需要在 ffconf. H 文件中使能该宏
![[../../annex/串行Flash文件系统FatFs_image_77.png]]
如果_FS_MINIMIZE 配置成 3，就不使能 lseek 函数

```main.c
 /**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   FATFS文件系统移植
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
#include "./flash/bsp_spi_flash.h"
#include "ff.h"			


FATFS fsobject;//占用空间大，定义为全局变量
FIL fp;//测试文件系统能不能读写数据，先尝试打开一个文件。首先要打开一个文件的话，先要创建一个文件句柄
//FIL也挺大的，定义为全局变量
const char wData[] = "欢迎使用秉火开发板!";//定义一个常量来知道字符串的大小（使用sizeof）
UINT bw;
char rData[4096] = "";
UINT br;


/*
 * 函数名：main
 * 描述  ：主函数
 * 输入  ：无
 * 输出  ：无
 * 提示  ：不要乱盖PC0跳帽！！
 */
int main(void)
{ 	
	FRESULT res;//FRESULT可以定义在内部
	
	LED_GPIO_Config();
	LED_BLUE;
	
	/* 配置串口为：115200 8-N-1 */
	USART_Config();
	printf("\r\n 这是一个FATFS文件系统移植实验 \r\n");

	//挂载文件系统--初始化
	f_mount(&fsobject,"1:",1);//第三个参数1表示立即挂载
	//0:表示第0个存储设备（diskio.c文件中#define SD_CARD 	0，0代表SD卡）。注意，不同版本的FATFS路径方式不同
//f_mount(NULL);//如果第一个成员为NULL，就是取消挂载。相当于释放文件系统

	//通过一个返回值FRESULT来确定f_mount函数的执行情况
	printf("\r\nf_mount res = %d",res);//正常返回 FR_OK（0）
	
	if(res == FR_NO_FILESYSTEM)
	{
		res = f_mkfs("1:",0,0);//第二个参数FDISK是0，SFD是1。第三个参数写0：会自动分配
		printf("\r\nf_mkfs res = %d",res);//正常返回 FR_OK（0）
		
		//格式化后要取消挂载再重新挂载文件系统
	//格式化后可能想要执行一些函数,但是在格式化后必须要重新挂载
		res = f_mount(NULL,"1:",1);//即取消挂载（NULL），再来重新挂载
		res = f_mount(&fsobject,"1:",1);//重新挂载其实就是再执行一次就可以了
		
		//检查第二次挂载是否正常
		printf("\r\nsecondf_mount res = %d",res);//正常返回 FR_OK（0）
		
		//flash已经存在了格式了（存在文件系统了），所以不执行这里。我们可以把文件系统的格式取消掉
		//如何取消：现在我们是在flash里面建立文件系统，但是我们重新执行一下程序，我们的例程会破坏掉我们的文件系统，因为它是直接写入的（没有带文件系统的程序）
		//（重新打开未写文件系统的keil文件并编译）
		//直接烧进来会往第一个扇区擦除数据。如果擦除第一个扇区的数据，基本上这个文件系统就被破坏掉了。然后再来下载格式化（本文件），相当于重新执行一次格式化
		
		//也可以，如果自己想要重新格式化文件系统的话，把 f_mkfs 放到 if 语句外面，就可以每次运行的时候直接格式化了。但是要注意，每次格式化后，数据都会丢失。
		
	}
	
	res = f_open(&fp,"1:abcd.txt",FA_OPEN_ALWAYS|FA_READ|FA_WRITE);//我们打开文件的时候，输入这三个参数，它会产生一个指针填充fp这个文件句柄，有了文件句柄后才能进行read/write等操作
	//创建一个文件，文件名为abcd，文件名还不支持中文。把所有想要给的权限都通过或语句|把这些宏或进来。
	printf("\r\nf_open res = %d",res);//正常返回 FR_OK（0）

	if(res == FR_OK)
	{
		res = f_write(&fp,wData,sizeof(wData),&bw);//不是说字符串不支持中文//第四个参数（如果写的数据比较长，不是一次f_write就写完的，可能是分多次写完。那么会通过第四个参数bw来告诉我们有多少个字节已经写入正常）
		printf("\r\nbw = %d",bw);

		if(res == FR_OK)
		{
			f_lseek(&fp,0);
			res = f_read(&fp,rData,f_size(&fp),&br);
		//如果按照我们的需求，我们就知道我们要读的数据大小其实就是要写进去的数据的大小。
		//但实际上我们有了文件操作，一般不会这么去指定（sizeof(rData)）。比如如果想把整个文件读取回来，用f_size函数。利用f_size来确认文件多大
			
			if(res == FR_OK)
				printf("\r\n文件内容：%s br = %d",rData,br);
		}
		f_close(&fp);
	}
	
	
	while(1);  
}


void Delay(__IO uint32_t nCount)
{
  for(; nCount != 0; nCount--);
}
/*********************************************END OF FILE**********************/

```

```bsp_spi_flash.c
 /**
  ******************************************************************************
  * @file    bsp_xxx.c
  * @author  STMicroelectronics
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   spi flash 底层应用函数bsp 
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

static __IO uint32_t  SPITimeout = SPIT_LONG_TIMEOUT;    
static uint16_t SPI_TIMEOUT_UserCallback(uint8_t errorCode);

/**
  * @brief  SPI_FLASH初始化
  * @param  无
  * @retval 无
  */
void SPI_FLASH_Init(void)
{
  SPI_InitTypeDef  SPI_InitStructure;
  GPIO_InitTypeDef GPIO_InitStructure;
	
	/* 使能SPI时钟 */
	FLASH_SPI_APBxClock_FUN ( FLASH_SPI_CLK, ENABLE );
	
	/* 使能SPI引脚相关的时钟 */
 	FLASH_SPI_CS_APBxClock_FUN ( FLASH_SPI_CS_CLK|FLASH_SPI_SCK_CLK|
																	FLASH_SPI_MISO_PIN|FLASH_SPI_MOSI_PIN, ENABLE );
	
  /* 配置SPI的 CS引脚，普通IO即可 */
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_CS_PIN;
	GPIO_InitStructure.GPIO_Speed = GPIO_Speed_50MHz;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_Out_PP;
  GPIO_Init(FLASH_SPI_CS_PORT, &GPIO_InitStructure);
	
  /* 配置SPI的 SCK引脚*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_SCK_PIN;
  GPIO_InitStructure.GPIO_Mode = GPIO_Mode_AF_PP;
  GPIO_Init(FLASH_SPI_SCK_PORT, &GPIO_InitStructure);

  /* 配置SPI的 MISO引脚*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_MISO_PIN;
  GPIO_Init(FLASH_SPI_MISO_PORT, &GPIO_InitStructure);

  /* 配置SPI的 MOSI引脚*/
  GPIO_InitStructure.GPIO_Pin = FLASH_SPI_MOSI_PIN;
  GPIO_Init(FLASH_SPI_MOSI_PORT, &GPIO_InitStructure);

  /* 停止信号 FLASH: CS引脚高电平*/
  SPI_FLASH_CS_HIGH();

  /* SPI 模式配置 */
  // FLASH芯片 支持SPI模式0及模式3，据此设置CPOL CPHA
  SPI_InitStructure.SPI_Direction = SPI_Direction_2Lines_FullDuplex;
  SPI_InitStructure.SPI_Mode = SPI_Mode_Master;
  SPI_InitStructure.SPI_DataSize = SPI_DataSize_8b;
  SPI_InitStructure.SPI_CPOL = SPI_CPOL_High;
  SPI_InitStructure.SPI_CPHA = SPI_CPHA_2Edge;
  SPI_InitStructure.SPI_NSS = SPI_NSS_Soft;
  SPI_InitStructure.SPI_BaudRatePrescaler = SPI_BaudRatePrescaler_4;
  SPI_InitStructure.SPI_FirstBit = SPI_FirstBit_MSB;
  SPI_InitStructure.SPI_CRCPolynomial = 7;
  SPI_Init(FLASH_SPIx , &SPI_InitStructure);

  /* 使能 SPI  */
  SPI_Cmd(FLASH_SPIx , ENABLE);
	
}
 /**
  * @brief  擦除FLASH扇区
  * @param  SectorAddr：要擦除的扇区地址
  * @retval 无
  */
void SPI_FLASH_SectorErase(u32 SectorAddr)
{
  /* 发送FLASH写使能命令 */
  SPI_FLASH_WriteEnable();
  SPI_FLASH_WaitForWriteEnd();
  /* 擦除扇区 */
  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();
  /* 发送扇区擦除指令*/
  SPI_FLASH_SendByte(W25X_SectorErase);
  /*发送擦除扇区地址的高位*/
  SPI_FLASH_SendByte((SectorAddr & 0xFF0000) >> 16);
  /* 发送擦除扇区地址的中位 */
  SPI_FLASH_SendByte((SectorAddr & 0xFF00) >> 8);
  /* 发送擦除扇区地址的低位 */
  SPI_FLASH_SendByte(SectorAddr & 0xFF);
  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();
  /* 等待擦除完毕*/
  SPI_FLASH_WaitForWriteEnd();
}

 /**
  * @brief  擦除FLASH扇区，整片擦除
  * @param  无
  * @retval 无
  */
void SPI_FLASH_BulkErase(void)
{
  /* 发送FLASH写使能命令 */
  SPI_FLASH_WriteEnable();

  /* 整块 Erase */
  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();
  /* 发送整块擦除指令*/
  SPI_FLASH_SendByte(W25X_ChipErase);
  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();

  /* 等待擦除完毕*/
  SPI_FLASH_WaitForWriteEnd();
}

 /**
  * @brief  对FLASH按页写入数据，调用本函数写入数据前需要先擦除扇区
  * @param	pBuffer，要写入数据的指针
  * @param WriteAddr，写入地址
  * @param  NumByteToWrite，写入数据长度，必须小于等于SPI_FLASH_PerWritePageSize
  * @retval 无
  */
void SPI_FLASH_PageWrite(u8* pBuffer, u32 WriteAddr, u16 NumByteToWrite)
{
  /* 发送FLASH写使能命令 */
  SPI_FLASH_WriteEnable();

  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();
  /* 写页写指令*/
  SPI_FLASH_SendByte(W25X_PageProgram);
  /*发送写地址的高位*/
  SPI_FLASH_SendByte((WriteAddr & 0xFF0000) >> 16);
  /*发送写地址的中位*/
  SPI_FLASH_SendByte((WriteAddr & 0xFF00) >> 8);
  /*发送写地址的低位*/
  SPI_FLASH_SendByte(WriteAddr & 0xFF);

  if(NumByteToWrite > SPI_FLASH_PerWritePageSize)
  {
     NumByteToWrite = SPI_FLASH_PerWritePageSize;
     FLASH_ERROR("SPI_FLASH_PageWrite too large!"); 
  }

  /* 写入数据*/
  while (NumByteToWrite--)
  {
    /* 发送当前要写入的字节数据 */
    SPI_FLASH_SendByte(*pBuffer);
    /* 指向下一字节数据 */
    pBuffer++;
  }

  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();

  /* 等待写入完毕*/
  SPI_FLASH_WaitForWriteEnd();
}

 /**
  * @brief  对FLASH写入数据，调用本函数写入数据前需要先擦除扇区
  * @param	pBuffer，要写入数据的指针
  * @param  WriteAddr，写入地址
  * @param  NumByteToWrite，写入数据长度
  * @retval 无
  */
void SPI_FLASH_BufferWrite(u8* pBuffer, u32 WriteAddr, u16 NumByteToWrite)
{
  u8 NumOfPage = 0, NumOfSingle = 0, Addr = 0, count = 0, temp = 0;
	
	/*mod运算求余，若writeAddr是SPI_FLASH_PageSize整数倍，运算结果Addr值为0*/
  Addr = WriteAddr % SPI_FLASH_PageSize;
	
	/*差count个数据值，刚好可以对齐到页地址*/
  count = SPI_FLASH_PageSize - Addr;
	/*计算出要写多少整数页*/
  NumOfPage =  NumByteToWrite / SPI_FLASH_PageSize;
	/*mod运算求余，计算出剩余不满一页的字节数*/
  NumOfSingle = NumByteToWrite % SPI_FLASH_PageSize;
	
	/* Addr=0,则WriteAddr 刚好按页对齐 aligned  */
  if (Addr == 0)
  {
		/* NumByteToWrite < SPI_FLASH_PageSize */
    if (NumOfPage == 0) 
    {
      SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumByteToWrite);
    }
    else /* NumByteToWrite > SPI_FLASH_PageSize */
    { 
			/*先把整数页都写了*/
      while (NumOfPage--)
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, SPI_FLASH_PageSize);
        WriteAddr +=  SPI_FLASH_PageSize;
        pBuffer += SPI_FLASH_PageSize;
      }
			/*若有多余的不满一页的数据，把它写完*/
      SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumOfSingle);
    }
  }
	/* 若地址与 SPI_FLASH_PageSize 不对齐  */
  else 
  {
		/* NumByteToWrite < SPI_FLASH_PageSize */
    if (NumOfPage == 0)
    {
			/*当前页剩余的count个位置比NumOfSingle小，一页写不完*/
      if (NumOfSingle > count) 
      {
        temp = NumOfSingle - count;
				/*先写满当前页*/
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, count);
				
        WriteAddr +=  count;
        pBuffer += count;
				/*再写剩余的数据*/
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, temp);
      }
      else /*当前页剩余的count个位置能写完NumOfSingle个数据*/
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumByteToWrite);
      }
    }
    else /* NumByteToWrite > SPI_FLASH_PageSize */
    {
			/*地址不对齐多出的count分开处理，不加入这个运算*/
      NumByteToWrite -= count;
      NumOfPage =  NumByteToWrite / SPI_FLASH_PageSize;
      NumOfSingle = NumByteToWrite % SPI_FLASH_PageSize;
			
			/* 先写完count个数据，为的是让下一次要写的地址对齐 */
      SPI_FLASH_PageWrite(pBuffer, WriteAddr, count);
			
			/* 接下来就重复地址对齐的情况 */
      WriteAddr +=  count;
      pBuffer += count;
			/*把整数页都写了*/
      while (NumOfPage--)
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, SPI_FLASH_PageSize);
        WriteAddr +=  SPI_FLASH_PageSize;
        pBuffer += SPI_FLASH_PageSize;
      }
			/*若有多余的不满一页的数据，把它写完*/
      if (NumOfSingle != 0)
      {
        SPI_FLASH_PageWrite(pBuffer, WriteAddr, NumOfSingle);
      }
    }
  }
}

 /**
  * @brief  读取FLASH数据
  * @param 	pBuffer，存储读出数据的指针
  * @param   ReadAddr，读取地址
  * @param   NumByteToRead，读取数据长度
  * @retval 无
  */
void SPI_FLASH_BufferRead(u8* pBuffer, u32 ReadAddr, u16 NumByteToRead)
{
  /* 选择FLASH: CS低电平 */
  SPI_FLASH_CS_LOW();

  /* 发送 读 指令 */
  SPI_FLASH_SendByte(W25X_ReadData);

  /* 发送 读 地址高位 */
  SPI_FLASH_SendByte((ReadAddr & 0xFF0000) >> 16);
  /* 发送 读 地址中位 */
  SPI_FLASH_SendByte((ReadAddr& 0xFF00) >> 8);
  /* 发送 读 地址低位 */
  SPI_FLASH_SendByte(ReadAddr & 0xFF);
	
	/* 读取数据 */
  while (NumByteToRead--) /* while there is data to be read */
  {
    /* 读取一个字节*/
    *pBuffer = SPI_FLASH_SendByte(Dummy_Byte);
    /* 指向下一个字节缓冲区 */
    pBuffer++;
  }

  /* 停止信号 FLASH: CS 高电平 */
  SPI_FLASH_CS_HIGH();
}

 /**
  * @brief  读取FLASH ID
  * @param 	无
  * @retval FLASH ID
  */
u32 SPI_FLASH_ReadID(void)
{
  u32 Temp = 0, Temp0 = 0, Temp1 = 0, Temp2 = 0;

  /* 开始通讯：CS低电平 */
  SPI_FLASH_CS_LOW();

  /* 发送JEDEC指令，读取ID */
  SPI_FLASH_SendByte(W25X_JedecDeviceID);

  /* 读取一个字节数据 */
  Temp0 = SPI_FLASH_SendByte(Dummy_Byte);

  /* 读取一个字节数据 */
  Temp1 = SPI_FLASH_SendByte(Dummy_Byte);

  /* 读取一个字节数据 */
  Temp2 = SPI_FLASH_SendByte(Dummy_Byte);

 /* 停止通讯：CS高电平 */
  SPI_FLASH_CS_HIGH();

  /*把数据组合起来，作为函数的返回值*/
	Temp = (Temp0 << 16) | (Temp1 << 8) | Temp2;

  return Temp;
}
 /**
  * @brief  读取FLASH Device ID
  * @param 	无
  * @retval FLASH Device ID
  */
u32 SPI_FLASH_ReadDeviceID(void)
{
  u32 Temp = 0;

  /* Select the FLASH: Chip Select low */
  SPI_FLASH_CS_LOW();

  /* Send "RDID " instruction */
  SPI_FLASH_SendByte(W25X_DeviceID);
  SPI_FLASH_SendByte(Dummy_Byte);
  SPI_FLASH_SendByte(Dummy_Byte);
  SPI_FLASH_SendByte(Dummy_Byte);
  
  /* Read a byte from the FLASH */
  Temp = SPI_FLASH_SendByte(Dummy_Byte);

  /* Deselect the FLASH: Chip Select high */
  SPI_FLASH_CS_HIGH();

  return Temp;
}
/*******************************************************************************
* Function Name  : SPI_FLASH_StartReadSequence
* Description    : Initiates a read data byte (READ) sequence from the Flash.
*                  This is done by driving the /CS line low to select the device,
*                  then the READ instruction is transmitted followed by 3 bytes
*                  address. This function exit and keep the /CS line low, so the
*                  Flash still being selected. With this technique the whole
*                  content of the Flash is read with a single READ instruction.
* Input          : - ReadAddr : FLASH's internal address to read from.
* Output         : None
* Return         : None
*******************************************************************************/
void SPI_FLASH_StartReadSequence(u32 ReadAddr)
{
  /* Select the FLASH: Chip Select low */
  SPI_FLASH_CS_LOW();

  /* Send "Read from Memory " instruction */
  SPI_FLASH_SendByte(W25X_ReadData);

  /* Send the 24-bit address of the address to read from -----------------------*/
  /* Send ReadAddr high nibble address byte */
  SPI_FLASH_SendByte((ReadAddr & 0xFF0000) >> 16);
  /* Send ReadAddr medium nibble address byte */
  SPI_FLASH_SendByte((ReadAddr& 0xFF00) >> 8);
  /* Send ReadAddr low nibble address byte */
  SPI_FLASH_SendByte(ReadAddr & 0xFF);
}


 /**
  * @brief  使用SPI读取一个字节的数据
  * @param  无
  * @retval 返回接收到的数据
  */
u8 SPI_FLASH_ReadByte(void)
{
  return (SPI_FLASH_SendByte(Dummy_Byte));
}

 /**
  * @brief  使用SPI发送一个字节的数据
  * @param  byte：要发送的数据
  * @retval 返回接收到的数据
  */
u8 SPI_FLASH_SendByte(u8 byte)
{
	 SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待发送缓冲区为空，TXE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_TXE) == RESET)
	{
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(0);
   }

  /* 写入数据寄存器，把要写入的数据写入发送缓冲区 */
  SPI_I2S_SendData(FLASH_SPIx , byte);

	SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待接收缓冲区非空，RXNE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_RXNE) == RESET)
  {
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(1);
   }

  /* 读取数据寄存器，获取接收缓冲区数据 */
  return SPI_I2S_ReceiveData(FLASH_SPIx );
}

 /**
  * @brief  使用SPI发送两个字节的数据
  * @param  byte：要发送的数据
  * @retval 返回接收到的数据
  */
u16 SPI_FLASH_SendHalfWord(u16 HalfWord)
{
	  SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待发送缓冲区为空，TXE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_TXE) == RESET)
	{
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(2);
   }
	
  /* 写入数据寄存器，把要写入的数据写入发送缓冲区 */
  SPI_I2S_SendData(FLASH_SPIx , HalfWord);

	 SPITimeout = SPIT_FLAG_TIMEOUT;
  /* 等待接收缓冲区非空，RXNE事件 */
  while (SPI_I2S_GetFlagStatus(FLASH_SPIx , SPI_I2S_FLAG_RXNE) == RESET)
	 {
    if((SPITimeout--) == 0) return SPI_TIMEOUT_UserCallback(3);
   }
  /* 读取数据寄存器，获取接收缓冲区数据 */
  return SPI_I2S_ReceiveData(FLASH_SPIx );
}

 /**
  * @brief  向FLASH发送 写使能 命令
  * @param  none
  * @retval none
  */
void SPI_FLASH_WriteEnable(void)
{
  /* 通讯开始：CS低 */
  SPI_FLASH_CS_LOW();

  /* 发送写使能命令*/
  SPI_FLASH_SendByte(W25X_WriteEnable);

  /*通讯结束：CS高 */
  SPI_FLASH_CS_HIGH();
}

/* WIP(busy)标志，FLASH内部正在写入 */
#define WIP_Flag                  0x01

 /**
  * @brief  等待WIP(BUSY)标志被置0，即等待到FLASH内部数据写入完毕
  * @param  none
  * @retval none
  */
void SPI_FLASH_WaitForWriteEnd(void)
{
  u8 FLASH_Status = 0;

  /* 选择 FLASH: CS 低 */
  SPI_FLASH_CS_LOW();

  /* 发送 读状态寄存器 命令 */
  SPI_FLASH_SendByte(W25X_ReadStatusReg);

  /* 若FLASH忙碌，则等待 */
  do
  {
		/* 读取FLASH芯片的状态寄存器 */
    FLASH_Status = SPI_FLASH_SendByte(Dummy_Byte);	 
  }
  while ((FLASH_Status & WIP_Flag) == SET);  /* 正在写入标志 */

  /* 停止信号  FLASH: CS 高 */
  SPI_FLASH_CS_HIGH();
}


//进入掉电模式
void SPI_Flash_PowerDown(void)   
{ 
  /* 通讯开始：CS低 */
  SPI_FLASH_CS_LOW();

  /* 发送 掉电 命令 */
  SPI_FLASH_SendByte(W25X_PowerDown);

  /*通讯结束：CS高 */
  SPI_FLASH_CS_HIGH();
}   

//唤醒
void SPI_Flash_WAKEUP(void)   
{
  /*选择 FLASH: CS 低 */
  SPI_FLASH_CS_LOW();

  /* 发送 上电 命令 */
  SPI_FLASH_SendByte(W25X_ReleasePowerDown);

   /* 停止信号 FLASH: CS 高 */
  SPI_FLASH_CS_HIGH();
}   
   

/**
  * @brief  等待超时回调函数
  * @param  None.
  * @retval None.
  */
static  uint16_t SPI_TIMEOUT_UserCallback(uint8_t errorCode)
{
  /* 等待超时后的处理,输出错误信息 */
  FLASH_ERROR("SPI 等待超时!errorCode = %d",errorCode);
  return 0;
}
   
/*********************************************END OF FILE**********************/

```

```diskio.c
/*-----------------------------------------------------------------------*/
/* Low level disk I/O module skeleton for FatFs     (C)ChaN, 2014        */
/*-----------------------------------------------------------------------*/
/* If a working storage control module is available, it should be        */
/* attached to the FatFs via a glue function rather than modifying it.   */
/* This is an example of glue functions to attach various exsisting      */
/* storage control modules to the FatFs module with a defined API.       */
/*-----------------------------------------------------------------------*/

#include "diskio.h"		/* FatFs lower layer API */
#include "./flash/bsp_spi_flash.h"
//#include "usbdisk.h"	/* Example: Header file of existing USB MSD control module */
//#include "atadrive.h"	/* Example: Header file of existing ATA harddisk control module */
//#include "sdcard.h"		/* Example: Header file of existing MMC/SDC contorl module */

/* Definitions of physical drive number for each drive */
//#define ATA		0	/* Example: Map ATA harddisk to physical drive 0 */
//#define MMC		1	/* Example: Map MMC/SD card to physical drive 1 */
//#define USB		2	/* Example: Map USB MSD to physical drive 2 */

#define SD_CARD 	0
#define SPI_FLASH 1


/*-----------------------------------------------------------------------*/
/* Get Drive Status                                                      */
/*-----------------------------------------------------------------------*/

DSTATUS disk_status (
	BYTE pdrv		/* Physical drive nmuber to identify the drive */
)
{
	DSTATUS stat;
//	int result;//没用到

	switch (pdrv) {
	case SD_CARD :
//		result = ATA_disk_status();

		// translate the reslut code here

		return stat;

	case SPI_FLASH :
		
		if (SPI_FLASH_ReadID() == sFLASH_ID)
		{
			//状态正常
			stat = 0;//为什么这么写，不写return 0;
			//因为加一个写保护(0000 0100)检测，可以再加stat |=STA_PROTECT。
			//为什么加|=呢，因为可能不仅是不存在，可能进行了保护。
			//最后再来返回stat，上层的ff.c就可以检测到好几个标志
		}
		else
		{
			//状态不正常
			stat = STA_NOINIT;
		}
		
//		//写保护检测
//		
//		stat |= STA_PROTECT;//用|= 。注意：没有写检测protect的函数，此处只是简单的示范一下
		
		return stat;

	}
	return STA_NOINIT;
}



/*-----------------------------------------------------------------------*/
/* Inidialize a Drive                                                    */
/*-----------------------------------------------------------------------*/

DSTATUS disk_initialize (
	BYTE pdrv				/* Physical drive nmuber to identify the drive */
)
{
	DSTATUS stat;
//	int result;

	switch (pdrv) {
	case SD_CARD :
//		result = ATA_disk_initialize();

		// translate the reslut code here

		return stat;

	case SPI_FLASH :
		
		SPI_FLASH_Init();
//		result = MMC_disk_initialize();
	
	return disk_status(SPI_FLASH);
	
//	return stat;//不需要return stat了。因为	return disk_status(SPI_FLASH)中已经有了
	}
	return STA_NOINIT;
}



/*-----------------------------------------------------------------------*/
/* Read Sector(s)                                                        */
/*-----------------------------------------------------------------------*/

DRESULT disk_read (
	BYTE pdrv,		/* Physical drive nmuber to identify the drive */
	BYTE *buff,		/* Data buffer to store read data */
	DWORD sector,	/* Sector address in LBA */
	UINT count		/* Number of sectors to read */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :
		// translate the arguments here

//		result = ATA_disk_read(buff, sector, count);

		// translate the reslut code here

		return res;

	case SPI_FLASH ://SPI_FLASH -- pdrv
	
		SPI_FLASH_BufferRead(buff,sector*4096,count*4096);//void 没有返回值,//默认返回都是正常的
	//第二个参数要根据扇区号算出地址，*4096（每个扇区4096个字节）
	//为什么要输出扇区号。文件系统上层其实都是通过最小操作单位都是扇区来操作，而不是字节
		res = RES_OK;//默认返回都是正常的
	
		return res;
	}

	return RES_PARERR;
}



/*-----------------------------------------------------------------------*/
/* Write Sector(s)                                                       */
/*-----------------------------------------------------------------------*/

#if _USE_WRITE
DRESULT disk_write (
	BYTE pdrv,			/* Physical drive nmuber to identify the drive */
	const BYTE *buff,	/* Data to be written */
	DWORD sector,		/* Sector address in LBA */
	UINT count			/* Number of sectors to write */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :
		// translate the arguments here

//		result = ATA_disk_write(buff, sector, count);

		// translate the reslut code here

		return res;

	case SPI_FLASH :
		
	//一定要先擦除再写入
		SPI_FLASH_SectorErase(sector*4096);//参数：要擦除的扇区的地址
	
		SPI_FLASH_BufferWrite((u8 *)buff,sector*4096,count*4096);
	//此处buff报错:指针类型不一致。要求输入的指针是u8，前面还是个const常量。需要把它转换过来，这里直接进行了强转
	
		res = RES_OK;//默认返回都是正常的//严谨来说应该加入读取SPI_FLASH的存储器状态/写保护状态来返回一些详细信息的
	
		return res;
	
	}

	return RES_PARERR;
}
#endif


/*-----------------------------------------------------------------------*/
/* Miscellaneous Functions                                               */
/*-----------------------------------------------------------------------*/

#if _USE_IOCTL
DRESULT disk_ioctl (
	BYTE pdrv,		/* Physical drive nmuber (0..) */
	BYTE cmd,		/* Control code */
	void *buff		/* Buffer to send/receive control data */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :

		// Process of the command for the ATA drive

		return res;

	case SPI_FLASH :

	switch(cmd)
	{
		//返回扇区个数
		case GET_SECTOR_COUNT:
					*(DWORD*)buff = 2048;//128*16=2048个扇区//报错error：空指针，空指针是没法进行操作的。所以我们需要给它一个类型
		break;
	
		//返回每个扇区的大小
		case GET_SECTOR_SIZE:
					*(WORD*)buff = 4096;
		break;
	
		//返回擦除扇区的最小个数(单位为扇区)。每次擦除4096个字节（一个扇区）
		case GET_BLOCK_SIZE:
					*(WORD*)buff = 1;//注意，不要写成了4096
		break;
	
	}
		// Process of the command for the MMC/SD card

		res = RES_OK;
		return res;
	}

	return RES_PARERR;
}
#endif

//返回时间//通过DWORD32个数据位返回一个时间
DWORD get_fattime(void)
{
	return 0;
}



```

```stm32f10x_it.c
/**
  ******************************************************************************
  * @file    Project/STM32F10x_StdPeriph_Template/stm32f10x_it.c 
  * @author  MCD Application Team
  * @version V3.5.0
  * @date    08-April-2011
  * @brief   Main Interrupt Service Routines.
  *          This file provides template for all exceptions handler and 
  *          peripherals interrupt service routine.
  ******************************************************************************
  * @attention
  *
  * THE PRESENT FIRMWARE WHICH IS FOR GUIDANCE ONLY AIMS AT PROVIDING CUSTOMERS
  * WITH CODING INFORMATION REGARDING THEIR PRODUCTS IN ORDER FOR THEM TO SAVE
  * TIME. AS A RESULT, STMICROELECTRONICS SHALL NOT BE HELD LIABLE FOR ANY
  * DIRECT, INDIRECT OR CONSEQUENTI
  
  AL DAMAGES WITH RESPECT TO ANY CLAIMS ARISING
  * FROM THE CONTENT OF SUCH FIRMWARE AND/OR THE USE MADE BY CUSTOMERS OF THE
  * CODING INFORMATION CONTAINED HEREIN IN CONNECTION WITH THEIR PRODUCTS.
  *
  * <h2><center>&copy; COPYRIGHT 2011 STMicroelectronics</center></h2>
  ******************************************************************************
  */

/* Includes ------------------------------------------------------------------*/
#include "stm32f10x_it.h"
#include "./usart/bsp_usart.h"


/** @addtogroup STM32F10x_StdPeriph_Template
  * @{
  */

/* Private typedef -----------------------------------------------------------*/
/* Private define ------------------------------------------------------------*/
/* Private macro -------------------------------------------------------------*/
/* Private variables ---------------------------------------------------------*/
/* Private function prototypes -----------------------------------------------*/
/* Private functions ---------------------------------------------------------*/

/******************************************************************************/
/*            Cortex-M3 Processor Exceptions Handlers                         */
/******************************************************************************/

/**
  * @brief  This function handles NMI exception.
  * @param  None
  * @retval None
  */
void NMI_Handler(void)
{
}

/**
  * @brief  This function handles Hard Fault exception.
  * @param  None
  * @retval None
  */
void HardFault_Handler(void)
{
	printf("\r\n hardfault error");
  /* Go to infinite loop when Hard Fault exception occurs */
  while (1)
  {
  }
}

/**
  * @brief  This function handles Memory Manage exception.
  * @param  None
  * @retval None
  */
void MemManage_Handler(void)
{
  /* Go to infinite loop when Memory Manage exception occurs */
  while (1)
  {
  }
}

/**
  * @brief  This function handles Bus Fault exception.
  * @param  None
  * @retval None
  */
void BusFault_Handler(void)
{
  /* Go to infinite loop when Bus Fault exception occurs */
  while (1)
  {
  }
}

/**
  * @brief  This function handles Usage Fault exception.
  * @param  None
  * @retval None
  */
void UsageFault_Handler(void)
{
  /* Go to infinite loop when Usage Fault exception occurs */
  while (1)
  {
  }
}

/**
  * @brief  This function handles SVCall exception.
  * @param  None
  * @retval None
  */
void SVC_Handler(void)
{
}

/**
  * @brief  This function handles Debug Monitor exception.
  * @param  None
  * @retval None
  */
void DebugMon_Handler(void)
{
}

/**
  * @brief  This function handles PendSVC exception.
  * @param  None
  * @retval None
  */
void PendSV_Handler(void)
{
}

/**
  * @brief  This function handles SysTick Handler.
  * @param  None
  * @retval None
  */
void SysTick_Handler(void)
{
}

/******************************************************************************/
/*                 STM32F10x Peripherals Interrupt Handlers                   */
/*  Add here the Interrupt Handler for the used peripheral(s) (PPP), for the  */
/*  available peripheral interrupt handler's name please refer to the startup */
/*  file (startup_stm32f10x_xx.s).                                            */
/******************************************************************************/

/**
  * @brief  This function handles PPP interrupt request.
  * @param  None
  * @retval None
  */
/*void PPP_IRQHandler(void)
{
}*/

/**
  * @}
  */ 


/******************* (C) COPYRIGHT 2011 STMicroelectronics *****END OF FILE****/

```



#### B 站 AI 视频总结

在移植文件系统时需要对配置文件进行设置包括挂载文件系统、初始化函数等。通过演示错误和配置不同的参数,展示了挂载文件系统和使用相关函数的方法。同时,还介绍了如何添加头文件和使用 f mt 函数来挂载文件系统以及如何释放空间和取消挂载文件系统。最后详细讲解了 f atfs 文件系统的类型和信息结构



## P 66 代码讲解--中文支持及 FLASH 空间分配

不是整片 FLASH 都格式化成我们的文件系统。（直接以文件系统格式存储在 FLASH 上。前 2 M ）
只是把后面 6 M 字节用来做文件系统而已。
![[../../annex/串行Flash文件系统FatFs_image_78.png]]
偏移扇区，USB 程序模拟 U 盘，它这个扇区就是从后面 6 M 字节开始读的。如果不偏移，U 盘读取到的空间和文件系统空间是不一致的。


编译 cc936. C 文件提示不需要使用这个文件就删掉
要在 ffconf. H 文件中的_CODE_PAGE 932 改成 936.
USE_LFN 0 改成非 0 值。（如果使用 1 的话，会把长文件名使用到的空间，因为长文件名要申请比较大的空间。所以会要求说可以把长文件名存储在 BSS 段（相当于全局变量）。如果是存储在 STAC 区的话，相当于是在局部变量中。还可以选择的 HEAP，即堆区。相当于使用 malloc 函数的时候申请的空间）一般情况下配置成 1，配置成 2 栈空间的话很容易就溢出了。
MAX_LFN 255, 长文件名支持的最长的长度
![[../../annex/串行Flash文件系统FatFs_image_79.png]]

USE_STRFUNC 1 配置成 0 的话，那么这些函数就不能用了
![[../../annex/串行Flash文件系统FatFs_image_80.png]]

```diskio.c
/*-----------------------------------------------------------------------*/
/* Low level disk I/O module skeleton for FatFs     (C)ChaN, 2014        */
/*-----------------------------------------------------------------------*/
/* If a working storage control module is available, it should be        */
/* attached to the FatFs via a glue function rather than modifying it.   */
/* This is an example of glue functions to attach various exsisting      */
/* storage control modules to the FatFs module with a defined API.       */
/*-----------------------------------------------------------------------*/

#include "diskio.h"		/* FatFs lower layer API */
#include "./flash/bsp_spi_flash.h"
//#include "usbdisk.h"	/* Example: Header file of existing USB MSD control module */
//#include "atadrive.h"	/* Example: Header file of existing ATA harddisk control module */
//#include "sdcard.h"		/* Example: Header file of existing MMC/SDC contorl module */

/* Definitions of physical drive number for each drive */
#define ATA		0	/* Example: Map ATA harddisk to physical drive 0 */
#define MMC		1	/* Example: Map MMC/SD card to physical drive 1 */
#define USB		2	/* Example: Map USB MSD to physical drive 2 */


#define SD_CARD   0
#define SPI_FLASH 1


/*-----------------------------------------------------------------------*/
/* Get Drive Status                                                      */
/*-----------------------------------------------------------------------*/

DSTATUS disk_status (
	BYTE pdrv		/* Physical drive nmuber to identify the drive */
)
{
	DSTATUS stat;
//	int result;

	switch (pdrv) {
	case SD_CARD :
//		result = ATA_disk_status();

		// translate the reslut code here

		return stat;

	case SPI_FLASH :		
	
		if (SPI_FLASH_ReadID() == sFLASH_ID)
		{
			//状态正常
			stat = 0;
		}
		else
		{
			//状态不正常
			stat = STA_NOINIT;		
		}

		return stat;
	}
	return STA_NOINIT;
}



/*-----------------------------------------------------------------------*/
/* Inidialize a Drive                                                    */
/*-----------------------------------------------------------------------*/

DSTATUS disk_initialize (
	BYTE pdrv				/* Physical drive nmuber to identify the drive */
)
{
	DSTATUS stat;
//	int result;

	switch (pdrv) {
	case SD_CARD :
//		result = ATA_disk_initialize();

		// translate the reslut code here

		return stat;

	case SPI_FLASH :
		
		SPI_FLASH_Init();
		SPI_Flash_WAKEUP();
//		result = MMC_disk_initialize();
	
		return  disk_status(SPI_FLASH);

	}
	return STA_NOINIT;
}



/*-----------------------------------------------------------------------*/
/* Read Sector(s)                                                        */
/*-----------------------------------------------------------------------*/

DRESULT disk_read (
	BYTE pdrv,		/* Physical drive nmuber to identify the drive */
	BYTE *buff,		/* Data buffer to store read data */
	DWORD sector,	/* Sector address in LBA */
	UINT count		/* Number of sectors to read */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :
		// translate the arguments here

//		result = ATA_disk_read(buff, sector, count);

		// translate the reslut code here

		return res;

	case SPI_FLASH :
		/* 扇区偏移2MB，外部Flash文件系统空间放在SPI Flash后面6MB空间 */
		sector += 512;
		SPI_FLASH_BufferRead(buff,sector*4096,count*4096);

		res = RES_OK;
	
		return res;

	}

	return RES_PARERR;
}



/*-----------------------------------------------------------------------*/
/* Write Sector(s)                                                       */
/*-----------------------------------------------------------------------*/

#if _USE_WRITE
DRESULT disk_write (
	BYTE pdrv,			/* Physical drive nmuber to identify the drive */
	const BYTE *buff,	/* Data to be written */
	DWORD sector,		/* Sector address in LBA */
	UINT count			/* Number of sectors to write */
)
{
	DRESULT res;
//	int result;

	switch (pdrv) {
	case SD_CARD :
		// translate the arguments here

//		result = ATA_disk_write(buff, sector, count);

		// translate the reslut code here

		return res;

	case SPI_FLASH :
		/* 扇区偏移2MB，外部Flash文件系统空间放在SPI Flash后面6MB空间 */
		sector += 512;
		
		//一定要先擦除再写入
		SPI_FLASH_SectorErase(sector*4096);
		SPI_FLASH_BufferWrite((u8 *)buff,sector*4096,count*4096);

		res = RES_OK;
	
		return res;


	}

	return RES_PARERR;
}
#endif


/*-----------------------------------------------------------------------*/
/* Miscellaneous Functions                                               */
/*-----------------------------------------------------------------------*/

#if _USE_IOCTL
DRESULT disk_ioctl (
	BYTE pdrv,		/* Physical drive nmuber (0..) */
	BYTE cmd,		/* Control code */
	void *buff		/* Buffer to send/receive control data */
)
{
	DRESULT res;

	switch (pdrv) {
	case SD_CARD :

		// Process of the command for the ATA drive

		return res;

	case SPI_FLASH :
		
		switch(cmd)
		{
			//返回扇区个数
			case GET_SECTOR_COUNT:
				/* 扇区数量：1536*4096/1024/1024=6(MB) */
				*(DWORD*)buff = 1536;
				
			break;
			//返回每个扇区大小
			case GET_SECTOR_SIZE:
				*(WORD*)buff = 4096;

			break;
			//返回擦除扇区的最小个数（单位为扇区）
			case GET_BLOCK_SIZE:
			 *(WORD*)buff = 1;
			
			break;
			
		}		
			

		// Process of the command for the MMC/SD card
		res = RES_OK;
		return res;

	}

	return RES_PARERR;
}
#endif

//返回时间
DWORD get_fattime (void)
{

	return 0;
}

```


```main.c
 /**
  ******************************************************************************
  * @file    main.c
  * @author  fire
  * @version V1.0
  * @date    2013-xx-xx
  * @brief   FATFS文件系统移植
  ******************************************************************************
  * @attention
  *
  * 实验平台:秉火 F103-指南者 STM32 开发板 
  * 论坛    :http://www.firebbs.cn
  * 淘宝    :http://firestm32.taobao.com
  *
  ******************************************************************************
  */ 
#include "stm32f10x.h"
#include "./usart/bsp_usart.h"
#include "./led/bsp_led.h"
#include "./flash/bsp_spi_flash.h"
#include "ff.h"			


FATFS fsObject;
FIL fp; 
const char wData[]="欢迎使用秉火开发板！";
char rData[4096]="";


UINT bw;
UINT br;

/*
 * 函数名：main
 * 描述  ：主函数
 * 输入  ：无
 * 输出  ：无
 */
int main(void)
{ 	
	FRESULT res;
	
	LED_GPIO_Config();
	LED_BLUE;
	
	/* 配置串口为：115200 8-N-1 */
	USART_Config();
	printf("\r\n 这是一个FATFS文件系统移植实验 \r\n");
	
	//挂载文件系统
	res = f_mount(&fsObject,"1:",1);

	printf("\r\nf_mount res =%d",res);
	
	if(res == FR_NO_FILESYSTEM)
	{
		res = f_mkfs("1:",0,0);
		printf("\r\nf_mkfs res =%d",res);
		//格式化后要取消挂载再重新挂载文件系统
		res = f_mount(NULL,"1:",1);
		res = f_mount(&fsObject,"1:",1);
		
		printf("\r\nsecond f_mount res =%d",res);
	}

	
	res = f_open(&fp,"1:中文文件名abcdefgadfasd.txt",FA_OPEN_ALWAYS|FA_READ|FA_WRITE);
	printf("\r\nf_open res =%d",res);
	
	if(res == FR_OK)
	{
		res = f_write(&fp,wData,sizeof(wData),&bw);
		printf ("\r\nbw= %d",bw);		

		if(res == FR_OK)
		{
			f_lseek(&fp,0);
			res = f_read (&fp,rData,f_size(&fp),&br);
			if(res == FR_OK)
				printf ("\r\n文件内容：%s br= %d",rData,br);		
		}	
		
		f_close(&fp);
	
	}
	

	while(1);  
}


void Delay(__IO uint32_t nCount)
{
  for(; nCount != 0; nCount--);
}
/*********************************************END OF FILE**********************/

```





#### B 站 AI 视频总结

如何支持中文文件名和长文件名, 以及如何偏移文件系统空间。通过修改配置和加入偏移, 实现了对文件系统的读写操作。同时, 介绍了 fresh 芯片上预留的空间用于存储字库文件和参数, 这些文件数据不使用文件系统存储而是以非文件系统格式存储。还提供了使用 usb 线连接电脑查看 fresh 上的文件的演示。

- 如何支持中文文件名和长文件名, 以及如何偏移文件系统空间。
00:01 通过测试修改配置，文件系统已正常运行
01:06 中文文件名不支持，需要偏移空间
05:38 预留空间存储字库文件等，不希望占用 STM 32 空间

- 使用非文件系统格式存储数据的方法以及如何对文件系统进行偏移。
08:06 数据存储在 flash 上，不使用文件系统格式
10:18 前两兆字节预留给非文件系统，后面六兆字节用于文件系统
14:28 标准程序里边的差异和中文支持

- 如何通过下载编译程序和下载数据将 Fresh 空间恢复到正常状态。
16:13 下载程序并连接 USB 设备，电脑会显示 U 盘
17:46 可以通过这种形式查看 U 盘里的文件
22:48 添加编码页和修改配置文件，支持中文和长文件名

- 文件系统的主要内容包括长文件名、中文支持和扇区偏移等, 并演示了文件系统的移植和应用。
24:16 介绍如何下载并查看文件系统
25:36 讲解文件系统的主要内容，包括长文件名和中文支持
27:35 演示如何移植文件系统到开发板，并进行文件写入测试



## P 67 代码讲解--文件系统常见应用

强调：使用文件系统的所有函数之前，一定要先挂载文件系统，包括要执行格式化的时候。
执行格式化之前也需要先调用 f_mount，因为 f_mount 不仅初始化了整个文件系统，而且会初始化一些底层的 stm 32 的 SPI 初始化。
如果整个工程没有调用 SPI 初始化，那么整个系统就不可能正常运行的。
总之，要用到文件系统相关的内容，首先第一个要考虑的就是要先挂载这个文件系统。挂载的时候可以通过一个返回值来确认是否挂载成功。调用了什么函数就查看返回值是什么。







































#### B 站 AI 视频总结

文件系统的应用和操作, 包括文件读写、文件系统的信息、存储器的信息、文件的信息以及扫描文件系统里的文件等内容。同时, 视频还介绍了如何使用文件系统来创建文件和目录并演示了这些操作的过程。此外视频还提到了文件系统所占用的存储空间以及在不同存储设备上使用文件系统的问题。

- 文件系统在开发板上的应用包括读写文件、扫描文件等操作, 并解释了文件系统所占用的空间。
02:27 扫描文件系统: 如何扫描文件系统里的文件包括文件系统信息、文件信息等。
04:58 存储设备空间计算: 文件系统的总空间会比实际空间少，这是因为文件系统本身会占用一些空间。
05:44 移植文件系统: 文件系统信息结构本身就占据了 300 多 kb，所以无法移植到 256 字节的 ROM 中

- 文件系统的使用方法包括创建文件、写入内容、创建目录、获取文件信息等操作。
06:44 文件系统存储和挂载: 介绍了文件系统存储和挂载的相关知识。
09:29 文件系统操作: 介绍了文件系统的创建、读取、写入、删除等操作。
10:34 文件扫描: 介绍了文件扫描的常用方法和应用场景。

- 如何使用 USBU 盘读取开发板上的文件系统并讲解了相关操作和注意事项。
12:01 USB 盘读取文件: 通过扫描目录和读取文件信息，展示了如何通过编程读取 USB 盘上的文件。
13:52 文件系统操作: 详细介绍了如何挂载和操作文件系统，以及如何处理文件系统操作可能出现的错误。
15:48 使用文件系统: 介绍了如何使用文件系统来进行文件的操作，包括打开、写入和关闭文件等。

- 如何通过调试函数返回值来解决文件系统调试错误并详细介绍了多项功能测试的过程。
17:52 返回值的使用和多功能测试
22:17 获取文件系统信息和操作

- 获取存储设备信息和读写文件的步骤包括使用函数和参数的方法。
23:01 获取存储设备信息和读写文件过程
27:31 读写文件的要点
27:52 创建和写入文件的过程

- 文件操作的参数设置, 包括打开、创建、追加等操作, 并详细讲解了 print 函数的用法。
28:28 操作文件的标志位和函数的使用
33:03 文件操作函数和字符设备驱动开发

- 使用 Python 的 os 模块中的函数来实现文件的读写操作包括定位光标、读取文件等。
33:29 文件操作: 光标移动和数据读写
34:29 文件读取: 缓冲区和字节数计算
37:05 读取文件: 多次读取和参数设置

- 通过文件读取参数来判断文件结尾并通过重命名演示了文件管理操作。
38:13 文件读取循环: 通过循环读取大文件，防止文件到达结尾导致读取出错。
40:07 文件操作: 介绍了如何重命名文件、打开文件夹等操作。
42:43 创建目录和文件: 讲解了如何创建目录和文件，以及文件的操作。

- 关于文件操作的一些基本函数, 包括创建、删除、重命名等操作, 以及如何读取文件属性。
43:32 删除和重命名文件的方法
47:20 长文件名和短文件名之间的转换

- 如何使用信息结构体获取文件信息以及如何使用 fvdir 函数扫描文件。
48:33 获取文件信息和属性
51:57 递归函数和文件扫描
53:35 递归函数中的变量分配和内存管理

- 如何使用长文件名和信息结构体来扫描文件和目录并避免溢出的方法。
53:41 长文件名和文件夹扫描
57:03 数组溢出和文件操作

- 如何使用递归函数扫描目录和文件并获取文件信息的过程。
58:52 使用 dir 函数获取文件信息
01:00:47 递归扫描目录和文件
01:03:30 文件操作示例: 创建文件、写入数据、追加数据、读取数据、删除文件

- Skin fire 函数的执行过程和应用, 以及取消挂载文件系统的使用方法和注意事项。
01:03:54 学习函数执行过程和扫描目录
01:04:33 挂载和取消挂载文件系统
01:06:02 恢复 spiflash 出厂内容
























































































