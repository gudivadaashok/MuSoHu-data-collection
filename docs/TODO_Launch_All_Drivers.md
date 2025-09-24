# TODO Document: Launch All Drivers and Record Data into a ROS2 Bag File

## Objective
Set up a unified ROS2 launch file to start all sensor drivers (ZED2, LiDAR, IMU, Microphone Array) and simultaneously record their data into a ROS2 bag file.

---

## Steps to Complete

### 1. Set Up the Workspace
- Create a ROS2 workspace if not already set up:
  ```bash
  mkdir -p ~/ros2_ws/src
  cd ~/ros2_ws
  ```
- Clone or create the necessary packages for the drivers:
  - ZED2: `zed_wrapper`
  - LiDAR: `velodyne_driver`
  - IMU: Specific driver (e.g., `microstrain_inertial_driver`)
  - Microphone Array: `audio_common`

### 2. Install Dependencies
- Use `rosdep` to install dependencies for all packages:
  ```bash
  cd ~/ros2_ws
  rosdep install --from-paths src --ignore-src -r -y
  ```

### 3. Write the Unified Launch File
- Create a package for the launch file (if not already created):
  ```bash
  ros2 pkg create my_robot_bringup --build-type ament_cmake
  ```
- Create a `launch` directory in the package:
  ```bash
  mkdir -p ~/ros2_ws/src/my_robot_bringup/launch
  ```
- Write the unified launch file (`all_sensors_with_bag.launch.py`) to:
  - Launch all sensor nodes.
  - Start a `ros2 bag record` process to record data from all relevant topics.

### 4. Verify Sensor Topics
- Identify the topics published by each sensor:
  - ZED2: `/zed2/left/image_raw`, `/zed2/right/image_raw`, `/zed2/depth`
  - LiDAR: `/velodyne_points`
  - IMU: `/imu/data`
  - Microphone Array: `/audio`
- Update the `ros2 bag record` command in the launch file to include these topics.

### 5. Build the Workspace
- Build the workspace to include the new launch file:
  ```bash
  cd ~/ros2_ws
  colcon build
  ```

### 6. Test the Launch File
- Source the workspace:
  ```bash
  source install/setup.bash
  ```
- Run the launch file:
  ```bash
  ros2 launch my_robot_bringup all_sensors_with_bag.launch.py
  ```
- Verify that:
  - All sensor nodes are running.
  - Data is being recorded into a ROS2 bag file.

### 7. Validate the Bag File
- After stopping the launch file, check the bag file:
  ```bash
  ros2 bag info sensor_data_bag
  ```
- Ensure all topics are recorded correctly.

### 8. Optimize and Finalize
- Add any missing parameters or configurations for the sensors.
- Test the setup in different environments to ensure reliability.
- Document the process for future reference.

---

## Deliverables
- A working launch file: `all_sensors_with_bag.launch.py`
- A ROS2 bag file containing data from all sensors.



## Sensor List

- ZED2 Camera
- LiDAR Robosense Model: H32F70
- ReSpecker Microphone Array
- IMU WIT Motion WT901 BLE CL5.0

## Hardware Platform
Jetson Orion Nano 8GB Dev Kit

## Operating System
Ubuntu 22.04

## ROS2 Version
ROS2 Humble Hawksbill





Topic List to be recorded in the bag file:

```
topics:      /action                                             7858 msgs    : nav_msgs/Odometry                      
             /rosout                                              977 msgs    : rosgraph_msgs/Log                       (4 connections)
             /rosout_agg                                          961 msgs    : rosgraph_msgs/Log                      
             /scan                                               2972 msgs    : sensor_msgs/LaserScan                  
             /tf                                                29906 msgs    : tf2_msgs/TFMessage                     
             /tf_static                                             2 msgs    : tf2_msgs/TFMessage                      (2 connections)
             /velodyne_points                                    2972 msgs    : sensor_msgs/PointCloud2                
             /zed2/zed_node/depth/camera_info                    6313 msgs    : sensor_msgs/CameraInfo                 
             /zed2/zed_node/depth/depth_registered/compressed    6313 msgs    : sensor_msgs/CompressedImage            
             /zed2/zed_node/imu/data                            13996 msgs    : sensor_msgs/Imu                        
             /zed2/zed_node/odom                                 8612 msgs    : nav_msgs/Odometry                      
             /zed2/zed_node/path_map                              600 msgs    : nav_msgs/Path                          
             /zed2/zed_node/path_odom                             600 msgs    : nav_msgs/Path                          
             /zed2/zed_node/pose                                 8610 msgs    : geometry_msgs/PoseStamped              
             /zed2/zed_node/pose_with_covariance                 8608 msgs    : geometry_msgs/PoseWithCovarianceStamped
             /zed2/zed_node/rgb/camera_info                     12135 msgs    : sensor_msgs/CameraInfo                 
             /zed2/zed_node/rgb/image_rect_color/compressed      6312 msgs    : sensor_msgs/CompressedImage
```



## Videos that show how to install the drivers and launch files for each sensor

   - [ZED2 Installation and Launch](https://www.youtube.com/watch?v=k1-naauRSz0)
   - [LiDAR Installation and Launch](https://www.example.com/lidar-install)
   - [Microphone Array Installation and Launch](https://www.example.com/microphone-install)
   - [IMU Installation and Launch](https://www.youtube.com/watch?v=ddnU1vAlg_I&t=2s)



## GitHub Repositories for each Driver
- [witmotion_ros2](https://github.com/ioio2995/witmotion_ros2.git)
https://wit-motion.gitbook.io/witmotion-sdk/wit-standard-protocol/sdk/ros-python-introduction
- [zed-ros2-wrapper](https://github.com/stereolabs/zed-ros2-wrapper.git)
- [respeaker_ros2](https://github.com/hcrlab/respeaker_ros.git)
- [LiDAR Robosense H32F70](https://github.com/RoboSense-LiDAR/rslidar_sdk.git)