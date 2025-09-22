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



## Stereo camera ZED2

Steps to install the ZED2 stereo camera driver and visualize the data using ROS and RViz.

check your Ubuntu version:
```bash
lsb_release -a
```

Install the SDK from [here](https://www.stereolabs.com/developers/release/)

https://download.stereolabs.com/zedsdk/5.0/cu11_trt8/ubuntu20



