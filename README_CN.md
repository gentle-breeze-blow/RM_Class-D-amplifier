### 简介

&nbsp;&nbsp;&nbsp;&nbsp;本项目的开发目的是为了实现一种高效率、低功耗的音频功率放大方案，以满足音频系统中对体积小、发热低以及音质良好的需求。本项目基于STM32G431CBT6和IR2104芯片，实现了数字音频信号的PWM调制及全桥功率放大输出功能。STM32G431CBT6具有高性能Cortex-M4内核、丰富的定时器资源以及高精度ADC等特性，最大主频高达170MHz,能够实现高分辨率PWM输出与音频信号采样处理；IR2104 具备高低侧驱动能力、自举电路支持及较高的开关速度，适用于构建高效率的功率驱动级，是较为优秀的解决方案。下图为模块的成品图展示。

![image1](./docs/image1.jpg)

***

### 开发工具

+ EDA工具：KiCAD 7.0.7 (VC++ 1936, 64bit)
+ 编译工具链：riscv-none-embed-gcc 8.2.0
+ 调试工具：OpenOCD 0.11.0+dev-gfad123a16- (2023-05-05-13:43)
+ 集成开发工具：Visual Studio Code
+ 烧录工具： STM32CubeProgrammer v2.6.0

***

### 目录结构

+ circuit：基于KiCAD的原理图及PCB设计

+ docs：README相关的图片及文档

+ progam：程序源码

**注意：生产制造相关的Gerber、BOM、POS文件及程序固件均在仓库的Release中发布**

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

***

### 固件烧录

&nbsp;&nbsp;&nbsp;&nbsp;你可以[点击此处](https://github.com/rm-controls/rm_usb2can/releases/download/firmware_v1_0/candleLight.bin)下载已经编译好的固件，或者是跟随[candleLight文档](https://github.com/candle-usb/candleLight_fw/tree/master#building)自行编译固件。最终你会得到.bin文件，我们使用[STM32CubeProgrammer](https://www.st.com/zh/development-tools/stm32cubeprog.html)来将固件通过USB烧录到STM32中。下面展示了下载固件的步骤：

1. 将STM32上BOOT0引脚的下拉电阻取下，焊接上上拉电阻。
2. 将板子连接到电脑的USB口中，打开STM32CubeProgrammer。
3. 在STM32CubeProgrammer中点击下图所示的三个按钮，搜寻可用的STM32设备。

![stm32_programer_1](https://raw.githubusercontent.com/rm-controls/rm_usb2can/main/image/stm32_programer_1.png)

4. 连接成功后点击“Read”按钮以读取固件，选择准备好的固件。

![stm32_programer_2](https://raw.githubusercontent.com/rm-controls/rm_usb2can/main/image/stm32_programer_2.png)

5. 点击“Download”以下载固件。下载成功后，可以看到成功的标识。

![stm32_programer_3](https://raw.githubusercontent.com/rm-controls/rm_usb2can/main/image/stm32_programer_3.png)

6. 将STM32上BOOT0引脚的上拉电阻取下，焊接上下拉电阻。此时再重新上电，固件将在STM32上开始运行。

***


### 许可证

源代码根据[LGPL-v3.0条款许可证](.//LICENSE)发布。

**组织：DynamicX <br>
维护人：朱彦臻, 2208213223@qq.com**

该产品已经在Ubuntu 18.04和20.04下进行了测试。这是一个研究代码，希望它经常更改，并且不承认任何特定用途的适用性。