# A-Loam_movebase_scout2_nav
A-loam纯Robosense SLAM建图，movebase导航，scout2松灵小车导航
# 环境ubuntu20.04 ROS1 noetic pcl 1.12 ceres 1.14
## Step1
```
    cd ~/catkin_ws/src
    git clone https://github.com/HKUST-Aerial-Robotics/A-LOAM.git
    cd ../
    catkin_make
```
## Step2
```
sudo gedit /etc/apt/sources.list
deb http://cz.archive.ubuntu.com/ubuntu trusty main universe
```
 
```
sudo apt-get install liblapack-dev libsuitesparse-dev libcxsparse3.1.2 libgflags-dev
sudo apt-get install libgoogle-glog-dev libgtest-dev
```
## Step3
```
wget ceres-solver.org/ceres-solver-1.14.0.tar.gz
tar -zxvf ceres-solver-1.14.0.tar.gz
```
## Step4
```
cd ceres-solver-1.14.0 
mkdir build
cd build
cmake .. 
make -j4
sudo make install
```
# 修改源码：
```
//改A-LOAM下的CMakeLists.txt
 
c++11 改为 c++14
 
 
//改A-LOAM/src/kittiHelper.cpp
 
CV_LOAD_IMAGE_GRAYCALE 改为 cv::IMREAD_GRAYSCALE       //91行和93行
 
 
//改A-LOAM/src/scanRegistration.cpp里的
 
#include <opencv/cv.h> 改为 #include <opencv2/imgproc.hpp>
 
 
//改A-LOAM/src/三个cpp文件laserMapping.cpp和scanRegistration.cpp和laserOdometry.cpp
 
//使用Ctrl+H快捷键快速替换
 
/camera_init 改为 camera_init

```
![image](https://github.com/user-attachments/assets/f4731bcb-3e86-4160-b58e-2fb0a7f2e5ef)

```
//将scanRegistration.cpp中的sub接口改一下
 
ros::Subscriber subLaserCloud = nh.subscribe<sensor_msgs::PointCloud2>("/velodyne_points", 100, laserCloudHandler);
 
//改为
 
ros::Subscriber subLaserCloud = nh.subscribe<sensor_msgs::PointCloud2>("/points_raw", 100, laserCloudHandler);
 
//改完后保存
```

