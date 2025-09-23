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