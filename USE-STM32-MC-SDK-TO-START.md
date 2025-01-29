# 从零开始使用 `STM32-MC-SDK` 工具链上手 FOC

## 配置 `MC workbench` 环境

生成 `STM32 MC workbench` + `STM32CUBEMX` + `keil` 的工程
- [创建MCSDK 6.30工程_mcsdk6-CSDN博客](https://blog.csdn.net/wlwx66/article/details/142472315)
- 其实在配置时只要选用合适的 $\displaystyle MCU$、功率驱动板还有电机模型即可
- 在这之后保存工程然后生成代码，会自动调用 `cubemx` 和 `keil` 生成工程代码
- 只需要打开生成好的 `keil` 编译下载即可
- 查看目前的硬件引脚资源只需要在 `MC workbench` 查看即可，后续除电机控制以外的其他功能添加可以使用 `cubemx` 添加
- 在连接 $\displaystyle MCU$ 时要注意 `MC MotorPilot` 的串口波特率和在 `MC workbench` 中设置的一样

在这其中遇到的工具相关问题
- 使用 `STM32 MC workbench` 生成时出错
	- [Attr not supported : Dmpu](https://blog.csdn.net/m0_46660770/article/details/143916220)
	- [MC workbench 生成工程报错 PDSC version is not supported解决办法](https://blog.csdn.net/qq_41839588/article/details/137562932)

- 编译错误
	- 编译器版本对不上
		- [Keil MDK5.37以上版本自行添加AC5(ARMCC)编译器的方法](https://blog.csdn.net/qcmyqcmy/article/details/125814461)
	- 使用 `arm_math.h` 这个头文件找不到，需要在 `keil` 中设置 $\displaystyle DSP$ 支持
		- [2022-02-28 keil中include“arm_math.h“的问题_keil中include math.h](https://blog.csdn.net/Vissence/article/details/123181599)
		- 注意并不需要立马在 `keil` 中进行设置，其实如果 `MC workbench` 生成的工程是正常的话，已经在项目成员中添加了 $\displaystyle CMSIS$ 的 $\displaystyle DSP$ 支持

- 下载失败
	- 可能是编译错误，根本就没编译成功
	- 确认编译成功后，还是提示 `Error: Flash Download failed`
		- [Error: Flash Download failed - “Cortex-M4“ keil verify failed](https://blog.csdn.net/m0_46660770/article/details/139323890?spm=1001.2014.3001.5502)
	- 生成的 `target` 名称对不上
		- [KEIL5工程不能编译和下载，运行时提示找不到.axf文件 Error: Flash Download failed - Could not load file.axf](https://blog.csdn.net/weixin_43716668/article/details/128952277)


- 在 `MC MotorPilot` 中连接失败
	- 注意波特率要一致
	- 连接后要按一下 $\displaystyle MCU$ 板子上的 $\displaystyle RESET$ 按钮重置一下 $\displaystyle MCU$ 才行，[参考连接](https://community.st.com/t5/stm32-mcus-motor-control/usb-com-interface-not-working-in-steval-spin3204-windows10/td-p/620934/page/2)

## 电机模型参数的设置

在这一步的设置通常需要得到电机的型号，然后去找淘宝商家或者网上的资料获取你买的电机参数

电机磁结构 `SM-PMSM` 和 `I-PMSM` 的区别于永磁体的安装位置和磁路结构
-  `SM-PMSM` 的永磁体安装在转子表面，通常通过胶粘或机械固定，也就是一般常见的小家电和航模中的无刷电机，磁路结构简单，用普通的矢量 $FOC$ 控制即可
- `I-PMSM` 的永磁体嵌入转子内部，通常位于转子铁芯的槽中，磁路结构复杂比较难控制

电机的槽极数 $\displaystyle xNyP$
- 极数用 $\displaystyle yP$ 表示，在 $\displaystyle FOC$ 中极对数就是 $\displaystyle \frac{y}{2}$ 

电机的最大许可电流和最大母线电压

绕线电阻、绕线电感和相电阻 $\displaystyle R_{S}$、相电感 $\displaystyle L_{S}$
- 在 `MC workbench` 中只用设置 $\displaystyle R_{S}$ 和 $\displaystyle L_{S}$

磁链常数 `Flux` 和反电动势常数的物理意义一样
- 但是要注意单位之间的换算
- 磁链常数的单位是韦伯 $\displaystyle \text{Wb}=\text{V}·\text{s}$
- 电动势常数的单位是一般是 $\displaystyle \frac{\text{V}}{\text{rpm}}$ 
- 其中的 $\displaystyle \text{rpm}$ 是每分钟转多少角度，要换算时注意分钟换成秒、角度有 $\displaystyle 2\pi$

电机的转动惯量和摩擦系数 $\displaystyle \mu N·m·s^{2}$


电机的霍尔效应传感器设置
- 传感器分布位置
- 安装电角度

电机的正交编码器的设置
- 机械旋转一圈得到的脉冲数，需要查看编码器的手册得到

## 参考链接

使用的开发板和驱动板引脚定义文档
- [NUCLEO-G474RE | Mbed](https://os.mbed.com/platforms/ST-Nucleo-G474RE/)
- [NUCLEO-G474RE - STM32 Nucleo-64 development board with STM32G474RE MCU, supports Arduino and ST morpho connectivity - STMicroelectronics](https://www.st.com/en/evaluation-tools/NUCLEO-G474RE.html)

电机参数设置参考
- [【电机参数】直流无刷电机2804电机参数测量](https://blog.csdn.net/qq_42681425/article/details/134489649)
- [12位可编程非接触式电位计（AS5600）的介绍](https://blog.csdn.net/mftang/article/details/144993217)

