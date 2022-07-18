---
sort: 3
---
## 控制模式
&emsp;&emsp;z1机械臂提供了多种上层控制模式，如关节空间控制、笛卡尔空间控制、MOVEJ、MOVEL等，理解这些控制模式使得用户在使用时能更好地知道如何给`z1_ws`发命令从而控制机械臂运动。
+ **关节空间控制**
<center>
<img src="../img/gazebo_ctrl4.gif" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">关节空间控制演示</div>
</center>
<br>

&emsp;&emsp;关节空间控制，故名思义，即用户直接地给定关节的角度命令，从而控制机械臂的运动，如上图所示例，该示例是机械臂依次转动J2、J3、夹爪的结果。但在实际场景中，我们可能更希望直接地给定机械臂的末端位姿，来控制机械臂的运动，这就是下文介绍的笛卡尔空间控制。

+ **笛卡尔空间控制**
<center>
<img src="../img/gazebo_catesian1.gif" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">笛卡尔空间控制演示</div>
</center>
<br>

<center>
<img src="../img/catesian_example.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">笛卡尔空间控制</div>
</center>
<br>

&emsp;&emsp;笛卡尔空间控制，即用户直接地给将期望的机械臂的末端姿态发给机械臂控制器`z1_ctrl`，`z1_ctrl`经过运动学、动力学解算后得到机械臂各关节应该到达的角度、角速度，然后将这些命令发送至机械臂，从而控制机械臂的运动。但在`z1_ctrl`中，功能严格意义上应该称为笛卡尔空间控制速度控制，即用户每次只给定机械臂末端位姿的增量。


+ **标签记录**
<center>
<img src="../img/test.png" style="zoom:90%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">标签记录演示</div>
</center>
<br>

&emsp;&emsp;`z1_ctrl`允许将机械臂某一时刻的各关节角度记录为一个标签，我们称此功能为 **标签记录**。如上图中，我们将该位置记录为标签"test"。记录的标签保存于savedArmStates.csv文件中。

+ **标签运行**
<center>
<img src="../img/gazebo_tostate.gif" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">标签运行演示</div>
</center>
<br>

&emsp;&emsp;`z1_ctrl`也允许用户给定一个标签来控制机械臂的运动，我们称此功能为 **标签运行**。`z1_ctrl`中也有许多有用的内置标签，如:`startFlat`、`forward`。上图为控制机械臂按`forward`、"test"标签依次运动的效果。


+ **MOVEJ**
<center>
<img src="../img/gazebo_moveJ.gif" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">MOVEJ演示</div>
</center>
<br>

&emsp;&emsp;在**笛卡尔空间控制**模式中，用户给定的是机械臂末端位姿的增量，而在**MOVEJ**中用户可以直接给定一个或多个期望的末端位姿，从而控制机械臂的运动。在该模式，虽然机械臂末端最终会到达给定的末端位姿，但其在一个末端位姿运动到下一个末端位姿的过程中，机械臂末端的运动轨迹未必是直线的，此与下文的**MOVEL**模式有别。


+ **MOVEL**
<center>
<img src="../img/moveLandmoveJ1.gif" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">MOVEL演示</div>
</center>
<br>

&emsp;&emsp;相信可以从上图中看出，**MOVEL**模式中，机械臂在一个末端位姿运动到下一个末端位姿的过程中，其末端的运动轨迹是直线的。