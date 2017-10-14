---
title: SPI串行外设接口
date: 2017-10-14 22:08:59
tags:
---


## 基本概念

- Motorola首先在其MC68HCXX系列处理器上定义的

-  SPI(Serial Peripheral Interface--串行外设接口)总线系统是一种同步串行外设接口，它可以使MCU与各种外围设备以串行方式进行通信以交换信息。在主器件的移位脉冲下，数据按位传输，低位在前，高位在后。

- SPI总线可直接与各个厂家生产的多种标准外围器件相连，包FLASH、EEPROM、网络控制器、LCD显示驱动器、A/D转换器和MCU等。
<!-- more -->
- 该接口一般使用4条线： 
   - 串行时钟线（SCLK）
   - 主机输入/从机输出数据线MISO
   - 主机输出/从机输入数据线MOSI
   - 低电平有效的从机选择线NSS 
      *有的SPI接口芯片带有中断信号线INT、有的        SPI接口芯片没有主机输   出/从机输入数据线MOSI*
- SPI有三个寄存器分别为：控制寄存器SPCR，状态寄存器SPSR，数据寄存器SPDR。

## 接口信号

- **一主一从**

![这里写图片描述](http://img.blog.csdn.net/20170930112905812?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzA1MjY1NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

- **一主多从**

![这里写图片描述](http://img.blog.csdn.net/20170930112748297?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzA1MjY1NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
**MOSI** – 主器件数据输出，从器件数据输入
**MISO** – 主器件数据输入，从器件数据输出
**SCLK** –时钟信号，由主器件产生,最大为fPCLK/2，从模式频率最大为fCPU/2
**NSS** – 从器件使能信号，由主器件控制,有的IC会标注为***CS*** (Chip select)

- 在点对点的通信中，SPI接口不需要进行寻址操作，且为全双工通信，显得简单高效。在多个从器件的系统中，每个从器件需要独立的使能信号，硬件上比I2C系统要稍微复杂一些。

- SPI接口在内部硬件实际上是两个简单的**移位寄存器**，传输的数据为8位，在主器件产生的从器件使能信号和移位脉冲下，按位传输，高位在前，低位在后。如下图所示，在SCLK的上升沿上数据改变，同时一位数据被存入移位寄存器。

## 工作模式

**SPI有四种工作模式,各个工作模式的不同在于SCLK不同, 具体工作由CPOL,CPHA决定**
**CPOL:** (Clock Polarity),时钟极性
1. 当CPOL为0时,时钟空闲时电平为低;
2. 当CPOL为1时,时钟空闲时电平为高;
**CPHA:**(Clock Phase),时钟相位
1. 当CPHA为0时,时钟周期的上升沿采集数据，时钟周期的下降沿输出数据;
2. 当CPHA为1时,时钟周期的下降沿采集数据，时钟周期的上升沿输出数据;

CPOL和CPHA，分别都可以是0或时1，对应的四种组合时序图如下：

![这里写图片描述](http://img.blog.csdn.net/20170930113620862?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMzA1MjY1NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



## 优点

全双工通信，数据传输速度总体来说比I2C总线要快，速度可达到几Mbps。信号线少，协议简单，相对数据速率高。


## 缺点

没有指定的流控制，没有应答机制确认是否接收到数据。
