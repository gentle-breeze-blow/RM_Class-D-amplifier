### 简介

&nbsp;&nbsp;&nbsp;&nbsp;本项目的开发目的是为了实现一种高效率、低功耗的音频功率放大方案，以满足音频系统中对体积小、发热低以及音质良好的需求。本项目基于STM32G431CBT6和IR2104芯片，实现了数字音频信号的PWM调制及全桥功率放大输出功能。STM32G431CBT6具有高性能Cortex-M4内核、丰富的定时器资源以及高精度ADC等特性，最大主频高达170MHz,能够实现高分辨率PWM输出与音频信号采样处理；IR2104 具备高低侧驱动能力、自举电路支持及较高的开关速度，适用于构建高效率的功率驱动级，是较为优秀的解决方案。下图为模块的成品图展示。

![image3](./docs/image3.png)


***

### 开发工具

+ EDA工具：KiCAD 9.0
+ 编译工具链：riscv-none-embed-gcc 8.2.0
+ 集成开发工具：Visual Studio Code
+ 烧录工具： Visual Studio Code

***

### 目录结构

+ circuit：基于KiCAD的原理图及PCB设计

+ docs：README相关的图片及文档

+ progam：程序源码


***

### 电路设计

#### 原理图设计要点

&nbsp;&nbsp;&nbsp;&nbsp;USB HUB电路设计：由于整车需要至少2路CAN总线来保证电机回传数据包的完整性，所以我们采用了GL850G作为USB HUB芯片实现USB一拖四的方案。另外，我们还通过使用施密特触发器来实现上电时序，保证USB设备枚举的顺序在每次上电后都是一致的。

![usb_hub](https://raw.githubusercontent.com/rm-controls/rm_usb2can/main/image/usb_hub.png)

&nbsp;&nbsp;&nbsp;&nbsp;USB转CAN电路设计：用STM32F072CBT6实现USB转CAN功能。CAN电平转换芯片采用MAX3051EKA芯片，该芯片使用3.3V供电，且封装为SOT23-8 (Small Outline Transistor)，为PCB小型化提供了基础。图中的R6、R7电阻用途为更改STM32的Boot模式，从而使STM32能够通过更改R6、R7的短接模式而在DFU (Device Firmware Upgrade) 烧录模式与Flash模式下切换，方便烧录固件。

![stm32_can](https://raw.githubusercontent.com/rm-controls/rm_usb2can/main/image/stm32_can.png)

#### PCB布局布线要点

1. 注意分地处理，分出功率地，模拟地，数字地三块地。
2. 电感底部挖空不铺铜，减少电磁干扰。
3. 晶振底部最好挖空不铺铜,周围做包地处理。
4. 电源线线宽应尽量大于0.3mm，建议采用铺铜的方式。
5. IR2104芯片输出电流达几百mA，走线也需适度加粗。



**组织：DynamicX 
维护人：俞增洋, 1737922498@qq.com**