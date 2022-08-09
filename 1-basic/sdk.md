---
sort: 1
---

# 机械臂SDK介绍

## 文件结构

所有关于机械臂SDK的文件会放在一个名叫z1_sdk_20xx.x.x.zip的压缩包,**Z1_sdk**是SDK的名字，**20xx.x.x**是该SDK的发布日期。
该压缩包中有三个子文件夹：z1_controller, z1_sdk与z1_ws，其中z1_ws属于ROS系统的一个工作空间。z1_controller存储着直接控制机械臂的源码，这些被封装成了库，用户是不可见的。
z1_sdk则是关于机械臂sdk `unitree_arm_sdk`的文件夹，包含了用于控制机械臂的一些接口。

在z1_controller文件夹中，用户关注的只需是CMakeLists.txt文件，根据需求更改选择ROS、UDP。

在z1_sdk文件夹中包含许多子文件夹，其作用分别为：

+ build:&emsp;为`unitree_arm_sdk`编译时储存可执行文件和中间文件的文件夹。（压缩包内不包含，需用户创建）
+ CMakeLists.txt:&emsp;为指导`unitree_arm_sdk`编译的文件，如果用户自己编写可执行文件需在其中添加路径。
+ examples:&emsp;存放着控制机械臂的例子的源码文件。
+ include:&emsp;存放着`unitree_arm_sdk`源码对应的头文件。
用户使用时只需在源文件中包含unitree_arm_sdk/unitree_arm_sdk.h文件即可
+ unitreeArm:&emsp;存放着unitreeArm类的源码文件，该类中有工程师为了用户能方便地调用机械臂API而封装的方法。该类与demo文件夹下的例子息息相关。用户也可借鉴该类代码编写自己的类。

## examples

我们提供了几个示例方便用户使用SDK。具体请查看unitreeArm类注释

### 1. example_keyboard_send

用户可以通过键盘发送指令用以控制机械臂，具体键位与指令的对应关系在[有限状态机](1-basics/FSM.md)小节具体介绍。

### 2. example_lowcmd_send

该文件展示了如何对电机进行直接发送pd参数。仿真参数和实际参数有所不同。

### 3. bigDemo

bigDemo通过对unitreeArm类继承，编写了一个对类中的成员函数进行调用示例。

### 4. getJointGripperState

该执行文件实时打印机械臂末端位姿，方便用户记录点位