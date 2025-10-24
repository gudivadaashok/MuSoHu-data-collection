# ZED SDK Docker Setup

This guide shows how to run ZED SDK in a Docker container for the MuSoHu data collection system.

## Prerequisites

- Docker installed
- NVIDIA GPU with CUDA support
- NVIDIA Docker runtime (`nvidia-docker2`)
- ZED camera connected via USB

## Install NVIDIA Docker Runtime

```bash
# Add NVIDIA package repositories
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

# Install nvidia-docker2
sudo apt-get update
sudo apt-get install -y nvidia-docker2

# Restart Docker daemon
sudo systemctl restart docker
```

## Pull ZED SDK Docker Image

```bash
# For runtime (production)
docker pull stereolabs/zed:4.0-runtime-cuda11.7-ubuntu22.04

# For development
docker pull stereolabs/zed:4.0-devel-cuda11.7-ubuntu22.04
```

## Run ZED SDK Container

### Basic Usage

```bash
docker run --gpus all -it --privileged stereolabs/zed:4.0-runtime-cuda11.7-ubuntu22.04
```

### With ROS2 Integration

```bash
docker run -d \
  --name zed_ros2 \
  --gpus all \
  --privileged \
  -p 6080:80 \
  -v "$(pwd)":/workspace \
  --shm-size=512m \
  --network host \
  stereolabs/zed:4.0-devel-cuda11.7-ubuntu22.04
```

### Access ZED Camera Test

Inside the container:

```bash
# Test ZED camera connection
/usr/local/zed/tools/ZED_Explorer

# Or run a simple viewer
/usr/local/zed/tools/ZED_Depth_Viewer
```

## Available Docker Images

| Image Type | Use Case | Tag Example |
|------------|----------|-------------|
| Runtime | Production deployment | `4.0-runtime-cuda11.7-ubuntu22.04` |
| Development | Building applications | `4.0-devel-cuda11.7-ubuntu22.04` |
| ROS2 | ROS2 integration | `4.0-ros2-humble-cuda11.7-ubuntu22.04` |

## Troubleshooting

### Camera Not Detected

```bash
# Check USB connection
lsusb | grep Stereolabs

# Verify permissions
sudo chmod 777 /dev/video*
```

### GPU Not Available

```bash
# Test NVIDIA runtime
docker run --rm --gpus all nvidia/cuda:11.7.0-base-ubuntu22.04 nvidia-smi
```

## Integration with MuSoHu System

To integrate ZED camera with the existing ROS2 VNC setup:

```bash
# Stop existing container if running
docker stop ros2_vnc

# Run combined container with ZED support
docker run -d \
  --name musohu_zed \
  --gpus all \
  --privileged \
  -p 6080:80 \
  -v "$(pwd)":/workspace \
  --shm-size=1024m \
  --network host \
  stereolabs/zed:4.0-ros2-humble-cuda11.7-ubuntu22.04
```

## References

- [ZED SDK Docker Documentation](https://www.stereolabs.com/docs/development/zed-sdk/docker)
- [ZED Docker Hub](https://hub.docker.com/r/stereolabs/zed)
- [ZED SDK Documentation](https://www.stereolabs.com/developers/release/)
