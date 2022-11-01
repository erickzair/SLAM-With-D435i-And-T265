# SLAM-With-D435i-And-T265
SLAM 3D With RealSense™ Cameras D435i &amp; T265 

## First Steps
This tutorial is based on the melodic ROS1 distribution for ubuntu 18.04 LTS
(it is compatible with all distributions of ROS1 with small changes).

- ⚙️ To find out what version of Ubuntu you have, use: `lsb_release -a`
and to find out what distribution of ROS you have, use: `rosversion -d`

If you don't have a ROS1 distribution you can install it through the following links:
 
- Install ROS Kinetic on [Ubuntu 16.04](http://wiki.ros.org/kinetic/Installation/Ubuntu).
- Install ROS Melodic on [Ubuntu 18.04](http://wiki.ros.org/melodic/Installation/Ubuntu).
- Install ROS Noetic on [Ubuntu 20.04](http://wiki.ros.org/noetic/Installation/Ubuntu). 

### Installation:
The D435i & T265 cameras allow to perform SLAM (mapping and localization) To perform 3D SLAM with both cameras, using the odometry of the T265 and the RGBD of the D435i camera, the following packages must be installed: 

-**Intel Realsense:** [GitHub IntelRealsense Ros](https://github.com/IntelRealSense/realsense-ros/tree/ros1-legacy).
-**imu_filter_madgwick:** ```sudo apt-get install ros-kinetic-imu-filter-madgwick```.
-**rtabmap_ros:** ```sudo apt-get install ros-kinetic-rtabmap-ros```.
-**robot_localization:** ```sudo apt-get install ros-kinetic-robot-localization```.


