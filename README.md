# MYFOC

## 配置 `MC workbench` 环境

生成 `STM32 MC workbench` + `STM32CUBEMX` + `KEIL` 的工程
- [创建MCSDK 6.30工程_mcsdk6-CSDN博客](https://blog.csdn.net/wlwx66/article/details/142472315)

在这其中遇到的工具相关问题
- 使用 `STM32 MC workbench` 生成时出错
	- [Attr not supported : Dmpu](https://blog.csdn.net/m0_46660770/article/details/143916220)
	- [MC workbench 生成工程报错 PDSC version is not supported解决办法](https://blog.csdn.net/qq_41839588/article/details/137562932)

- 编译错误
	- 编译器版本对不上
		- [Keil MDK5.37以上版本自行添加AC5(ARMCC)编译器的方法](https://blog.csdn.net/qcmyqcmy/article/details/125814461)
	- 使用 `arm_math.h` 这个头文件找不到，需要在 `KEIL` 中设置 `DSP` 支持
		- [2022-02-28 keil中include“arm_math.h“的问题_keil中include math.h](https://blog.csdn.net/Vissence/article/details/123181599)

- 下载失败
	- 可能是编译错误，根本就没编译成功
	- 确认编译成功后，还是提示 `Error: Flash Download failed`
		- [Error: Flash Download failed - “Cortex-M4“ keil verify failed](https://blog.csdn.net/m0_46660770/article/details/139323890?spm=1001.2014.3001.5502)
	- 生成的 `target` 名称对不上
		- [KEIL5工程不能编译和下载，运行时提示找不到.axf文件 Error: Flash Download failed - Could not load file.axf](https://blog.csdn.net/weixin_43716668/article/details/128952277)


## 电机参数设置

在这一步的设置通常需要得到电机的型号，然后去找淘宝商家或者网上的资料获取你买的电机参数

电机磁结构 `SM-PMSM` 和 `I-PMSM` 的区别于永磁体的安装位置和磁路结构
-  `SM-PMSM` 的永磁体安装在转子表面，通常通过胶粘或机械固定，也就是一般常见的小家电和航模中的无刷电机，磁路结构简单，用普通的矢量 $FOC$ 控制即可
- `I-PMSM` 的永磁体嵌入转子内部，通常位于转子铁芯的槽中，磁路结构复杂比较难控制

电机的槽极数 $\displaystyle yPxN$
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

参考链接
- [【电机参数】直流无刷电机2804电机参数测量](https://blog.csdn.net/qq_42681425/article/details/134489649)
- [12位可编程非接触式电位计（AS5600）的介绍](https://blog.csdn.net/mftang/article/details/144993217)
