---
sort: 1
---

# 环境搭建

## 概要

机械臂SDK与机械臂控制器均基于Ubuntu18.04构建，同时在仿真控制中依赖的ROS系统为Melodic版本。故为了能更好地使用，请用户尽量之保持一致。如果仅使用实机操控，也无需安装ROS

除此之外，机械臂SDK与机械臂控制器还依赖了许多第三方工具，用户在使用前需要将这些第三方工具安装好。

为了方便用户使用，我们也在z1_sdk/thirdparty/目录下提供了Eigen及RBDL两个库包，解压后可以直接根据以下步骤进行安装。

## 依赖库安装

+ build-essential

```shell
sudo apt install build-essential
```

+ Boost (1.5.4版本 或 更高)

```shell
dpkg -S /usr/include/boost/version.hpp      # check boost version
sudo apt install libboost-dev               # install boost
```

+ CMake (2.8.3版本 或 更高)
  
```shell
cmake --version             # check cmake version
sudo apt install cmake      # install cmake
```

+ GCC(GLIBCXX 3.4.22版本或更高)

可以通过以下命令行查看当前GLIBCXX版本信息

```shell
strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep GLIBCXX
```

upgrade

```shell
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-4.9
sudo apt-get upgrade libstdc++6
```

+ Eigen (3.3.9版本)

如果已安装旧版本Eigen，可以先卸载，如果没有可以直接进行下一步安装。

卸载旧版本

```shell
cd /usr/include
sudo rm -rf ./eigen3
```

编译安装

```shell
cd eigen-3.3.9
mkdir build
cd build
cmake ..
sudo make install

sudo ln -s /usr/local/include/eigen3  /usr/include/eigen3
sudo ln -s /usr/local/include/eigen3/Eigen  /usr/local/include/Eigen
```

<!-- + LCM (1.4.0版本)

```shell
cd lcm-1.4.0
mkdir build && cd build
cmake ..
make
sudo make install
``` -->

+ RBDL (2.6.0版本，2022.10.21版本后无需安装)

```shell
cd rbdl-2.6.0
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=release ..
sudo make install

sudo su    (input password)
echo /usr/local/lib >> /etc/ld.so.conf  # set path
ldconfig
exit                                    # 退出超级管理员模式
```

## ROS(melodic)安装

参考ROS官方[melodic安装教程](http://wiki.ros.org/melodic/Installation/Ubuntu)

其中中国用户在使用在使用rosdep时可能会因为无法连接外网而发生错误，可以尝试通过国内小鱼ROS开发的rosdepc命令执行。

添加ROS源

```shell
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```

安装melodic桌面版ros-melodic-desktop-full

```shell
sudo apt update
sudo apt install ros-melodic-desktop-full
```

安装ros依赖

```shell
sudo apt-get install python3-pip 
sudo pip3 install rosdepc
sudo rosdepc init
rosdepc update
```

添加ros环境变量

```shell
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

安装结束

## 网络配置

### PC防冲突IP更改

机械臂的默认IP为192.168.123.110，用户需要在使用SDK之前更改PC的IP，使用192.168.123网段。

以更改为192.168.123.162示例，在终端中运行ifconfig，您将找到您的端口名称。例如，enpxxx.

```shell
sudo ifconfig enpxxx down        # enpxxx is your PC port 
sudo ifconfig enpxxx 192.168.123.162/24 
sudo ifconfig enpxxx up 
```

以上方式为临时更改IP使用，您也可以将永久更改PC的IP，操作如下

```shell
sudo vim /etc/network/interfaces
```

添加一下文本至上述打开的interfaces文件中

```shell
auto enpxxx
iface enpxxx inet static
address 192.168.123.162
netmask 255.255.255.0
```

此时可以通过ping 192.168.123.110指令检测与机械臂连接是否正常。

### 机械臂控制程序IP更改

如果用户更改了机械臂控制板IP，比如将原本的192.168.123.110改为192.168.123.111，
此时需要使控制程序z1_ctrl的IP与其保持一致。

打开z1_controller/config.xml文件，更改其中IP即可。

该文件下的另一个control参数用于改变机械臂控制方式，分别为键盘、SDK、以及两种狗的手柄控制，用户无需变动，只使用SDK即可。(即set = "2")

## 手爪配置

**注**：2022.10.21版本后无此配置

在config.xml有关于gripper的设置参数，如果选择1则末端无手爪，2则末端为机械臂自带手爪.选择3则可根据用户输入的参数自定义手爪。

该设置参数仅为程序内配置，实际机械臂有无手爪并不影响程序的执行，但当参数不一致z1_controller会发出警告。

同时我们允许用户根据user_gripper定义自己的手爪参数，其中endPosLocal为手爪坐标相对于关节5末端平面坐标系(0.049, 0, 0.1605)的相对坐标；mass为手爪质量，单位kg；com(center of mass)为手爪质心位置；I为惯量矩阵，默认均匀分布，故只设置了矩阵迹上的值。
