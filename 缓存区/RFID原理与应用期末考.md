
#### 已经学过的 RFID 解决方案
（1-4 硬件连接图、文字说明、主要参数、功能模块名称、源代码）

##### 1 .设计 Arduino <font color="#ff0000">继电器控制终端</font>电器的解决方案 
###### （1）电路、硬件图
![[Pasted image 20230614220640.png]]
![[Pasted image 20230614221336.png]]
连接电路
```arduino
 * 接口列表：
 *  Relay         Arduino Uno R3
 *   DC+               5V
 *   DC-               GND
 *   IN                 7
```
继电器：IN、GND、VCC
Arduino：~7 、GND、5 V
①用一条“公对公”跳线让面包板<font color="#ff0000">负极连接负极</font>，即接地；

②用一条“公对公”跳线让Arduino Uno主板上的<font color="#ff0000">“GND”</font>与面包板<font color="#ff0000">负极</font>连接，即接地；

③用一条“公对公”跳线让Arduino Uno主板上的<font color="#ff0000">“5V”</font>与面包板<font color="#ff0000">正极</font>连接；

<font color="#ff0000">④</font>用一条“公对公”跳线让面包板上的<font color="#ff0000">“a33”</font>与<font color="#ff0000">继电器模块“VCC”</font>连接；

⑤用一条“公对公”跳线让面包板上的<font color="#ff0000">“b33”</font>与面包板<font color="#ff0000">正极</font>连接；

<font color="#ff0000">⑥</font>用一条“公对母”跳线让<font color="#ff0000">继电器模块上的</font><font color="#ff0000">“GND”</font>与面包板<font color="#ff0000">负极</font>连接，即接地；

⑦用一条“公对母”跳线让面包板的<font color="#ff0000">正极</font>与面包板<font color="#ff0000">正极</font>连接；

<font color="#ff0000">⑧</font>用一条“公对母”跳线让<font color="#ff0000">继电器模块IN</font>与ArduinoUno主板上“<font color="#ff0000">~7</font>”连接。


###### （2）组件 ：
1. USB 数据线 * 1
2. 面包板 * 1
3. 继电器模块 * 1
4. 跳线若干
5. 
![[Pasted image 20230623212020.png]]

###### （3）源代码
const int <font color="#ff0000">relayPin =7</font>;//继电器模块的 s 连接到 7

<font color="#ff0000">void setup() {</font>

  // put your setup code here, to run once:

  <font color="#ff0000">pinMode(relayPin,OUTPUT);//将继电器初始化为输出</font>

<font color="#ff0000">}</font>

<font color="#ff0000">void loop() {</font>

  // put your main code here, to run repeatedly:

   <font color="#ff0000">digitalWrite(relayPin,HIGH);//关闭继电器</font>

<font color="#ff0000">  delay(1000);//延时1s</font>

<font color="#ff0000">  digitalWrite(relayPin,LOW);//断开继电器</font>

<font color="#ff0000">  delay(1000);//延时1s</font>

<font color="#ff0000">}</font>

###### （4）实验现象
![[Pasted image 20230614221110.png]]
我们观察到，将程序上传成功后，每隔1秒钟时间，继电器控制LED闪烁绿光，同时伴随着“滴答”声，这是常开触点打开和常开触点闭合的声音。

######  扩展
继电器通常可以用于弱电驱动强电, 低电压驱动高电压, 左侧为弱电电路控制端, 右侧为强电电路控制端

Vcc: 弱电电路正极
GND: 弱电电路负极
<font color="#ff0000">In: 弱电电路信号引脚</font>。
Com: 强电电路公共极
NC: In 输入低压信号, NC 与 com 闭合, NO 与 com 断开
NO: In 输入高压信号, NC 与 com 断开, NO 与 com 闭合
————————————————
版权声明：本文为 CSDN 博主「宝全哥哥」的原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接及本声明。
原文链接： https://blog.csdn.net/weixin_41832302/article/details/119788641


##### 2 .设计 Arduino <font color="#ff0000">LED 闪烁报警</font>解决方案。
###### 1.电路、硬件图
![[Pasted image 20230614222028.png]]
![[Pasted image 20230614222035.png]]
双色 LED 模块：GND、S、G
电路连接
①用一条“公对公”跳线让面包板负极连接负极，即接地；

