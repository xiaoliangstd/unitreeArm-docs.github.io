---
sort: 1
---

# 机械臂SDK介绍

## 文件结构

所有关于机械臂SDK的文件会放在一个名叫z1_sdk_20xx.x.x.zip的压缩包,**Z1_sdk**是SDK的名字，**20xx.x.x**是该SDK的发布日期。
该压缩包中有三个子文件夹：z1_controller, z1_sdk与z1_ws，其中z1_ws属于ROS系统的一个工作空间。z1_controller存储着直接控制机械臂的源码，这些被封装成了库，用户是不可见的。
z1_sdk则是关于机械臂sdk `unitree_arm_sdk`的文件夹，包含了用于控制机械臂的一些接口。

在z1_controller文件夹中，用户关注的只需是CMakeLists.txt文件（根据需求更改选择ROS、UDP）和config.xml（根据需求更改配置信息）。

在z1_sdk文件夹中包含许多子文件夹，其作用分别为：

+ build:&emsp;为`unitree_arm_sdk`编译时储存可执行文件和中间文件的文件夹。（压缩包内不包含，需用户创建）
+ CMakeLists.txt:&emsp;为指导`unitree_arm_sdk`编译的文件，如果用户自己编写可执行文件需在其中添加路径。
+ examples:&emsp;存放着控制机械臂的例子的源码文件。
+ include:&emsp;存放着`unitree_arm_sdk`源码对应的头文件。
用户使用时只需在源文件中包含unitree_arm_sdk/unitree_arm_sdk.h文件即可
+ control：&emsp;其中包含CtrlComponents（程序配置参数）以及unitreeArm:该类中有工程师为了用户能方便地调用机械臂API而封装的方法。该类与demo文件夹下的例子息息相关。用户也可借鉴该类代码编写自己的类。

## examples

我们提供了几个示例方便用户使用SDK。具体请查看unitreeArm类注释

### 1. example_lowcmd_send

该文件展示了如何对电机进行直接发送pd参数。用户可以通过编写自己的机械臂程序对电机直接进行控制，实现自主开发。

CtrlComponents下的sendRecvThread是调用unitreeArm的函数进行指令操作，如运行至forward视为一条指令，而运行lowcmd时建议采用自己定义的线程，执行run函数，run函数开始通过计算确定当前需要发给电机的命令，最后调用sendRecv发送udp报文。

**注意**：发给电机的实际kp=kp*25.6; 实际kd=kd*0.0128；

### 3. bigDemo

bigDemo通过对unitreeArm类继承，编写了一个对类中的成员函数进行调用示例。

其中在Z1ARM类中定义了四个函数，分别演示四种使用上层命令控制机械臂的方式。

```cpp
    void armCtrlByFSM();
    void armCtrlByTraj();
    void armCtrlTrackInJointCtrl();
    void armCtrlTrackInCartesian();
```

① void armCtrlByFSM()

以模拟键盘的方式状态机的切换，进而实现各种功能。例如通过MoveJ(posture)函数可以自动切换到MoveJ状态机并运行至指定姿态posture；

注：这种方式下MoveJ、MoveL、MoveC暂未引出速度定义接口。

② void armCtrlByTraj()

这个函数涉及两个状态机，**State_Trajectory** 和 **State_SetTraj**。

通过setTraj()函数自动切换至State_SetTraj（必须在关节空间控制下），并记录当前设定的TrajCmd，必须保证trajOrder连续，
其中该轨迹的起点是上一个轨迹的终点。退出State_SetTraj是将会记录所有轨迹，此时可以进入State_Trajectory，依次执行记录的轨迹。

如果在这过程中并未通过State_SetTraj设定新的轨迹，那么可以进入State_Trajectory继续执行记录的轨迹。

```text
注：由于SDK中状态机的切换是通过模拟键盘的方式，所以这两个状态机在键盘中也是有实际的对应键位，分别是“-”和“l”，
但实际上单纯通过键盘是无法进行任何操作，值得注意的是，如果用户之间在键盘中按下减号“-”键，机械臂会运行一个默认的演示动作。
同时如果bigdemo在运行至State_Trajectory下，由于命令已经发送，此时如果终止bigdemo程序，z1_ctrl仍会执行完毕当前轨迹。
```

③ void armCtrlTrackInJointCtrl()

这个函数运行在关节空间状态机下，存在一个track标志位，正常情况下该值为false，通过键盘控制时可以按键位去控制机械臂移动，但在编程时无法进行操作。
当设置track为true时，机械臂以当前jointCmd下的q和qd命令运行，直至track再次被设置为false。

④ void armCtrlTrackInCartesian()

该函数运行在笛卡尔空间状态机下，和armCtrlTrackInJointCtrl()同理，但跟随的命令从jointCmd的q和qd变为trajCmd.posture[0]。