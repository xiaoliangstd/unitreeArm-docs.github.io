---
sort: 6
---

# 遥控器控制

## 控制模式

遥控器控制固定在机器狗上的机械臂，有三种控制模式。

1.关节空间控制。当处于此模式时，遥控器可控制机械臂单个关节转动。

2.笛卡尔空间控制。当处于此模式时，机械臂作为一个整体，遥控器控制机械臂在工具坐标系下沿XYZ轴方向移动以及绕XYZ轴转动。

3.固定轨迹运动。机械臂内置演示用的固定动作。

## 遥控器键位介绍

### 关节空间控制

当机器狗停止时，同时按住L1+L2键，切换到要控制控制机械臂，机器狗不再运动。此时按下R2键则进入关节空间控制模式，用遥控器可控制单个关节转动，对应键位如下图所示。运动结束后，按下R2可使机械臂回到原位。

<center>
<img src="../img/remote_joint control.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">关节空间遥控器控制</div>
</center>
<br>

### 笛卡尔空间控制

当机器狗停止时，同时按住L1+L2键，切换到要控制控制机械臂，机器狗不再运动。此时按下R1键则进入笛卡尔空间控制模式，或者当机械臂处于关节空间控制模式下，按下R1键位也可进入笛卡尔空间控制模式，对应键位如下图所示。运动结束后，按下R2可使机械臂回到原位。

<center>
<img src="../img/remote_cartesian control.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">笛卡尔空间遥控器控制</div>
</center>
<br>

<center>
<img src="../img/catesian_example.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">机械臂工具坐标系</div>
</center>
<br>

### 固定轨迹运动

当机器狗停止时，同时按住L1+L2键，切换到要控制控制机械臂，机器狗不再运动。此时按下R2键后，按SELECT键机械臂到固定位置，再按下SELECT机械臂回到初始位置，第三次按SELECT键机械臂会一直处于运动状态。

# 注意事项

由于机器狗和机械臂用同一个遥控器进行控制，存在部分键位共用，因此，控制机器狗移动时，要同时按下**R1+R2**将机械臂切入阻尼状态。判断机械臂是否在阻尼状态，可用手拖动机械臂，若能拖动则表明已切入阻尼。控制机械臂运动时，则需要机器狗处于停止状态。机械臂运动完成后，需要同时按下**L1+L2**键，遥控器可控制机器狗运动。

# 更新日志

+ 2022.8.29

    初稿完成。
    