②用一条“公对公”跳线让Arduino Uno主板上的“<font color="#ff0000">GND</font>”与面包板<font color="#ff0000">负极</font>连接，即接地；

③用一条“公对公”跳线让Arduino Uno主板上的“<font color="#ff0000">5V</font>”与面包板<font color="#ff0000">正极</font>连接；

④用一条“公对公”跳线让Arduino Uno主板上的“<font color="#ff0000">～11</font>”与面包板“<font color="#ff0000">f1</font>”连接；

⑤用一条“公对公”跳线让Arduino Uno主板上的“<font color="#ff0000">～10</font>”与面包板“<font color="#ff0000">g2</font>”连接；

⑥用一条“公对母”跳线让<font color="#ff0000">双色LED模块</font>上的“<font color="#ff0000">GND</font>”与面包板<font color="#ff0000">负极</font>连接，即接地；

⑦用一条“公对母”跳线让<font color="#ff0000">双色LED模块</font>上的“<font color="#ff0000">S</font>”与面包板“<font color="#ff0000">j1</font>”连接；

⑧用一条“公对母”跳线让<font color="#ff0000">双色LED模块</font>上的“<font color="#ff0000">G</font>”与面包板“<font color="#ff0000">j2</font>”连接。

###### 2.硬件
1. Arduino Uno 主板 * 1

2. USB 数据线 * 1

3. 双色 LED 模块 * 1

4. 面包板 * 1

5. 9V 方型电池 * 1

6. 跳线若干

###### 3.源代码
Int <font color="#ff0000">LedPin = 9</font>; //定义数字 9 接口
 
Void setup ()
{
PinMode (<font color="#ff0000">LedPin</font>, OUTPUT);//定义小灯接口为输出接口
}
Void loop ()
{
DigitalWrite (<font color="#ff0000">LedPin</font>, HIGH); //点亮小灯
Delay (1000); //延时 1 秒
DigitalWrite (<font color="#ff0000">LedPin</font>, LOW); //熄灭小灯
Delay (1000); // 延时 1 秒
}

connection :
————————————————————
Dual-color LED    Arduin Uno R3

R                                   11

GND                            GND

G                                   10

————————————————————

int <font color="#ff0000">rgb_R=11</font>;//接到板子上面的PWM口11 R

int <font color="#ff0000">rgb_G=9</font>;//接到板子上面的PWM口9 G

int <font color="#ff0000">rgb_B=10</font>;//接到板子上面的PWM口10 B

void setup()

{

   pinMode(<font color="#ff0000">rgb_R</font>,OUTPUT);//设置rgb_R的控制口为输出模式

   pinMode(<font color="#ff0000">rgb_G</font>,OUTPUT);//设置rgb_G的控制口为输出模式

   pinMode(<font color="#ff0000">rgb_B</font>,OUTPUT);//设置rgb_B的控制口为输出模式

}

void color(<font color="#ff0000">int red,int green,int blue</font>)//注意其中各个参数范围为0到255，值不要超过255

{

   <font color="#ff0000">analogWrite</font>(rgb_R,red);

   analogWrite(rgb_G,green);

   analogWrite(rgb_B,blue);

}

void loop()

