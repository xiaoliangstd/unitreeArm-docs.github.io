---
sort: 2
---
## 软件安装

### 概要
&emsp;&emsp;机械臂SDK与机械臂控制器均基于Ubuntu18.04构建，同时在仿真控制中依赖的ROS系统为Melodic版本。故为了能更好地使用，请用户尽量之保持一致。除此之外，机械臂SDK与机械臂控制器还依赖了许多第三方工具，用户在使用前需要将这些第三方工具安装好。
+ build-essential
+ Boost (1.5.4版本 或 更高)
+ CMake (2.8.3版本 或 更高) 
+ Eigen (3.3.9版本) 
+ LCM (1.4.0版本) 
+ RBDL (2.6.0版本) 

为了方便用户使用，我们也在z1_sdk/thirdparty/目录下提供了Eigen及RDBL两个库包，解压后可以直接根据以下步骤进行安装。

#### RBDL安装

编译安装
```
cd rbdl-2.6.0
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=release ..
sudo make install
```
设置文件路径
```
sudo su    (input password)
sudo echo /usr/local/lib >> /etc/ld.so.conf
sudo ldconfig
```
#### Eigen安装
如果已安装旧版本Eigen，可以先卸载，如果没有可以直接进行下一步安装。

卸载旧版本
```
cd /usr/include
sudo rm -rf ./eigen3
```
编译安装
```
cd eigen-3.3.9
mkdir build
cd build
cmake ..
sudo make install

sudo ln -s /usr/local/include/eigen3  /usr/include/eigen3
sudo ln -s /usr/local/include/eigen3/Eigen  /usr/local/include/Eigen
```
