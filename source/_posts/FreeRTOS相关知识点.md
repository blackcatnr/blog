---
title: FreeRTOS相关知识点
date: 2023-08-29
tags: ['星海']
categories: 星海
id: FreeRTOS相关知识点
---
<!-- more -->
# FreeRTOS相关知识点
# 目录
      - [挂起：挂起的任务类似暂停，可恢复（解挂）；删除任务，无法恢复。被挂起的任务绝不会得到CPU的使用权，不管该任务具有什么优先级。vTaskSuspend() 函数调用时需要将宏INCLUDE_vTaskSuspend() 配置成1。](#section.1)<a name="context.1"> </a>
      - [恢复：恢复被挂起的任务。==任务恢复就是让挂起的任务重新进入就绪状态，恢复的任务会保留挂起前的状态信息，在恢复时根据挂起时的状态继续运行。vTaskResume() 函数调用时需要将宏INCLUDE_vTaskResume() 配置成1==](#section.2)<a name="context.2"> </a>
        - [FromISR：==带FromISR后缀是在中断函数中专用的API==。无论调用过多少次的任务挂起vTaskSuspend() ，只需要调用一次vTaskResumeFromISR()  即可解挂。==使用该函数时，需要在FreeRTOSconfig.h中把INCLUDE_vTaskSuspend()和INCLUDE_vTaskResumeFromISR()  都定义为1。==vTaskResumeFromISR()函数不能用于任务和中断的同步（因为中断随时可能出现）。](#section.3)<a name="context.3"> </a>
------------------------------------------------  

​    FreeRTOS支持的三种调度方式：
  （1）抢占式调度：主要是针对优先级不同的任务，每一个任务都有一个优先级，优先级高的任务可以抢占优先级低的任务。
  （2）时间片调度：主要是针对优先级相同的任务，当多个任务的优先级相同时，任务调度器会在每一个时钟节拍到的时候切换任务。         （时间片1ms）在FreeRTOS中，一个时间片就是SysTick中断周期。更改时间片的大小，可由更改滴答定时器中断周期去实现。
​     补充：时间片：同等优先级的任务轮流的享有相同的CPU时间。在FreeRTOS中，一个时间片就等于一个SysTick中断周期
  （3）协程式调度：当前执行的任务会一直执行，同时高优先级的任务不会抢占低优先级的任务。

1. ## FreeRTOS中任务共存在四种状态：

  （1）运行态：正在执行的任务，该任务就处于运行态，注意在STM32中，同一时间仅有一个任务处于运行态。
  （2）就绪态：如果该任务已经能够被执行，但当前还未被执行，那么该任务处于就绪态。
  （3）阻塞态：如果一个任务因为延时或等待外部事件发生，那么这个任务就处于阻塞态。
  （4）挂起态：类似暂停，需要调用函数vTaskSuspend()进入挂起态，调用解挂vTaskResume()才可以进入到就绪态。==注意：任务处于挂起态的时候经过解挂不会直接进入到运行态，只会进入到就绪态。同样阻塞态也不会直接进行到运行态，会进入到就绪态。仅就绪态可转变为运行态，其他状态的任务想运行，必须先转为就绪态。==

2. ## 系统配置文件：

  FreeRTOSConfig.h 配置文件作用：对FreeRTOS进行功能配置和裁剪，以及API函数的使能。
  （1）“INCLUDE”： 配置FreeRTOS中可选的API
  （2）“config” ：完成FreeRTOS的功能配置和裁剪
  （3）其他的配置项：完成PendSV（中断）宏定义、SVC宏定义

3. ## 任务的创建和删除的API函数

      ​    ==任务的创建和删除的本质就是调用FreeRTOS的API函数==
      ​       两种创建方式：（1）==动态方式创建==（xTaskCreate）———任务的任务控制块以及任务的栈空间所需的内存，==均由FreeRTOS从FreeRTOS管理的堆中分配==。（2）==静态方式创建==（xTaskCreateStatic）———任务的任务控制块以及任务的栈空间所需的内存，==需要用户分配提供。==任务删除函数（xTaskToDelete）待删除任务的任务句柄，用于删除已被创键的任务，被删除的任务将从就绪态任务列表、阻塞态任务列表、挂起态任务列表和事件列表中移除。

      静态创建任务流程：（1）使用静态创建任务，需要将宏configSUPPORT_STATIC_ALLOCATION配置成1
                                        （2）定义空闲任务和定时器任务的任务堆栈及TCB
                                        （3）实现两个接口函数vApplicationGetldleTaskMemory()、vApplicationGetTimerTaskMemory()
                                        （4） 编写任务函数

      删除任务流程：        （1）使用删除任务函数，需要将宏INCLUDE_vTask Delete配置为1 

      ​                                   （2）入口参数输入需要删除的任务句柄（NULL代表删除本身）。删除任务自身，需要先添加等待删除列表，内     存释放将放在空闲任务执行 ，空闲任务会负责释放被删除任务中由系统分配的内存，但是由用户在任务删除前申请的内存，则需要由用户在任务被删除前提前释放，否则会导致内存泄漏。

4. ## 任务的挂起与恢复的API函数

  ​      三个API函数：（1）vTaskSuspend()                     ———挂起任务    
  ​                               （2）vTaskResume()                      ———恢复被挂起的任务
  ​                               （3）vTaskResumeFromISR()       ———在中断中恢复被挂起的任务

##### [挂起：挂起的任务类似暂停，可恢复（解挂）；删除任务，无法恢复。被挂起的任务绝不会得到CPU的使用权，不管该任务具有什么优先级。vTaskSuspend() 函数调用时需要将宏INCLUDE_vTaskSuspend() 配置成1。](#context.1)<a name="section.1"> </a>

##### [恢复：恢复被挂起的任务。==任务恢复就是让挂起的任务重新进入就绪状态，恢复的任务会保留挂起前的状态信息，在恢复时根据挂起时的状态继续运行。vTaskResume() 函数调用时需要将宏INCLUDE_vTaskResume() 配置成1==](#context.2)<a name="section.2"> </a>

###### [FromISR：==带FromISR后缀是在中断函数中专用的API==。无论调用过多少次的任务挂起vTaskSuspend() ，只需要调用一次vTaskResumeFromISR()  即可解挂。==使用该函数时，需要在FreeRTOSconfig.h中把INCLUDE_vTaskSuspend()和INCLUDE_vTaskResumeFromISR()  都定义为1。==vTaskResumeFromISR()函数不能用于任务和中断的同步（因为中断随时可能出现）。](#context.3)<a name="section.3"> </a>

  ​    vTaskSuspend(TaskHandle_t  vTaskSuspend) 形参为 vTaskSuspend，待挂起任务的任务句柄。注意：当传入的参数为NULL，则代表挂起任务自身（当前运行的任务）。
  ​    vTaskResume(TaskHandle_t  vTaskResume) 形参为vTaskResume，恢复指定任务的任务句柄。注意：任务无论被挂起多少次，只需要调用vTaskResume()恢复一次，就可以继续运行。且被恢复的任务进入到就绪态。
    ==BaseType_t== xTaskResumeFromISR(TaskHandle_t  xTaskResume)  被标记的为函数的返回值类型，形参为xTaskResume，待恢复的任务句柄。返回值类型有两种：①pdTRUE ———任务恢复后需要进行任务切换 （==进行任务切换的情况可能为进行抢占式调度，当前恢复的任务的优先级大于正在执行的任务的优先级==）②pdFALSE———任务恢复后不需要进行任务切换。==注意：中断服务程序中要调用FreeRTOS的API函数则中断优先级不能超过FreeRTOS所管理的最高优先级。==

  ​               ==小知识点补充：任务优先级是数值越大优先级越高，中断优先级是数值越小优先级越高。==

5. ## 中断管理

​           中断:让CPU打断正常运行的程序，转而去处理紧急的事件。中断属于异步异常。

​           异常：导致处理器脱离正常运行转向执行特殊代码的任何事件，如果不及时处理，轻则系统出错，重则导致系统瘫痪。异常分类： 同步异常和异步异常。有内部事件引起的异常叫做同步异常。异步异常主要指由外部异常源产生的异常。

​          中断执行机制：1.中断请求：外设产生中断请求（GPIO外部中断、定时器中断等）2.响应中断：CPU停止执行当前程序，转而去执行中断处理程序（ISR）3.退出中断：执行完毕，返回被打断的程序处，继续执行。

​         中断优先级分组设置：1.低于configMAX_SYSCALL_INTERRUPT_PRIORITY优先级的中断里才允许调用FreeRTOS的API函数
​                                                2.建议将所有优先级位指定为抢占优先级位，方便FreeRTOS管理。（调用函数HAL_NVIC_SePriorityGrouping(NVIC_PRIORITYGROUP_4）

​     三个系统中断优先级配置寄存器，分别为SHPR1、SHPR2、SHPR3，通过SHPR3将PendSV 和SysTick的中断优先级设置成最低优先级，保证系统任务切换不会阻塞系统其他中断的响应。

​    三个中断屏蔽寄存器，分别为PRIMASK、FAULTMASK、BASEPRI。FreeRTOS所使用的中断管理就是利用的BASEPRI寄存器，BASEPRI：屏蔽优先级低于某一个阈值的中断。

补充知识点：PendSV**定义：**可悬起异常，如果我们把它配置为最低[优先级](https://so.csdn.net/so/search?q=优先级&spm=1001.2101.3001.7020)，那么如果同时有多个异常被触发，他会再其他异常执行完毕后再执行，而且任何异常都可以打断它。systick**定义**（就是一个基于M3、M4内核的一个简单的24bit,倒计时,自动重装载定时器，倒计时结束会产生一个中断。常用于做延时，在实时系统中做心跳时钟，用于任务的切换。

​                                     1、systick定时器，就是系统滴答定时器

​                                    2、24位自动重装载倒计时定时器

​                                    3、当倒计时到0时，将从RELOAD寄存器中自动重装载初值

​                                   4、只要不清除SysTick控制及状态寄存器的使能位，就永不停息，在睡眠模式下也能工作

​                                    5、systick倒计时结束，会产生一个中断，且中断优先级可以设置

7. ## 临界段代码保护

 ==临界段代码也叫做临界区，是指那些必须完整运行，不能被打断的代码段。==适用场合：①外设：需要严格按照时序初始化的外设；②系统：系统自身；③用户自身

FreeRTOS在进入临界段代码的时候需要关闭中断，当处理完临界段代码以后再打开中断。

临界段代码保护函数：
                              ①taskENTER_CRITICAL() ———任务级进入临界段（关中断）
                              ②taskEXIT_CRITICAL() ———任务级退出临界段（开中断）
                              ③taskENTER_CRITICAL_FROM_ISR() ———中断级进入临界段（关中断）
                              ④taskEXIT_CRITICAL_FROM_ISR() ———中断级退出临界段（开中断）

8. ## 任务调度器的挂起和恢复

​       挂起任务调度器，调用此函数不需要关闭中断。

函数：①vTaskSuspendAll()  挂起任务调度器
           ②xTaskResumeAII()   恢复任务调度器

注意   1.与临界区不一样的是，挂起任务调度器，不需要关闭中断
           2.它仅仅是防止任务之间的资源争夺，中断照样可以直接响应；
           3.挂起调度器的方式，适用于临界区位于任务于任务之间，既不用去延时中断，又可以做到临界区的安全。

9. ## FreeRTOS的列表和列表项

列表是FreeRTOS中的一个数据结构，概念上和链表类似，列表用来跟踪FreeRTOS中的任务。列表项就是放在列表中的项目。

列表根节点定义：
`typedef struct xLIST`

`{UBaseType_t uxNumberOfItems;`//用于定于链表的节点计数器。用于表示该链表下有多少个节点，根节点除外。
   `ListItem_t * pxIndex;`//链表节点索引指针，用于遍历节点。
`MinListItem_t xListEnd;`//链表的最后一个节点。
`};`

列表项：

​      `struct xLIST_ITEM`
`{`
​         `listFIRST_LIST_ITEM_INTEGRITY_CHECK_VALUE        用于检测列表项的数据完整（未用到可以不看）`

​        `configLIST_VOLATILE_TickType_t xltemValue          列表项的值`
​        `struct xLIST_ITEM\*configLIST_VOLATILE  pxNext     下一个列表项`
​        `struct xLIST_ITEM\*configLIST_VOLATILE  pxPrevious    上一个列表项`
​        `void* pvOwner                                                                 列表项的拥有者`
​        `struct xLIST_ITEM*configLIST_VOLATILE  pxContainer          列表项所在列表`
​        `listSECOND_LIST_ITEM_INTEGRITY_CHECK_VALUE                 用于检测列表项的数据完整性（未用到可以不看）`

`};`

（1）成员变量xltemValue为列表项的值
（2）成员变量pxNext 、 pxPrevious表示列表中列表项下一个列表项和上一个列表项
（3）成员变量pvOwner用于指向包含列表项的对象
（4）成员变量pxContainer用于指向列表项所在的列表

迷你列表项：也是列表项，但迷你列表项仅用于标记列表的末尾和挂载在其他插入列表中的列表项。

      struct xMINI_LIST_ITEM
`{`
        `listFIRST_LIST_ITEM_INTEGRITY_CHECK_VALUE         用于检测列表项的数据完整（未用到可以不看)`
        `configLIST_VOLATILE_TickType_t xltemValue            列表项的值 `

`           struct xLIST_ITEM\*configLIST_VOLATILE  pxNext     下一个列表项`
          `struct xLIST_ITEM\*configLIST_VOLATILE  pxPrevious    上一个列表项`

`}`;

（1）成员变量xltemValue为列表项的值，这个值多用于按升序对列表中的列表项进行排序
（2）成员变量pxNext 、 pxPrevious表示列表中列表项下一个列表项和上一个列表项
（3）迷你列表项仅用于标记列表的末尾和挂载在其他插入列表中的列表项，因此不需要成员变量pvOwner和pxContainer，以节省内存开销。

10. ## 任务切换

任务切换的实质：就是CPU寄存器的切换。

 假设由任务A切换到任务B，主要分成两步：①需要暂停当前任务A的执行，并将此时任务A的寄存器保存到任务堆栈中，这个过程叫做任保存现场（压栈）。②将任务B的各个寄存器的值（被存于任务堆栈中）恢复到CPU寄存器中，这个过程叫做恢复现场（出栈）。对任务A的保存现场，任务B的恢复现场也叫做上下文切换。
注意：任务切换的过程在PendSV()中断服务函数里边完成。
             PendSV中断是如何触发的？（1）滴答定时器中断调用（2）执行FreeRTOS提供的相关API函数：portYIELD()。本质：通过向中断控制和状态寄存器ICSR（地址：0xE000_ED04）的bit写1挂起PendSV来启动PendSV中断。

11. ## 时间片调度

使用时间片调度需要把宏configUSE_TIME_SLICING和configUSE_PREEMPTION置1。

时间统计API函数——void vTaskGetRunTimeStats(char* pcWriteBuffer)   此函数用于统计任务的运行时间，使用此函数需要将宏configGENERATE_RUN_TIME_STAT、configUSE_STATS_FORMATTING_FUNCTIONS置1。pcWriteBuffer接受任务运行时间信息的缓存指针。
时间统计API函数使用流程
（1）将宏configGENERATE_RUN_TIME_STAT置1.
（2）将宏configUSE_STATS_FORMATTING_FUNCTIONS置1.
（3）将宏configGENERATE_RUN_TIME_STAT置1后 ，还需要实现两个宏定义：①portCONFIGURE_TIME_FOR_RUNTIME_STATE():用于初始化用于配置任务运行时间统计的时基定时器；②portGET_RUN_TIME_COUNTER_VALUE():用于获取该功能时基硬件定时器计数的计数值。
延时函数：
        相对延时（vTaskDelay()）：指每次延时都是从执行函数vTaskDelay()开始，直到延时指定时间结束
        绝对延时（vTaskDelayUntil()）:指将整个任务的运行周期看成一个整体，适用于需要按照一定频率运行的任务。                      

12. ## 队列

队列又称为消息队列，队列可以在任务与任务之间、中断与任务之间传递信息，实现了任务接收来自于其他任务或中断的不固定长的数据。 任务能够从队列中读取消息，当队列中的消息为空时，读取消息的任务将被阻塞。

FreeRTOS中的队列的特点：
 ①数据出入队方式：队列采用”先进先出“（FIFO）的数据存储缓冲机制，即先入队的数据会先从队列出来。 
 ②数据传递方式： FreeRTOS中队列采用实际值传递，即将数据拷贝到队列中进行传递，FreeRTOS采用拷贝数据传递，也可以传递指针，所以在传达较大的数据的时候采用指针传递。
 ③多人任务访问：队列不属于任何某个认任务，任何任务和中断都可以向队列发送/读取消息。
 ④出队、入队阻塞：当任务向一个队列发送消息时，可以指定一个阻塞时间。
      入队阻塞：队列已满，此时写不进去数据。①将该任务的状态列表项挂载在pxDelayedTasksList()(阻塞任务列表);②将任务的事件列表项挂载在xTaskWaitingToSend()(等待发送列表)。
      出队阻塞：队列为空 ，此时读取不了数据。①将该任务的状态列表项挂载在pxDelayedTasksList()(阻塞任务列表)；②将任务的事件列表项挂载在xTasksWaitingToReceive(); 	

13. ## 信号量

信号量是一种解决同步问题的机制，可以实现对共享资源的有序访问。是一种实现任务间通信的机制，可以实现任务之间同步或临界资源的互斥访问。==信号量是一个非负的整数，==所有获取它的任务都会将整数减一，当该整数值为0时，所有试图获取它的任务都将处于阻塞态。通常一个信号量的计数值用对应有效的资源数，表示剩下的可被占用的互斥资源数。

==当计数值大于0时，代表有信号量资源；当释放信号量，信号量计数值加1；当任务获取信号量时，信号量计数值减1。==

队列和信号量的区别：

|                             队列                             |                            信号量                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| 可以容纳多个数据；创建队列有两部分内存：队列结构体（消息队列的控制块）、队列项存储空间（队头+队尾） | 仅存放计数值，无法存放其他数据；创建信号量，只需分配信号量的结构体 |
|                写入队列：当队列满时，可阻塞；                | 释放信号量：不可阻塞，计数值进行++；当计数值超过最大值时，返回失败 |
|             读取队列：当队列没有数据时，可阻塞；             |       获取信号量：计数值- -；当没有资源时，可以阻塞；        |

二值信号量：本质上就是一个队列长度为1的队列，该队列就只有空和满两种情况。通常用于互斥访问或任务同步，与互斥信号量比较类似，但是二值信号量有可能会导致优先级翻转问题，二值信号量更适合用于任务同步，互斥信号量更适合用于临界资源的访问。
==二值信号量和互斥信号量区别：互斥量有优先级继承机制，二值信号量没有这个机制。==
在FreeRTOS中，信号量用于同步，如任务与任务的同步、中断与任务的同步，可以大大提高效率。

计数型信号量：本质上相当于队列长度大于1的队列，因此计数型信号量能够容纳多个资源，这个是在计数型信号量创建时确定的。
计数型信号量适用场合：①事件计数：当每次事件发生后，在事件处理函数中释放计数型信号量（计数值加1），其他任务会获计数型信号量（计数值减1），这个场合一般在创建时将初始计数值设置为0；②资源管理：信号量表示有效的资源数目。任务必须先获取信号量（信号量数值减1）才能获取资源控制权。当计数值减到0时，表示没有资源。当任务用完资源后，必须释放信号量（计数值加1）。信号量创建时计数值应等于最大资源数目。























