{

   <font color="#ff0000">color(255,255,255)</font>;//white

   delay(1000);//延时1s

   color(<font color="#ff0000">255,0,0</font>);//red

   delay(1000);//延时1s

   color(<font color="#ff0000">0,255,0</font>);//green

   delay(1000);//延时1s

   color(<font color="#ff0000">0,0,255</font>); //blue

   delay(1000);//延时1s
——————————————————————

###### 4.实验现象
下载完程序就可以看到我们的10 口外接小灯在闪烁了，此时实验现象为 LED 不停闪烁，间隔大约为一秒。
（1）当串口监视器数值在<font color="#ff0000">255</font>左右时候，双色 LED 显示<font color="#ff0000">红</font>灯。
（2）当串口监视器数值在<font color="#ff0000">0</font>左右时候，双色LED显示<font color="#ff0000">绿</font>灯。

##### 3 .手机 APP <font color="#ff0000">蓝牙+LED </font>遥控应用终端的解决方案。
###### 1.电路、硬件连接
![[Pasted image 20230614222727.png]]
![[Pasted image 20230614222818.png]]
蓝牙模块：VCC、GND、TXD--0、RXD--1
连接电路

①用一条跳线让Arduino Uno主板上的“～0”与面包板“g43”相连；

②用一条跳线让Arduino Uno主板上的“~1”与面包板“g44”连接；

③用一条跳线让Arduino Uno主板上的“<font color="#ff0000">～2</font>”与面包板“<font color="#ff0000">b25</font>”相连；

④用一条跳线让Arduino Uno主板上的“5V”与面包板“g41”相连；

⑤用一条跳线让Arduino Uno主板上的“GND”与面包板“g42”相连；

⑥用一条跳线让Arduino Uno<font color="#ff0000">主板</font>上的“<font color="#ff0000">VIN</font>”与面包板“<font color="#ff0000">a37</font>”相连；

⑦用一条跳线让<font color="#ff0000">蓝牙模块</font>的<font color="#ff0000">VCC</font>与面包板“<font color="#ff0000">i41</font>”相连；

⑧用一条跳线让蓝牙模块的<font color="#ff0000">GND</font>与面包板“<font color="#ff0000">i42</font>”相连。

⑨用一条跳线让蓝牙模块的<font color="#ff0000">TXD</font>与面包板“<font color="#ff0000">i43</font>”相连；

⑩用一条跳线让蓝牙模块的<font color="#ff0000">RXD</font>与面包板“<font color="#ff0000">i44</font>”相连；

⑪<font color="#ff0000">灯泡正极</font>插入面包板“<font color="#ff0000">b37</font>”，<font color="#ff0000">负极</font>插入面包板“<font color="#ff0000">b36</font>”；

⑫用<font color="#ff0000">电阻</font>连接面包板“<font color="#ff0000">b25</font>”处的跳线与面包板上“<font color="#ff0000">b36</font>”处的<font color="#ff0000">灯泡负极</font>。


###### 2.实验原理
通过蓝牙模块-BT04-A的控制，达到LED灯泡亮灭的变化的原理。通过蓝牙调试器遥控实验，达到按下遥控键1时，LED灯泡亮起；按下遥控键2时，LED灯泡熄灭的效果。

###### 3.硬件 ：

1.Arduino Uno 主板 * 1

2.USB 数据线 * 1

3.蓝牙模块：BT04-A 模块*1

4.面包板 * 1

5.手机 APP-蓝牙调试器

6.跳线若干

7. LED 灯

###### 4.源代码

```
int led=2;//定义板子上数字2口

void setup()

{

 Serial.begin(9600);//定义串口设置

 pinMode(led,OUTPUT);//定义小灯为输出模式

}

void loop()

{

  while(Serial.available())//判断是否接收到蓝牙数据

   {

     char c=Serial.read();//读取蓝牙传输的值

     if ('A' == c) { //app中按键1被按下

        digitalWrite(led,HIGH);//对应的LED小灯亮起               

      }else if ('B' == c){//app中按键2被按下

         digitalWrite(led,LOW);//对应的LED小灯熄灭

      }

      Serial.print(c);//串口打印输出的值

   }

}
```

###### 5.观测蓝牙模块运行情况 （实验效果）

成功上传源代码后，整个硬件设备通电，此时灯泡亮起。蓝牙模块处的红灯一闪一闪的亮，表明未与手机APP-蓝牙调试器相连接。我们将手机开启蓝牙，通过蓝牙调试器APP找到我们的蓝牙模块设备型号：BT04-A蓝牙模块，随后相连，连接后发现蓝牙模块从一闪一闪的红灯变为了常亮的红灯状态，这表明我们的蓝牙模块成功连接，手机上也已显示：BT04-A已连接。

随后我们通过蓝牙调试器进行调控：我们从对话模式中，输入A，系统反馈回A，此时灯泡依旧亮起，然后我们输入B，系统回馈B，灯泡熄灭，反复操作验证，发现灯泡在蓝牙模块的控制下，通过蓝牙调试器可以达到：输入A为灯泡亮起，输入B为灯泡熄灭。

我们通过按钮控制也可达到相同的效果。按下按钮1，系统回馈A，灯泡亮起；我们按下按钮2，系统回馈B，灯泡熄灭。反复验证得到：按下按钮1，灯泡亮起；按下按钮2，灯泡熄灭。这与我们的源代码一致，至此，实验完成。




##### 4 .手机 APP+<font color="#ff0000">蓝牙+LED+继电器</font>遥控终端的解决方案。
###### 1. 电路、硬件连接图
![[Pasted image 20230614223233.png]]
![[Pasted image 20230614223331.png]]



连接电路（<font color="#ff0000">①~④为 LCD1602接线，⑤~⑭为 LED、电阻以及蓝牙模块接线，⑮~⑰为继电器模块接线，⑱~㉑为 Arduino 与面包板的正极等其他接线</font>）

①用一条跳线让Arduino Uno主板上的“GND”与LCD1602“GND”相连，即接地；

②用一条跳线让面包板的正极与<font color="#ff0000">LCD1602的“VCC”</font>连接；

③用一条跳线让 Arduino Uno 主板上的“<font color="#ff0000">A4</font>”与 LCD1602的“<font color="#ff0000">SDA</font>”相连；

④用一条跳线让 Arduino Uno 主板上的“<font color="#ff0000">A5</font>”与 LCD1602的“<font color="#ff0000">SCL</font>”相连；

⑤用一条跳线让Arduino Uno主板上的“<font color="#ff0000">～0</font>”与面包板“g43”相连；

⑥用一条跳线让Arduino Uno主板上的“<font color="#ff0000">~1</font>”与面包板“g44”连接；

⑦用一条跳线让Arduino Uno主板上的“<font color="#ff0000">～2</font>”与面包板“b25”相连；

⑧用一条跳线让Arduino Uno主板上的“5V”与面包板“g41”相连；

⑨用一条跳线让Arduino Uno主板上的“GND”与面包板“g42”相连；

⑩用一条跳线让Arduino Uno主板上的“VIN”与面包板“a37”相连；

⑪用一条跳线让蓝牙模块的<font color="#ff0000">VCC</font>与面包板“<font color="#ff0000">i41</font>”相连；

⑫用一条跳线让蓝牙模块的<font color="#ff0000">GND</font>与面包板“<font color="#ff0000">i42</font>”相连。

⑬用一条跳线让蓝牙模块的<font color="#ff0000">TXD</font>与面包板“<font color="#ff0000">i43</font>”相连；

⑭用一条跳线让蓝牙模块的<font color="#ff0000">RXD</font>与面包板“<font color="#ff0000">i44</font>”相连；

⑮用一条跳线让面包板上的正极与<font color="#ff0000">继电器模块“VCC”</font>连接；

⑯用一条跳线让Arduino Uno主板上的<font color="#ff0000">GND</font>与<font color="#ff0000">继电器模块“GND”</font>连接，即接地；

⑰用一条跳线让Arduino Uno主板上的“<font color="#ff0000">~7</font>”与<font color="#ff0000">继电器模块“IN”</font>连接；

⑱用一条跳线让Arduino Uno主板上的“<font color="#ff0000">5V</font>”与面包板的“<font color="#ff0000">正极</font>”相连；

⑲用一条跳线让面包板上两边的正极相连；

⑳<font color="#ff0000">灯泡正极</font>插入面包板“<font color="#ff0000">b37</font>”，<font color="#ff0000">负极</font>插入面包板“<font color="#ff0000">b36</font>”；

㉑用<font color="#ff0000">电阻</font>连接面包板“<font color="#ff0000">b25</font>”处的跳线与面包板上“<font color="#ff0000">b36</font>”处的<font color="#ff0000">灯泡负极</font>。


###### 2 .实验原理
蓝牙模块控制黄色LED灯泡的亮灭，继电器模块反应程序的响应情况，并发出声音提示。

###### 3.硬件
Arduino Uno 主板 * 1

USB数据线 * 1

BTO4-A蓝牙模块 *1

继电器模块 * 1

LCD1602液晶显示屏 * 1

面包板 * 1

跳线若干

###### 4. 源代码
connection :
Arduino UNO  BT04-A蓝牙
5V                       VCC
GND                    GND
0                         TXD
1                          RXD
——————————————

```

#include <LiquidCrystal_I2C.h>//LCD1602 IIC 液晶屏库

LiquidCrystal_I2C lcd(0x27,16,2); //设置LCD1602设备地址0x27

const int relayPin =7;//继电器模块的 s 连接到 7

int led=2;//定义板子上数字2口

void setup()

{

  lcd.init();                  // LCD初始化

  lcd.backlight();             //设置LCD背景灯亮

 Serial.begin(9600);//定义串口设置

 pinMode(led,OUTPUT);//定义小灯为输出模式

 pinMode(relayPin,OUTPUT);//将继电器初始化为输出

}

void loop()

{     lcd.setCursor(0,0); //液晶显示第一行位置

      lcd.print("The Yellow LED:");

  while(Serial.available())//判断是否接收到蓝牙数据

   {

      char c=Serial.read();//读取蓝牙传输的值

      if ('A' == c) { //app中按键1被按下

        lcd.setCursor(0,1); //液晶显示第二行位置

        lcd.print("Snuffed out  ");

        digitalWrite(led,HIGH);//对应的LED小灯亮起                

      }else if ('B' == c){//app中按键2被按下

        lcd.setCursor(0,1); //液晶显示第二行位置

        lcd.print("Light up    ");

         digitalWrite(led,LOW);//对应的LED小灯熄灭

      }

      Serial.print(c);//串口打印输出的值

  digitalWrite(relayPin,HIGH);//关闭继电器

  delay(1000);//延时1s

  digitalWrite(relayPin,LOW);//断开继电器

  delay(1000);//延时1s

   }

}
```

###### 5.观测蓝牙模块运行情况 （实验效果）

成功上传源代码后，此时未连接蓝牙BT04-A，蓝牙模块的红灯一闪一闪，继电器模块的红灯亮起，绿灯一闪一闪，并伴随滴答作响；黄色LED灯泡亮起，整个硬件设备通电。

接着，我们通过手机连接蓝牙，用蓝牙适配器连接蓝牙BT04-A，此蓝牙模块红色不再是一闪一闪的亮，而是常亮。继电器模块绿灯也变为常亮。

随后，我们按下蓝牙的按钮控制功能，按下按钮A，黄色LED灯泡熄灭，同时继电器模块的绿灯伴随着滴答两声而先灭后亮起，并保持常亮。

我们通过按下按钮 B，发现此时黄色 LED 灯泡熄灭，同时继电器模块的绿灯伴随着滴答两声而先灭后亮起，并保持常亮。

反复实验后，发现实验情况与上述一致。

至此，实验完成。



(1-4 要画出硬件连接图，并做文字说明; 准确标注功能模块名称和主要技术参数; 写出全部源代码。)

##### 5 .一般<font color="#ff0000">连锁门店条码应用</font>解决方案 (要写出<font color="#ff0000">店内码编码方案</font>、<font color="#ff0000">条码设备选用</font>和<font color="#ff0000">管理信息系统</font>)。

![[18e85c6aeb90e0e0f8b94b78b2541cd.jpg]]

![[6cbb0cf5ea5114b16e1f5b09779e462.jpg]]





##### 6 -8. RFID 门店<font color="#ff0000">防止</font>商品<font color="#ff0000">盗出</font>系统。<font color="#ff0000">ETC</font> 不停车收费系统。RFID <font color="#ff0000">双频运动</font>管理系统。(6-8 要画<font color="#ff0000">系统结构图</font>，<font color="#ff0000">标注主要功能模块名称和技术参数</font>。)

###### 6. RFID 门店防止商品盗出系统（超高频，915 MHz）
![[Pasted image 20230615114041.png]]
![[Pasted image 20230615114553.png]]
[RFID电子标签在商品防窃（EAS系统）中的应用—铨顺宏 _RFID世界网 (rfidworld.com.cn)](http://solution.rfidworld.com.cn/2020_08/f8b364daa0fde78a.html)


###### 7.ETC 不停车收费系统（微波，2.5 GHz）
![[a4265ef1215664b72e9c78635f71935.jpg]]

![[192f3b6bc282dfd432b560f68b90c0e.jpg]]

![[f63934611fca4aaa2cb8b91f7aaff8d.jpg]]

![[2803580173bbd4d041ade449e1afafa.jpg]]





###### 8. RFID 双频运动管理系统（腕表，低频，125 KHz；电子标签，微波，2.45 GHz）
  
![[d8fdbc3b9d69819737953410957c0fa.jpg]]




#### 考试要求
![[51cd551d6ed16b853fe7ea6bbe5225c.jpg]]
![[a63e9c70246b243f016cd8c3ac6e7ab.jpg]]