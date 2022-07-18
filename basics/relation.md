## 关系
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