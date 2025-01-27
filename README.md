# MYFOC

## 配置环境

`STM32 MC workbench` + `STM32CUBEMX` + `KEIL`
-  [使用ST的Motor Control Workbench6.2.0生成代码，并让电机跑起来_motorcontrol workbench-CSDN博客](https://blog.csdn.net/m0_46660770/article/details/139324823)

遇到的问题
- 使用 `STM32 MC workbench` 生成时出错
	- [【已解决】Attr not supported : Dmpu-CSDN博客](https://blog.csdn.net/m0_46660770/article/details/143916220)
	- [ST Motor Control Workbench生成工程报错PDSC version is not supported解决办法-CSDN博客](https://blog.csdn.net/qq_41839588/article/details/137562932)

- 编译错误
	- 编译器版本对不上
		- [Keil MDK5.37以上版本自行添加AC5(ARMCC)编译器的方法-CSDN博客](https://blog.csdn.net/qcmyqcmy/article/details/125814461)
	- 使用 `arm_math.h` 这个头文件找不到，需要在 `KEIL` 中设置 `DSP` 支持
		- [2022-02-28 keil中include“arm_math.h“的问题_keil中include math.h-CSDN博客](https://blog.csdn.net/Vissence/article/details/123181599)

- 下载失败
	- 可能是编译错误，根本就没编译成功
	- 确认编译成功后，还是提示 `Error: Flash Download failed`
		- [【已解决】Error: Flash Download failed - “Cortex-M4“_keil verify failed-CSDN博客](https://blog.csdn.net/m0_46660770/article/details/139323890?spm=1001.2014.3001.5502)
	- 生成的 `target` 名称对不上
		- [KEIL5工程不能编译和下载，运行时提示找不到.axf文件(Error: Flash Download failed - Could not load file“.axf“)_could not load file .axf-CSDN博客](https://blog.csdn.net/weixin_43716668/article/details/128952277)
