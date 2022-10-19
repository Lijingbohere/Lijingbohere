# ORB_SLAM2部署

## 运行环境

- 硬件环境：Jetson nano 、 STME32
- 软件环境：Ubuntu18.04、ROS melodic

## 安装依赖

### Eigen

Eigen库是矩阵运算需要的库，最低版本要求3.1.0，可以使用命令行安装如下：

```linux
sudo apt-get install libeigen3-dev
```

安装前可以查看，一般在安装好软件环境后，能查询到Eigen3.3.4版本

### 安装OpenCV

安装OpenCV3.2以上版本

### 安装Pangolin

### 安装Ceres

安装Ceres须与Eigen版本匹配，建议先安装Eigen3.3.4，后安装Ceres1.14.0与之搭配

## 安装ORB_SLAM2

```
mkdir -p ~/orbslam2_workspace/src
cd ~/orbslam2_workspace/src
catkin_init_workspace
```

下载orb_slam2到src文件夹中

```
cd ~/orbslam2_workspace/src
git clone  #如果通过命令行下载不下来（一般需要fq才能下下来），可以通过其他途径下载好后把文件解压后放进去
```

之后进入~/orbslam2_workspace/src/orb_slam2

里面Thirdparty文件夹中包含了DBoW2和g2o文件，需要对其进行编译安装

```
// 编译安装DBoW2
cd Thirdparty/DBoW2
mkdir build
cd build
cmake ..
make

// 编译安装g2o
cd Thirdparty/g2o
mkdir build
cd build
cmake ..
make
```

还需要解压Vocabulary文件夹中的ORBvoc.txt.tar.gz

之后修改Examples/ROS/ORB_SLAM2文件夹中的CMakeLists.txt

```
-lboost_system.so
```

还需修改

。。。

添加

```
#include <unistd.h>
```

之后

```
chmod +x build_ros.sh
./build_ros.sh
```



## 通过外部摄像头实时接收数据

