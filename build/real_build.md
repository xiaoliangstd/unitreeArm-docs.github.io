---
sort: 1
---
# 实物安装

## 1. 安全说明

### 1.1 简介

机械臂使用人员应充分了解风险，使用前必须认真阅读本手册，严格遵守手册中的规范和要求。
宇树科技致力于提供卓越产品，但由于机械臂危险性大，即使一切操作按照手册说明使用，也不能保证不会造成人身和财产方面的损失，宇树科技对此不承担责任。

### 1.2 注意事项

1. 请务必按照本手册中的要求安装机械臂、连接线缆
2. 确保机械臂的活动范围内不会碰撞到人或其他物品，以免发生意外
3. 在使用前，需要专业的人员进行调试
4. 在使用SDK时，必须确保输入的参数和操作流程是正确的
5. 机械臂在运行过程中会产生热量，在运行或刚停止时，请不要触摸机械臂
6. 请注意机械臂运行速度，过快时务必小心
7. 机械臂使用结束后，请务必断电
8. 避免在潮湿或粉尘的环境下使用机械臂
9. 请务必将机械臂存放、安装到儿童碰不到的地方，以免发生危险

## 2. 硬件安装

### 2.1 硬件组成

+ 机械臂（图-1）
+ 电源适配器（图-2）
+ 网线（图-3）
+ 夹爪（选配）

### 2.2 技术规格

#### 2.2.1 产品参数

<table border="1">
    <tr>
        <td>产品名称</td><td colspan="2">Z1</td>
    </tr>
    <tr>
        <td>自由度</td><td colspan="2">6</td>
    </tr>
    <tr>
        <td>自重</td><td colspan="2">不超过4.5kg</td>
    </tr>
    <tr>
        <td><p>最大负载（air）</p>最大负载（pro）</td><td colspan="2"><p>2kg</p>3-5kg</td>
    </tr>
    <tr>
        <td>最大臂展</td><td colspan="2">740mm</td>
    </tr>
    <tr>
        <td>电源需求</td><td colspan="2">电压24V 电流>20A</td>
    </tr>
    <tr>
        <td>重复定位精度</td><td colspan="2">~0.1mm</td>
    </tr>
    <tr>
        <td rowspan="6">关节运动范围</td><td>J1</td><td>±150°</td>
    </tr>
    <tr>
        <td>J2</td><td>0-180°</td>
    </tr>
    <tr>
        <td>J3</td><td>-165°-0</td>
    </tr>
    <tr>
        <td>J4</td><td>±80°</td>
    </tr>
    <tr>
        <td>J5</td><td>±85°</td>
    </tr>
    <tr>
        <td>J6</td><td>±160°</td>
    </tr>
    <tr>
        <td>关节最大速度</td><td colspan="2">180°/s</td>
    </tr>
    <tr>
        <td>关节最大扭矩</td><td colspan="2">33 N·m</td>
    </tr>
    <tr>
        <td>峰值功率</td><td colspan="2">500W</td>
    </tr>
    <tr>
        <td>底座接口</td><td colspan="2">Ethernet</td>
    </tr>
    <tr>
        <td>材质</td><td colspan="2">铝合金</td>
    </tr>
    <tr>
        <td>用户控制系统</td><td colspan="2">Ubuntu</td>
    </tr>
</table>

#### 2.2.2 产品尺寸

### 2.3 安装

#### 2.3.1 安装环境

机械臂切勿在潮湿和粉尘环境中使用
安装务必检查螺栓是否拧紧

#### 2.3.2 安装 Z1 机械臂

## 机械臂固定

&emsp;&emsp;机械臂实物首先需要将机械臂牢牢固定。用户可根据机械臂底座孔位尺寸及真实环境自行设计安装台架，机械臂的固定台架不仅需要承受机械臂自身的重量，还要承受最大加速度运动时的瞬时动态作用力。机械臂使用4颗M6螺栓，用内六角扳手安装机械臂。底座安装图如下

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

## 机械臂上电

机械臂上电前需确保机械臂各关机处于零位，这是为了让控制算法中的理论零位与实际机械零位重合。机械臂零位姿态如下图，J1、J6关节缝隙两侧的线完全对应。
<center>
<img src="../img/arm_powerOn.png" style="zoom:100%" alt=" 图片不见了。。。 "/>
<br>
<div style="color:orange; border-bottom: 0.1px solid #d9d9d9;
display: inline-block;
color: #999;
padding: 1px;">机械臂上电零位连接图</div>
</center>
<br>