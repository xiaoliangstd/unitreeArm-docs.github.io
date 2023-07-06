---
sort: 1
---

# SDK介绍

z1机械臂共提供4个文件夹供用户使用，分别是z1_controller，z1_sdk以及unitree_ros, z1_ros。

+ [z1_controller](https://github.com/unitreerobotics/z1_controller) 存储着直接控制机械臂的源码。
+ [z1_sdk](https://github.com/unitreerobotics/z1_sdk) 包含了用于控制机械臂的一些接口，用户在创建自己的程序使用z1机械臂时需要包含该文件夹。
+ [unitree_ros](https://github.com/unitreerobotics/unitree_ros) 用于机械臂仿真，其中包含了宇树四足产品Go1, A1, Aliengo, Laikago和机械臂产品Z1的仿真文件。
+ [z1_ros](https://github.com/unitreerobotics/z1_ros/tree/noetic)（包含unitree_ros）, 提供noetic moveit1支持

**注意**：z1_ros中也包含`z1_controller`和`z1_sdk`， 提供的接口可以直接发送UDP结构体进行通信，与上述z1_sdk不兼容。

## 1. z1_controller

除以下提到的文件外，用户无需查看该文件夹下的其他文件。

### 1.1 build/z1_ctrl

用户创建build文件后编译程序，最终可执行程序名为z1_ctrl，每次用户使用机械臂时都需要执行该程序。

+ 调用`./z1_ctrl -v`可以常看当前机械臂版本信息，如果此时可以正常连接机械臂，也会显示下位机版本信息。
+ 调用`./z1_ctrl k`可以使用键盘控制机械臂运动
+ 直接调用`./z1_ctrl`需使用SDK控制机械臂

### 1.2 CMakeLists.txt

用户在使用时需要根据自身使用方式自行选择采用实际控制或仿真，只需注释掉不需要的一行即可。更改完毕后需重新编译一下程序。

```cmake
set(COMMUNICATION UDP)
# set(COMMUNICATION ROS)
```

### 1.3 unitreeArmTools

该文件用于更改机械臂IP（默认192.168.123.110）
使用方法：使用网线连接好机械臂和PC（**更改IP需连接右端备用网口**），终端执行`python3 unitreeArmTools.py`，按提示输入信息即可。

### 1.4 config/config.xml

该文件只在z1_ctrl启动时读取一次：

1. IP & Port

   **IP**：此为通过unitreeArmTools更改的机械臂IP，当机械臂IP更改后，需要更改此地址以便z1_ctrl和机械臂正常通信

   **Port**：机械臂的端口为8880，此项更改的是PC执行z1_ctrl时绑定的本机端口，默认8881，用于同一台PC控制多个机械臂

2. collision

    包含碰撞检测相关的设置

    **open**: 设置为Y或N选择是否打开碰撞检测

    **limitT**: 为进行检测时计算扭矩与反馈扭矩的差值检测阈值，单位为NM

    **load**: 挂载在机械臂末端的负载重量，单位kg，该值与open设置无关，会一直参与末端负载的动力学计算。

### 1.5 config/saveArmStates.csv

该文件存储**labelRun**可到达的点位，记录的数据为该位姿下关节坐标。
点位可通过**labelSave**创建，亦可手动修改文件输入。

## 2. z1_sdk

### 2.1 include

该文件夹存放着unitre_arm_sdk的头文件，用户可以查看其中注释了解函数作用

#### 2.1.1 unitree_arm_sdk/control

该文件夹下共两个文件，`ctrlComponents.h` 和 `unitreeArm.h`，其中ctrlComponents.h主要是将所有的控制参数放在了一个类中便于调用，unitreeArm封装了用于控制机械臂的所有接口，用户使用时只要查看该类中信息。

#### 2.1.2 unitree_arm_sdk/model

该文件夹下包含armModel类，内部包含z1机械臂的正逆运动学、逆动力学以及空间雅克比计算等函数。
通过在unitreeArm类下调用`_ctrlComp->armModel`使用各种方法。

### 2.2 examples

为方便用户使用，该文件夹下内含机械臂sdk的使用示例文件。

#### 2.2.1 highcmd_basic

如果用户希望**简单了解**如何使用机械臂，可以查看该示例。

该示例下共含三种使用机械臂的方式。

① armCtrlByFSM()

该方法直接调用unitreeArm中提供的函数运行机械臂，如MoveJ、MoveL、MoveC、backToStart等。
所有函数与通过`./z1_ctrl k`命令直接使用键盘控制时的效果一致。

② armCtrlInJointCtrl()

该方法相当于在`highcmd_development`的基础上再进一步封装，原本用户需要输入关节命令
$$q \And \dot{q}$$，现在只需输入希望电机运行的方向即可。
函数内直接会进行如下命令计算：  

$$\dot{q} = directions*\omega$$  
$$q_{k} = q_{k-1} + \dot{q}*\delta t$$

采用该方法时相当于在键盘控制时按下2键后通过键盘直接控制机械臂。

③ armCtrlInCartsian()

与上例类似，在笛卡尔空间下，原本需要用户控制空间速度旋量 $$V =[\omega \quad v]'$$ ，即`unitreeArm.twist`从而控制机械臂运转，该方法在此基础上进行了封装，用户直接控制希望机械臂末端运行的方向即可。
函数内直接会进行如下命令计算：
其中T是由R，p组成的齐次变换矩阵，[ω]为ω的反对称矩阵  

$$ posture_k = posture_{k-1}+posture_{\Delta}$$  
$$[\omega] = \log{(R_{k-1}^T R_k)}$$  
$$v=p_\Delta$$  

采用该方法时相当于在键盘控制时按下3键后通过键盘直接控制机械臂。

#### 2.2.2 highcmd_development

如果用户希望自行开发，通过**规划轨迹**运行机械臂，可以查看该示例。

当sendRecvThread->start()执行后，该线程会以500HZ的频率执行arm.sendRecv()函数。

关节下控制时，该函数会发送unitreeArm下的q, qd, gripperQ, gripperW给z1_controller,
笛卡尔下控制时，该函数会发送unitreeArm下的twist给z1_controller，用户只需不断更改这些参数即可控制机械臂。也可自己编写线程进行调用unitreeArm.sendRecv()进行通信。

#### 2.2.3 lowcmd_development

如果用户希望直接从**底层控制**电机的 $$q, \dot{q}, \tau_f, k_p, k_d$$ 参数，可以查看该示例。

该文件展示了如何对电机进行直接发送p, d参数。用户可以通过编写自己的机械臂程序对电机直接进行控制，实现自主开发。电机最终输出的力矩如下

$$ \tau = k_p * 25.6 * (q_d - q) + k_d * 0.0128 * (\dot{q_d} - \dot{q}) + \tau_f$$

25.6与0.0128为与电机通信协议中的缩放倍数。

CtrlComponents下的sendRecvThread是调用unitreeArm的函数进行指令操作，如运行至forward视为一条指令，而运行lowcmd时建议采用自己定义的线程，执行run函数，run函数开始通过计算确定当前需要发给电机的命令，最后调用sendRecv发送udp报文。

#### 2.2.4 lowcmd_multirobots

该程序提供了控制多个机械臂的示例。详见SDK运行

### 2.3 examples_py

这里面包含机械臂sdk的python版本接口。

接口设计在arm_python_interface.cpp文件中，其中已经包含的函数及变量可直接在python中使用，用户也可自行更改，详情查看[pybind11 documentation](https://pybind11.readthedocs.io/en/stable/)

**调用方式**:

编译完成后z1_sdk/lib中会生成一个名为unitree_arm_interface的库

在第一个终端运行`./z1_ctrl`

在z1_sdk/examples_py目录下打开第二个终端执行`python3 example_highcmd.py`
