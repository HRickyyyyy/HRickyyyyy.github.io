# ROS_canopen安装及使用
这篇文章记录了ros_canopen的安装和使用过程，系统版本为ubuntu16.04 并且已经安装了ROS（kienect)。安装过程可能会出现错误，一般是由于缺少依赖包引起的。
## 一、源码安装canopen
### 1.创建一个文件夹
$ mkdir -p canopen/src
$ cd canopen/
$ catkin_make
### 2.从官网下载canopen至Ubuntu，下载地址为：https://github.com/ros-industrial/ros_canopen/tree/kinetic-devel （可能需要科学上网）
### 3.将canopen文件解压到/canopen/src文件目录下
### 4.进入工作空间执行编译
$ catkin_make

## 二、配置CAN
参考官网链接：http://wiki.ros.org/socketcan_interface?distro=kinetic 本项目使用的can卡是peak_usb
### 1.加载驱动
$ sudo modprobe peak_usb
### 2.初始化网卡
（本项目的线控底盘 can0 bitrate 为500K）
$ sudo ip link set can0 up type can bitrate 500000

## 三、测试
### 1.查看can端口是否打开
$ ifconfig
### 2.接收底盘发送的can数据
启动一个节点将底盘的can数据用一个ros中的话题发布
$ cd canopen/
$ source devel/setup.bash
$ rosrun socketcan_bridge socketcan_to_topic_node
### 3.查看底盘发送的can数据
底盘的can数据通过话题/received_messages节点发布，使用rostopic ehco命令即可读取can数据。
$ cd canopen/
$ source devel/setup.bash
$ rostopic echo /received_messages

### 4.成功展示
