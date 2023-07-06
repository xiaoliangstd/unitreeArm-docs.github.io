---
sort: 3
---

# SDK运行

## 实机控制

**①** 首先对机械臂进行通电（**摆放至零位**），并通过ping 192.168.123.110指令查看是否通信正常
（强调：网线需要插在左边**主网口**）

**②** 编译z1_controller，在该文件夹下创建build文件夹

```shell
mkdir build & cd build
cmake ..
make
```

**③** 执行`./z1_ctrl`

当执行该条命令后，终端会不断地打印`[WARNING] UDPPort::recv, unblock version, connect wit z1_sdk wait time out`语句，这是正常的，因为我们还没有启动机械臂SDK与机械臂控制器通信。

**④** 打开第二个终端，打开z1_sdk文件夹，创建build文件并编译

**⑤** 在Z1_SDK中执行`./highcmd_basic`

## ROS仿真

**①** 打开仿真

如果用户对ros文件不太熟悉，请在home下创建`unitree_ws/src`文件并将`unitree_ros`文件移动到`/home/username/unitree_ws/src/unitree_ros`
同时下载[unitree_legged_msgs](https://github.com/unitreerobotics/unitree_ros_to_real)放入`unitree_ws/src/`目录下

```shell
cd ~/unitree_ws                                             #打开该文件夹
catkin_make                                                 #初始化ROS工作空间
echo “source ~/unitree_ws/devel/setup.bash”>>~/.bashrc     #将ros路径添加到环境变量，可由pwd命令获取当前路径替换该路径
source ~/.bashrc                                            #更新环境变量
```

在终端执行`roslaunch unitree_gazebo z1.launch`，如果成功配置此时可以显示出gazebo的仿真界面。

可以通过选择`UnitreeGripperYN`配置仿真是否有无手爪（默认带手爪）。如

```shell
roslaunch unitree_gazebo z1.launch UnitreeGripperYN:=false
```

**②** 编译z1_controller，在该文件夹下创建build文件夹（打开第2个终端）

```shell
mkdir build & cd build
cmake ..
make
```

**③** 执行 `./sim_ctrl`

当执行该条命令后，终端会不断地打印`[WARNING] UDPPort::recv, unblock version, connect wit z1_sdk wait time out`语句，这是正常的，因为我们还没有启动机械臂SDK与机械臂控制器通信。

**各种信息都会在这个窗口打印，用户使用使请多观察此窗口内容。**

**④** 执行./highcmd_basic, 会执行一个示例动作

```shell
./highcmd_basic
```

此时我们已经完成仿真操作，整个流程为&emsp;**运行ROS-->运行z1_ctrl-->运行SDK实例**

## 多机控制

对于需要控制多个Z1机械臂的用户，SDK提供了lowcmd_multirobots示例代码。

**<1>** 打开z1_controller，设置编译条件为UDP， 复制z1_controller文件夹，如命名为z1_controller_111

**<2>** 打开z1_controller_111

**①** main.cpp

将L51的与SDK通信的端口设置为如下示例

```cpp
ctrlComp->cmdPanel = new ARMSDK(events, emptyAction, "127.0.0.1", 8074, 8073, 0.002);
```

**②** config.xml

更改IP为192.168.123.111, 更改端口为8882

**③** unitreeArmTools.py

更改第二台机械臂的IP为192.168.123.111

```txt
如果使用两个USB转网口与机械臂通信，而非使用交换机组网，那么两个机械臂必须设置在不同网段。

如第一个在192.168.123.110, 第二个在192.168.120.110
```

**<3>** 根据之前所述，进行如下操作

1. 连接实体机械臂110并在第一个终端执行z1_controller下的z1_ctrl
2. 连接实体机械臂111并在第二个终端执行在z1_controller_111的z1_ctrl
3. 在第三个终端运行z1_sdk中的lowcmd_multirobots

此时可以看到两个机械臂的第一个关节都转动了一定角度。
