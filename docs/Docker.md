# Use Docker to Set Up ROS2 Environment with All Drivers

## Prerequisites
- Pull the Docker image that contains ROS2 Humble and necessary dependencies:
  ```bash
  docker pull ros:humble
  ```
  - Save the image data on your local machine:


## Run a ROS 2 Container
```bash
docker volume create musohu_persistent_data
docker run -it --privileged --name ros2_musohu \
  -v "$(pwd)":/workspace \
  -w /workspace \
  -v musohu_persistent_data:/data \
  ros:humble bash
```


## Run With Workspace Mount
From the repo root:
```bash
docker run -it --privileged --name ros2_musohu \
  -v "$(pwd)":/workspace \
  -w /workspace \
  -v musohu_persistent_data:/data \
  ros:humble bash
```

## Reattach To Existing Container
```bash
docker start ros2_musohu
docker exec -it ros2_musohu bash
```

## Remove Container
```bash
docker rm -f ros2_musohu
```

## Persistent Data Volume (Optional)
To persistently store logs, bag files, or other outputs from your container, create and mount a Docker volume named `musohu_persistent_data`:

```bash
docker volume create musohu_persistent_data

docker run -it --privileged --name ros2_musohu \
  -v "$(pwd)":/workspace \
  -w /workspace \
  -v musohu_persistent_data:/data \
  ros:humble bash
```

- All files written to `/data` inside the container will be saved in the `musohu_persistent_data` volume and persist even if the container is removed.
- Use this for ROS bag files, logs, or any output you want to keep between runs.

To copy data out:
```bash
docker cp ros2_musohu:/data ./local_data
```

To inspect the volume:
```bash
docker volume inspect musohu_persistent_data
```


## Networking Notes (Mac)
Host networking (-–network host) is not supported on Docker Desktop for Mac. For nodes exposing TCP/HTTP ports map them:
```bash
docker run -it --privileged --name ros2_dev \
  -p 8080:8080 \
  -v "$(pwd)":/workspace -w /workspace \
  -v musohu_persistent_data:/data \
  ros:humble bash
```
ROS 2 DDS traffic (UDP multicast) may need same LAN / use CycloneDDS config if discovery issues occur.


## Preserve Bash History
```bash
docker run -it --privileged --name ros2_musohu \
  -v "$(pwd)":/workspace -w /workspace \
  -v ros2_bash_history:/root \
  -v musohu_persistent_data:/data \
  ros:humble bash
```

## Build a Custom Image (Add Drivers Later)
Create Dockerfile:
```Dockerfile
FROM ros:humble
RUN apt-get update && apt-get install -y \
    python3-colcon-common-extensions git \
    && rm -rf /var/lib/apt/lists/*
# COPY driver source or clone repos here
```
Build:
```bash
docker build -t ros:humble-drivers .
```
Run:
```bash
docker run -it --privileged --name ros2_musohu \
  -v "$(pwd)":/workspace -w /workspace \
  -v musohu_persistent_data:/data \
  ros:humble-drivers bash
```

## Source ROS 2 Inside Container
```bash
source /opt/ros/humble/setup.bash
```

## Build Your ROS 2 Workspace
Assuming src/ exists in mounted workspace:
```bash
colcon build --symlink-install
source install/setup.bash
```
