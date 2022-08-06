---
sort: 2
---

# 机械臂安装


## 机械臂运动范围

在安装机械臂时，要考虑机械臂的运动范围一面造成不必要的损失。
机械臂的运动范围如下图所示

<center>
<img src="../img/range.png" style="zoom:70%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;"></div>
</center>
<br>

各个关节的运动范围如下表所示

|关节|最小角度|最大角度|
|:-:|:-:|:-:|
|J1|-150°|150°|
|J2|0|180°|
|J3|-165°|0|
|J4|-80°|80°|
|J5|-85°|85°|
|J6|0|90°|

实际计算采用弧度制，还有其他如坐标轴、惯性矩阵等信息都在~/z1_ws/src/z1_ros/z1_description/xacro的文件里有描述。

## 安装机械臂

用户在固定机械臂时可根据机械臂底座孔位尺寸及真实环境自行设计安装台架，机械臂的固定台架不仅需要承受机械臂自身的重量，还要承受最大加速度运动时的瞬时动态作用力。
机械臂使用4颗M6螺栓，用内六角扳手安装机械臂。底座安装图如下

<center>
<img src="../img/arm_buttom.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">机械臂底座螺丝安装图</div>
</center>
<br>

当然，我们提供了固定板及G夹，方便直接固定到桌面上。
<center>
<img src="../img/arm_guding.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">机械臂安装图</div>
</center>
<br>

## 线缆连接

机械臂线缆主要有两种：供电线和通信线。机械臂供电线的接头具备防呆功能，供电线插入下图所示的机械臂供电接口即可。同时将通信线(即网线)的一端插入下图所示的机械臂网口并锁紧即可，通信线的另一端连接电脑。

<center>
<img src="../img/arm_xianglan.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">机械臂线缆连接图</div>
</center>
<br>

## 上电

机械臂重新上电前一定要**确保运动程序关闭**，否则会有危险发生的可能。并让机械臂处于零位，
机械臂零位姿态如下图，J1、J6关节缝隙两侧的线完全对应，其他关节摆放到位即可。

<center>
<img src="../img/arm_powerOn.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">机械臂上电零位连接图</div>
</center>
<br>

成功上电时，自检通过机械臂绿灯会常亮，蓝灯闪烁。

需注意，每次使用前将机械臂各关节转到零位，这是为了让控制算法中的理论零位与实际机械零位重合。另外，机械臂末端电机的黑白电源线是有DC24V电的，如果不需要，请务必把黑白电源线用绝缘胶带缠绕固定，防止短路等发生危险。

<center>
<img src="../img/z1_arm_cooridinate.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">z1机械臂关节序号及关节转动正方向定义</div>
</center>
<br>

在开始控制前，我们有必要了解下机械臂关节的定义及转动的正方向定义。宇树Z1机械臂是六自由度机械臂，其各关节序号从J1开始，逐个递增至J6。在上图中`+`键表示关节转动的正方向，`-`键表示关节转动的负方向。了解转动的正方向对之后的关节空间控制的使用很有帮助。