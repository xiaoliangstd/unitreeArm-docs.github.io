---
sort: 2
---

# 机械臂参数

## 关节及坐标系定义
<center>
<img src="../img/z1_arm_cooridinate.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">z1机械臂关节序号及关节转动正方向定义</div>
</center>
<br>

在开始控制前，我们有必要了解下机械臂关节的定义及转动的正方向定义。宇树Z1机械臂是六自由度机械臂，其各关节序号从J1开始，逐个递增至J6。在上图中`+`键表示关节转动的正方向，`-`键表示关节转动的负方向。了解转动的正方向对下文中的关节空间控制的使用很有帮助。

## 关节运动范围

|joint|min angle|max angle|
|:-:|:-:|:-:|
|J1|-150|150|
|J2|0|180|
|J3|-165|0|
|J4|-80|-80|
|J5|-85|85|
|J6|-160|160|
|gripper|0|90|

实际计算采用弧度制，还有其他如坐标轴、惯性矩阵等信息都在~/z1_ws/src/z1_ros/z1_description/xacro的文件里有描述。