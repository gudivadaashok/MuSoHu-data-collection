#MuSoHu Build Instructions


### Install VNC server 


What is UFW? UFW (Uncomplicated Firewall) is a user-friendly interface for managing firewall rules on Linux systems.

```bash
sudo apt update
sudo apt install tightvncserver
sudo apt install ufw
sudo ufw enable  # Enable the firewall
sudo ufw allow 5901  # Allow VNC port through firewall
```

For a complete GUI + VNC setup guide (x11vnc, TigerVNC, or GNOME RDP), see `docs/UI_VNC_Setup.md`.

### Change computer name 
```bash
sudo nano /etc/hostname
```
Change the name to your desired computer name and save the file. Then run:

```bash
sudo hostnamectl set-hostname NEW_NAME
sudo reboot
```

### Set up Wi-Fi Hotspot on Jetson Nano






### This will be password for the Nivida Jetson Nano 
Computer_Name: Robotixx-MuSoHu-1
Username: jetson
Password: Robotixx




## Install ROS Humble Hawksbill


[ROS Humble Hawksbill Installation Guide](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html)




## Stereo camera ZED2

Steps to install the ZED2 stereo camera driver and visualize the data using ROS and RViz.

check your Ubuntu version:
```bash
lsb_release -a
```

Install the SDK from [here](https://www.stereolabs.com/developers/release/)

https://download.stereolabs.com/zedsdk/5.0/cu11_trt8/ubuntu20


### Fixing Browser Issue on Jetson Orin Nano

If you encounter a browser issue on the Jetson Orin Nano, you can resolve it by installing a specific version of `snapd`:

1. Download the required revision of `snapd`:
   ```bash
   snap download snapd --revision=24724
   ```

2. Acknowledge the downloaded snap package:
   ```bash
   sudo snap ack snapd_24724.assert
   ```

3. Install the snap package:
   ```bash
   sudo snap install snapd_24724.snap
   ```




### Add ROS 2 to PATH

To ensure the `ros2` command is available, add the ROS 2 setup script to your PATH:

```bash
echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc && source ~/.bashrc
```

This command appends the sourcing of the ROS setup script to your `~/.bashrc` file and reloads it immediately.



