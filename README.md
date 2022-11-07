# SLAM-With-D435i-And-T265-In-Turtlebot2
SLAM 3D With RealSense™ Cameras D435i &amp; T265 in the Turtlebot2

## First Steps
This tutorial is based on the melodic ROS1 distribution for ubuntu 18.04 LTS
(it is compatible with all distributions of ROS1 with small changes).

- ⚙️ To find out what version of Ubuntu you have, use: `lsb_release -a`
and to find out what distribution of ROS you have, use: `rosversion -d`

If you don't have a ROS1 distribution you can install it through the following links:
 
- Install ROS Kinetic on [Ubuntu 16.04](http://wiki.ros.org/kinetic/Installation/Ubuntu).
- Install ROS Melodic on [Ubuntu 18.04](http://wiki.ros.org/melodic/Installation/Ubuntu).
- Install ROS Noetic on [Ubuntu 20.04](http://wiki.ros.org/noetic/Installation/Ubuntu). 

## Installation:
The D435i & T265 cameras allow to perform SLAM (mapping and localization) To perform 3D SLAM with both cameras, using the odometry of the T265 and the RGBD of the D435i camera, the following packages must be installed: 

- **Intel Realsense:** [GitHub IntelRealsense Ros](https://github.com/IntelRealSense/realsense-ros/tree/ros1-legacy).
- **imu_filter_madgwick:** ```sudo apt-get install ros-kinetic-imu-filter-madgwick```.
- **rtabmap_ros:** ```sudo apt-get install ros-kinetic-rtabmap-ros```.
- **robot_localization:** ```sudo apt-get install ros-kinetic-robot-localization```.

If you had some issue you can check solutions to possible errors in the file [Issues](/Issues.md)

## Configuration:
Now with all the packages installed, changes must be made so that SLAM can work correctly with both cameras.

### Rviz:
First you must change the permissions of the folder `config` of the path `/opt/ros/melodic/share/rtabmap_ros/launch/`.

- For this, use the command `cd /opt/ros/melodic/share/rtabmap_ros/launch/` and after use the command `sudo chmod 777 config/*` and now inside of the path `/opt/ros/melodic/share/rtabmap_ros/launch/config/` put the file **[rgbd.rviz](config/rgbd.rviz)** found in this repository in the folder [**config**](config), once this is done you can put the permissions back with the commands `cd ..` and `chmod 644 config/*`

**Note:** _If you do not change this file you will not be able to display the map correctly_



### Mounting:

For the "static transfer publisher" node to work correctly, the distance of the cameras in the assembly must be taken into account.
The STL of the design made based on [Intel mount](https://github.com/IntelRealSense/realsense-ros/blob/dd97d1ff5b428b06d268c8eb8516d1e4a8bc24a4/realsense2_camera/meshes/mount_t265_d435.stl) is shared into the folder [MountingD&T](MountingD&T/MountingD435i&T265.stl) in this repository, this montage is modified to be used in the turlebot 2.


![TF](https://user-images.githubusercontent.com/72427631/199650200-2b55a6b4-60c0-486c-909e-c3cdc45cecca.png)



With the D435i camera, the Depth topics, Point Cloud Topics, and RGB Topics from which the map cloud is extracted are used to perform the mapping

![Nube de puntos CAM REALSENSE D435i](https://user-images.githubusercontent.com/72427631/199648223-b0eb740d-a4ee-4bc2-b6e4-ca33e2e8b9be.gif)

And with the T265 camera, the odometry topics that are used for the location in the mapping are obtained.

### Using TurtleBot 2:

To use the TurtleBot 2 for navigation you have to go to the next link: [Turtlebot 2 on Ros melodic](https://github.com/gaunthan/Turtlebot2-On-Melodic) and follow the instructions to make it work on your computer. It was also verified that it works on the Raspberry Pi 3.


### Mapping:

To start mapping you must connect the two cameras in 3.0 ports (this is mandatory), and after having everything connected and mounted on the turtlebot2 you must download the launch [D435i_And_t265.launch](/D435i_And_t265.launch) and put it in the catkin workspace `/catkin_ws/src/realsense-ros/realsense2_camera/launch` and after open the launch with the command:

```
roslaunch realsense2_camera D435i_And_t265.launch
``` 


**If everything is configured correctly RVIZ should open and start mapping at the starting point**





https://user-images.githubusercontent.com/72427631/199746806-92fe07ae-10e0-47d8-b7ed-1765cecafe71.mp4



### Saving:

To save the rosbag with the necessary topics once starting the mapping you must put this command in a new terminal:

```
rosbag record -O silla+D+T.bag /d435i/aligned_depth_to_color/camera_info  d435i/aligned_depth_to_color/image_raw /d435i/color/camera_info /d435i/color/image_raw /t265/imu /t265/imu_info /tf_static /rtabmap/odom /rtabmap/mapData /tf /rtabmap/mapPath /rtabmap/cloud_map /rtabmap/proj_map 
```

and to play the rosbag file:
 - Open one terminal and put there `roscore`
 - Open another terminal and put `rviz` and open the config of rgbdD&T.rviz
 - Put the next code:

```
roscore >/dev/null 2>&1 &
rosparam set use_sim_time true
rosbag play NAME_ROS_BAG.bag --clock
```

To save the 2D map of what has already been mapped use the command:
`rosrun map_server map_saver map:=/rtabmap/grid_map -f my_map_1 --occ 90 --free 10`

![my_map_2](https://user-images.githubusercontent.com/72427631/199769783-8bfb7d54-93d4-432d-b32f-eced447b134f.jpg)

To save the 3D point cloud use the command:
`rosrun pcl_ros pointcloud_to_pcd input:=/rtabmap/cloud_map`
