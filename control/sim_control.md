---
sort: 1
---

## 仿真控制
### 概要
&emsp;&emsp;在[关系](#关系)小节中我们介绍过`机械臂`、机械臂控制器`z1_ws`和机械臂SDK`unitree_arm_sdk`的关系，故在仿真控制中我们只需要依次启动它们即可，当然前提是用户已经将环境搭建起来并且机械臂SDK编译已经成功。在仿真控制中它们的启动方法为：
1. `机械臂`: &emsp; `roslaunch z1_gazebo z1.launch`
2. 机械臂控制器`z1_ws`: &emsp; `rosrun z1_controller z1_ctrl`
3. 机械臂SDK`unitree_arm_sdk`: &emsp; `./{相对应的可执行文件}`

同时别忘了，在[机械臂SDK介绍](#机械臂SDK介绍)中说的，我们可以对仿真机械臂控制或实物机械臂控制，故还需要修改机械臂控制器`z1_ws`中的编译指导文件`CMakeLists.txt`并重新编译。下面让我们跟随小试牛刀小节来实际尝试一下吧。

### 小试牛刀
&emsp;&emsp;在本小节中，会尽可能详细地带领大家一步一步尝试在仿真中对机械臂的控制。

**1.修改z1_ws的CMakeLists.txt文件** 
&emsp;到z1_ws/src/z1_controller路径下找到CMakeLists.txt文件，打开它，找到下图所式内容并按下图所示修改，然后保存该文件(Ctrl+s)
<center>
<img src="../img/virtual/virtual_CmakeLists.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">z1_ws的CMakeLists.txt文件设置(仿真控制)</div>
</center>
<br>

**2.重新编译z1_ws** 
&emsp;打开一个终端，并将工作路径切换至z1_ws/路径，然后按照    节方法重新编译。若没有编译成功，则需要解决问题，直至编译成功。


**3.启动仿真机械臂** 
&emsp;打开一个终端，输入命令`roslaunch z1_gazebo z1.launch`来启动仿真环境中的机械臂
<center>
<img src="../img/sim_control/sim_control_gazebo.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">仿真环境中的机械臂</div>
</center>
<br>
如上图所示，当弹出gazebo仿真器并正常显示机械臂，且该终端没有红色的错误提示信息即可认为此步骤执行成功。