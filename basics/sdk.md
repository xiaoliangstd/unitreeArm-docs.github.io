---
sort: 1
---
## 机械臂SDK介绍
### 概要
&emsp;&emsp;宇树机械臂可以实现关节空间控制、笛卡尔空间控制等多种上层控制模式，还可以实现对底层关节电机的底层控制，用户可以基于此开发自己的控制算法。要实现上述控制则依赖于机械臂SDK的使用。目前通过机械臂SDK对机械臂的控制方式有两种：
+ **程序代码控制** \
程序代码控制方式即我们可以通过编写C++程序来调用机械臂开发接口来控制机械臂。
+ **键盘控制** \
键盘控制方式即我们可以通过键盘直接地控制机械臂。

机械臂SDK还支持对机械臂的实物控制和仿真控制，其中仿真控制使用的仿真器是Gazebo。



### 文件结构
&emsp;&emsp;所有关于机械臂SDK的文件会放在一个名叫z1_sdk_20xx.x.x.zip的压缩包,**Z1_sdk**是SDK的名字，**20xx.x.x**是该SDK的发布日期。该压缩包中有两个子文件夹：z1_sdk与z1_ws，其中z1_ws里存放着z1机械臂的控制器`z1_ctrl`，其属于ROS系统的一个工作空间。z1_sdk则是关于机械臂sdk `unitree_arm_sdk`的文件夹。打开z1_sdk文件夹可以发现里面有很多子文件夹:
<!-- ![z1_sdk_struc](../../img/z1_sdk_struc.png) -->
<center>
<img src="../img/z1_sdk_struc.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">z1_sdk文件夹内容</div>
</center>
<br>


&emsp;&emsp;其中**unitree_arm_sdk**子文件夹里便存放着机械臂sdk `unitree_arm_sdk`，其余子文件夹则为`unitree_arm_sdk`的依赖。打开**unitree_arm_sdk**文件夹发现其内也有许多子文件\子文件夹:
<!-- ![unitree_arm_sdk_struct](../../img/unitree_arm_sdk_struct.png)\ -->
<center>
<img src="../img/unitree_arm_sdk_struct.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">unitree_arm_sdk文件夹内容</div>
</center>
<br> 

其作用分别为：
+ build:&emsp;为`unitree_arm_sdk`编译时储存可执行文件和中间文件的文件夹。
+ CMakeLists.txt:&emsp;为指导`unitree_arm_sdk`编译的文件，通过修改其中的内容可以修改对机械臂的仿真控制或实物控制。
+ demo: &emsp;存放着用程序代码控制方式控制机械臂的例子的源码文件。
+ examples:&emsp;存放着用键盘控制方式控制机械臂的例子的源码文件。
+ include:&emsp;存放着`unitree_arm_sdk`源码对应的头文件。
+ src:&emsp;存放着`unitree_arm_sdk`底层运行逻辑的源码。
+ unitreeArm:&emsp;存放着unitreeArm类的源码文件，该类中有工程师为了使用者能方便地调用机械臂API而封装的方法。该类与demo文件夹下的例子息息相关。

### 关系
&emsp;&emsp;下面介绍一下`unitree_arm_sdk`与`z1_ctrl`与`机械臂`间的关系,这里`机械臂`指的是仿真中的机械臂或真实世界中的机械臂。这将有助于我们理解下文中介绍的启动对机械臂控制时输入的命令。
<!-- ![关系图](../../img/relationAbout_z1_ws_sdk.png) -->
<center>
<img src="../img/relationAbout_z1_ws_sdk.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">关系图</div>
</center>
<br>

如上图所示，在仿真控制时机械臂的控制器`z1_ctrl`通过ros系统对gazebo中的机械臂发布命令进行控制，在实物控制中则通过udp对实物机械臂发布命令进行控制，而机械臂相关的状态(如：关节信息、末端位姿等)则通过同样的方式返回至`z1_ctrl`。机械臂sdk`unitree_arm_sdk`通过udp将用户的控制命令(不管是上层控制命令还是底层控制命令)发送至`z1_ctrl`，同时`z1_ctrl`将机械臂返回的状态信息通过udp反馈至`unitree_arm_sdk`，用户通过读取相关结构体获取相关信息。\
&emsp;&emsp;故因为这三者存在上述所述关系，用户在实际使用时的启动流程应是：
1. 启动`机械臂`：如是实物控制，则检查机械部是否上电成功和设备灯是否闪烁。若是仿真控制，则检查是否启动`roslaunch z1_gazebo z1.launch`命令(下文介绍)。
2. 启动`z1_ctrl`:检查是否启动`z1_ctrl`。
3. 启动`unitree_arm_sdk`: 检查是否启动`unitree_arm_sdk`。        

            







