### 1. If you have the error:

CMake Error at /opt/ros/melodic/share/catkin/cmake/catkinConfig.cmake:83 (find_package):
  Could not find a package configuration file provided by
  "ddynamic_reconfigure" with any of the following names:

    ddynamic_reconfigureConfig.cmake
    ddynamic_reconfigure-config.cmake

  Add the installation prefix of "ddynamic_reconfigure" to CMAKE_PREFIX_PATH
  or set "ddynamic_reconfigure_DIR" to a directory containing one of the
  above files.  If "ddynamic_reconfigure" provides a separate development
  package or SDK, be sure it has been installed.

You can solve this with the next line in terminal:

`sudo apt-get install ros-melodic-ddynamic-reconfigure`

And if you are using another version of ROS you can try to change the version word, for example:

`sudo apt-get install ros-kinetic-ddynamic-reconfigure`


### 2. If you have the problem:
 
You can see the image in realsense-viewer but you can't see any on rviz and present the next message:

23/04 18:13:47,017 WARNING [140493716674304] (messenger-libusb.cpp:42) control_transfer returned error, index: 768, error: Resource temporarily unavailable, number: 11

You can solve it changing the <arg name="pointcloud_texture_stream" default="RS_STREAM_COLOR"/> for <arg name="pointcloud_texture_stream" default="RS_STREAM_ANY"/>

in the directory:
/opt/ros/melodic/share/realsense2_camera/launch/rs_camera.launch

The solution was found in:

First ticket:
https://github.com/IntelRealSense/realsense-ros/issues/1663

Second ticket:
https://github.com/IntelRealSense/realsense-ros/issues/1758

You can modify it with:
`sudo gedit /opt/ros/melodic/share/realsense2_camera/launch/rs_camera.launch`

To install gedit:
`sudo apt install gedit`

### 3. If you have the problem:
Camera only extract the point cloud but is not mapping, you can put the following in the terminal:

```
cd ~/catkin_ws/src/realsense-ros/realsense2_camera/launch
sudo gedit opensource_tracking.launch
```
And change the line: 

`<arg name="args" value="--delete_db_on_start"/>`

To:
`<arg name="args" value="--delete_db_on_start --RGBD/LoopClosureReextractFeatures true --Vis/MinIniliers 10"/>`

### 4. If you have the error:
[ERROR] [1663441379.166610446]: PluginlibFactory: The plugin for class 'octomap_rviz_plugin/ColorOccupancyGrid' failed to load.  Error: According to the loaded plugin descriptions the class octomap_rviz_plugin/ColorOccupancyGrid with base class type rviz::Display does not exist. Declared types are  rtabmap_ros/Info rtabmap_ros/MapCloud rtabmap_ros/MapGraph rviz/Axes rviz/Camera rviz/DepthCloud rviz/Effort rviz/FluidPressure rviz/Grid rviz/GridCells rviz/Illuminance rviz/Image rviz/InteractiveMarkers rviz/LaserScan rviz/Map rviz/Marker rviz/MarkerArray rviz/Odometry rviz/Path rviz/PointCloud rviz/PointCloud2 rviz/PointStamped rviz/Polygon rviz/Pose rviz/PoseArray rviz/PoseWithCovariance rviz/Range rviz/RelativeHumidity rviz/RobotModel rviz/TF rviz/Temperature rviz/WrenchStamped rviz_plugin_tutorials/Imu

You have to install octomap rviz plugins:
`sudo apt-get install ros-kinetic-octomap-rviz-plugins`

### 5. If you have the error:
Err:8 http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial InRelease
  403  Forbidden [IP: ""]
Reading package lists... Done                     
E: Failed to fetch http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo/dists/xenial/InRelease  403  Forbidden [IP: ""]
E: The repository 'http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial InRelease' is not signed.

You have to open the app "Software & Updates", go to "Other Software" and remove http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo
