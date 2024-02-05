
vTaskDelay 保证休眠的时间是一样的。
vTaskDelayUntil 保证每次执行的时刻的间隔是一样的，任务启动的时刻是周期性的

![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_1.png]]
这两个延时函数有什么差别呢？

vTaskDelay 表示的是任务延时的时间，这段时间是固定的。
vTaskDelay(N)表示进去和出来的延迟时间
![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_2.png]]

如果想让任务周期性的执行
![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_3.png]]
从 t1、t2、t3 开始执行，由图可知 t1、t2、t3 的间隔是不一样的。


![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_4.png]]
如果想让△t1、△t2、△t3 一致，让它固定的周期性地执行，怎么办？
使用 vTaskDelayUntil 函数，传入参数：t1，△t
```
vTaskDelayUntil(t1,△t);
```
使用这个函数时，使用了 Until 指定了终点。
无论何时执行、执行多久，终点是不变的。于是使用这个函数可以让任务周期性的执行。
![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_5.png]]

![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_6.png]]

用滚轮调整 Grid，1 ms 就是 1 Tick。
![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_7.png]]

![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_8.png]]

![[../../../annex/06-3_vTaskDelay和vTaskDelayUntil_image_9.png]]
```
1、直到*pxPre+△t时刻的时候，这个函数才会从里面退出来
2、传入指针的意思就是会去更新这个值：*pxPre=*pxPre+△t。这样下次的时候又可以重新去调用它，不需要修改时间，只需传入参数即可。
```

![[../../../annex/Pasted image 20240205222219.png]]

到达 t2 后，会从这个函数（vTaskDelayUntil (&tStart, 20)）里面退出来。除此之外还会更新 tStart=t2=t1+20

![[../../../annex/Pasted image 20240205222625.png]]
任务启动的时刻是周期性的
![[../../../annex/Pasted image 20240205222752.png]]
波形是周期性出现高电平的


弹幕：同优先级因为是交错执行且每个任务执行一个中断时间，若有一任务执行时间过长则会经过多个中断时间执行完 
弹幕：超过时间延时函数直接返回错误，不会进行延时
弹幕：去掉 int 前的 static，就可以添加进逻辑分析仪了
弹幕：vTaskDelayUntil(&tStart, 20);   //能够保证这一次启动到下一次启动的时间一样，但休眠时间不一样
弹幕：一个延时是固定时间。一个是任务加延时是固定时间
弹幕：一个是相对的延时，一个是绝对的延时
弹幕：一个延时一个时间段，一个延时到某个时刻
弹幕：delay 就是让任务执行完延迟20个 tick，delayuntil 就是每隔20个 tick 都会执行一次
弹幕：一个是低电平时间固定，一个是周期固定但占空比不一样




















































































































