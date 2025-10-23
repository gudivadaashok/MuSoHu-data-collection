# 1. Pull the ROS2 image with VNC already configured
docker pull tiryoh/ros2-desktop-vnc:humble

# 2. Run the container with VNC and ROS2
docker run -d \
  --name ros2_vnc \
  -p 6080:80 \
  -v "$(pwd)":/workspace \
  --shm-size=512m \
  --security-opt seccomp=unconfined \
  tiryoh/ros2-desktop-vnc:humble

# 3. Access via browser
# Open: http://localhost:6080

# 4. Inside the VNC browser session, open a terminal and run ROS2 applications
# The ROS2 environment is already sourced
# Example:
ros2 run turtlesim turtlesim_node
# Or
rviz2

### this is working 