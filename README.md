# 原作者开源地址

[STM32H7的花式玩转SPI Flash章节也更新了，含MDK下载算法制作和STM32CubeProg下载算法制作](https://www.armbbs.cn/forum.php?mod=viewthread&tid=101225&fromuid=58)

基于原作者的工程进行轻量化处理，并适配手头已有的QSPI Flash MT25QL256，在此对原作者表示感谢！省去大量的重复造轮子时间。

# 如何使用？
## 0.准备软件

STM32CubeProgrammer

Keil 5

## 1. clone工程
使用github的 `Code-Download zip`或者
```
git clone https://github.com/vrxiaojie/External_Loader_MT25QL256.git
```

## 2. 使用Keil打开工程
进入`Project->MDK-ARM(AC5)`，使用Keil打开project.uvprojx文件

## 3. 选择你的设备
例如我的是STM32H743ZGT6，就在如下地方搜索并切换到该设备
![image](https://github.com/user-attachments/assets/c5be38dc-ea16-40d7-b8f3-6cf4ec4cea66)

## 4.修改源文件
**修改bsp_qspi_MT25QL256.c文件**
### 4.1 配置命令
如果你的Flash与我的不一致，需要根据Flash数据手册，修改读取、写入、擦除、初始化等操作的指令线、地址线、数据线个数以及空周期数量。

例如，我要修改`QSPI_EraseSector`，则打开数据手册，找到其扇区擦除的命令0xDC对应的表格，如下图：

![image](https://github.com/user-attachments/assets/347d6257-c132-48bb-8e7b-df8fe3e5b1be)

根据表格，对应修改代码

![image](https://github.com/user-attachments/assets/2af43b01-159e-41aa-b8c3-9f96a9d4a58f)

其他函数同理。

### 4.2 修改GPIO引脚
对应板子上的GPIO引脚，挨个配置QSPI的6个引脚
![image](https://github.com/user-attachments/assets/2a7f7f4b-c3c5-4e19-b60b-ffe873c8063c)

## 5. 编译
点击Rebuild按钮，正常来讲若没有报错，你能在Project文件夹内找到`STM32H7x_QSPI_MT25QL256.stldr`文件。

## 6. 复制文件
将`STM32H7x_QSPI_MT25QL256.stldr`文件复制到`C:\Program Files\STMicroelectronics\STM32Cube\STM32CubeProgrammer\bin\ExternalLoader`文件夹下

## 7.测试效果
打开STM32CubeProgrammer软件，连接ST-Link。

按下图所示操作，勾选STM32H7x_QSPI_MT25QL256

![image](https://github.com/user-attachments/assets/6a070558-2a7d-4330-a2a1-85681e5d2afe)


读取测试
![image](https://github.com/user-attachments/assets/c43a768d-354d-418a-8cf1-5e0ca01b72cb)

扇区擦除测试
![image](https://github.com/user-attachments/assets/07bcfc7b-6f58-4254-b875-fe4b6b5e257e)
可以看到刚刚全为0x00的区域都被擦除成了0xFF
![image](https://github.com/user-attachments/assets/0a0b792d-c3c5-4dd2-b0da-5a7d6c6f465c)


